---
title: Java BDD with JBehave and Watij in Eclipse with JUnit
author: Ivan Zlatev
layout: post
permalink: /2011/05/25/java-bdd-with-jbehave-and-watij-in-eclipse-with-junit/
ratings_users:
  - 0
ratings_score:
  - 0
ratings_average:
  - 0
views:
  - 12152
dsq_thread_id:
  - 313622719
categories:
  - Coding
tags:
  - BDD
  - HowTo
  - Java
---
The purpose of this post is to explain how to write BDD acceptance tests in Java using the gherkin syntax, run those with JUnit and implement them using Web Browser automation.

Before I begin let me assure you that I haven&#8217;t left the .NET church for the Java one and while I really don&#8217;t enjoy writing in Java and using Eclipse sometimes a man has to do what a man has to do.

So yeah I spend an afternoon trying to figure out the state of BDD (Behavior Driven Development) tools for Java and it&#8217;s pretty poor so far. If you want to write gherkin syntax based behavioral specifications, which are executable inside Eclipse as JUnit tests you are stuck only with [JBehave][1]. JBehave however is a monster with poor documentation, not so easily discoverable fluent API and little to none third party documentation content on forums and sites. BTW don&#8217;t get fooled like me &#8211; insetad of getting the core package you want to get the Web package because the latter has all the .jar dependencies bundled unlike the first one. Enough bashing, lets get down to business.

Let&#8217;s say we have the following &#8220;Search&#8221; story and a more specific scenario &#8211; &#8220;display an error dialog if I don&#8217;t fill in the search form&#8221;:

<pre class="brush: plain; title: ; notranslate" title="">Narrative: I should be able to search

Scenario: I should see an error if I try to search without filling in the search form fields.

When I open 'http://mysite.com'
When I click on the 'Search' button
Then I should see an error</pre>

First thing you will notice is that JBehave doesn&#8217;t support the &#8220;And&#8221; keyword so we can&#8217;t wring &#8220;When I open &#8230; AND I click on the Seach button&#8221;.

Similar to other gherkin/cucumber based frameworks JBehave allows us to implement each step of the scenario as a method in a class, which optionally takes parameters mapped to words from the scenario. The above one can be stubbed like bellow. Also not that if properly configured you can make JBehave junit runner spit out the stubs for you to copy paste in your code (more about that later):

<pre class="brush: java; title: ; notranslate" title="">public class Search
{
	@When("I open '$page'")
	public void whenIOpen(String page)
	{
		// TODO
	}

	@When("I click on the '$buttonText' button")
	public void whenIClickOnTheButton(String buttonText)
	{
		// TODO
	}

	@Then("I should see an error")
	public void thenIShouldSeeAnError(){
		// TODO
	}
}</pre>

To implement the browser automation in Java there exist a Java variant of the Ruby watir library (and the .NET Watin library) called &#8230; surprise&#8230; watiJ. It&#8217;s available [here][2]. Major pitfall with it though is that it actually embeds the IE COM component and mozilla&#8217;s ghecko engine instead launching the real IE/Firefox (like the above mentioned framework do) and it only supports IE6 and I saw some C++ runtime problems with the ghecko engine. Nevertheless to give you a feel of how the above tests can be implemented:

<pre class="brush: java; title: ; notranslate" title="">public class Search extends StoryBase {

	@When("I open '$page'")
	public void whenIOpen(String page)
	{
		WebBrowser.open(page).pauseUntilReady();
	}

	@When("I click on the '$buttonText' button")
	public void whenIClickOnTheButton(String buttonText)
	{
		Tag button = WebBrowser.find.button().with.name("search");
		assertNotNull(button);
		button.click().pauseUntilReady();
	}

	@Then("I should see an error")
	public void thenIShouldSeeAnError(){
		Tag errorDialogTitleDiv = WebBrowser.find.span().with.className("dijitDialogTitle");
		assertNotNull(errorDialogTitleDiv);
		assertEquals("Error", errorDialogTitleDiv.get.innerHTML());
	}
}</pre>

Now, how about running them? A few things before that:

  * JBehave expects you to save stories as .story files in your source code tree
  * JBehave expects you to implement and configure problematically a *JUnitStory *for each story, so that it&#8217;s runnable through JUnit.
  * It will take your JUnitStory subclass implementation &#8211; for example &#8220;*HelloWorldStory extends JUnitStory&#8221; *and look for the plain text file *hello\_world\_story.story*

To save myself from having to configure each story  I present you my *StoryBase *base class, which:

  * Initializes WatiJ and exposes it to subclasses
  * Configures JBehave to look for the .story files in the same directory as the class.
  * Configures JBehave/JUnit to look for the steps test methods of a story scenario in the subclass.
  * Configures JBehave to print out detailed information in the Console whilst running (such as the method stubs if any are missing)
  * Configures JBehave to spit out HTML report in *myproject/target/jbehave-report/*

Here it is:

```
public abstract class StoryBase extends JUnitStory {
    protected final static WebSpec WebBrowser = new WebSpec().ie();

    @Override
    public Configuration configuration() {
        return new MostUsefulConfiguration()
            .useStoryLoader(new LoadFromClasspath(this.getClass().getClassLoader()))
    	    .useStoryReporterBuilder(
    	    		new StoryReporterBuilder()
    	    			.withDefaultFormats()
    	    			.withFormats(Format.HTML, Format.CONSOLE)
    	    			.withRelativeDirectory("jbehave-report")
			);
    }

    @Override
    public List candidateSteps() {
        return new InstanceStepsFactory(configuration(), this).createCandidateSteps();
    }
}
```

Then all that&#8217;s left is to make the Search class extend the StoryBase class and it is now runnable in JUnit inside and outside Eclipse.

<pre class="brush: java; title: ; notranslate" title="">public class Search extends StoryBase {
}</pre>

[<img class="aligncenter size-full wp-image-764" title="Project Tree" src="{{ site.url }}/wp-content/uploads/2011/05/Untitled.png" alt="" width="178" height="108" />][3]  
[<img class="aligncenter size-full wp-image-763" title="junit" src="{{ site.url }}/wp-content/uploads/2011/05/junit.png" alt="" width="649" height="85" />][4]

&nbsp;

 [1]: http://jbehave.org/
 [2]: http://watij.com/
 [3]: {{ site.url }}/wp-content/uploads/2011/05/Untitled.png
 [4]: {{ site.url }}/wp-content/uploads/2011/05/junit.png