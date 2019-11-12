---
layout: post
title:      "my first rails app (that I actually understood)"
date:       2019-11-11 22:07:01 -0500
permalink:  my_first_rails_app_that_i_actually_understood
---



This is not my first Rails application. I built my very first Rails app in a 2-day [Rails Girls](http://railsgirls.com/) workshop when I was in college.

I followed the steps, typing in commands I did not understand, and BOOM, a website was born. (Yes, that was rails g scaffold.) It felt amazing. I was able to follow the naming conventions and patterns to create new routes and methods, but without understanding the magic behind it, I was clueless when it came to debugging. 

I still recommend Rails Girls workshops - it was a great entry point to understand that programming is not all that scary, but I'm so happy to finally understand why Rails knew what I wanted to do when I gave it a noun and/or its pluralized form. Among other things. 

Now back to my (real) first Rails application - **Mockit**, a mock interview and networking platform for students and young professionals to schedule mock interviews with seasoned industry professionals.


You can find my app here: [https://github.com/jasmine1226/Mockit](http://https://github.com/jasmine1226/Mockit)


Features:
* Users can register as either Interviewer or Interviewer with the registration form or via Facebook Oauth.
* Interviewees can schedule interviews with active interviewers. 
* Interviewers can set their hourly rate; interviewees will be charged accordingly when scheduling interviews.

New features to be added:
* Create workflow for interviewees to cancel pending interview requests, and for interviewers to confirm or decline interview requests; calculate cost and charge accordingly.
* Update Oauth from Facebook to LinkedIn.
* Display upcoming interview on home page for logged-in users.










