# Refresher on MVC

## Overview

We'll review the Model-view-controller (MVC) paradigm, how Rails implements it, and learn some new key concepts.

## Objectives
1. Understand why we compartmentalize an application's functionality
2. Examine an implementation of MVC
3. Understand the Rails implementation of an MVC architecture

## Forward

Within the curriculum, you have been working hard learning defined small scale technical concepts, problems, and solutions. To understand MVC, we need to back up and take on a macroscopic, application wide, lens. For this lesson, we are assuming the mindset of a [_software architect_][software_architect].

One of the initial challenges to understanding MVC is that it was specifically designed to _abstract away from the user how an application is functioning_. It was created as a general purpose solution which bridged the gap between a user's mental model of a program and a computer's _actual_ model of a program. As we have been unknowing 'users' of the MVC pattern most of our lives, we need to change some of our wiring to understand it as programmers.

## Separation of Concerns

At its core, MVC is designed to modularize distinct functionality within an application. While the MVC pattern can be used with many different types of applications, let's identify its distinct parts in the context of a web application:
  * ```View```: what an end user on a website experiences when interacting with a program in their browser
  * ```Model```: where the actual data, (be it information about restaurants, train times, or rare marsupials), resides and is altered
  * ```Controller```: what manages communication between the two

Now that we have identified the core components, let's examine what they actually do when a user engages with our web application:

1. A user, through interaction with the ```view```, (in this case, the browser's GUI), requests data from a server
2. The request is sent across the internet to a server
3. The server interprets the request and ```routes``` it to the appropriate ```controller``` function designed to fulfill that request
4. The ```controller``` function (_which does not change data itself!_) asks the ```model``` to either send it data or to change its data
5. The ```model``` accesses/manipulates the actual data and returns the desired result to the ```controller```
6. The ```controller``` packs a response and sends it back to the client that sent the initial request
7. The ```view``` _represents_ the data

In considering the above steps, try to answer the following question:
* Do the **model** and **view** have any actual direct interaction?
* To what MVC component does the **routing** functionality, described in step 3, belong to?
* Is the data represented in the ```view```, that the user sees and interacts with, anything but a virtual copy of the _actual_ data they perceive they are interacting with?

### An Advantage of Modularity
In our example, we have identified three distinct parts, and two distinct communication layers: TODO GRAPHIC? (```model``` <--> ```controller``` <--> ```view```). These communication layers have a 'standardizing' effect. They enable us to de-couple different parts of our ```MVC``` and re-use them! Let's look at how this might be possible in the following example:

Imagine we had two databases, one populated with names, weights, and images of the hippos that live on our hippopotamus farm and another populated with names, weights, and images of the groundhogs that live in our backyard. Naturally, we want two separate websites to display basic information about both of these animals.

Instead of doing all our work twice, we can swap out and re-use modularized parts of our MVC! Our ```view``` can remain the same and simply display animals' images, names, and weights. Our ```controller``` can be expected to handle the same types of request: i.e. "give me all the animals", "give me an animal by name", "change this animals weight".

Using an MVC architecture, we would only need to replace our ```model``` and voilá! We are well on our way towards sharing our passion, via the magic of MVC and [Al Gore's internet][al_gore].

## Quick Recap
###### MVC is designed to:
* Be language and application agnostic
* Be modular: every part is distinct, encapsulated, and can be replaced without breaking the rest of the application
* Use a ```controller``` to facilitate communication between the ```view```, which the user interfaces with, and the ```model```
* Stores 'truth', the _actual data_, in the ```model```, which is far abstracted and independent from its representation in the ```view```

## Where Rails Fits In
Now that we have reviewed MVC outside of the context of Rails, let's recap on how Rails implements the paradigm.

#### Controller and Model:
The ```controller``` and ```model``` implementation in Rails is straight forward to align with our understanding of MVC. Not only are they named aptly, but they encapsulate their responsibilities well. The controller actions do not actually change any data. They call methods on our ```models``` (via instance and class methods) to handle that. The controller then couriers this information to the ```view```!.

It is important to recognize that, while our chosen database (in our case so far an SQL iteration) is a distinct entity from our Rails application, it is considered encapsulated in the ```model``` of our Rails application with our classes. It would be appropriate to consider the database as the _actual_ ledger of true data in our model, and our Rails models/Active Record as the logic on top that allows fetching/altering of that data.

#### View:

The Rails implementation of the ```view``` in MVC has a few caveats. In order to understand it, we will examine its implementation in two common use cases of a Rails application:

* ###### As data server:
Consider the ```view``` paradigm when our Rails application is being used solely to return data (i.e. JSON) to requests. In this example, Rails neither manages the _presentation_ of the data it is serving (that's up to the client/browser who received the JSON!), nor does it describe what things belonging to the ```view``` the controller should be listening for (again, that's up to the client!). In this example, Rails is sending data, fetched from the ```model```, through the ```controller```, into the void of the internet for something else (the browser/client) to use however it wants. In this JSON server example, we can see how the responsibilities of the ```view``` are abstracted out of our Rails application entirely!

* ###### As a static website server:
Now, consider Rails being used as a static website server that users can fill out there exciting medical billing information through. In this instance, the browser has received HTML and CSS directly from Rails. This HTML and CSS **tells the browser how to display the view** and **what user input, via form submissions, it should relay back to the controller**. In contrast to our JSON server example, we can now see Rails re-assuming some responsibilities of the ```view```.

## Summary
* The MVC paradigm is language and application agnostic
* MVC describes an architecture of three modularized parts: ```model```, ```controller```, ```view```
* While Rails emulates the majority of an MVC structure, it is not a perfect match and depends on how it is used

<p data-visibility='hidden'>View <a href='https://learn.co/lessons/refresher-on-mvc'>Refresher On MVC</a> on Learn.co and start learning to code for free.</p>

[software_architect]: https://en.wikipedia.org/wiki/Software_architecture
[al_gore]: https://goo.gl/QeiWDG
