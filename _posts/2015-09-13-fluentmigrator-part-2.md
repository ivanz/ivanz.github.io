---
title: "FluentMigrator Part 2/3: Continuous Integration and Testing with database migrations"
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

# In this series

* [FluentMigrator Part 1/3: Creating and running our first database migration](/2015/09/12/fluentmigrator-part-1/)
* FluentMigrator Part 2/3: Continuous Integration and Testing with database migrations (this post)
* FluentMigrator Part 3/3: Continuous Delivery with database migrations

Having created and ran our first database migration - in this post we are going to cover the following scenario:

* We want to write integration tests that hit a real database in our local developer environment
* We want to also run those tests on our build server as part of every check-in

# The Plan

We want to be able to do the following:

* Create a temporary unique database for use with our test fixture only
* Migrate the database to the latest version to match the code
* Run the tests
* Delete the temporary database

The end goal is to be able to write tests like this using a nicely reusable base test fixture that will do all the magic for us.

```C#
public class SavingCarsNUnitTest : DatabaseTestFixtureBase
{

    [Test]
    public void When_a_car_is_saved_in_the_database_it_should_succeed()
    {
        CarsDbContext context = new CarsDbContext(base.ConnectionString);
        context.Cars.Add(new Car() {
            Make = "make",
            Model = "model",
        });
        context.SaveChanges();
    }
}

[TestFixture]
public abstract class DatabaseTestFixtureBase
{
    private IntegrationTestLifecycle _testLifeCycle = new IntegrationTestLifecycle();

    protected string ConnectionString {
        get { return _testLifeCycle.ConnectionString; }
    }

    [TestFixtureSetUp]
    public virtual void SetUpFixture()
    {
        _testLifeCycle.SetUp(Settings.Default.ConnectionStringTemplate);
    }

    [TestFixtureTearDown]
    public virtual void TearDownFixture()
    {
        _testLifeCycle.TearDown();
        _testLifeCycle.Dispose();
    }
}
```

# Automating the creation and deletaion of the database for the test fixture

The first problem that we face is that we need a database to run our tests against. However:

* We need a *temporary* database to avoid sharing it across test runs and thus inconsistent states and false positives
* Whichever way we automate the database creation process - we need to tell our tests where to connect to.

The way we've solved this problem at [MarketInvoice](https://marketinvoice.com) is by:

1. Running a SQL Server instance (free through MSDN) to be used by our integration tests
2. We've built an `IntegrationTestLifecycle` component that:
	* Creates a temporary database with a unique name
	* Provides a connection string to the consumer
	* Cleans up and deletes the temporary database on request

Let's dive in the code of the `IntegrationTestLifecycle` class a bit...


```C#
public class IntegrationTestLifecycle : IDisposable
{
// <..snip...>
    public string ConnectionString { get; private set; }

    public void SetUp(string connectionStringTemplate)
    {
        string uniqueDatabaseConnectionString = GenerateUniqueDatabaseName(connectionStringTemplate);

        CreateDatabase(uniqueDatabaseConnectionString);

        DatabaseMigrationRunner.MigrateDatabase("SqlServer", uniqueDatabaseConnectionString);



        ConnectionString = uniqueDatabaseConnectionString;
    }
// <..snip...>
}
```

It's pretty straightforward what's happening here:

1. We are generating a unique database name to use:

	```C#
	private static string GenerateUniqueDatabaseName(string originialConnectionString)
	{
	    SqlConnectionStringBuilder connectionInfo = new SqlConnectionStringBuilder(originialConnectionString);
	    connectionInfo.InitialCatalog = String.Format(CultureInfo.InvariantCulture, 
                                                     "{0}_{1}", 
                                                     connectionInfo.InitialCatalog, 
                                                     Guid.NewGuid().ToString());
	
	    return connectionInfo.ToString();
	}
	```

2. We are creating a new database on the database server using that name:

	```C#
	private void CreateDatabase(string connectionString)
	{
	    SqlConnectionStringBuilder connectionInfo = new SqlConnectionStringBuilder(connectionString);
	
	    SqlConnectionStringBuilder master = new SqlConnectionStringBuilder(connectionString) {
	        InitialCatalog = "master"
	    };
	
	    using (SqlConnection sqlConnection = new SqlConnection(master.ToString()))
	    using (SqlCommand createDbCommand = sqlConnection.CreateCommand()) {
	        sqlConnection.Open();
	        createDbCommand.CommandTimeout = DEFAULT_SQL_COMMAND_TIMEOUT;
	        createDbCommand.CommandText = String.Format(
	@"CREATE DATABASE [{0}] 
	ALTER DATABASE [{0}] SET ALLOW_SNAPSHOT_ISOLATION ON 
	ALTER DATABASE [{0}] SET RECOVERY SIMPLE", connectionInfo.InitialCatalog);
	        createDbCommand.ExecuteNonQuery();
	        sqlConnection.Close();
	    }
	}
	```

3. We are migrating the database to the latest version (covered a bit later in this post)
4. We are exposing the brand new connection string to the consumer of the component.


# Migrating the newly created database


In my previous blog post we looked at how to write a new migration on top of an existing schema. In the case of our integration tests' life-cycle we are creating a new database, so we need to import the schema. We can do that by creating a version `0` migration.

To do that we are going to:

1. Export the current schema using SQL Server Management Studio using the `Generate Scripts` functionality

 ![](/content/2015-09-13-fluentmigrator-part2/export-schema.png)


2. Remove any `GO`, `USE` statements and tell it not to generate the "CREATE DATABASE" statement. It will end up looking like this:

	```SQL
	SET ANSI_NULLS ON
	
	SET QUOTED_IDENTIFIER ON
	
	CREATE TABLE [dbo].[Cars](
		[Id] [int] IDENTITY(1,1) NOT NULL,
		[Model] [nvarchar](max) NOT NULL,
	PRIMARY KEY CLUSTERED 
	(
		[Id] ASC
	)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
	) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY]
	```

3. Add the `.sql` file as `0_CreateSchema.sql` in our `DatabaseMigrations` project and set it to be an **Embedded Resource** (note how the file name will match our migration name):

 ![](/content/2015-09-13-fluentmigrator-part2/migration.png)

4. Write our migration:

	```C#
	[Migration(0)]
	public class _0_CreateSchema : ForwardOnlyMigration
	{
	    public override void Up()
	    {
	        Execute.EmbeddedScript("0_CreateSchema.sql");
	    }
	}
	```

5. Finally let's expose a simple Migration runner from our `DatabaseMigrations` project using the `FluentMigrator.Runners` NuGet package:

	```C#
	public class DatabaseMigrationRunner
	{
	    public static bool MigrateDatabase(string databaseType, string connectionString, Assembly migrationsAssembly)
	    {
	        try {
	            RunnerContext runnerContext = new RunnerContext(new TextWriterAnnouncer(Console.Out)) {
	                Database = databaseType,
	                Connection = connectionString,
	                Targets = new[] { migrationsAssembly.Location },
	            };
	
	            new TaskExecutor(runnerContext).Execute();
	        } catch {
	            return false;
	        }
	
	        return true;
	    }
	}
	```

That's it:

![](/content/2015-09-13-fluentmigrator-part2/tests.png)

# Downloading and running the sample code

You can find all code in this article at: [https://github.com/ivanz/FluentMigratorSample](https://github.com/ivanz/FluentMigratorSample)

To edit the SQL Server used by the tests edit the `Settings.settings` file.

# Speeding up test runs

The way we speed up test runs is by:

* Running tests in multiple assemblies at the same time (For MSpec we use our [parallel runner for TeamCity](https://github.com/ivanz/Machine.Specifications.TeamCityParallelRunner))
* Sharing a database instance between a group of common tests.

