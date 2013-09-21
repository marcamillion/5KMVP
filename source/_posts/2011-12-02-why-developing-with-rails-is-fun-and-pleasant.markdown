---
layout: post
title: "Why developing with Rails is fun and pleasant"
date: 2011-12-02 23:37
comments: true
categories: rails ruby-on-rails ruby
---

*Disclaimer: This post is NOT for experienced Rails developers. It is for designers and non-developers thinking about diving into code and trying to decide which language/framework they should use.*

**Why did I learn Ruby &amp; Rails?**

I am fairly new to Rails &amp; Ruby. I just started learning about 8 months ago when I started developing <a href="http://prod-compv.heroku.com/imdex.html">CompVersions</a>.

<a href="http://prod-compv.heroku.com/imdex.html">CompVersions</a> is a web application that allows designers to upload multiple versions of their designs &ndash; so that their clients can easily give them a thumbs up/down and add comments as necessary.

{% img /images/posts/2011/12/01-CompV-2-up-view-hover-menu.PNG CompV 2-up view hover menu %}

The idea for <a href="http://prod-compv.heroku.com/imdex.html">CompVersions</a> comes from my previous experiences working with multiple designers. I have, at various points in the past, worked on projects where I was either project lead or project manager and had to coordinate designs and feedback from multiple designers and multiple stakeholders. 

Getting &amp; forwarding emails with PDF files as attachments and notes specifying the line and page number in the PDF was not cutting it anymore. I tried <a href="http://www.basecamphq.com/">Basecamp</a>, and it wasn't designed for this problem, so it didn't work the way I needed it. So I decided to build my own solution.

**Why write this post ?**

A friend of mine (wassup <a href="http://twitter.com/StephenHector">Hector</a>) is doing a similar journey (i.e. building a web app/site while learning), but he is in Microsoft land (Dot Net, IIS, MSDN, the whole nine yards).

I was explaining to him why I love Rails &amp; Ruby, and how it completely abstracts the pain of doing things like SQL queries (although, if you want to work with SQL you surely can).

I was trying to find a real-world example of what I meant, but all I found were the regular 'build a blog in 15 minutes' and tutorials and articles that take you through the entire app process. I didn't want that, so I started looking through my code. I realized that I have the perfect use case to highlight how wonderful it is to use Rails. He appreciated it, and understood what I meant, so I figured I would write it up and release for everybody else.

**What is my example?**

Rails is known for allowing the developer to focus more on the business logic of the application, rather than the technical behind the scenes stuff.

**What does that even mean? Here is an example:**

I was hooking up the email functionality where once a designer registers they get an email from the app. I use a rails gem (the equivalent of a plugin) called <a href="https://github.com/plataformatec/devise">devise</a> that makes it relatively easy to get up and running with an authentication solution. It handles users logging in and out. Registering, forgotten and resetting passwords. Locking and Unlocking an account (say if your application requires high security where you only allow X login attempts, then the account is locked). Plus a few other features. All in one plugin. All done to industry standard best practices - e.g. when a user forgets their password a unique link is sent where they can reset their password (i.e. a password is not sent via plaintext in email, that's very insecure). I get all of this functionality with very little effort (relatively speaking given the amount of functionality I get).

So the first question I was faced with is, how do I address the designer? Do I say "Hi John!" ? But what if there is no first name, how do I fallback to their username (sexyjohn79) and then subsequently their email (<a href="mailto:sexyjohn79@email.com">sexyjohn79@email.hotmail.com</a>). That's 'business logic'.

**How do you implement this business logic?**

The registration form for <a href="http://www.compversions.com/">CompVersions</a> has First &amp; Last Names, Usernames &amp; Email addresses. A user can login with both their username and email address. But I haven't decided whether or not I am going to let first &amp; last names be mandatory. Most likely will, but I wanted to implement it to gracefully handle the situations when it doesn't have one or the others.

Rails uses what is called an MVC structure. <a href="http://blog.elliottheis.com/post/3461828705/explanation-of-mvc-ruby-on-rails">Model, View, Controller</a>. Rails encourages you to put all of your business logic in the 'Model' portion of your app.

**Show me some code, damnit!**

In my User model (which is where Rails handles all the business logic related to the 'Users' of the app), I have a method called *pretty_name* that handles this option. This is how my *pretty_name* method looks:

	def pretty_name
		f_name || username || email
	end

Please explain that code to me

To break it down for my non-developer friends out there:

Line 1 is the definition of the method. The method is what I will call to 'execute/invoke' this block of code. This will become clearer shortly.

Line 2 basically says "If the value *f_name* exists, return that value" - i.e. if there is a first name (or if the first name is NOT null) return the first name. Then it moves on to say "...OR ( || = OR ), if that doesn't exist check to see if a *username* exists. If it does, return that value..." and continues to "...OR if no *f_name* and *username* exist, return the *email_address*". The assumption here is that a username MUST exist because the user will be getting this message in their email. Ruby executes left-to-right.

Line 3 ends this method

That's it. That's the business logic.

Then, in my template for the actual email (i.e. the 'View' from the MVC), this is what the first few lines look like:


	<p>Hi <%= @resource.pretty_name %></p>

	<p> Thank you for registering with CompVersions.</p>


The system says *Hi* and it executes the method *pretty_name* on the object *@resource* (*@resource* is an object that devise - from earlier - uses to refer to the current user account) and then returns the best value. So if the person's first name is John and it is there (i.e. the value is not NULL), the email will say *Hi John*. If there is no first name, it will say *Hi sexyjohn79*, where *sexyjohn79* is the username. If there is no username it will say *Hi <a href="mailto:sexyjohn79@email.hotmail.com">sexyjohn79@email.hotmail.com</a>*. 

Here is an example of how the end product looks. All for the simple *Hi Marc*!

{% img /images/posts/2011/12/02-finished-email.PNG %}

**So is Rails really a walk in the park, for non-developers?**

I don't want you to misunderstand what I am saying. I am not implying that in order to get from nothing to a full app with login, for a non-Rails developer it is easy and quick. It's not that easy. But as a general rule, the things you will be struggling with (once you get the basics down) are how the app will function - the business logic - rather than missing semi-colons and errant SQL queries.

Like all languages and frameworks, it has its quirks. Rails has a set of conventions that make sense and force you to develop one way (many consider it 'the right way'). There are some that it doesn't work for. It has worked for me, and makes total sense - based on what I have seen so far.

I hope that helps encourage you to try Rails, or even consider it for your next project.

There are many awesome tutorials out there, for starters I would use <a href="http://railsforzombies.org/">Rails for Zombies</a>. I am in no way affiliated with them, I just love their product.

If you liked this, you should follow me on <a href="http://www.twitter.com/marcgayle">Twitter here</a>.

****

Here are some more screenshots of CompVersions:

{% img /images/posts/2011/12/03-CompV-Designer-Dashboard.PNG %}
{% img /images/posts/2011/12/04-CompV-2-up-view.PNG %}
{% img /images/posts/2011/12/05-CompV-3-up-view.PNG %}
{% img /images/posts/2011/12/06-CompV-4-up-view.PNG %}
