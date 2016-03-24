---
layout: post
title: "Build a quick RESTful View in Drupal 8"
date: 2016-03-21 20:06:54 -0600
comments: true
categories: [Drupal, REST, Views, API, Drupal8]
---
----

Drupal 8 is the new version of Drupal that actually came out just a couple of months ago and which development phase started at 2011, so it brings a lot of new chages to the table including [Symphony components](http://symfony.com/blog/symfony2-meets-drupal-8), [Twig as the template engine](https://www.drupal.org/theme-guide/8/twig) and many [other cool stuff](https://www.drupal.org/8).

One of the new changes added to Drupal8 is that Views module is now [part of the core](https://www.drupal.org/node/1912118), which is something that makes a lot of sense, since Views is part of pretty much all Drupal sites.  

With RESTful Web Services also in Core, we now have all the tools we need to create highly customizable solutions out of the box in regards to RESTful services. 

In this blog post, I will show you how to create a view that returns a list of cars (content-type) in JSON via the REST API. Let’s get started!

First you need a Drupal 8 installation, if you don't have one, it's very easy to get up and running using [Drupal Console](https://drupalconsole.com/)'s command `$ drupal site:new` or if you prefer a more visual way, you can download and use [Acquia's Dev Desktop](https://www.acquia.com/downloads) and use the wizard to create a new site. 

#Activate required modules#
For our little example, we will need to activate Drupal Core's **RESTful Web Services** and **Serialization** modules. To do this go to ***Extend*** and find/activate the modules mentioned above. Also make sure that **Views** and **Views UI** are enabled (they usually are by default).

{% img /images/modules.png %}

#Create the content type#

I want a service that returns a list of cars in sale in JSON format, so Drupal will take care of managing the content, and on a later post i'm currently working on, we'll consume the API with Angular2. 

Let's create a new content type called `Car` and add some fields for `Price`, `Photo`, `Brand`, `Model` and `Type`.  Go to ***Structure > Content Types > Add Content Type*** and fill the requested fields. For the `Title` field label it as `Name`. Here's a picture of how my content type looks:
{% img /images/content_type.png %}

Now, we might want to create a few cars before we create the service so we have content to return. Also you can use the [Devel](https://www.drupal.org/project/devel) to generate "Dummy" content for testing. If you are using Drupal Console just use `$ drupal module:download devel` and then `$ drupal module:install devel`. Then just go to ***Configuration > Development > Generate content***, and create a bunch of cars.

#Creating the view#
Now let’s create our view. Go to ***Structure > Views > Add new view***. Name it `Cars` and select `Cars` in the **type** selector.  We do not need to create a page or block, so uncheck those options to keep things simple and check `Provide a REST export` and define the path you wich the API respond to, in my case `api/cars/view`.
{% img /images/view_setup.png %}

After you hit `Save and edit` you will be prompted with a regular Drupal View UI, the only difference is that the output will be JSON. As any other View, you can control access by role, use contextual filters, sorting, etc. By default, you will see all the data for every car, including metadata. You might want to pick the fields you wish to send on the JSON response by switching to `Show: Fields` in the **FORMAT** section. Now you can select all the fields you want to display on the **FIELDS** section, in my case I selected `Title`, `Brand`, `Price`, `Type`, `Photo`, and `Body`(description). 
{% img /images/view-final.png %}

You can view your results in the path we defined when we created the view, im my case is `api/cars`. I have a LIVE example in the following URL: `http://dev-cars-api.pantheonsite.io/api/cars`. Right now as i'm writing this post looks like this: 

```
[{
	"name": "Toyota Prado",
	"brand": "Toyota",
	"price": "$30000.00",
	"type": "4X4",
	"photo": "http://dev-cars-api.pantheonsite.io/sites/default/files/styles/large/public/2016-03/download.jpeg?itok=3QRT2liF",
	"description": "New version, full extras. brand new"
}, {
	"name": "Toyota Vitz",
	"brand": "Toyota",
	"price": "$7500.00",
	"type": "Sedan",
	"photo": "http://dev-cars-api.pantheonsite.io/sites/default/files/styles/large/public/2016-03/Toyota_Yaris_front_20080104.jpg?itok=uJMZTbHm",
	"description": "Small and compact, cheap and fast"
}, {
	"name": "Land Rover Defender",
	"brand": "Land Rover",
	"price": "$15000.00",
	"type": "4X4",
	"photo": "http://dev-cars-api.pantheonsite.io/sites/default/files/styles/large/public/2016-03/L550_15ACC_EXT_LOC02_V1_04__293-111196_500x330.jpg?itok=w5Gud-8b",
	"description": "A great car in great conditions, it only has 4000miles and new whells"
}, {
	"name": "Mitsubishi Montero Sport",
	"brand": "Mitsubishi",
	"price": "$10000.00",
	"type": "4X4",
	"photo": "http://dev-cars-api.pantheonsite.io/sites/default/files/styles/large/public/2016-03/1342471999-2000_Mitsubishi_Montero_Sport_0.JPG?itok=91O63bRS",
	"description": "Beautiful 4x4u00a0Mitsubishi Montero Sport 99' model for 7 passengers"
}]
```
You might be wondering how I changed the labels of each field so it doesn't display like `field_photo` for example? Well it's quite simple, just click in the **field settings** located at the **FORMAT** section, and you will be able to define an alias for each field.

{% img /images/alias.png %}

**And that's it!** We have our endpoint returning the cars data as JSON and we can consume it wherever we want. If you have any questions shoot me a comment, a tweet or an email.  

