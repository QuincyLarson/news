---
author: Bertil Muth
authorTwitter: https://twitter.com/BertilMuth
authorFacebook: false
title: "The truth is in the code"
subTitle: "Sooner or later, every software developer will hear something like this:..."
coverSrc: https://cdn-images-1.medium.com/max/1600/1*Fw8F2fRNVfkcE-0VGyDZhQ.png
url: https://medium.freecodecamp.org/the-truth-is-in-the-code-86a712362c99
id: the-truth-is-in-the-code-86a712362c99
date: 2017-04-21T15:14:26.459Z
tags: [
  "Software Development",
  "Programming",
  "Life Lessons",
  "Tech",
  "Technology"
]
---
# The truth is in the code



![](https://cdn-images-1.medium.com/max/1600/1*Fw8F2fRNVfkcE-0VGyDZhQ.png)

[shoppingapp](https://github.com/bertilmuth/requirementsascode/tree/master/requirementsascodeexamples/shoppingappjavafx) model, example of [requirementsascode](https://github.com/bertilmuth/requirementsascode)



Sooner or later, every software developer will hear something like this:

> “Truth can only be found in one place: the code.”

> – Robert C. Martin, [Clean Code](https://www.amazon.de/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)

But what does that mean?

The [Agile Manifesto](http://agilemanifesto.org/) values “working software over comprehensive documentation.”

Developers write comprehensive documentation of the software’s behavior all the time, though. The code.

Code comments, and external specifications, document the software’s behavior as well. But they may not get updated when the code changes. Then they stop reflecting the software’s behavior soon.

In contrast, code _always_ reflects the software’s behavior. It defines it.

That’s why the truth is in the code.

### Writing for your readers

Code is documentation. Any kind of documentation should be understandable by its readers.

The readers of code are a compiler or interpreter, and other developers.

So it is not enough if your code compiles. Other developers need to understand it as well. They need to work on your code in the future, change it and extend it.

A common suggestion to make code understandable is that you write clean code. Code that uses understandable language for variable and method names. That also makes a lot of code comments unnecessary.

Clean code should express the intent: _what_ somebody can achieve by calling a method. Not _how_ the method achieves it.

Guess what this method does:

<pre name="354d" id="354d" class="graf graf--pre graf-after--p">BigDecimal addUp(List<BigDecimal> ns){..}</pre>

How about rather writing this:

<pre name="20e9" id="20e9" class="graf graf--pre graf-after--p">BigDecimal calculateTotal(List<BigDecimal> individualPrice){..}</pre>

Clean code is a good idea. But I don’t think it is sufficient.

### The importance of shared understanding

When there’s a new requirement, you need to understand how implementing it affects the existing code.

That can be a challenge if your software has been around for some time. Quite often, I have heard a dialogue like this:

_X_: We can’t go on with feature _foo_.

_Y_: Why?

_X_: Cause _Z_ is the only one who knows about the code. He has implemented the code that we need to change now.

_Y:_ Well, why don’t we ask him?

_X_: Because he is sick / on vacation / at a conference / no longer at the company.

_Y_: Oh…

Here’s the thing. To find out if your code is understandable, somebody else should try to understand it.

There are techniques for that. [Pair programming](https://en.m.wikipedia.org/wiki/Pair_programming) is a good one. Or you sit down with other developers. You walk them through the code you have written.

Still, what if many developers are involved with a product? What if the development teams change their members? That makes it harder to write code that enough other people understand.

### The story

Clean code gives you the right _words_.

The question is: what _story_ will you tell with them in your code?

I have no idea.

But for a typical business application, I am pretty sure what story I want to read in the code.

After introducing you to a brief example, I will outline that story.

### The glove shop example

As a user of software, I want to [reach a desired outcome](https://medium.freecodecamp.com/nobody-wants-to-use-software-a75643bee654?source=linkShare-a74297325869-1489339708). For example, I want to own a new pair of gloves to keep my fingers warm in winter.

So I go online and see there is a new online shop specialized in gloves. The shop’s website lets me buy gloves. The “basic flow” (also called “happy day scenario”) of the use case could look like this one:

*   The system starts with an empty shopping cart.
*   The system displays a list of gloves.
*   I add the gloves I like to the shopping cart. The system adds the gloves to my order.
*   I check out.
*   I enter shipping information and payment details. The system saves this information.
*   The system displays a summary of the order.
*   I confirm. The system initiates shipping of my order.

After a few days, I get my gloves.

### Here’s the story I want to read in code.

### Chapter 1: Use cases

The first chapter of the story is about use cases. When I read code, I want to follow a use case in the code step by step to the desired outcome.

I want to understand how the system reacts when something goes wrong. From a user’s perspective.

I also want to understand the possible turns along the way. The user tries to go back from the payment details to the shipping information, for example. What happens? Is that even possible?

I want to understand what code to look at for each part of a use case.

#### So what are the _parts_ of a use case?

The fundamental part of a use case is a _step_ that brings a user closer to a desired outcome. For example: “The system displays a list of gloves.”

Not all users may be able to run a step, but only members of certain user groups (the “_actors”_). End customers buy gloves. Marketing people enter new glove offers into the system.

The system runs some of the steps on its own. Like when it displays the gloves. No user interaction necessary there.

Or a step is an interaction with the user. The system _reacts_ to some _user event_. For example: The user enters shipping information. The system saves the information.

I want to understand which _data_ to expect with the event. Shipping information includes the user’s name, address etc.

The user can run only a subset of steps at any given time. The user can enter payment details only after shipping information. So there is a _flow_ that defines the order of the steps in a use case. And a _condition_ that defines if the system can react, depending on the system’s state.

#### To understand the code, you need an easy way to know several things.

For a use case (like “buy gloves”):

*   The _flow(s)_ of _steps_

For each step:

*   Which _actors_ have access to it (that is, which user groups)
*   Under which _condition_ the system reacts
*   If the step is _autonomous_, or based on a _user interaction_
*   The _system_ _reaction_

For each step that is a user interaction:

*   The _user event_ (like “user entered shipping information”)
*   The _data_ that comes with the event

Once I know where to find a use case and its parts in the code, I can drill deeper.

### Chapter 2: Breaking things down into steps through components

Let’s call an encapsulated, replaceable building block of your software a _component._ A component’s _responsibilities_ are available to the world outside the component.

A component could be:

*   a technical component like a database repository,
*   a service like “shopping cart service”,
*   an entity in your domain model.

That depends on your software design. But no matter what your components are: you usually need several of them to realize one step of a use case.

Let’s look at the _system reaction_ of the step “The system displays a list of gloves”. You probably need to develop at least two _responsibilities_. One finds the gloves in the database, and one turns the list of gloves into a webpage.

When reading code I want to understand the following things:

*   What are a component’s _responsibilities._ For example: “find gloves” for the database repository.
*   What are the _inputs_ / _outputs_ of each responsibility. Example input: criteria for which gloves to find. Example output: list of gloves.
*   Who _coordinates_ the responsibilities. For example: find gloves first. Turn result into a webpage second.

### Chapter 3: What components do

A component’s code fulfills responsibilities.

That often happens in a _domain model_. The domain model uses terms relevant in the business domain.

For the example, a term could be Glove. Another term could be Order.

The domain model describes the _data_ for each term. Each Glove has a color, a brand, a size, a price and so on.

The domain model also describes computations on the data. The total price of an Order is the sum of the prices of each Glove bought by the user.

A component can also be a technical component like a database repository. The code needs to answer: How does the repository create, find, update and delete elements in the database?

### Telling your story

Maybe your story looks similar to the one above. Maybe it’s different. Whatever your story is, programming languages give you great freedom to express yourself and tell that story.

That’s a good thing because it allows developers to adapt to different contexts and requirements.

It also bears the risk that developers tell too many different stories. Even for the same product. That makes it harder than necessary to understand code that somebody else has written.

One way to address this is the use of design patterns. They allow you to structure your code. You can agree on that common structure in your team or even across teams.

For example, the Rails framework is based on the well-known Model View Controller pattern.

The model is the place for _domain_ _data._

The view is the client side user interface, like HTML pages. It is the origin of _user_ _events._

The controller receives the user events on server side. It is responsible for _flow._

So if several developers use Rails, they know which part of the code to look at for certain parts of their story.

They could find out what is missing when sharing their understanding. Then, they could agree on further conventions on where to put which part of their story.

If that works for you, that is just fine. But I want to go further than that.

### Requirements as Code

Many of my clients ask me how to deal with long term software documentation.

When working in an agile context, how do you create documentation for software maintenance?

What requirements have been implemented so far?

Where do you find their realization in the code?

For a long time I had no satisfying answer. Except, of course: the importance of well written, automated tests. Clean production code. Shared understanding.

But a few years ago, I started thinking:

> If the truth is in the code, the code should be able to speak the truth.

In other words: if you took great care of telling your story in the code, why would you want to tell it again?

There needs to be a better way. It must be possible to extract the story, and generate documentation from it. Documentation that non-technical stakeholders understand as well.

Documentation that is always up-to-date, because it comes from the same source that defines the software’s behavior.

The only reliable source: the code itself.

After a lot of experiments, I had some results. I made them public in a Github project called [requirementsascode](https://github.com/bertilmuth/requirementsascode).

### How it works



![](https://cdn-images-1.medium.com/max/1600/1*rZAA0h24T9SdEZYdE7stIQ@2x.png)



*   A UseCaseModel instance defines the _actors_, _use cases_, their _flows_ and _steps_. It tells chapter 1 of the story. You find an example of such a model at the start of this article.
*   A use case model configures UseCaseModelRunner instances. Every user has her own runner, because every user may take a different path through the use cases in the model.
*   The runner reacts to a _user event_ from the frontend by calling the _system reaction_ in the backend.The frontend communicates to the backend only through the runner.
*   But the runner only reacts if the user is at the right position of the _flow_ and the step’s _condition_ is fulfilled. For example, the runner only reacts to the “EnterPaymentDetails“ event if the user has entered the shipping information right before.
*   The _system_ _reaction_ is a single method. The method’s body is responsible for coordinating the components to realize the step, as described in chapter 2.
*   Chapter 3 is out of scope of requirementsascode. It is left up to the application. That makes requirementsascode compatible with arbitrary software designs.

So the UseCaseModelRunner controls the user visible behavior of the software. Based on a UseCaseModel.

With [requirementsascodeextract](https://github.com/bertilmuth/requirementsascode/tree/master/requirementsascodeextract), you can generate documentation from the same use case model that configures the runner. That way, the documentation always reflects how the software works.

Requirementsascodeextract uses the FreeMarker template engine. That allows you to generate any plain text documentation you like. For example HTML pages. Further processing could turn it into other documentation formats, like PDF.

### Your feedback will help me improve this project

I started working on requirementsascode several years ago, but just recently made the project public. It has gone through significant improvement since the beginning.

To learn whether the approach scales, I tried it on an application with several thousand lines of code. It worked. I tried it on smaller applications as well.

Still, so far, requirementsascode has been my hobby project.

That’s why I need your help. Please give me feedback.

What do you think of the idea? Can you imagine that it works in the context of the software you develop? Any other feedback?

You can drop me a note in the comments or contact me on [Twitter](https://twitter.com/BertilMuth) or [LinkedIn](https://www.linkedin.com/in/bertilmuth).

You can [clone](https://github.com/bertilmuth/requirementsascode) the project and try it out yourself.

Or you can [contribute](https://github.com/bertilmuth/requirementsascode/blob/master/CONTRIBUTING.md) to documenting the truth in the code.








