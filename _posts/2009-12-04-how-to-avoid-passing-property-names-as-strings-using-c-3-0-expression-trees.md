---
title: 'How to avoid passing property names as strings using C# 3.0 Expression Trees'
author: Ivan Zlatev
layout: post
permalink: /2009/12/04/how-to-avoid-passing-property-names-as-strings-using-c-3-0-expression-trees/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 19411
dsq_thread_id:
  - 49817459
categories:
  - Coding
tags:
  - .NET
  - 'C#'
  - NHibernate
---
Referencing property names via strings is evil. Consider this simplistic example:

<pre class="brush: csharp; title: ; notranslate" title="">private int _myProperty;

public int MyProperty
{
    get { return _myProperty; }
    set {
        _myProperty = value;
        NotifyPropertyChanged (this, "MyProperty")
    }
}

// Somewhere in a different project or file...
private void NotifyPropertyChanged (object sender, string propertyName)
{
    if (propertyName == "MyProperty")
        Console.WriteLine ("Property Changed");
}
</pre>

If at some point the property gets renamed the code will compile fine but a bug will be introduced as all those strings that contain the property name will remain unchanged. Evil.

So I have been toying with NHibernate lately and yesterday I was writing some repositories. Let&#8217;s assume that theoretically they look like this:

<pre class="brush: csharp; title: ; notranslate" title="">public class Repository &lt;TEntity&gt;
{
    public virtual TEntity FindById (object id)
    {
        ...
    }
}

public class UserRepository : Repository&lt;User&gt;
{
    public IList&lt;User&gt; FindByName (string name)
    {
        // query code
    }

    public IList&lt;User&gt; FindByEmail (string email)
    {
        // query code
    }
}
</pre>

Both *FindByName *and *FindByEmail *in *UserRepository *will in theory contain the same queries but with different parameters. However I don&#8217;t really want to have to write individual queries so I considered a utility method:

<pre class="brush: csharp; title: ; notranslate" title="">public class Repository &lt;TEntity&gt;
{
    public virtual TEntity FindById (object id)
    {
        ...
    }

    protected virtual IList&lt;TEntity&gt; FindByProperty (string propertyName, object value)
    {
        string columnName = NHibernateUtil.GetPropertyColumnName&lt;TEntity&gt; (propertyName);

        // Query here, e.g: "SELECT .... WHERE .... columnName = value" , etc.
    }
}

public class UserRepository : Repository&lt;User&gt;
{
    public IList&lt;User&gt; FindByName (string name)
    {
        return base.FindByProperty ("Name", name);
    }

    public IList&lt;User&gt; FindByEmail (string email)
    {
        return base.FindByProperty ("Email", email);
    }
}
</pre>

Again this is very bad code and I hated the idea of it, so I sat down and started thinking. I remembered reading somewhere about C# 3.0 Expression Trees so I did some research. When using the special *Expression *type this seems to tell the compiler to create and expose an AST to us of e.g. a lambda function&#8217;s body, so if we pass a property reference expression we can parse the AST and extract the property name from there.

The theory in practice:

<pre class="brush: csharp; title: ; notranslate" title="">public class UserRepository : Repository&lt;User&gt;
{
    public IList&lt;User&gt; FindByName (string name)
    {
        return base.FindByProperty (user =&gt; user.Name, name);
    }

    public IList&lt;User&gt; FindByEmail (string email)
    {
        return base.FindByProperty (user =&gt; user.Email, email);
    }
}

public class Repository &lt;TEntity&gt;
{
    public virtual TEntity FindById (object id)
    {
        ...
    }

    protected virtual IList&lt;TEntity&gt; FindByProperty (Expression&lt;Func&lt;TEntity, object&gt;&gt; propertyRefExpr,
                                                     object value)
    {
        string propertyName = GetPropertyName (propertyRefExpr);

        // Query code
    }

    private string GetPropertyName (Expression propertyRefExpr)
    {
        if (propertyRefExpr == null)
            throw new ArgumentNullException ("propertyRefExpr", "propertyRefExpr is null.");

        MemberExpression memberExpr = propertyRefExpr.Body as MemberExpression;
        if (memberExpr == null) {
            UnaryExpression unaryExpr = propertyRefExpr.Body as UnaryExpression;
            if (unaryExpr != null && unaryExpr.NodeType == ExpressionType.Convert)
                memberExpr = unaryExpr.Operand as MemberExpression;
        }

        if (memberExpr != null && memberExpr.Member.MemberType == MemberTypes.Property)
            return memberExpr.Member.Name;

        throw new ArgumentException ("No property reference expression was found.",
                         "propertyRefExpr");
    }
}
</pre>

Also as a helper class:

<pre class="brush: csharp; title: ; notranslate" title="">public static class PropertyUtil
{
    public static string GetPropertyName&lt;TObject&gt; (this TObject type,
                                                   Expression&lt;Func&lt;TObject, object&gt;&gt; propertyRefExpr)
    {
        return GetPropertyNameCore (propertyRefExpr.Body);
    }

    public static string GetName&lt;TObject&gt; (Expression&lt;Func&lt;TObject, object&gt;&gt; propertyRefExpr)
    {
        return GetPropertyNameCore (propertyRefExpr.Body);
    }

    private static string GetPropertyNameCore (Expression propertyRefExpr)
    {
        if (propertyRefExpr == null)
            throw new ArgumentNullException ("propertyRefExpr", "propertyRefExpr is null.");

        MemberExpression memberExpr = propertyRefExpr as MemberExpression;
        if (memberExpr == null) {
            UnaryExpression unaryExpr = propertyRefExpr as UnaryExpression;
            if (unaryExpr != null && unaryExpr.NodeType == ExpressionType.Convert)
                memberExpr = unaryExpr.Operand as MemberExpression;
        }

        if (memberExpr != null && memberExpr.Member.MemberType == MemberTypes.Property)
            return memberExpr.Member.Name;

        throw new ArgumentException ("No property reference expression was found.",
                         "propertyRefExpr");
    }
}
</pre>

As you can see it contains two generic methods that operate either on types:

<pre class="brush: csharp; title: ; notranslate" title="">string propertyName = PropertyUtil.GetName&lt;User&gt; (u =&gt; u.Email);
</pre>

or instances:

<pre class="brush: csharp; title: ; notranslate" title="">User user = GetUser();
string propertyName = user.GetPropertyName (u =&gt; u.Email);
</pre>

Great, isn&#8217;t it? And safe to refactor.

Of course there is no such thing as a free lunch and the use of expression trees comes at the expense of some performance.

BTW is the font size of the code snippets sufficiently readable or is it too tiny?

**UPDATE:** Added support for Convert expressions (implicit/explicit casting)