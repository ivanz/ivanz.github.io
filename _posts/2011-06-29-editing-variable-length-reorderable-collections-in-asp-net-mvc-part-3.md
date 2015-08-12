---
title: 'Editing Variable Length Reorderable Collections in ASP.NET MVC &#8211; Part 3: Knockout JS'
author: Ivan Zlatev
layout: post
permalink: /2011/06/29/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-3/
aktt_notify_twitter:
  - yes
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 9750
aktt_tweeted:
  - 1
dsq_thread_id:
  - 345465706
categories:
  - Coding
tags:
  - ASP.NET MVC
  - jQuery
  - KnockoutJS
---
[In Part 1 of this series][1] I discussed the problems that we face when implementing an editor for a variable length, reorderable collection and reuse our server-side ASP.NET MVC views, model binding and validation.

[In Part 2 of this series][2] I began moving some aspects of the collection editing to the client-side with jQuery Templates, but we realized that it&#8217;s not good enough, because instead of dealing with data we are juggling with our ASP.NET MVC views generated HTML.

In this final Part 3 of the series I want to move the collection editing completely to the client-side using the [Knockout JS library][3] JavaScript library. I will look at:

  * Knockout JS data-binding and tempalting
  * JSON Form (data really &#8211; no form) submission
  * jQuery Client-Side Validation 

I will re-implement our sample collection editor and it is going to look exactly the same as it did before. Before I begin just a reminder that [all of the source code is available on GitHub as a Visual Studio solution][4].

#### What is Knockout JS?

I am going to quote its creator (Steven Sanderson) from his introductory post, where he provides [a very good overview][5]:

> Knockout is a JavaScript library that makes it easier to create rich, desktop-like user interfaces with JavaScript and HTML, using *observers *to make your UI automatically stay in sync with an underlying data model. It works particularly well with the MVVM pattern, offering *declarative bindings *somewhat like Silverlight but without the browser plugin.

I suggest you read the above linked post or checkout the Knockout JS web site to learn more about it, but to quickly summarize how it works:

  * A JavaScipt view model is defined that holds all the data. All the data properties are initialized as (or wrapped in) observable values and arrays.
  * HTML elements can be data-bound to a property (similar to WinForms/XAML(WPF/Silverlight)) from the view model using the *data-bind *attribute syntax. We can bind an input&#8217;s value, a span&#8217;s text, etc. to a view model property.
  * Whenever a value/or array changes the data-binding kicks in and the HTML element are automatically updated.
  * Knockout uses jQuery and supports templating via jQuery Templates.

#### Our Sample

To understand this lets jump straight into our sample app.

I have added a third edit option to our sample:

[<img class="aligncenter size-full wp-image-856" title="knockout-edit" src="http://ivanz.com/wp-content/uploads/2011/06/knockout-edit.png" alt="" width="304" height="223" />][6]

which surprise, surprise looks exactly the same as in the previous iteration. Apart from one extra feature &#8211; the favourite movies counter I&#8217;ve added to give you a hint of the power of Knockout JS:

[<img class="aligncenter size-full wp-image-857" title="knockout-edit-screen" src="http://ivanz.com/wp-content/uploads/2011/06/knockout-edit-screen.png" alt="" width="571" height="383" />][7]

#### View and ViewModel

Let&#8217;s firstly look at the first iteration of our view model implementation:

<pre class="brush: xml; title: ; notranslate" title="">&lt;script type="text/javascript"&gt;
var viewModel = {
    Id: ko.observable(@Model.Id),
    Name: ko.observable("@Model.Name"),
    FavouriteMovies: ko.observableArray(@Html.Json(@Model.FavouriteMovies) || []),

    maxMovies: 10,
    
    addMovie: function() {
        viewModel.FavouriteMovies.push(@Html.Json(new Movie()));
    },

    removeMovie: function(movie) {
        ko.utils.arrayRemoveItem(viewModel.FavouriteMovies, movie);
    }
};

$(function () {
    ko.applyBindings(viewModel);
});
&lt;/script&gt;</pre>

We:

  * Serialize our ASP.NET MVC *User* Model to JSON (*Id*, *Name*, *FavouriteMovies*). *Html.Json* is a custom Html helper that simply wraps a JSON serializer &#8211; you can [see it in the source code][8].
  * Wrap its values in Knockout observables and then once the page is loaded we initialize Knockout JS with our model
  * Define some add/remove movie methods to edit the FavouriteMovies collection

**What&#8217;s crucial here is that we are not dealing with nor manipulating HTML markup**, which is something that we were doing heavily in the previous parts.

When processed server-side the above view model will look like roughly like this in the browser:

<pre class="brush: xml; title: ; notranslate" title="">var viewModel = {
        Id: ko.observable(1),
        Name: ko.observable("Ivan Zlatev"),
        FavouriteMovies: ko.observableArray([{"Title":"Movie 1","Rating":5},
                                                           {"Title":"Movie 2","Rating":10},
                                                           {"Title":"Movie 3","Rating":12}] || []),

        maxMovies: 10,

        addMovie: function() {
            viewModel.FavouriteMovies.push({"Title":null,"Rating":0});
        },

        removeMovie: function(movie) {
            ko.utils.arrayRemoveItem(viewModel.FavouriteMovies, movie);
        }
}</pre>

Let&#8217;s look at the first iteration of our edit view (*EditKnockoutJS.cshtml*) to better understand how does Knockout help us:

<pre class="brush: xml; title: ; notranslate" title="">@model CollectionEditing.Models.User
@{ ViewBag.Title = "Edit My Account With Knockout JS"; }

&lt;script src="@Url.Content("~/Scripts/jquery.validate.min.js")" type="text/javascript"&gt;&lt;/script&gt;
&lt;script src="@Url.Content("~/Scripts/jQuery.tmpl.min.js")" type="text/javascript"&gt;&lt;/script&gt;
&lt;script src="@Url.Content("~/Scripts/knockout-1.2.1.debug.js")" type="text/javascript"&gt;&lt;/script&gt;
&lt;script src="@Url.Content("~/Scripts/knockout.jQueryUI-sortable.js")" type="text/javascript"&gt;&lt;/script&gt;

&lt;h2&gt;Edit&lt;/h2&gt;

@using (Html.BeginForm("EditKnockoutJS", "User", FormMethod.Post, new { id = "profileEditorForm" })) {
    @Html.ValidationSummary(false)
    &lt;fieldset&gt;
        &lt;legend&gt;My Details&lt;/legend&gt;

        &lt;input name="userId" type="hidden" data-bind="value: Id"/&gt;

        &lt;label for="name"&gt;Name:&lt;/label&gt;
        &lt;input type="text" id="name" name="name" data-bind="value: Name" /&gt;
    &lt;/fieldset&gt;
    
    &lt;fieldset&gt;
        &lt;legend&gt;My Favourite Movies&lt;/legend&gt;

        &lt;ul id="moviesEditor" style="list-style-type: none"
            data-bind="template: { name: 'moviesTemplate', data: FavouriteMovies }, 
                       sortableList: viewModel.FavouriteMovies" &gt;
        &lt;/ul&gt;
        &lt;p&gt;You have &lt;span data-bind="text: FavouriteMovies().length"&gt;&lt;/span&gt; favourite movies.&lt;/p&gt;

        &lt;script id="moviesTemplate" type="text/x-jquery-template"&gt;
            {{each(i, movie) $data}}
                &lt;li data-bind="sortableItem: movie"&gt;
                    &lt;div data-bind="template: { name: 'movieTemplate', data: movie }"&gt;&lt;/div&gt;
                &lt;/li&gt;
            {{/each}}
        &lt;/script&gt;

        &lt;script id="movieTemplate" type="text/x-jquery-template"&gt;
            &lt;img src="@Url.Content("~/Content/images/draggable-icon.png")" style="cursor: move" alt=""/&gt;

            &lt;label&gt;Title:&lt;/label&gt;
            &lt;input type="text" data-bind="value: Title"/&gt;

            &lt;label&gt;Rating:&lt;/label&gt;
            &lt;input type="text" data-bind="value: Rating"/&gt;

            &lt;a href="#" data-bind="click: function() { viewModel.removeMovie(this); }"&gt;Delete&lt;/a&gt;
        &lt;/script&gt;
    
        &lt;button data-bind="click: addMovie, enable: FavouriteMovies().length &lt; maxMovies"&gt;Add another&lt;/button&gt;
    &lt;/fieldset&gt;

    &lt;p&gt;
        &lt;input type="submit" value="Save" /&gt;
        &lt;a href="/"&gt;Cancel&lt;/a&gt;
    &lt;/p&gt;
}</pre>

In our view we use Knockout JS to data-bind our HTML input fields to the data in the viewModel (notice the &#8220;*data-bind*&#8221; attributes).

For the collection editor:

  * Similar to Part 2, we have a movie jQuery template, which defines the HTML fields of a movie, but this time we don&#8217;t use any ASP.NET MVC helpers.
  * We make the HTML list reorderable via Drag and Drop with the two special *sortableList* and *sortableItem* bindings.This is a custom Knockout JS binding I&#8217;ve created and hosted on [GitHub here][9]. Behind the scenes it magically creates a JQuery UI Sortable keeps our *viewModel.FavouriteMovies* collection in sync when the widget changes.
  * We limit the number of favourite movies a user can add via the &#8220;Add Another&#8221; button to ten by using a &#8220;*enabled*&#8221; binding with a boolean expression. Behind the scenes Knockout JS will manage the state of the button and toggle between enabled/disabled depending on the evaluated value of the expression.
  * Finally we data-bind a *span* element to display the current number of favourite movies the user has added. Again, this is something that Knockout will keep up to date for us.

Right now with this viewModel and that view we have a working drag and drop add/remove favourite movies editor. What we are still missing however is:

  * Form submission and server-side handling
  * Form validation

#### Form Submission and Handling

Let&#8217;s add some more code to our view model and view to handle form submission.

Firstly we are going to implement a save function in our view model. Because we are no longer using ASP.NET model views, we can&#8217;t simply submit the form as is &#8211; the &#8220;standard&#8221; form values based ASP.NET MVC model binding won&#8217;t work properly and especially so for the collection editing part. 

Thankfully starting from version 3 ASP.NET MVC supports out of the box JSON model binding. It kicks in if the HTTP request Content-Type is set to &#8220;application/json&#8221;. So instead of using form submission we are going to post our view model data via an AJAX post request to our MVC action, containing the data in JSON serialized form:

<pre class="brush: jscript; title: ; notranslate" title="">var viewModel = {
       ... snip ...

        saveFailed: ko.observable(false),

        // Returns true if successful
        save: function()
        {
            var saveSuccess = false;
            viewModel.saveFailed(false);

            // Executed synchronously for simplicity
            jQuery.ajax({
                type: "POST",
                url: "@Url.Action("EditKnockoutJS", "User")",
                data: ko.toJSON(viewModel),
                dataType: "json",
                contentType: "application/json",
                success: function(returnedData) {
                    saveSuccess = returnedData.Success || false;
                    viewModel.saveFailed(!saveSuccess);
                },
                async: false
            });

            return saveSuccess;
        }
};</pre>

Then we will suppress the default form submission behaviour and replace it with our own:

<pre class="brush: xml; title: ; notranslate" title="">&lt;p&gt;
    &lt;input type="submit" value="Save" 
             data-bind="click: function() { viewModel.save(); return false; }"/&gt;
    &lt;a href="/"&gt;Cancel&lt;/a&gt;
&lt;/p&gt;
    
&lt;p data-bind="visible: saveFailed" class="error"&gt;A problem occurred saving the data.&lt;/p&gt;</pre>

Note that I have added a new &#8220;*saveFailed*&#8221; property in the viewModel which is set by *save()*. I have also used a &#8220;*visible*&#8221; data binding on the value of that property to display the error message paragraph only if the save operation has failed.

Now let&#8217;s also look at the MVC action to handle the form submission:

<pre class="brush: csharp; title: ; notranslate" title="">[HttpPost]
public ActionResult EditKnockoutJS(User user)
{
    FormResponseData responseData = new FormResponseData() {
        Success = false
    };

    if (this.ModelState.IsValid) {
        CurrentUser = user;
        responseData.Success = true;
    }

    return Json(responseData);
}

class FormResponseData {
    public bool Success { get; set; }
}</pre>

Pretty simple action, which just validates the Model using our validation rules/data annotations and returns a *FormReponseData* object serialized to JSON, which contains a Success flag expected by the client side.

We can now persist our view model and also use our server-side model validation to prevent the client from doing nasty things:

[<img src="http://ivanz.com/wp-content/uploads/2011/06/knockout-edit-server-validation.png" alt="" title="knockout-edit-server-validation" width="558" height="207" class="aligncenter size-full wp-image-905" />][10]

We are not done just yet. Because we no longer use the ASP.NET html helpers we no longer pull the server-side validation error messages and are currently left with the generic &#8220;Something went wrong.&#8221; error message. This isn&#8217;t exactly nice nor user friendly. We can solve this by the use of client-side JavaScript based validation. Remember that we should never ever rely solely on the client-side (and we don&#8217;t in our case).

#### Client-Side Validation

Unfortunately at the time of writing I couldn&#8217;t find anything that validates against the Knockout JS viewModel instead of the HTML markup, so I had to resort to [jQuery Validation][11] for the client-side validation. The problem with that is that we need to ensure that we have set proper unique &#8220;name&#8221; attributes on our form fields and especially so for our collection editor elements.

Fortunately Knockout JS already has a binding for that called the &#8220;*uniqueName* binding&#8221;, so all we have to do is add that to our favourite movie collection editor fields like I&#8217;ve showed below and don&#8217;t worry about generating them ourselves. This is great, because as you saw in Part 2 injecting a unique name during jQuery template evaluation is relatively tricky.

<pre class="brush: xml; title: ; notranslate" title="">&lt;script id="movieTemplate" type="text/x-jquery-template"&gt;
    &lt;img src="@Url.Content("~/Content/images/draggable-icon.png")" style="cursor: move" alt=""/&gt;

    &lt;label&gt;Title:&lt;/label&gt;
    &lt;input type="text" data-bind="value: Title, uniqueName: true" class="required"/&gt;

    &lt;label&gt;Rating:&lt;/label&gt;
    &lt;input type="text" data-bind="value: Rating, uniqueName: true" class="required"/&gt;

    &lt;a href="#" data-bind="click: function() { viewModel.removeMovie(this); }"&gt;Delete&lt;/a&gt;
&lt;/script&gt;</pre>

There are multiple ways we can use jQuery Validation and I have decided to go for the simplest declarative one, where we decorate our fields with extra attributes to describe the value constraints that we want to enforce. Let&#8217;s update the above fields to make the *Title* and *Rating* required:

<pre class="brush: xml; title: ; notranslate" title="">&lt;label&gt;Title:&lt;/label&gt;
&lt;input type="text" data-bind="value: Title, uniqueName: true" class="required"/&gt;

&lt;label&gt;Rating:&lt;/label&gt;
&lt;input type="text" data-bind="value: Rating, uniqueName: true" class="required"/&gt;</pre>

Finally let&#8217;s plug-in jQuery Validation. To do that we will remove the previous submit button handler and use jQuery Validation instead:

<pre class="brush: xml; title: ; notranslate" title="">... snip... 

        &lt;input type="submit" value="Save"/&gt;

... snip... 

&lt;script type="text/javascript"&gt;
    $(function () {
        ko.applyBindings(viewModel);

        $("#profileEditorForm").validate({
            submitHandler: function(form) {
                if(viewModel.save())
                    window.location.href = "/";

                return false;
            }
        });
    });
&lt;/script&gt;</pre>

Which gives us nice per-field validation error messages as we type:

[<img src="http://ivanz.com/wp-content/uploads/2011/06/knockout-edit-client-validation.png" alt="" title="knockout-edit-client-validation" width="676" height="276" class="aligncenter size-full wp-image-909" />][12]

#### Wrapping up

[Download all of the source code from GitHub as a Visual Studio solution.][4]

In this part we looked at Knockout JS and how great it is in terms of helping us avoid manipulation of HTML markup among many other things. This is generally something that can get really messy quickly and become hard to maintain and track. We also looked at how it helps us neatly solve our collection editing problem without having to worry about practically anything. In the expense of the sacrifice of our Html helpers and partial views, etc we get a lot of flexibility on the client side to add more interactivity in a neat and clean way.

#### Final Words

If you have a simple collection editing scenario and you are heavily reliant on your ASP.NET MVC views, partial views, editors and displays the approach documented in [Part 1][13] should be sufficient. And if pulling in new entries/rows via AJAX is not an option (e.g. for speed, performance or any other reasons) you can pull in [Part 2][14]. However IMHO the better solution most of the time is to use Knockout JS and what I have described in this part as it opens the doors for more complex client-side interactions in a neat and clean way.

Thank you for reading the series. I will appreciate your feedback in the comments!

 [1]: http://ivanz.com/2011/06/16/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-1/ "Editing Variable Length Reorderable Collections in ASP.NET MVC – Part 1"
 [2]: http://ivanz.com/2011/06/20/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-2/ "Editing Variable Length Reorderable Collections in ASP.NET MVC – Part 2"
 [3]: http://knockoutjs.com/
 [4]: http://github.com/ivanz/ASP.NET-MVC-Collection-Editing
 [5]: http://blog.stevensanderson.com/2010/07/05/introducing-knockout-a-ui-library-for-javascript/
 [6]: http://ivanz.com/wp-content/uploads/2011/06/knockout-edit.png
 [7]: http://ivanz.com/wp-content/uploads/2011/06/knockout-edit-screen.png
 [8]: https://github.com/ivanz/ASP.NET-MVC-Collection-Editing/blob/master/CollectionEditing/Infrastructure/JsonHtmlExtensions.cs
 [9]: https://github.com/ivanz/knockout.jQueryUI-sortable.js/blob/master/knockout.jQueryUI-sortable.js
 [10]: http://ivanz.com/wp-content/uploads/2011/06/knockout-edit-server-validation.png
 [11]: http://docs.jquery.com/Plugins/validation
 [12]: http://ivanz.com/wp-content/uploads/2011/06/knockout-edit-client-validation.png
 [13]: http://ivanz.com/2011/06/16/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-1/
 [14]: http://ivanz.com/2011/06/20/editing-variable-length-reorderable-collections-in-asp-net-mvc-part-2/