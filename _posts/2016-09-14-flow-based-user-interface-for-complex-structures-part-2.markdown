---
layout: post
title:  "Flow based user interface for complex structures (part 2)"
date:   2016-09-14 16:12:00 +0200
categories: ui dev
comments: true
author: "Dion"
---

[Part 1](https://dionsnoeijen.github.io/ui/dev/2016/08/11/flow-based-user-interface-for-complex-structures-part-1.html "Part 1") was the introduction to this article. Read it if you like a little more background on the subject. Otherwise just skip it and dive in head first.

To explore this idea I have for a node or flow based ui for web development I need to define from where I'm reasoning.

First, I speak of web development, but my main goal is to create a user interface that enables a simplified way of creating a website that's content managed. From there on I see a lot of other possibilities but for now I stick to the plan.

Second, this is a concept and the fictional website is intentionally simplified.

Last, I try to create a user interface that's simple, but not too simple. I favor flexibility over simplicity.

The fictional website I'm aiming to build is nothing more than website containing products, showing a list of products and a product detail page. Those products are managed through a backend that's behind a user login page. Containing a product list for visitors and a way to add and edit products for content editors. To spice things up a little it's a multi lingual setup containing only two languages namely dutch and english. Adhering to my last point, the product has only the most essential fields. There is no need for more because I only want to convey the idea.

First, I imagine, I would want to model the product. The product is mainly made up of fields. Those fields are more than a simple column in the database. In fact they are much more like a field as seen in some content management systems like ExpressionEngine or Craft where a field is a way of defining much more than just a table column. It also provides all the ui elements needed for the content editors. The content's of the field can easily be used inside a template. It depends on the field type what it can provide.

Let's start by creating a breadboard on the grid.

![Breadboard product model](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/breadboard-product-1.PNG)

<span class="sub">The breadbox should be resizable at any time</span>

Surely I would like to have a name for this breadboard, and, I think I need types for a breadboard in certain cases. I will get back to that later on. But this is a "model" type breadboard.

I think those type of settings should be available by double clicking or tapping it. It would open a dialogue box that contains fields for the name and type.

![Breadboard model type dialogue](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/breadboard-dialog.PNG)

For this product we probably need a name, let's start by adding a input text field. Again, this field would need some settings. Double clicking on the field gives a dialogue belonging to this specific input field. A name and a handle would be essential. Maybe an optional default text. The field on the breadbox shows it's label by default. I think there should be one block reserved for the icon belonging to the field. To the left we would have a menu containing all elements you can stick to the breadboard or onto the grid itself. More on the menu later on.

![Breadboard with input text attached](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/breadboard-product-2.PNG)

To the left we have an input circle available. It's meant to accept connecting wires. To the right we have an output circle available, you can drag a connection wire from it.

This field on itself does not contain enough functionality on it's own. I need extra functionality to make this work in a real world context. For starters, I would like it to be a required field. But I also want the input length limited and from this field I would like to generate an anchor or slug that I can use later on. (If the name is "Super product" the anchor would be `super-product`) At this point the breadboard comes in handy. Instead of already creating wires I can simply stick modifiers to the board enabling those simple modifications for this field.

![Breadboard with modified input text output](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/breadboard-product-3.PNG)

So, instead of having the output of the field connected to the edge of the breadboard I sized it down to the middle. First I connected a required modifier. Then I have a length modifier and that output is on the right edge of the breadboard making an output circle on the breadboard or node level. Below I have an anchor modifier. The output from the length modifier is also fed into the anchor modifier that's bringing it's own output to the right edge of the node. Now we have a defined a required input field, limited characters and we defined an anchor based on this field.

The next field is a description field. In this case I imagine a rich text editor type of field, also limited but not required.

![Added the description field to the breadboard](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/breadboard-product-4.PNG)

The product also requires a price. I would like to min / max the price input, just because I can. And this field is required. Also, I feel I want output in different currencies because we are creating a multi lingual setup. The required modifier provides output, we can make connectors on the breadboard splitting up the output to other modifiers. In this case it goed directly to the euro output and I route it to a math modifier so I can make up for the currency difference and direct it to the dollar output.

![Added the price field to the breadboard](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/breadboard-product-5.PNG)

<span class="sub">The MM modifier is for limiting the numeric input to only contain a value between 0 and 100</span>

Now I just add two date fields, containing the creation and update date. Both are required. On creation both are set to the creation date. The last modifier is an output modifier. Later on it will become clear what it's used for.

![Added the creation and update date fields](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/breadboard-product-6.PNG)

Let's start by using this model for some basic crud functionality for the content editors. We need a list of products and a way to add a new product. Since I have two languages a tabbed interface might do just fine. But I need a way to have the same fields two times in my layout. Actually I need a way to make groups of fields so I can differentiate between them while creating the template for the create / edit page. So I would create two new breadboards of the same size and add a grouping modifier to it. This modifier accepts input from fields, duplicating the field setup.

![The language field groups](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/the-language-field-groups.PNG)

<span class="sub">It would be quite awesome to overrule specific values like the label.</span>

At this point I'm starting to think about metadata that should be coming from the breadboard. Metadata should be sent along the flow, providing context where needed. In this case I think I would at least need a group identification so I can keep apart the fields in my template. Speaking of templates, it's time to add a render node. Again I create a breadboard and I stick a render action to it. To it's output I add a post modifier that I can use to act upon post data.

![The render node](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/first-render-node.PNG)

<span class="sub">The render node I deem simple enough to leave out the breadboard title. The filename, icon and P modifier speak for themselves. Having that said, you should be able to add a title to a breadboard at any time.</span>

Let's wire things up to see what we are getting at.

![Wired breadboards](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/wired-product-admin-boards.PNG)

Ok, we defined fields, grouped them and I have a template (*Product add edit) that produces post data, handled by the P variable attached to the template output. We use this post data to create or update the product. We need a "save" action, that comes equipped with an if block. That way we have the ability to add output on success and on failure. It know's failure or success because the stream of data provides context. It has post data related to the model that also resides in the stream. The model has validation, that's what the outcome is based upon.
 
On success we render a new page, telling the content editor everything is fine. Also, we drag two lines back to the model date fields. Depending on if this is a new entry, we need a creation date. And if it's an update, we need the updated date. This immediately poses some questions. Mainly, is the date updated after the fact? I think not, the connections to the model should only indicate that we also need to generate one of these dates. The system behind the interpretation of this entanglement should be smart enough to know how to act.

Now, we have a very basic setup on how the data should flow on the administrator side of the application, but we are missing some essential parts. For starters, there's no routing available. And if we have routing, we need a way of authentication and authorization aswel. So let's start with that.

First I would need an entrance point, something representing a user making a request. This is represented by a circle with a cloud. It needs a simple output node. In case of the content editors part, we would need to see if the user is logged in. If so, lead the request straight to the admin routes. Depending on the information in the request it chooses it's route. Say we were to visit the admin/products route. First we need to fetch the products. This can be done with the products query. We continue to link the route output to the sql input of the products query node. This is a breadboard made up of a SQL action and an IF action. The sql action is responsible for the query. On success it immediately goes on to connect to the render node. Responsible for rendering the products overview template for the administrator view. The query is using an offset and a limit to accommodate for pagination. How can it know? Maybe we should introduce some get variables like this: ?offset=0&limit=25 but I can imagine more elegant ways to do so. This is for a later article where I explore ways to make this entire setup even more elegant. For now this kind of detail only obfuscates the general idea.

This completes the flow from a request to a certain route, fetching the items and rendering an overview page.

![Completed flow to product admin overview](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/without-user-and-frontend.PNG)

How does it know how we are expecting to return to the admin page you might ask yourself by now? This is because of the route. Every step the flow progresses is aware of the previous steps meaning it's aware of the route that was requested. Based on that the system knows where to go. This brings up the need for metadata again. The breadboards need a way to know where they belong so to speak. The route and the render node should know they belong to the admin side of things.

We probably want to know about the logged in user aswell. To do that we have to make a user model.

![User model](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/user-model.PNG)

The user model is connected to a query breadboard, fetching the user if not already present. When unable to fetch the user it will redirect to the login page. Otherwise the information is passed to the template aswel. Making available a lot of useful data we can use in our template. I imagine a twig template, that at this point would have access to the products, user and the request. In a pseudo Json form it might look like something like this.
    
    {
        "products": [
            {
                "name": "Lorem Ipsum",
                "description": "Dolor Sit",
                "price_euro": "€200,00",
                "price_dollar": "$224,23",
                "creation_date": "YYYY-MM-DDThh:mmTZD"
            },
            {
                "...":"..."
            }
            ...
        ],
        "user": {
            "name": "Henkie",
            "email": "henkie@example.me",
            "...":"..."
        },
        "request": {
            "url": "admin/product/super-cool/edit",
            "...": "..."
        }
    }

<span class="sub">This is much simplified</span>

The other routes for the admin part probably start to make sense on their own now. So I leave it for you to see if you can make something of it. We haven't covered what happends when a user attempts to login but fails. The user may have to create an account. however, I have ideas on how to make that flow more sense using groups like I discussed in the previous article. I will come back to that in the next article. Now it's enough to discuss an over simplified login flow.
 
If the request comes in and finds itself to need a valid user whilst it hasn't. The user node flows to the User routes. Triggering an action to the login page. The post values are used to query the user table and return to the admin routes on success.

![User login flow](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/included-user-flow.PNG)

Now! We also need the front-end, for user to browse the products and gaze at their relentless beauty. This flow is much simpler. A lot of what we need is already in place. We have the models and queries already in place. All we need is an extra block with routes. This block is accompanied with a languages block. The languages block has the ability to give output in a variable type of way. Making the {lang} usable in the routes blok. Prefixing the routes with language. I can imagine more elegant ways, to make routes totally dependent on the language. More on that in the next article ;). I think the front-end flow speaks for itself. One thing worth to mention is het product model. I made a modifier that duplicates it's output nodes. That way you have a way better organize the flow, again preventing too many lines all over the place.

The menu should be simple, a matter of dragging and dropping stuff on the grid. The inner workings of the breadboard, especially on the ux side needs some thinking still.

![The menu](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/the-menu.PNG)

Here is what the user interface looks like at this point.

![Completed flow](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/everything.PNG)

Here is a pdf file with the design: [PDF](https://s3-eu-west-1.amazonaws.com/dionsnoeijen/nodes/nodes.pdf)

As you can see, we still have a lot going on and the risk of making an unintelligible mess is all too prevalent. In fact there still are a lot of open ends to the current status of this concept. I have a lot of ideas that I want to explore in the next article. Hopefully I can gather input that I can use for the refinement needed to make this a potentially powerful method of building logical and working application flows. Thank you!

