---
layout: post
title: "Drupal Views: The Power of Contextual Filters"
date: 2016-03-23 19:12:31 -0600
comments: true
categories: [Drupal, REST, Views, API, Drupal8]
---

On my [last blog entry](http://kevinblanco.io/blog/2016/03/21/build-a-quick-restful-view-in-drupal-8/) I talked about how to build a quick RESTful view in Drupal 8 taking advantage of the RESTful Web Services module that is now in Core. The small endpoint I built returns an array of cars in sale, with relative information about each car. You can see the endpoit working here: `http://dev-cars-api.pantheonsite.io/api/cars` and here's my view configuration until now: 
{% img /images/view-final.png %}

Very straigtforward, i'm just showing fields of the `Cars` content type.

#What are Contextual Filters useful for?

Right now my view endpoint return all cars in sale, and there's no chance for filtering the results. In a real life app where we would consume this API, we would want to filter them by **brand** or **type**, and even **price range**, etc. We can accomplish this with contextual filters.

**Contextual Filters are, in scence, URL arguments that filter the content of the view**, that's the simplest explanation. So let's say that perhaps you want to get all `Land Rover` cars in sale, or all `Toyota` **and** `4x4` cars, so let's do it. 

-------


#Filter car by Brand and Type#

**Add the contextual filter:**

Go to your view and find the `Advanced` area, usually located right side or the screen and click on **"Add"** button next to the **"Contextual Filters"** label. Now find the field you want to add as an URL argument, in my case is called `Brand` and you will be prompted with some options

- *WHEN THE FILTER VALUE IS NOT AVAILABLE*  **=>** What to display when the argument is not present in the URL. In my case if the argument is not sent I want all cars to be returned, but you can change it for your use case.  You can provide a fixed value, or a "No results found" response, etc. 
- *WHEN THE FILTER VALUE IS AVAILABLE OR A DEFAULT IS PROVIDED* **=>** Here you can define settings like overriding the view title or defining a validation criteria, this is a very powerful configuration that I won't cover in this post, but I will expand it later. 

Go ahead and click save as it is for now, save the View configuration as well. **And that's it!!** Let's give it a try, I want all Toyota cars in sale, just go to `http://dev-cars-api.pantheonsite.io/api/cars/Toyota` and you will see on the response only the toyota cars. If you remove the `Toyota` from the URL you will still get all of them. What goes after the `http://dev-cars-api.pantheonsite.io/api/cars/` it's the argument, in this case `{Toyota}`.

```
[
  {
	"name": "Toyota Vitz",
	"brand": "Toyota",
	"price": "$7500.00",
	"type": "Sedan",
	"photo": "http://dev-cars-api.pantheonsite.io/sites/default/files/styles/large/public/2016-03/Toyota_Yaris_front_20080104.jpg?itok=uJMZTbHm",
	"description": "Small and compact, cheap and fast"
  }
]
```

This query works as a SQL `LIKE` statement so you can send just a piece of the name and Views will try to find all records `LIKE` that argument, example, I have a brand called  `Land Rover` but if I just send `Land` as an argument it will work `http://dev-cars-api.pantheonsite.io/api/cars/Land`

```
[
  {
	"name": "Land Rover Defender",
	"brand": "Land Rover",
	"price": "$15000.00",
	"type": "4X4",
	"photo": "http://dev-cars-api.pantheonsite.io/sites/default/files/styles/large/public/2016-03/L550_15ACC_EXT_LOC02_V1_04__293-111196_500x330.jpg?itok=w5Gud-8b",
	"description": "A great car in great conditions, it only has 4000miles and new whells"
  }
]
```
Now go ahead and do the same but this time add a Contextual Filter by `Type`. In this case my `Type` field defines the car type (sedan, 4x4, truck, etc) so it's a very important filter I want to have. 

This will be a second argument in the URL, so it will be `api/cars/{brand}/{type}`. Now let's find all the Toyota cars that are 4x4 by making a request to `http://dev-cars-api.pantheonsite.io/api/cars/toyota/4x4`. There you got! Now we are not only filtering by `Brand` but also `Type`. 

So now we have finally implemented 2 contextual filters and our little basic API can filter out content, isn't great? On following episodes I will explain about validation criterias which can be really usefull in some cases. 

If you have any questions shoot me a comment, a tweet or an email. 


