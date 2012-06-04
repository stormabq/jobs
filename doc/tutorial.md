#Jobs: A Marionette Tutorial

Jobs is a tutorial about Derick Bailey's amazing [Marionette](https://github.com/derickbailey/backbone.marionette) framework which sits on top of [Backbone](http://documentcloud.github.com/backbone/).  The goal of the tutorial is to show how Marionette works through a simple application that displays information located in a [JSON](http://www.json.org/) file.  

The code is intentionally simple to follow and understand.  There are two files.

* index.html
* jobs.js

The dependencies are the standard Backbone ones along with [Handlebars](http://handlebarsjs.com/).  The fact that I am using Handlebars gives you another example of how to use Marionette with Handlebars.

Thanks goes to Derick Bailey for taking the time to write and maintain Marionette and [David Koblas](https://github.com/koblas) for quickly showing me how to integrate Handlebars into Marionette.  Its two lines of code and you can see it in action in this example.

```js
Backbone.Marionette.TemplateCache.compileTemplate = function(template) {
    return(Handlebars.compile(template));
}
```

The Css is based on Bootstrap which most people should already be vaguely familiar with.

To fire up the application on your own simply clone the github repository and bring up your static web server in the directory where the index.html file is located.

```bash
python -m SimpleHTTPServer 3000
```

Once this is running, go to <http://localhost:3000/>. 

The port number of 3000 is what I use but you can pick any one you want.

###The Data Model Part I

```js
data:[{
 "uuid":0,
 "url":"http://www.zrato.com",
 "title":"D3 is the core to our visualization framework !",
 "description":"Self starter with some javascript skills in previous projects.",
 "options":{"city":"albuquerque","descriptiontags":["javascript"],"titletags":["d3"]}}
```
 
As you can see the data model is pretty simple.  The key pieces of information are the job title with its associated url and the job description.

The data model is intentionally simple because the idea down the road is to use this data model for displaying information in other data domains.  The domains I am interested in besides showing cool jobs available on the web is financial data and bioinformatics data.  But in general the data model is a standard model that is returned from search queries. When you enter a search term in Google, Blekko, Duck Duck Go or your search engine of choice you always get back a URL, the title associated with that URL and a small description of the web site you are in search of.  So the data model we use is very similar and should provide a nice way to take this data model and possibly meld it into your data needs.

You will note that there is on other field called the options field.  This is the field that everyone can change to suit their specific application.  For the jobs data set it makes sense to know what city the job is in and tags or keywords associated with the job title and the job description.  For other domains of knowledge I can envision other option tags that would be suitable.  But the dropdowns will work just fine for the other options tags that you insert instead.  More on this topic later.

###The Html file

In backbone, as everyone already knows, templates are mission critical to the application.  The reason being that it is a combination of your data in the model and the structure of your content on the page that gets melded together to form the final piece of magical HTML that the browser displays.  If you think about what Backbone really does is it provides a simple framework to move data to and from the web page in combination with a template which is a representation of the skeleton content.  Besides this, it makes it really easy to detect user key stokes, hand gestures, and mouse moves via the events.

In the jobs html file which is called index.html you will see a set of templates that are tied to different functionality in the application.  The only static html in the index.html file are the tabs plus the following three lines of code plus a standard bootstrap footer.  The rest of the HTML in the application are templates.  So the code should be pretty simple to digest and understand if you are so inclined.  That is the idea behind backbone.  Keep it simple and pretty much use all templates for content generation.

The three key lines of code in the HTML file are the following div id's:
MenuHeader, MenuDropdown and Content

```html
<div class="container">
	    <div id="menuheader"></div>
	    <div id="menudropdown"></div>
		<div id="content" class="content"></div>
</div>
```

With this in mind, if I describe what each one of the templates does along with the associated code then you should understand the code in detail and be able to go off and write your own application that visualizes a static JSON data file.  The hardest part to visualizing data with backbone is coming up with a generic enough data model that can then be used across other projects. 

###The Templates

The MenuHeader Template and the PageTop Template

This template only has one parameter in it which is the MenuHeader.  Examples of MenuHeaders include:

* Tab 01 : Select a City or Job from the Dropdowns
* Tab 02 : Complete List of Jobs
* Tab 03 : Complete List of Jobs with Descriptions
* Tab 04 : Experimental Section
* Tab 05 : Documenation Section

* Dropdown Cities : A name of a city that you select in the dropdown
* Dropdown Titles : A name of a title tag that you select in the dropdown
* Dropdown Descriptions : A name of a description tag that you select in the dropdown

If you look at the code you will see that these menu headers get set in the constructor of the Backbone.Model called PageTopModel.

###The Data Model Part II

The JSON data files are in the directory public/data.

You will note that there are two files.  The sample file which I am using in this tutorial and the real data file which is what the application is driven off of.  If you want to run the application with the sample file simply change the line of code in the index.html file and you will see the sample data instead of the bigger data set.

If you are curious to view the JSON data structure I would recommend viewing it in the 

[Json Viewer](http://jsonviewer.stack.hu/)

If you get it working you should see the five top level tags.  Make sure to remove this stuff from the front of the JSON file.

```js
var mydata =
```

To review the data model further there are five top level tags in the data model.

I have already reviewed one of the top level tags called "data" above.

One of the other top level tags is

```js
tags:["lisp","python","handlebars","ruby","d3","ceo","cfo","search","rails"]
```

However this is really simple to understand because it is not being used, it is here simply for the user edification process to better understand what tags are available in the system.  However, you will note that the titletags and descriptiontags are a subset of the tags which makes sense.  I am showing what tags are possible and then the resulting data set looks for these words in titles and descriptions and comes up with a final result in each one of the numerous data tags.

So we have already reviewed the top level tag "data" and "tags".

The final three remaining tags are 

```js
cities:["dothan","pittsburgh","cleveland","albuquerque"],
titletags:["ceo","cfo","d3","python"],
descriptiontags:["search","handlebars","ruby","rails"]
```

and this brings us to the menudropdown template which uses these remaining tags to build its structure.

### The MenuDropdown Template

If you have not used [Handlebars](http://handlebarsjs.com/) in the past it is really very simple.  I would go off and take about 10 minutes to read the page and by then you will pretty comfortable understanding the following concept.

This template is fairly simple to understand.  The only minor tricky part is the use of the 

```html
{{#each city}}
   <li><a href="#city/{{this}}">{{this}}</a></li>
{{/each}}
```

As you can see built in to the Handlebars template engine is the each iterator very similar to the one in underscore.  So it reads the cities tag above and generates out HTML for each one of the cities. 

The core view type that the other Marionette views extend from is

```js
Marionette.View = Backbone.View.extend
```

One of the most interesting things I do in the code is to over ride the serializeData method which is located in the top level Marionette.View.  The reason I do this is because I did want to introduce yet another model into my code but rather I simply wanted to grab the data I aleady had and since it is static and never changes I figured why not just grab the data with the serializeData method.  This is a nice technique that allows you to get static data from your JSON file if for some reason you find your self needing to do this in your Marionette applications.  Simply override this method and you magically have your data sitting there ready for your view.

```js
var MenuDropdownView = Backbone.Marionette.ItemView.extend({
  template: "#menudropdown-template",
  
  serializeData: function(){
    var citytitledescription = {
        "city": mydata.cities,
        "title": mydata.titletags,
        "description": mydata.descriptiontags
    };
    return(citytitledescription);
  }
});
```

If you go back up and review the three key lines of code in the HTML file you will notice that we have now already covered two of the three lines.  The final thing to review is the content.

These templates get executed when you select a city, titletag, or descriptiontag from the dropdown menu.  When you first come to the page the following two templates are not executed.  It is the act of selecting something from the dropdown that fires off an event that executes the router and then these templates get executed in Handlebars.

It should be noted that I am using the Bootstrap Javascript function called [bootstrap-dropdown.js](http://twitter.github.com/bootstrap/javascript.html#dropdowns).  As you can see in order to use this code you simply place the bootstrap.js file in your html file and then use the proper class type in your HTML and you magically get the desired functionality.

### The PageJob Template

The PageJob template is represented by a PageJobView which extends the Backbone.Marionette.ItemView

### The PageTop Template

The PageTop Template uses the Marionette concept of [CompositeViews](http://lostechies.com/derickbailey/2012/04/05/composite-views-tree-structures-tables-and-more/).  If you are not familiar with this concept in Marionette I would go ahead and take another ten minutes and read Derick's excellent description of this concept in his blog.  In there you will find some example JSFiddles which elucidate the power of the CompositeView and in my humble opinion one of the most exciting ways that Backbone is extended today.

This application is yet another example of a way to better understand the CompositeView concept.

The PageJobsView uses as its template the pagetop-template.

One of the interesting aspects of the code is the following three functions which all do the same thing. They take in a collection and returns a subset of the collection with only models that match the specific city tag, title tag, or description tag.

* cityFilter
* tagTitleFilter
* tagDescriptionFilter

It is the collection returned from these functions represented by the var subset which get passed into the constructor of the PageJobsView along with the model PageTopModel.

If you have made it this far in the tutorial you are almost done !

###The Two Core Models

We are only using two models in the system even though there are many more views.  I believe you will find that in Backbone single page applications there are usually more views than models.  I can not think of an example where you would have more models than views unless you are composing models into other models.  To keep this application simple we are only using two models.  Actually there are three, but the third one doesn't really count for now as it is in the "Experimental" section of the application called MiniLayout.

The first model we just discussed above called PageTopModel.

The second important model is the Job Model.  This model gets populated at initialization time just before the application starts up.

Moving the data from the static JSON file to the Job Model enables a slightly simpler representation of the data.  If you have a JSON file which is slightly more complicated or convoluted and you want to simplify the Model and View representation then populating your Model's by hand is probably the way to go and that is what I am doing to get the data into the Job Model.  I simply iterate over each job in the top level tag called data and then populate the Job model by calling new Job and passing the attributes into the constructor. 

### Bootstrap Configuration

@navbarBackground            #ccf600
@navbarBackgroundHighlight   #ff200
@navbarText                  @purple
@navbarLinkColor             @red
@navbarLinkColorHover        @green



