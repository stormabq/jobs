Jobs is a tutorial about Derick Bailey's amazing [Marionette](https://github.com/derickbailey/backbone.marionette) framework which sits on top of Backbone.  The idea is to show how Marionette works through a simple application that displays information located in a JSON file.  

The code is intentionally simple to follow and understand.  There are two files.

index.html and jobs.js

The dependencies are the standard backbone ones along with handlebars.  The fact that I am using handlebars gives you another example of how to use Marionette with Handlebars.
Thanks to David Koblas for quickly showing me how to integrate Handlebars into Marionette.  Its two lines of code and you can see it in action in this example.

Backbone.Marionette.TemplateCache.compileTemplate = function(template) {
    return(Handlebars.compile(template));
}

The Css is based on Bootstrap which most people should already be vaguely familiar with.

To fire up the application on your own simply clone the github repository and bring up your static web server in the directory whre the index.html file is located.

python -m SimpleHTTPServer 3000 

The port number of 3000 is what I use but you can pick any one you want.

The Data Model

{"uuid":3,
 "url":"http://www.zrato.com",
 "title":"D3 is the core to our visualization framework !",
 "description":"Self starter with some javascript skills in previous projects.",
 "options":{"city":"albuquerque","descriptiontags":["javascript"],"titletags":["d3"]}}
 
As you can see the data model is pretty simple.  The key pieces of information are the job title with its associated url and the job description.

The data model is intentionally simple because the idea down the road is to use this data model for displaying information in other data domains.  The domains I am interested in besides showing cool jobs available on the web is financial data and bioinformatics data.  But in general the data model is a standard model that is returned from search queries. When you enter a search term in Google, Blekko, Duck Duck Go or your search engine of choice you always get back a URL, the title associated with that URL and a small description of the web site you are in search of.  So the data model we use is very similar and should provide a nice way to take this data model and possibly meld it into your data needs.

You will note that there is on other field called the options field.  This is the field that everyone can change to suit their specific application.  For the jobs data set it makes sense to know what city the job is in and tags or keywords associated with the job title and the job description.  For other domains of knowledge I can envision other option tags that would be suitable.  But the dropdowns will work just fine for the other options tags that you insert instead.  More on this topic later.

The Html file

In backbone, as everyone already knows, templates are mission critical to the application.  The reason being that it is a combination of your data in the model and the structure of your content on the page that gets melded together to form the final piece of magical HTML that the browser displays.  If you think about what Backbone really does is it provides a simple framework to move data to and from the web page in combination with a template which is a representation of the skeleton content.  Besides this, it makes it really easy to detect user key stokes, hand gestures, and mouse moves via the events.

In the jobs html file which is called index.html you will see a set of templates that are tied to different functionality in the application.  The only static html in the index.html file are the tabs plus the following three lines of code plus a standard bootstrap footer.  The rest of the HTML in the application are templates.  So the code should be pretty simple to digest and understand if you are so inclined.  That is the idea behind backbone.  Keep it simple and pretty much use all templates for content generation.

<div class="container">
	    <div id="menuheader"></div>
	    <div id="menudropdown"></div>
		<div id="content" class="content"></div>
</div>

With this in mind, if I describe what each one of the templates does along with the associated code then you should understand the code in detail and be able to go off and write your own application that visualizes a static JSON data file.  The hardest part to visualizing data with backbone is coming up with a generic enough data model that can then be used across other projects. 

The Templates







 
