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

###CSS

There are 3 css files located in the directory public/css.

* bootstrap.css [This is the default bootstrap file]
* bootstrap-responsive.css [This is the default bootstrap file for mobile devices]
* jobs.css [small 1285 bytes custom css file]

The CSS is based on Bootstrap with very minor modifications in a file called jobs.css.  The Bootstrap CSS was built with [Customize](http://twitter.github.com/bootstrap/download.html).  The parameters I input to this web page are at the bottom of this tutorial.

### Bringing up the Jobs Application

The application is available live on the internet at this URL: <http://stormabq.github.com/jobs/>.

If you have a slow internet connection I would advise running the application locally or be patient while the 1.6M data file downloads.  Once the file has been downloaded your application will be fast, however the latency time to download may be several seconds.

The beauty and power of Backbone is that you can serve a large scale web application simply with a static web server like [Github:Pages](http://pages.github.com/).

To fire up the application on your own machine simply clone the github repository and bring up your static web server in the directory where the index.html file is located.

```bash
python -m SimpleHTTPServer 3000
```

Once this is running, go to <http://localhost:3000/>. 

The port number of 3000 is what I use but you can pick any one you want.

###The Data Model

```js
data:[{
 "uuid":0,
 "url":"http://www.zrato.com",
 "title":"D3 is the core to our visualization framework !",
 "description":"Self starter with some javascript skills in previous projects.",
 "options":{"city":"albuquerque","descriptiontags":["javascript"],"titletags":["d3"]}}
```
 
As you can see the data model is pretty simple.  The key pieces of information are the job title with its associated url and the job description.

The data model is intentionally simple because the idea down the road is to use this data model for displaying information in other data domains.  The data model is a standard model that is returned from search queries. When you enter a search term in Google, Blekko, Duck Duck Go you always get back a URL, the title associated with that URL and a small description of the web site you are in search of.  So the data model we use is very similar and should provide a nice way to take this data model and possibly meld it into your data needs.

You will note that there is one other field called the **options** field. In the future, this field will be the one to change for other types of data domains.

###The Html file

In backbone templates are mission critical to the application.  The reason being that it is a combination of your data in the model and the structure of your content on the page that gets melded together to form the final piece of HTML that the browser displays.

The three key lines of code in the HTML file are:

```html
<div class="container">
	    <div id="menuheader"></div>
	    <div id="menudropdown"></div>
		<div id="content" class="content"></div>
</div>
```

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

###The JSON Data Model

The JSON data files are in the directory public/data.

You will note there are two files.

* dataset.js : This is the default file the application is using
* sampledata.js :  If you want to play with the application use this file instead.

Change the one line of code in index.html to switch between the two datasets.

If you are curious to view the JSON data structure I would recommend viewing it in the 

[Json Viewer](http://jsonviewer.stack.hu/)

If you get it working you should see the five top level tags.  Make sure to remove *"var mydata ="* from the head of the JSON file.

```js
var mydata =
```

To review the data model further there are four top level tags in the data model.

The final three remaining tags are 

```js
cities:["austin","boston","newyork","seattle","sfbay"],
titletags:["backbone","django","javascript","node","python","ruby","rails"],
descriptiontags:["backbone","d3","django","handlebars","javascript","node","python","rails","ruby"]
```

and this brings us to the menudropdown template which uses these three remaining tags to build its structure.

### Handlebars.js and the MenuDropdown Template

This is [Handlebars](http://handlebarsjs.com/) template.

This template is fairly simple to understand.

```html
{{#each city}}
   <li><a href="#city/{{this}}">{{this}}</a></li>
{{/each}}
```

As you can see built in to the Handlebars template engine is the each iterator very similar to the one in underscore.  So it reads the cities tag above and generates out HTML for each one of the cities. 

### serializeData

The core view type that the other Marionette views extend from is

```js
Marionette.View = Backbone.View.extend
```

One of the most interesting things I do in the code is to over ride the serializeData method which is located in the top level Marionette.View.  The reason I do this is because I did not want to introduce yet another model into my code but rather I simply wanted to grab the JSON data I aleady have. This is a nice technique that allows you to get static data from your JSON file.  Simply override this method and then you have your data sitting there ready for your view.

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

The PageTop Template uses the Marionette concept of [CompositeViews](http://lostechies.com/derickbailey/2012/04/05/composite-views-tree-structures-tables-and-more/).  If you are not familiar with this concept in Marionette I would go ahead and read Derick's excellent description of this concept in his blog.  In there you will find some example JSFiddles which elucidate the power of the CompositeView and in my humble opinion one of the most exciting ways that Backbone is extended today.

This application is yet another example of a way to better understand the CompositeView concept.

The PageJobsView uses as its template the pagetop-template.

One of the interesting aspects of the code is the following three functions which all do the same thing. They take in a collection and returns a subset of the collection with only models that match the specific city tag, title tag, or description tag.

* cityFilter
* tagTitleFilter
* tagDescriptionFilter

It is the collection returned from these functions represented by the var subset which get passed into the constructor of the PageJobsView along with the model PageTopModel.

If you have made it this far in the tutorial you are almost done !

###The Two Core Models

We are only using two models in the system even though there are many more views. To keep this application simple we are only using two core models and one experimental model in the "Experimental" section of the application called MiniLayout.

The first model we just discussed above called PageTopModel.

The second important model is the Job Model.  This model gets populated at initialization time just before the application starts up.

Moving the data from the static JSON file to the Job Model simply iterate over each job in the top level tag called data and then populate the Job model by calling new Job and passing the attributes into the constructor. 

### Bootstrap Customize Configuration Blue Black White

* @navbarBackground            #0055cc
* @navbarBackgroundHighlight   #0055cc
* @navbarText                  #ffffff
* @navbarBrandColor            #ffffff
* @navbarLinkColor             #ffffff
* @navbarLinkColorHover        #000000
* @navbarLinkColorActive       #000000






