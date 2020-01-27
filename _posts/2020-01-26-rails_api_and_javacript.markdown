---
layout: post
title:      "rails api and javacript frontend"
date:       2020-01-26 22:17:10 -0500
permalink:  rails_api_and_javacript
---


My 4th project is a SPA (single-page application) with Rails API and Javascript frontend. This time, instead of trying to build out all the features I would love to have, I tried to focus on two things:
1.  meeting project requirements (duh)
2.  focusing on separation of concerns and keeping my code clean

My app is, um, named Career Path Wizard (I know, I didn't want to spend an entire weekend trying to come up with a nmae). The functions include:
* Fetching all career paths from Rails API
* Fetching courses based on the selected career path
* Adding a new course to the selected career path

To provive some context, I was chatting with one of the learning techonologits on my team, and he mentioned they're trying to revamp and enhance our team site, where everything is being manually maintained currently, due to resource constraint. One thing he wanted is allowing authorized users (e.g. HRBP) to build customized career paths for different career tracks. I thought this project would be the prefect opportunity to build a demo app with that capaibility, which can later be transplanted since it's connected to the backend via API.

It didn't take me long to build up the limited feature, but I spent most of my time refactoring and cleaning up my code. It is definitely less intersting and really frustrating to keep breaking features that were already working and fixing them again. However, I think it's a great exprience to try following best practices and keeping my codes logical and clean.

The next step would be building out robust error handling, and giving it a facelift, and building more features. 

You can find my project here: https://github.com/jasmine1226/career_path_wizard
