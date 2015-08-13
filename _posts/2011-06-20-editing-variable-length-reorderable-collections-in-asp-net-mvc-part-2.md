---
title: 'Editing Variable Length Reorderable Collections in ASP.NET MVC – Part 2: jQuery Templates'
author: Ivan Zlatev
layout: post
permalink: /2011/06/20/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-2/
aktt_notify_twitter:
  - yes
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 8729
aktt_tweeted:
  - 1
dsq_thread_id:
  - 337570296
categories:
  - Coding
tags:
  - .NET
  - ASP.NET MVC
  - jQuery
---
[In Part 1 of this series][1] I discussed the problems that we face when implementing an editor for a variable length, reorderable collection and reuse our server-side ASP.NET MVC views, model binding and validation. And while the solution provided in Part 1 is nice and clean (and I personally use it in a few projects) one field for improvement in true Agile Blogging fashion is to find a way to remove the AJAX request we use for fetching new editor rows.

One way to bring this completely to the client side is by the use of [jQuery Template][2] (or any other JavaScript templating engine/library).

A reminder of what our editor looks like before I begin and a second remind that the source code is available on [GitHub][3]:

[<img class="size-full wp-image-780 aligncenter" title="Sample Editor" src="{{ site.url }}/wp-content/uploads/2011/06/image3.png" alt="" width="550" height="435" />][4]

To start with I have added a second link to our sample&#8217;s home page titled &#8220;Edit with jQuery templates&#8221;:

[<img class="aligncenter size-full wp-image-803" title="Home Page of the sample" src="{{ site.url }}/wp-content/uploads/2011/06/home-jquery.png" alt="" width="298" height="202" />][5]

Corresponding controller actions:

<pre class="brush: csharp; title: ; notranslate" title="">public ActionResult EditJQueryTemplate()
{
    return View(CurrentUser);
}

[HttpPost]
public ActionResult EditJQueryTemplate(User user)
{
    if (!this.ModelState.IsValid)
        return View(user);

    CurrentUser = user;
    return RedirectToAction("Display");
}</pre>

And a new editor view (*EditJQueryTemplate.cshtml*) which is exactly the same as the one from Part 1 apart from the core editor bit:

<pre class="brush: xml; highlight: [16,26]; title: ; notranslate" title="">&lt;legend&gt;My Favourite Movies&lt;/legend&gt;

@if (Model.FavouriteMovies == null || Model.FavouriteMovies.Count == 0) {
    &lt;p&gt;None.&lt;/p&gt;
}
&lt;ul id="moviesEditor" style="list-style-type: none"&gt;
    @if (Model.FavouriteMovies != null) {
        foreach (Movie movie in Model.FavouriteMovies) {
            Html.RenderPartial("MovieEntryEditor", movie);
        }
    }
&lt;/ul&gt;
&lt;script src="@Url.Content("~/Scripts/jQuery.tmpl.min.js")" type="text/javascript"&gt;&lt;/script&gt;

&lt;script type="text/x-jquery-tmpl" id="movieTemplate"&gt;
    @Html.CollectionItemJQueryTemplate("MovieEntryEditor", new Movie())
&lt;/script&gt;

&lt;script type="text/javascript"&gt;
    $(function () {
        $("#moviesEditor").sortable();
    });

    var viewModel = {
        addNew: function () {
            $("#moviesEditor").append($("#movieTemplate").tmpl({ index: viewModel._generateGuid() }));
        },

        _generateGuid: function () {
            // Source: http://stackoverflow.com/questions/105034/how-to-create-a-guid-uuid-in-javascript/105074#105074
            return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function (c) {
                var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
                return v.toString(16);
            });
        }
    };
&lt;/script&gt;
            
&lt;a id="addAnother" href="#" onclick="viewModel.addNew();"&gt;Add another&lt;/a&gt;</pre>

Note the new *@Html.CollectionItemJQueryTemplate* helper, which takes a collection item instance with default values and the unchanged partial view *MovieEntryEditor* from Part 1:

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
</pre>

Basically the helper renders the partial with an ${$index} jQuery template placeholder for which we supply a GUID at run-time on the client-side when we render the template. The output of the helper looks like this:

<pre class="brush: xml; title: ; notranslate" title="">&lt;script type="text/x-jquery-tmpl" id="movieTemplate"&gt;
&lt;li style="padding-bottom:15px"&gt;

    &lt;input autocomplete="off" name="FavouriteMovies.Index" type="hidden" value="${index}" /&gt;

    &lt;img src="/Content/images/draggable-icon.png" style="cursor: move" alt=""/&gt;

    &lt;label&gt;Title&lt;/label&gt;
    &lt;input name="FavouriteMovies[${index}].Title" type="text" value="" /&gt;

    &lt;label&gt;Rating&lt;/label&gt;
    &lt;input name="FavouriteMovies[${index}].Rating" type="text" value="0" /&gt;

    &lt;a href="#" onclick="$(this).parent().remove();"&gt;Delete&lt;/a&gt;
&lt;/li&gt;
&lt;/script&gt;</pre>

Everytime the &#8220;Add another&#8221; button is pressed this template is rendered using a new GUID index value supplied by the *viewModel* class.

Here is the code of the *@Html.CollectionItemJQueryTemplate* helper:

<pre class="brush: csharp; title: ; notranslate" title="">public static MvcHtmlString CollectionItemJQueryTemplate&lt;TModel, TCollectionItem&gt;(this HtmlHelper&lt;TModel&gt; html, 
                                                                                    string partialViewName, 
                                                                                    TCollectionItem modelDefaultValues)
{
    ViewDataDictionary&lt;TCollectionItem&gt; viewData = new ViewDataDictionary&lt;TCollectionItem&gt;(modelDefaultValues);
    viewData.Add(JQueryTemplatingEnabledKey, true);
    return html.Partial(partialViewName, modelDefaultValues, viewData);
}</pre>

It renders the supplied partial view, but before that it sets a flag on the ViewContext&#8217;s ViewData dictionary to indicate that the view is to be rendered as jQuery collection item template. This is then handled by the *@Html.BeginCollectionItem* by generating an index template placeholder based .Index hidden field for model binding instead of one with an actual GUID value:

<pre class="brush: csharp; title: ; notranslate" title="">public static IDisposable BeginCollectionItem&lt;TModel&gt;(this HtmlHelper&lt;TModel&gt; html, string collectionName)
{
    string collectionIndexFieldName = String.Format("{0}.Index", collectionName);

    string itemIndex = null;
    if (html.ViewData.ContainsKey(JQueryTemplatingEnabledKey)) {
        itemIndex = "${index}";
    } else {
        itemIndex = GetCollectionItemIndex(collectionIndexFieldName);
    }

    string collectionItemName = String.Format("{0}[{1}]", collectionName, itemIndex);

    TagBuilder indexField = new TagBuilder("input");
    indexField.MergeAttributes(new Dictionary&lt;string, string&gt;() {
        { "name", collectionIndexFieldName },
        { "value", itemIndex },
        { "type", "hidden" },
        { "autocomplete", "off" }
    });
// snip---</pre>

That&#8217;s it! We are still reusing our ASP.NET views, model binding and validation, but no longer require an AJAX request to fetch a new editor row!.

In Part 3 I will look at Knockout JS templating, data binding and moving variable length reorderable collection editing completely on the client side with the MVVM pattern and using JavaScript form serialization and JSON data submission and model binding.

**Next: [Part 3 and Knockout JS client-side collection editing, JSON model binding and client-side validation.][6]**

 [1]: {{ site.url }}/2011/06/16/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-1/ "Editing Variable Length Reorderable Collections in ASP.NET MVC – Part 1"
 [2]: http://api.jquery.com/category/plugins/templates/
 [3]: http://github.com/ivanz/ASP.NET-MVC-Collection-Editing
 [4]: {{ site.url }}/wp-content/uploads/2011/06/image3.png
 [5]: {{ site.url }}/wp-content/uploads/2011/06/home-jquery.png
 [6]: {{ site.url }}/2011/06/29/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-3/