---
layout: post
title: "Analysis of Market Demand for Web Programming Languages"
date: 2013-01-01 00:30
comments: true
categories: programming web-development market-analysis research
---
    
  A few months ago, I got the idea that one way to get leads for remote freelance gigs was to scour Craigslist. So, after doing the manual work of 'crawling' through at least 100 job postings by hand, I wrote a Ruby script to do the heavy lifting and filtering for me.
  
  Once I started looking through the data, some interesting things started jumping out at me. Even though I don't actually live in the Valley (I live in Jamaica), I consume a lot of the news, blog posts and articles that come from the Valley. Suffice it to say, I am affected by the 'Valley echo-chamber'. One side effect of that is an obsession with Ruby and Rails as my development stack...and a general expectation that the rest of the world has woken up to the beauty and elegance that is Ruby &amp; Rails.
  
Alas, much to my surprise, that is not the case. 
  
Before diving into the data, let me explain exactly what this script does. Throughout Craigslist, there are two URL subpaths that tend to have the majority of the web development freelance gigs - /cpg/ and /web/. 

So the script creates a list of all the cities on Craigslist (because CL doesn't provide a clean, RESTful API that allows you to get this info easily) and then simply adds /cpg/ and /web/ to the end of that URL.
  
  Then, on each link it checks to see if the current link actually has gigs posted in that city. The reason for this is that whenever there is no gig posted in the current city, what CL does is shows gigs from 'Nearby cities'. To prevent duplication, the script automagically checks for that and eliminates those cities that don't have 'uniquely posted' gigs. However, it does not eliminate a gig that has the exact text and posted in two different cities - because, well...I hadn't gotten there yet. 
  
  Once the script has a list of 'valid' cities with gigs posted, then it starts to parse each of the links on the first page of those cities (i.e. up to 100 links in each city - CL does pagination by the 100 links) for keywords that I specified. The upside to only using the last 100 links in each city is that those are most recent. The downside is that in active cities, the last 100 isn't always a good sample from the entire population. 
  
  For the Rails results, I have the following keywords: "rails, (ruby on rails), (ruby on rails 3), (rails 3), (rails 2)". 
  
  For the Ruby results, I have the following keywords: "ruby, (ruby 1.8.7), (ruby 1.9.2), (ruby 1.9.3), ruby187, ruby192, ruby193". 
  
  The searches are case-insensitive - so any link containing Ruby, rUbY or RUBY will be found and included.
  
  I am trying to capture every permutation that someone would use 'Rails' in a web dev sense. 
  
  The downside to this 'basic' approach is that for technologies that share similar keywords - e.g. 'Ruby on Rails' (the framework) and Ruby (the language) - there will be overlap. So in this case, the Ruby results contain a ton of Rails links. i.e. Rails is basically a subset of Ruby.
  
  I have done my best to fine-tune as many obviously spam-like CL posts out of the results, to really get at the legitimate posts. 
  
 **So it is fair to say, I think, that these results give us a relative proxy for what the marketplace for freelance programming gigs are actually looking for.**
  
  Without further ado, here is the data and analysis.
  
#### General Stats

  * Cities Parsed: 720
  * Total Gigs Found: 11,992 - 12,076 (footnote: scripts were run multiple times - for accuracy purposes - and the results returned were within this range)
  * Time Script Takes to Run: 16 mins - 1.25 hrs


#### Languages Searched For

##### Server-Side Languages &amp; Frameworks
  * C# (C-Sharp)
  * CodeIgniter (PHP Framework)
  * Django (Python Framework)
  * Dot-Net
  * Java
  * Lisp
  * Perl
  * PHP
  * Python
  * Rails (Ruby Framework)
  * Ruby


##### Client-Side Languages &amp; MVC Frameworks

Batch 1 - Languages

  * HTML
  * CSS
  * Javascript
  * Flash

Batch 2 - Javascript MVC Frameworks

  * Backbone.js
  * Closure
  * Ember.js
  * Knockout.js
  * Node.js

{% img /images/posts/2013/01/TC-Server-Side-Language-Demand-Distribution-Graph.PNG Server Side Language Demand Distribution Graph %}

{% img /images/posts/2013/01/TC-Server-Side-Language-Demand-Distribution-Table.PNG Server Side Language Demand Distribution Table %}

On the server side, you can see that PHP wins by a long shot - with an almost <a href="http://www.youtube.com/watch?v=2O7K-8G2nwU">Bolt-like</a> performance, blowing everybody else out of the water. In fact, Ruby comes in at a paltry 5th place, Java coming in at 2nd and Dot-Net and C# coming in 3rd and 4th respectively.

{% img /images/posts/2013/01/TC-Client-Side-Language-1-Demand-Distribution-Graph.PNG Client Side Language 1 Demand Distribution Graph %}

{% img /images/posts/2013/01/TC-Client-Side-Language-1-Demand-Distribution-Table.PNG Client Side Language 1 Demand Distribution Table %}

The most surprising result from the above is that Flash is still in demand - with all the "Flash is dead" rhetoric flying around the tech presses and blogs. Almost as much in demand as Javascript! Who woulda thunk?!?

{% img /images/posts/2013/01/TC-Client-Side-Language-2-Demand-Distribution-Graph.PNG Client Side Language 2 Demand Distribution Graph %}

{% img /images/posts/2013/01/TC-Client-Side-Language-2-Demand-Distribution-Table.PNG Client Side Language 2 Demand Distribution Table %}

As for the Javascript frameworks, there is a silent battle going on with the multitudes of Javascript frameworks in existence and being released regularly. Here is where the unscientific-ness and impreciseness of this little exercise rears it's head again. Upon first glance of the results, the script says that Ember had an <a href="https://github.com/marcamillion/craigslist-ruby-crawler/blob/master/output/client-side/ember-gigs-Sep-28-2012.html#L2">output of 14 gigs</a>. However, because the keyword being searched for is 'ember', what happens is that Ruby finds any string with a substring of 'ember'. So it had <a href="https://github.com/marcamillion/craigslist-ruby-crawler/blob/master/output/client-side/ember-gigs-Sep-28-2012.html">14 links</a> with 'Member' and 'Membership' in the link title. Not one with Ember.js or the Ember we were looking for. So after I manually reviewed the links, 0 results were returned. So the only two client-side MVC frameworks that the script found that is in-demand is Backbone and Node. Both, just barely.

That being said, please take that with a grain of salt. Here is an alternative data point for you. 

I was told by one of the founders of <a href="http://grouptalent.com">GroupTalent</a> a few months ago, that the largest demand from clients they are seeing is in-fact for client-side JS frameworks. Even more so than server-side frameworks.

Here is all the data altogether.

{% img /images/posts/2013/01/TC-Language-Demand-Distribution-Graph.PNG Language Demand Distribution Graph %}

{% img /images/posts/2013/01/TC-Language-Demand-Distribution-Table.PNG Language Demand Distribution Table %}


### Conclusion

This post is not meant to start a flamewar between the various camps. It is just an unscientific analysis of what the general marketplace (using Craigslist as a proxy for that marketplace) is looking for in web development talent.

If you are considering learning one of these languages or frameworks, using what the marketplace requests is one factor to weigh in your decision making process. I wouldn't necessarily encourage that though, I certainly didn't. 

I chose Ruby &amp; Rails and love every minute of it. I would encourage you to try out various languages and see which you feel most comfortable with - because the vast majority of the time you spend in the language (assuming you really want to get better) will be non-billable stuff.

In the <a href="http://5kmvp.com">web applications I build</a> for clients at 5KMVP, I use Ruby &amp; Rails because that's what I love. Clients have been satisfied and seem to love it too.

If you want me to do an analysis of anything else - say Mobile vs Server languages or anything else, let me know in the comments or drop me a line on <a href="http://twitter.com/marcgayle">Twitter</a>. 

If you found this interesting, you may find a piece <a href="http://marcgayle.com/how-dropbox-is-printing-money">I wrote about Dropbox</a>, or a <a href="http://marcgayle.com/hackers-guide-to-cashflow-vs-profit">guide to understanding cashflow vs profit</a>, similarly interesting.

You can find the Ruby script I created <a href="https://github.com/marcamillion/craigslist-ruby-crawler">on Github<a/>, along with some <a href="https://github.com/marcamillion/craigslist-ruby-crawler/tree/master/output">sample output files</a> that have a list of all the gigs generated for this article. 

Do look around and if you submit any pull requests for any improvements you may have, the Karma Fairy shall multiply your lineage ten-fold and your seed shall outnumber the celestial bodies.