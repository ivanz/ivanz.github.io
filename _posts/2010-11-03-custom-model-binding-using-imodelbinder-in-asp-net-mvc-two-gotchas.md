---
title: 'Custom model binding using IModelBinder in ASP.NET MVC &#8211; two gotchas'
author: Ivan Zlatev
layout: post
permalink: /2010/11/03/custom-model-binding-using-imodelbinder-in-asp-net-mvc-two-gotchas/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 17255
dsq_thread_id:
  - 166656283
categories:
  - Coding
tags:
  - ASP.NET MVC
---
There are a few blog posts around the web regarding ASP.NET MVC custom model binding, but there are two key pieces of information missing.

Just a quick intro &#8211; to implement custom model binding in ASP.NET MVC we need to implement the IModelBinder interface:

<pre class="brush: csharp; title: ; notranslate" title="">public interface IModelBinder
{
    object BindModel(ControllerContext controllerContext, ModelBindingContext bindingContext);
}
</pre>

For the sake of the example let&#8217;s say we have a class (below) that doesn&#8217;t have a public constructor and for one or another reason we don&#8217;t want to have a view model.

<pre class="brush: csharp; title: ; notranslate" title="">public class Quantity
{
    public Quantity (float value, MeasurementUnits units)
    {
          this.Value = value;
          this.Units = units;
    }

    public float Value { get; set; }
    public MeasurementUnits Units { get; set; }
}

public enum MeasurementUnits  { mg, g, kg, l, kcal }
</pre>

With this class in ASP.NET MVC the model binding won&#8217;t work, because it doesn&#8217;t expose a public constructor. We have three options:

  1. &#8220;Mirror&#8221; the Quantity class in a View Model class, but that also requires mirroring each class that has a property of type *Quantity * as well
  2. Manually build an instance by retrieving the units and value from the Request parameters. However this will have to be done in each controller action (code duplication is bad).
  3. **Use custom model binding.**

Here is the model binder:

<pre class="brush: csharp; title: ; notranslate" title="">public class QuantityModelBinder : IModelBinder
{
    public object BindModel (ControllerContext controllerContext, ModelBindingContext bindingContext)
    {
        if (controllerContext == null)
            throw new ArgumentNullException ("controllerContext", "controllerContext is null.");
        if (bindingContext == null)
            throw new ArgumentNullException ("bindingContext", "bindingContext is null.");

        MeasurementUnits? units = TryGet&lt;MeasurementUnits&gt; (bindingContext, "Units");
        float? value = TryGet&lt;float&gt; (bindingContext, "Value");

        if (units.HasValue && value.HasValue)
            return new Quantity (value.Value, units.Value);

        return null;
    }

    private Nullable&lt;T&gt; TryGet&lt;T&gt; (ModelBindingContext bindingContext, string key) where T : struct
    {
        if (String.IsNullOrEmpty (key))
            return null;

        ValueProviderResult valueResult = bindingContext.ValueProvider.GetValue(bindingContext.ModelName + "." + key);
        if (valueResult == null && bindingContext.FallbackToEmptyPrefix == true)
            valueResult = bindingContext.ValueProvider.GetValue (key);

        bindingContext.ModelState.SetModelValue (bindingContext.ModelName, valueResult);

        if (valueResult == null)
            return null;

        try {
            return (Nullable&lt;T&gt;)valueResult.ConvertTo (typeof (T));
        } catch (Exception ex) {
            bindingContext.ModelState.AddModelError (bindingContext.ModelName, ex);
            return null;
        }
    }
}</pre>

The two important gotchas are:

  1. If your binding validation fails you should log an appropriate validation error via *ModelState.AddModelError . *It is useful to know that if you add exceptions of type FormatException to the model errors, ASP.NET MVC is smart enough to automatically convert them to string errors post-binding but before it returns control to your controller action.
  2. If  you log an error you **must **call *ModelState.SetModelValue* or otherwise if you return null it will cause a *NullReferenceException* in the default ASP.NET MVC binder. This is so because unless you call that method the rest of the ASP.NET MVC code (as well as your own code) won&#8217;t be able to access *ModelState[propertyName]* and because it doesn&#8217;t handle that it blows up.

P.S:  If I have to be pedantic and the code path is not critical I won&#8217;t use strings in the model binder to refer to the Quantity properties. Instead I would use something like what I&#8217;ve written about in will use the method described in [How to avoid passing property names as strings using C# 3.0 Expression Trees][1] .

 [1]: {{ site.url }}/2009/12/04/how-to-avoid-passing-property-names-as-strings-using-c-3-0-expression-trees/