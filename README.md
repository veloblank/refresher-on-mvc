# Refresher on MVC

## Overview

We'll review what a Model-view-controller (MVC) is, the problems it solves, and learn some new key concepts.
Before bombarding you with information about how an MVC pattern works, we will describe the problem it solves.

## Objectives
1. Understand the problem that MVC was created to answer
2. Understand why we compartmentalize an application's functionality
3. Examine an implementation of MVC
4. Understand the Rail's implementation of an MVC architecture

## Forward

Within the curriculum, you have been working hard learning defined small scale technical concepts, problems, and solutions. To understand MVC, we need to back up and take on a macroscopic, application wide, lens. For this lesson, we are assuming the mindset of a [_software architect_][software_architect].

One of the initial challenges to understanding MVC is that it was specifically designed to _abstract away from the user how an application is functioning_. As we have been unknowing 'users' of the MVC pattern most of our lives, we need to change some of our wiring to understand it as programmers.

## The Problem and the Solution

Human brains and computers function differently. As a result, it can be difficult for individuals to conceive of and create complex applications and user interfaces while maintaining code structure, modularity, and maintainability. The MVC pattern describes one (very popular) way of thinking about and structuring applications with a simplified user experience in mind.

Programmers, confronted with creating increasingly complex programs that non-technical users needed to meaningfully engage with, introduced MVC in the late '70s. It was created as a general purpose solution which bridged the gap between a user's mental model of a program and a computer's _actual_ model of a program.

The pattern has gained popularity over the decades. While initially implemented in desktop computing solutions, web frameworks such as Ruby's Rails and Python's Django have played a significant role in popularizing MVC for web applications.

## Separation of Concerns
TODO: tick wrapper for mvc

At its core, MVC is designed to modularize distinct functionality within an application. Specifically, MVC modularizes a very common type of application -- one that has persistent data a user interacts with. Let's identify the distinct parts of MVC:
  * ```View```: what an end user on a website experiences when interacting with a program in their browser
  * ```Model```: where the actual data, (be it information about restaurants, legal statutes, or rare marsupials), resides and is altered
  * ```Controller```: what manages communication between the two

From an end-user perspective, a successful MVC implementation reduces these three distinct parts into a single experience. The user perceives that they are interacting with the **model** directly -- altering and accessing the actual data freely.

As programmers, we know that is not the case. Let's use a simplified web-application as an example of the common pattern that MVC bears out:

1. A user, through interaction with the ```view```, (in this case, the browser's GUI), requests data from a server
2. The request is sent across the internet to a server
3. The server interprets the request and ```routes``` it to the appropriate ```controller``` function designed to fulfill that request
4. The ```controller``` function (_which does not change data itself!_) asks the ```model``` to either send it data or to change its data
5. The ```model``` accesses/manipulates the actual data and returns the desired result to the ```controller```
6. The ```controller``` packs a response and sends it back to the client that sent the initial ```request```
7. The ```view``` _represents_ the data

In considering the above steps, try to answer the following question:
* Do the **model** and **view** have any actual direct interaction?
* To what MVC component does the **routing** functionality described in step 3 belong to?
* Is the data represented in the ```view```, that the user sees and interacts with, anything but a carbon copy of the _actual_ data they perceive they are interacting with?

### An Advantage of Modularity
In our example, we have identified three distinct parts, and two distinct communication layers: TODO GRAPHIC? (```model``` <--> ```controller``` <--> ```view```). In our application, for example, communication over an internet protocol bridges the ```controller``` <--> ```view```. These communication layers  have a 'standardizing' effect. They enable us to de-couple different parts of our ```model``` and re-use them! Let's look at how this might be possible in the following example:

Imagine we had two databases, one populated with names, weights, and images of the hippos that live on our hippopotamus farm and another populated with names, weights, and images of the groundhogs that live in our backyard. Naturally, we want two separate websites to display basic information about both of these wonderful animals.

Instead of doing all our work twice, we can swap out and re-use modularized parts of our MVC! Our ```view``` can remain the same and simply display animals' images, names, and weights. Our ```controller``` can be expected to handle the same types of request (i.e. "give me all the animals", "give me an animal by name", "change this animals weight"). Using an MVC architecture, we would only need to replace our ```model``` and voilá! We are well on our way towards sharing our passion, via the magic of MVC and [Al Gore's internet][al_gore].

## Quick Recap
###### MVC is designed to:
* Be language and application agnostic
* Give users the impression of direct data interaction and manipulation
* Help programmers architect their applications and consider high level design problems
* Be modular -- every part is distinct, encapsulated, and can be replaced without breaking the rest of the application
* Stores 'truth', the _actual data_, in the ```model```: far abstracted and independent from its representation in the ```view```
* Be used in many different types of software application, of which web applications are only one

## Where Rails Fits In
It is important to understand MVC outside of the context of Rails, as it has a few unique facets that aren't 1:1 representations of an MVC implementation. Now that you have had some experiencing developing in Rails, you can likely identify how the MVC pattern is used within its structure. Let's highlight the parts of Rails that lines up well with our academic description above, examine where it diverges, and dispel a few common misconceptions:

#### Controller and Model:
The ```controller``` and ```model``` implementation in Rails is straight forward to align with our understanding of MVC. Not only are they named aptly, but they encapsulate their responsibilities well. The controller actions do not actually change any data. They call methods on our ```models``` (via instance and class methods) to handle that. The controller then couriers this information to the ```view```!.

It is important to recognize that, while our chosen database (in our case so far an SQL iteration) is a distinct entity from our Rails application, it is considered encapsulated in the ```model``` of our Rails application with our classes. It would be appropriate to consider the database as the _actual_ ledger of true data in our model, and our Rails classes/Active Record as the logic on top that allows fetching/altering of that data.

Similar to how the DB connected to the Rails application is encapsulated in the ```model```, we may be tempted to lump the ```routing``` portion of Rails into the controller. Ultimately, Rails is not a perfect and sole representation of an MVC pattern, and the ```routing``` technically sits outside of the MVC. Considering how ```routing``` works in rails, we can recognize that its actually _directing_ requests to the appropriate controller class (of which we may have many in Rails), _not_ managing requests once they are within the ```controller```. Instead, ```routing``` sits in an intermediate step between requests from our ```view``` and our ```controller```.

Having finished due diligence and provided the 'technical' explanation, it's not unreasonable to mentally lump ```routing```, and all of our Rails controllers, into one, overarching, ```controller``` MVC concept.  

#### View:

In order to understand the role of the ```view``` within Rails, let's examine it under two distinct use cases of Rails:

* ###### As a JSON server:
Consider the ```view``` paradigm when our Rails application is being used solely to return JSON to requests received over the internet. In this example, Rails neither manages the _presentation_ of the data it is serving (that's up to the client/browser who received the JSON!), nor does it describe what things belonging to the ```view``` the controller should be listening for (again, that's up to the client!). In this example, Rails is sending data, fetched from the ```model```, through the ```controller```, into the void of the internet for something else (the browser/client) to use however it wants. In this JSON server example, we can see how the responsibilities of the ```view``` are abstracted out of our Rails application.

* ###### As a static website server:
Now, consider Rails acting as a static website server that users can fill out there exciting medical billing information through. In this instance, the browser has received HTML and CSS directly from Rails. This HTML and CSS **tells the browser how to display the view** and **what user input, via form submissions, it should relay back to the controller**. In contrast to our JSON server example, we can now see Rails re-assuming some responsibilities of the ```view```.

Of course, the receiver of this HTML and CSS (e.g. the browser) continues to play an integral role in displaying and interacting with the ```view```. After all, we know browsers are complex and growing instruments with tools, customization, and decision making of their own. For simplicities sake, let's assume the instructions that Rails sent via HTML and CSS on how and what to display are followed perfectly, giving the ownership and responsibility of the ```view``` fully to Rails.


(Following is the old description we were using. It is nice and quick and dirty, but inaccurate and distracting from the core, language agnostic, description. Furthermore, students have been working in Rails. I believe to provide a metaphor instead of an example in actual context is a mistake, and serves to further distance students from an understanding of MVC. The proposed new description is also omitting the 'presentation' and 'business' logic verbiage. I tried to avoid this because it doesn't aid in understanding the core concepts, and instead requires the students to keep two more hot terms in their head to understand the overarching concept. The above is long, no doubt. Perhaps it should/can be shortened, perhaps it should be broken into two parts (MVC && MVC and Rails?))


-------
# Refresher on MVC

## Overview

We'll review the concepts of MVC and learn some new key concepts.

## Objectives

1. Understand the value of placing logic in expected places.
2. Know what *presentation logic* is and how it differs from *business
   logic*.

## Separation of Concerns

Separation of Concerns is a design principle that helps us keep code logically organized by ensuring that each part of an application concerns itself with a specific job.

Remember the restaurant analogy? In a restaurant, there is a separation of concerns among the staff. A *server* takes your order, a *cook* prepares your order, a *food runner* brings it to your table, and a *busser* cleans up after you — and all of them work together to provide the cohesive experience of eating out.

Sure, the cook could do everything. In a small restaurant with only a few customers, it might be manageable for the same person who takes your order to be the one who cooks it. But, if that restaurant is bigger than, say, a food truck, there's a good chance that things would get out of control, and the cook would never know what she or he is supposed to do at any given time. Likewise, nobody else at the restaurant would know what the cook is supposed to be doing, and everyone would go looking for the cook for *everything*, which seems like kind of a nightmare!

## Model-View-Controller Review

In Rails, the Model-View-Controller paradigm helps us separate our concerns and know where to put certain kinds of code.

We could put all of our code in one file, but then the file is in the same spot as that overworked cook — doing so much that nobody knows what it's supposed to be doing. In a tiny application with only one developer, this might work for a while, but as the application and the team grow everyone needs to know where to go to find certain kinds of code.

With that in mind, let's look again at the components of the MVC pattern:

+ **Models:** Provide the *business logic* of the application and access and store data. Models do most of the 'heavy lifting' of manipulating data and enforcing the 'rules' of the application.
+ **Views:** Provide the *presentation logic* of the application and allow for user interaction. Views only care about showing a user formatted data and giving them ways to interact with it.
+ **Controllers:** Provide communications between the models and the views and help *control* the flow of data.

Okay, models are responsible for *business logic* and views are responsible for *presentation logic*, but what does that mean?

**Business logic** is the code that deals with the data and the "business" or "real-world" rules that govern an application. This is also sometimes called *domain* logic because it's all the stuff that's specific to the domain of the application. Sticking with the restaurant analogy, the business logic governs things like recipes and prices and how old you have to be to order a margarita.

**Presentation logic** is the code that deals with what the user sees. If business logic tells us what food the restaurant has and how much it costs, presentation logic puts all that together into a menu that looks great and helps a customer choose what to eat (and also entices them to order that margarita).

To put that into context with Rails MVC, picture a blog application. The view is only concerned with the presentation of blog posts. That's what it knows how to do, and that's all it knows how to do. The model is concerned with the content of all of the posts. The controller is concerned with which blog post you want to see. By keeping them separate, we get to do things like create a single view that can be used to display any post by having the controller ask for a specific post from the model.

By knowing which component is responsible for which concern, we can know where to go to find or add code based on what it does. If we need to write code to hit the database and search for specific blog posts, that seems like business logic and belongs in the model. If we need to write code to display those search results in a nice way and make sure the post date is formatted properly, that's presentation logic and belongs in the view.

## Separating Even More Concerns

We know the view is responsible for presentation logic, but what happens when our application grows and some presentation logic needs to be shared across more than one view? That's where `helpers` come in, and we'll be tackling those in the next lesson.

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/refresher-on-mvc'>Refresher On MVC</a> on Learn.co and start learning to code for free.</p>
