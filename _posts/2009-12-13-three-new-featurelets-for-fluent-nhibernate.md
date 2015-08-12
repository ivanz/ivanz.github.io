---
title: Three new featurelets for Fluent NHibernate
author: Ivan Zlatev
layout: post
permalink: /2009/12/13/three-new-featurelets-for-fluent-nhibernate/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 1113
dsq_thread_id:
  - 51601312
categories:
  - Coding
tags:
  - .NET
  - 'C#'
  - NHibernate
  - Testing
---
Lately I have been using [Fluent NHibernate][1] and [NHibernate][2] respectively for my ORM (Object-Relational Mapping) when working on my top secret pet project. Simply put &#8211; it&#8217;s great. Mapping is done using a fluent C# interface which makes use of lambda expressions for referencing properties instead of strings (check my [previous post][3] for more info) so the domain model is **safe to refactor** because the mapping will be kept up-to-date too.

I spend some time hacking on Fluent NHibernate to add three little features I needed for my integration unit testing via Fluent NHibernate&#8217;s *PersistenceSpecification*. They all currently live in my GitHub(also used by Fluent NHibernate) fork [here][4] and have all been submitted upstream. **UPDATE:** The changes are as of now (21.12.2009) officially part of Fluent NHibernate. I can only hope you will find them useful as well.

### PersistenceSpecification: More Informational Error Reporting

Particularly useful to troubleshoot failing mapping integration unit tests when a property check fails it will print both expected and actual types of the values. Also notice that chained properties (object.PropA.PropB) are now supported as well:

<p style="padding-left: 30px;">
  <em>For property &#8216;Nutrition.Calories&#8217; expected &#8216;400&#8217; of type &#8216;System.Int32&#8242; but got &#8216;400&#8217; of type &#8216;System.Single&#8217;.</em>
</p>

instead of:

<p style="padding-left: 30px;">
  <em>Expected &#8216;400&#8217; but got &#8216;400&#8217; for Property &#8216;NutritionCalories&#8217;</em>
</p>

### PersistenceSpecification: Entity Components Property Testing

It is now possible to test entity component properties. For example given this domain model:

<pre class="brush: csharp; title: ; notranslate" title="">public class Food
{
    public Food ()
    {
        Nutrition = new NutritionInfo ();
    }

    public virtual NutritionInfo Nutrition { get; protected set }
    public virtual int Id { get; set; }

}

public class NutritionInfo
{
    public virtual float? Fat { get; set; }
}

</pre>

and this mapping:

<pre class="brush: csharp; title: ; notranslate" title="">public class FoodMap : ClassMap&lt;Food&gt;
{
    public FoodMap ()
    {
        Id (food =&gt; food.Id)
            .GeneratedBy.Native ();

        Component&lt;NutritionInfo&gt; (food =&gt; food.Nutrition,
            mapping =&gt; {
                mapping.Map (nutrition =&gt; nutrition.Fat)
                    .Nullable ();
            }
        ).Access.BackingField ();
    }
}

</pre>

One can now use this in unit tests:

<pre class="brush: csharp; title: ; notranslate" title="">[TestMethod]
public void Food_Mapping ()
{
    new PersistenceSpecification&lt;Food&gt; (Session)
    .CheckProperty (food =&gt; food.Id, 1)
    .CheckProperty (food =&gt; food.Nutrition.Fat, 40f) // &lt;-- This one
    .VerifyTheMappings ();

}

</pre>

Prior to my patch this wasn&#8217;t possible because Fluent Nhibernate *PersistenceSpecification* did not support chained properties and was trying to set a *Fat* property on the *food* object. Note that this is useful only for components and <span style="text-decoration: underline;">not</span> references. For the latter *.CheckReference (&#8230;) *should be used which will also commit the reference to the Database before doing anything else.

### PersistenceSpecification: IEqualityComparer for Individual Properties

Given the setup from above it is now possible to set an IEqualityComparer per property, e.g.:

<pre class="brush: csharp; title: ; notranslate" title="">[TestMethod]
public void Serving_Mapping ()
{

    new PersistenceSpecification&lt;Food&gt; (Session, new EqualityComparerForAllProperties ())
    .CheckProperty (food =&gt; food.Id, 1)
    .CheckProperty (food =&gt; food.Nutrition.Fat, 40f, FloatEqualityComparer.Instance)
    .VerifyTheMappings ();
}

</pre>

If there is no comparer set for the property it will fall back to using the *EqualityComparerForAllProperties* and if that is not specified it will just use Object.Equals.

 [1]: http://fluentnhibernate.org/
 [2]: http://nhforge.org
 [3]: http://ivanz.com/2009/12/04/how-to-avoid-passing-property-names-as-strings-using-c-3-0-expression-trees/
 [4]: http://github.com/ivanz/fluent-nhibernate