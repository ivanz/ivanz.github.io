---
title: "FluentMigrator Part 1/3: Creating and running our first database migration"
author: Ivan Zlatev
layout: post
categories:
  - Coding
---

# In this series

* FluentMigrator Part 1/3: Creating and running our first database migration (this post)
* [FluentMigrator Part 2/3: Continuous Integration and Testing with database migrations](/2015/09/13/fluentmigrator-part-2/)
* FluentMigrator Part 3/3: Continuous Delivery with database migrations


In this post we are going to cover:

* What FluentMigrator is
* How to create a database migration
* How to run our database migration and then roll it back

# What is FluentMigrator

[FluentMigrator](https://github.com/schambers/fluentmigrator/) is a migration framework for .NET much like Ruby on Rails Migrations. Migrations are a structured way to alter your database schema and are an alternative to creating lots of sql scripts that have to be run manually by every developer involved. Migrations solve the problem of evolving a database schema for multiple databases (for example, the developer’s local database, the test database and the production database). Database schema changes are described in classes written in C# that can be checked into a version control system. [[source](https://github.com/schambers/fluentmigrator/blob/master/README.textile)]

# Before we start

* You can download all the code from this post at: [https://github.com/ivanz/FluentMigratorSample](https://github.com/ivanz/FluentMigratorSample)
* You will need either a SQL Server Express, SQL Server or an Azure SQL Database if you want to run the example.


## Our Visual Studio solution

Let's say we are building a Web App to catalogue cars. Our solution contains:

* `Domain` - class library for our model and data access
* `WebApp`- Simple MVC app
* `DatabaseMigrations` - class library for our database migrations

This is what it looks like in Visual Studio:

![](/content/2015-09-12-fluentmigrator-part1/solution.png)

And this is what it looks like when we run it:

![](/content/2015-09-12-fluentmigrator-part1/webapp.png)

**Note:** Any Entity Framework automatic database initialization/migration has been disabled in the data context. 

## Domain model and initial database schema

Our domain model:

```csharp
public class Car
{
    public int Id { get; set; }
    public string Model { get; set; }
}
```

And our initial schema that matches that. Let's assume for now that this schema is already in place in the database. We will revisit that later. 

```sql
USE [master]
CREATE DATABASE [CarsDb]

USE [CarsDb]
CREATE TABLE [dbo].[Cars] (
    [Id]    INT            NOT NULL IDENTITY,
    [Model] NVARCHAR (MAX) NOT NULL,
    PRIMARY KEY CLUSTERED ([Id] ASC)
);

```

# Creating our first database migration

Let's add a new property to the our `Car` - let's say `Make`, so now it looks like this:

```csharp
public class Car
{
    public int Id { get; set; }
    public string Model { get; set; }

	// new:
    public string Make { get; set; }
}
```

To create our first database migration we need to add the [FluentMigrator NuGet package](https://www.nuget.org/packages/FluentMigrator) to our `DatabaseMigrations` project. 

Then we can use the fluent syntax to change the database schema to include the new column:

`201509121634_Add_Make_Column_To_Car.cs`

```csharp
[Migration(201509121634)]
public class Add_Make_Column_To_Car : Migration
{
    public override void Up()
    {
        Create.Column("Make").OnTable("Cars").AsString(int.MaxValue).Nullable();
    }

    public override void Down()
    {
        Delete.Column("Make").FromTable("Cars");
    }
}
```

## Anatomy of a database migration

Every migration has a version and the `MigrationAttribute` enables us to define the version of the database migration.

Using a timestamps-based version number (when the migration file was created) in the format of `YearMonthDateHourMinute` works really well for both the file name and the migration version, because:

* Files in Visual Studio are displayed in the order they were created
* Version number clashes are extremely unlikely

`Up`/`Down` methods (unless it's a `ForwardOnlyMigration`) - to upgrade and downgrade the schema.

The fluent syntax of FluentMigrator supports pretty much everything you can think off:

* Creating, altering columns, tables, foreign keys, indices, etc. .
* Executing SQL statements through `Execute.Sql("statements")`
* Executing embedded `.sql` files through `Execute.Script("filename")` / `Execute.EmbeddedScript("filename")`.


# Running our first migration and rolling it back

There are many ways to run the database migration we just created:

* Command Line
* PowerShell script
* MSbuild task
* NAnt task

Let's use the command line tool provided by the [FluentMigrator.Tools NuGet package](https://www.nuget.org/packages/FluentMigrator.Tools).

Once installed `Migrator.exe` can be found at `packages\FluentMigrator.Tools.1.6.0\tools\AnyCPU\40` and to migrate our database to the latest version we just have to point it to our database and supply the assembly that contains the database migrations.

```batch
"packages\FluentMigrator.Tools.1.6.0\tools\AnyCPU\40\Migrate.exe"^
 -a DatabaseMigrations\bin\Debug\DatabaseMigrations.dll^
 -provider SqlServer^
 -connection="Server=.;Database=CarsDb;Integrated Security=True;"

-------------------------------------------------------------------------------
=============================== FluentMigrator ================================
-------------------------------------------------------------------------------
Source Code:
  http://github.com/schambers/fluentmigrator
Ask For Help:
  http://groups.google.com/group/fluentmigrator-google-group
-------------------------------------------------------------------------------
[+] Using Database SqlServer and Connection String Server=.;Database=CarsDb;Integrated Security=True;
-------------------------------------------------------------------------------
201509121634: Add_Make_Column_To_Car migrating
-------------------------------------------------------------------------------
[+] Beginning Transaction
[+] CreateColumn Cars Make String
[+] Committing Transaction
[+] 201509121634: Add_Make_Column_To_Car migrated
[+] Task completed.
```

In the background this will create a new `VersionInfo` table that Fluent Migrator will use to track which version the database schema is at:

![](/content/2015-09-12-fluentmigrator-part1/versioninfo-table.png)

And to rollback one version:

```batch
"packages\FluentMigrator.Tools.1.6.0\tools\AnyCPU\40\Migrate.exe"^
 --task rollback^
 --steps=1^
 -a DatabaseMigrations\bin\Debug\DatabaseMigrations.dll^
 -provider SqlServer^
 -connection="Server=.;Database=CarsDb;Integrated Security=True;"

-------------------------------------------------------------------------------
=============================== FluentMigrator ================================
-------------------------------------------------------------------------------
Source Code:
  http://github.com/schambers/fluentmigrator
Ask For Help:
  http://groups.google.com/group/fluentmigrator-google-group
-------------------------------------------------------------------------------
[+] Using Database SqlServer and Connection String Server=.;Database=CarsDb;Integrated Security=True;
-------------------------------------------------------------------------------
201509121634: Add_Make_Column_To_Car reverting
-------------------------------------------------------------------------------
[+] Beginning Transaction
[+] DeleteColumn Cars Make
[+] Committing Transaction
[+] 201509121634: Add_Make_Column_To_Car reverted
[+] Task completed.
```

# Summary

That's pretty much the high level overview and in the next post we are going to look at how do we utilize FluentMigrator in a Continuous Integration scenario. 

