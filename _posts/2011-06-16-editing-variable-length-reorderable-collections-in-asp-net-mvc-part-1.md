---
title: 'Editing Variable Length Reorderable Collections in ASP.NET MVC &#8211; Part 1: ASP.NET MVC Views'
author: Ivan Zlatev
layout: post
permalink: /2011/06/16/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-1/
aktt_notify_twitter:
  - yes
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 37227
aktt_tweeted:
  - 1
dsq_thread_id:
  - 334236720
categories:
  - Coding
tags:
  - .NET
  - ASP.NET MVC
---
### Introduction

I have decided to write a short series of blog posts on editing collections and more specifically variable length collections in ASP.NET MVC. I will take two different implementation approaches:

  1. [In **Part 1**][1] I look at implementing collection editing by sticking to facilities provided to us by ASP.NET MVC such as views, partial views, editor templates, model binding, model validation, etc.
  2. [In **Part 2**][2] I further enhance the sample by using jQuery Templates, but still utilize the same ASP.NET MVC views from Part 1.
  3. [In **Part 3**][3] I will look at Knockout JS templating, data binding and moving variable length reorderable collection editing completely on the client side with the MVVM pattern and JSON data submission and model binding.

The aspects I will consider are:

  * Dynamically adding, removing and reordering items to/from the collection
  * Validation implications
  * Code Reusability and Refactoring implications

I will assume that you are already familiar with ASP.NET MVC and basic JavaScript concepts.

### Source Code

<a href="http://github.com/ivanz/ASP.NET-MVC-Collection-Editing" target="_blank">All source code is available on GitHub</a>

### The Sample

What I am going to build is a little sample where we have a user who has a list of favourite movies. It will look roughly like on the image below and will allow for adding new favourite movies, removing favourite movies and also reordering them up and down using the drag handler.

[<img style="background-image: none; margin: 10px auto; padding-left: 0px; padding-right: 0px; display: block; float: none; padding-top: 0px; border-width: 0px;" title="image" src="{{ site.url }}/wp-content/uploads/2011/06/image_thumb3.png" border="0" alt="image" width="554" height="439" />][4]

### Domain Model

The domain model is basically:

<pre class="brush: csharp; title: ; notranslate" title="">public class User
{
    public int? Id { get; set; }
    [Required]
    public string Name { get; set; }
    public IList&lt;Movie&gt; FavouriteMovies { get; set; }
}</pre>

and

<pre class="brush: csharp; title: ; notranslate" title="">public class Movie
{
    [Required]
    public string Title { get; set; }
    public int Rating { get; set; }
}</pre>

Let’s get cracking!

### An Edit View

Let’s start by creating a first-pass edit view for our Person to look like the one on the image above:

<pre class="brush: xml; title: ; notranslate" title="">@model CollectionEditing.Models.User
@{ ViewBag.Title = "Edit My Account"; }

&lt;h2&gt;Edit&lt;/h2&gt;

@using (Html.BeginForm()) {
    @Html.ValidationSummary(true)
    &lt;fieldset&gt;
        &lt;legend&gt;My Details&lt;/legend&gt;

        @Html.HiddenFor(model =&gt; model.Id)

        &lt;div class="editor-label"&gt;
            @Html.LabelFor(model =&gt; model.Name)
        &lt;/div&gt;
        &lt;div class="editor-field"&gt;
            @Html.EditorFor(model =&gt; model.Name)
            @Html.ValidationMessageFor(model =&gt; model.Name)
        &lt;/div&gt;
    &lt;/fieldset&gt;
    
    &lt;fieldset&gt;
        &lt;legend&gt;My Favourite Movies&lt;/legend&gt;

        @if (Model.FavouriteMovies == null || Model.FavouriteMovies.Count == 0) {
            &lt;p&gt;None.&lt;/p&gt;
        } else {
            &lt;ul id="movieEditor" style="list-style-type: none"&gt;
                @for (int i=0; i &lt; Model.FavouriteMovies.Count; i++) {
                    &lt;li style="padding-bottom:15px"&gt;
                        &lt;img src="@Url.Content("~/Content/images/draggable-icon.png")" style="cursor: move" alt=""/&gt;

                        @Html.LabelFor(model =&gt; model.FavouriteMovies[i].Title)
                        @Html.EditorFor(model =&gt; model.FavouriteMovies[i].Title)
                        @Html.ValidationMessageFor(model =&gt; model.FavouriteMovies[i].Title)

                        @Html.LabelFor(model =&gt; model.FavouriteMovies[i].Rating)
                        @Html.EditorFor(model =&gt; model.FavouriteMovies[i].Rating)
                        @Html.ValidationMessageFor(model =&gt; model.FavouriteMovies[i].Rating)

                        &lt;a href="#" onclick="$(this).parent().remove();"&gt;Delete&lt;/a&gt;
                    &lt;/li&gt;
                }
            &lt;/ul&gt;
            &lt;a href="#"&gt;Add another&lt;/a&gt;
        }

        &lt;script type="text/javascript"&gt;
            $(function () {
                $("#movieEditor").sortable();
            });
        &lt;/script&gt;
    &lt;/fieldset&gt;
    
    &lt;p&gt;
        &lt;input type="submit" value="Save" /&gt;
        &lt;a href="/"&gt;Cancel&lt;/a&gt;
    &lt;/p&gt;
}</pre>

The view is creating a list of editing controls for each of the movies in *Person.FavouriteMovies*. I am using a jQuery selector and dom function to remove a movie when the user clicks “Delete”  and also a <a href="http://jqueryui.com/demos/sortable/" target="_blank">jQuery UI Sortable</a> to make the items from the HTML list drag and droppable up and down.

With this done we immediately face the first problem: <span style="text-decoration: underline;">We haven’t implemented the “Add another”</span>. Before we do that let’s consider how ASP.NET MVC model binding of collections works.

### ASP.NET MVC Collection Model Binding Patterns

There are two patterns for model binding collections in ASP.NET MVC. The **first one** you have just seen:

<pre class="brush: xml; title: ; notranslate" title="">@for (int i=0; i &lt; Model.FavouriteMovies.Count; i++) {
    @Html.LabelFor(model =&gt; model.FavouriteMovies[i].Title)
    @Html.EditorFor(model =&gt; model.FavouriteMovies[i].Title)
    @Html.ValidationMessageFor(model =&gt; model.FavouriteMovies[i].Title)
…
}</pre>

which generates similar HTML:

<pre class="brush: xml; title: ; notranslate" title="">&lt;label for="FavouriteMovies_0__Title"&gt;Title&lt;/label&gt;
&lt;input id="FavouriteMovies_0__Title" name="FavouriteMovies[0].Title" type="text" value="" /&gt;
&lt;span class="field-validation-error"&gt;The Title field is required.&lt;/span&gt;</pre></p> 

This is really great for displaying collections and editing static length collections, but problematic when we want to edit variable length collections, because:

  1. **Indices have to be sequential (0, 1, 2, 3, …). If they aren’t ASP.NET MVC stops at the first gap. E.g. if you have item 0, 1, 3, 4 after the model binding has finished you will end up with a collection of two items only – 1 and 2 instead of four items.**
  2. **If you were to reorder the list in the HTML ASP.NET MVC will apply the indices order not the fields order when doing model binding.**

This basically means that add/remove/reorder scenarios are no go with this. It’s not impossible but it will be big big mess tracking add/remove/reorder actions and re-indexing all field attributes.

Now, someone might say – “*Hey, why don’t you just implement a non-sequential collection model binder?”* .

Yes, you can write the code for a non-sequential collection model binder. You will however face two major issues with that however. The first being that the *IValueProvider* doesn’t expose a way to iterate through all values in the *BindingContext* which you can workaround* *by hardcoding the model binder to access the current HttpRequest Form values collection (meaning that if someone decides to submit the form via Json or query parameters your model binder won’t work) or I’ve seen one more insane workaround which checks the *BindingContext* one by one from *CollectionName[0]* to *CollectionName[Int32.MaxValue]* (that’s <span style="text-decoration: underline;">2 billion</span> iterations!).

Second major issue is that once you create a sequential collection from the non-sequential indices and items and you have a validation error and you re-render the form view your ModelState will no longer match the data. An item that used to be at index X is now at index X-1 after another item before it was deleted, however the ModelState validation message and state still point to X, because this is what you submitted.

So, even a custom model binder won’t help.

Thankfully there is a **second pattern**, which mostly helps for what we want to achieve (even though I don’t think it was designed to solve exactly this):

<pre class="brush: xml; title: ; notranslate" title="">&lt;input type="hidden" name="FavouriteMovies.Index" value="indexA"/&gt;
&lt;input name="FavouriteMovies[indexA].Title" type="text" value="" /&gt;
&lt;input name="FavouriteMovies[indexA].Rating" type="text" value="" /&gt;
&lt;input type="hidden" name="FavouriteMovies.Index" value="indexB"/&gt;
&lt;input name="FavouriteMovies[indexB].Title" type="text" value="" /&gt;
&lt;input name="FavouriteMovies[indexB].Rating" type="text" value="" /&gt;</pre></p> 

Notice how we have introduced an “.Index” hidden field for each collection item. By doing that we tell ASP.NET MVC’s model binding “Hey, don’t look for a standard numeric collection index, but instead look for the custom Index value we have specified and just get me the list of items in a collection when you are done”. How does this help?

  1. We can specify any index value we want
  2. The index doesn’t have to be sequential and items will be put in the collection in the order they are in the HTML when submitted.

Bam! That’s solves most, but not all of our problems.

### The Solution

Firstly, ASP.NET MVC doesn’t have HTML helpers to generate the “[something].Index” pattern which is major problem since it means we can’t use validation and custom editors. We can fix that by utilizing some ASP.NET templating fu. What we are going to do is move the Movie editor to a its own partial view (MovieEntryEditor.cshtml):

<pre class="brush: xml; title: ; notranslate" title="">@model CollectionEditing.Models.Movie
           
&lt;li style="padding-bottom:15px"&gt;
    @using (Html.BeginCollectionItem("FavouriteMovies")) {
        &lt;img src="@Url.Content("~/Content/images/draggable-icon.png")" style="cursor: move" alt=""/&gt;

        @Html.LabelFor(model =&gt; model.Title)
        @Html.EditorFor(model =&gt; model.Title)
        @Html.ValidationMessageFor(model =&gt; model.Title)

        @Html.LabelFor(model =&gt; model.Rating)
        @Html.EditorFor(model =&gt; model.Rating)
        @Html.ValidationMessageFor(model =&gt; model.Rating)

        &lt;a href="#" onclick="$(this).parent().remove();"&gt;Delete&lt;/a&gt;
    }
&lt;/li&gt;
</pre></p> 

And update our Edit view to use it:

<pre class="brush: xml; title: ; notranslate" title="">&lt;ul id="movieEditor" style="list-style-type: none"&gt;
    @foreach (Movie movie in Model.FavouriteMovies) {
        Html.RenderPartial("MovieEntryEditor", movie);
    }
&lt;/ul&gt;
&lt;p&gt;&lt;a id="addAnother" href="#"&gt;Add another&lt;/a&gt;</pre>

Notice two things – firstly the Movie partial edit view uses standard Html helpers and secondly there is a call to something custom called *Html.BeginCollectionItem. *You might even ask yourself: Wait a second. This won’t work, because the partial view will produce names such as “Title” instead of “FavouriteMovies[xxx].Title”, so let me show you the source code of *Html.BeginCollectionItem*:

<pre class="brush: csharp; title: ; notranslate" title="">public static IDisposable BeginCollectionItem&lt;TModel&gt;(this HtmlHelper&lt;TModel&gt; html,                                                       string collectionName)
{
    string itemIndex = Guid.NewGuid().ToString();
    string collectionItemName = String.Format("{0}[{1}]", collectionName, itemIndex);

    TagBuilder indexField = new TagBuilder("input");
    indexField.MergeAttributes(new Dictionary&lt;string, string&gt;() {
        { "name", String.Format("{0}.Index", collectionName) },
        { "value", itemIndex },
        { "type", "hidden" },
        { "autocomplete", "off" }
    });

    html.ViewContext.Writer.WriteLine(indexField.ToString(TagRenderMode.SelfClosing));
    return new CollectionItemNamePrefixScope(html.ViewData.TemplateInfo, collectionItemName);
}

private class CollectionItemNamePrefixScope : IDisposable
{
    private readonly TemplateInfo _templateInfo;
    private readonly string _previousPrefix;

    public CollectionItemNamePrefixScope(TemplateInfo templateInfo, string collectionItemName)
    {
        this._templateInfo = templateInfo;

        _previousPrefix = templateInfo.HtmlFieldPrefix;
        templateInfo.HtmlFieldPrefix = collectionItemName;
    }

    public void Dispose()
    {
        _templateInfo.HtmlFieldPrefix = _previousPrefix;
    }
}</pre></p> 

This helper does two things:

  * Appends a hidden Index field to the output with a random GUID value (remember that using the .Index pattern an index can be any string)
  * Scopes the execution of the helper via an *IDisposable* and sets the template rendering context (html helperes and display/editor templates) to be “*FavouriteMovies[GUID].*”, so we end up with HTML like this:

<pre class="brush: xml; title: ; notranslate" title="">&lt;input autocomplete="off" name="FavouriteMovies.Index" type="hidden" value="6d85a95b-1dee-4175-bfae-73fad6a3763b" /&gt;
&lt;label&gt;Title&lt;/label&gt;
&lt;input class="text-box single-line" name="FavouriteMovies[6d85a95b-1dee-4175-bfae-73fad6a3763b].Title" type="text" value="Movie 1" /&gt;
&lt;span class="field-validation-valid"&gt;&lt;/span&gt;</pre>

This solves the problem of using Html field templates and basically reusing ASP.NET facilities instead of having to write html by hand, but it leads me to the second quirk that we need to address.

Let me show you the second and final problem. Disable client side validation and delete the title of e.g. “Movie 2” and click submit. Validation will fail, because Title of a movie is a required field, but while we are shown the edit form again** there are no validation messages**:

[<img style="background-image: none; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" src="{{ site.url }}/wp-content/uploads/2011/06/image_thumb4.png" border="0" alt="image" width="544" height="174" />][5]

Why is that? It’s the same problem I mentioned earlier in this post. Each time we render the view we assign different names to the fields, which do not match the ones submitted and leads to a *ModelState *inconsistency. We have to figure out how to persist the name and more specifically the Index across requests. We have two options:

  1. Add a hidden CollectionIndex field and CollectionIndex property on the *Movie* object to persist the *FavouriteMovies.Index*. This however is intrusive and suboptimal.
  2. Instead of polluting the *Movie* object with an extra property be smart and in our helper *Html.BeginCollectionItem* reapply/reuse the submitted *FavouriteMovies.Index* form values.

Let’s replace in *Html.BeginCollectionItem* this line:

<pre class="brush: csharp; title: ; notranslate" title="">string itemIndex = Guid.New().ToString();</pre>

with:

<pre class="brush: csharp; title: ; notranslate" title="">string itemIndex = GetCollectionItemIndex(collectionIndexFieldName);</pre>

And here&#8217; is the code for GetCollectionItemIndex:

<pre class="brush: csharp; title: ; notranslate" title="">private static string GetCollectionItemIndex(string collectionIndexFieldName)
{
    Queue&lt;string&gt; previousIndices = (Queue&lt;string&gt;) HttpContext.Current.Items[collectionIndexFieldName];
    if (previousIndices == null) {
        HttpContext.Current.Items[collectionIndexFieldName] = previousIndices = new Queue&lt;string&gt;();

        string previousIndicesValues = HttpContext.Current.Request[collectionIndexFieldName];
        if (!String.IsNullOrWhiteSpace(previousIndicesValues)) {
            foreach (string index in previousIndicesValues.Split(','))
                previousIndices.Enqueue(index);
        }
    }

    return previousIndices.Count &gt; 0 ? previousIndices.Dequeue() : Guid.NewGuid().ToString();
}</pre></p> 

We get all submitted values for e.g. “*FavouriteMovie.Index*” put them in a queue, which we store for the duration of the request. Each time we render a collection item we dequeue its old index value and if none is available we generate a new one. That way we preserve the Index across requests and can have a consistent *ModelState* and see validation errors and messages:

[<img style="background-image: none; margin: 0px; padding-left: 0px; padding-right: 0px; display: inline; padding-top: 0px; border-width: 0px;" title="image" src="{{ site.url }}/wp-content/uploads/2011/06/image_thumb5.png" border="0" alt="image" width="684" height="150" />][6]

All that is left is to implement the “Add another” button functionality and we can do that easily by appending a new row to the movie editor, which we can fetch using Ajax and use our existing MovieEntryEditor.cshtml partial view like that:

<pre class="brush: csharp; title: ; notranslate" title="">public ActionResult MovieEntryRow()
{
    return PartialView("MovieEntryEditor");
}</pre>

And then add the follwing &#8220;Add Another&#8221; click handler:

<pre class="brush: jscript; title: ; notranslate" title="">$("#addAnother").click(function () {
    $.get('/User/MovieEntryRow', function (template) {
        $("#movieEditor").append(template);
    });
});</pre>

Done.

### Conclusion

While not immediately obvious editing variable length reorderable collections with standard ASP.NET MVC is possible and what I like about this approach is that:

  * We can keep using traditional ASP.NET html helpers, editor and display templates (Html.EditorFor, etc.) with in our collection editing
  * We can make use of the ASP.NET MVC model validation client and server side

What I don’t like that much however is:

  * That we have to use an AJAX request to append a new row to the editor.
  * That we need to use the name of the collection in the movie editor partial view, but otherwise when doing the standalone AJAX get request the name context won’t be properly set for the partial template fields.

I would love to hear your thoughts. The sample source code is available on my <a href="http://github.com/ivanz/ASP.NET-MVC-Collection-Editing" target="_blank">GitHub</a>

Next: **[Part 2 and how to avoid having an AJAX request with the use of jQuery Templates][2]**

 [1]: {{ site.url }}/2011/06/16/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-1/
 [2]: {{ site.url }}/2011/06/20/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-2/
 [3]: {{ site.url }}/2011/06/29/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-3/
 [4]: {{ site.url }}/wp-content/uploads/2011/06/image3.png
 [5]: {{ site.url }}/wp-content/uploads/2011/06/image4.png
 [6]: {{ site.url }}/wp-content/uploads/2011/06/image5.png