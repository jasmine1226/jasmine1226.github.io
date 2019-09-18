---
layout: post
title:      "my first mvc sinatra web app - amazereads"
date:       2019-09-18 03:16:53 -0400
permalink:  my_first_mvc_sinatra_web_app_-_amazereads
---


I got back into reading after I deleted all social media apps from my phone last year, and had been enjoying marking books as "read" on goodreads.com. As my not-so-creative website title suggests, I thought it'd be nice if Amazon has its own goodreads.com that's *FULLY* integrated with and tailor made for Amazon.com, Kindle, Amazon Bookstore, Alexa, etc. 

Of course my app doesn't do any of that, yet. It's a basic MVC Sinatra web app that can -
* create, read, update, and delete a user account
* create and read books
* create, read, update, and delete a book review
* favorite and unfavorite books
* login validation

I used bootstrap to make everything clean and simplistic. Otherwise I'd spend days, if not weeks, just playing with CSS layouts. 

After hitting the basics, I started a new branch to work on new features, including:
* adding a new class that creates has many:, :through relationship between books and users.
* allow readers to mark books as "reading" or "read" and keep track of reading progress
* update favorite and unfavorite function to be built through the new class

I haven't got to finish my new feature. Between my relatively new full-time job and moving to a new house, there is only so much time I could squeeze out for the project. I constantly felt like I'm behind and failing on every aspect of my life. Yet it got easier once I broke it into executable steps/work blocks to reduce the emotional barrier.

With that being said, I definitely enjoyed building this web app and understanding how things work in the back end. Really looking forward to diving into Rails!


