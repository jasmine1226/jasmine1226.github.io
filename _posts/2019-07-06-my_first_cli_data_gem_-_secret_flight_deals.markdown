---
layout: post
title:      "my first CLI data gem - secret flight deals"
date:       2019-07-06 22:39:19 +0000
permalink:  my_first_cli_data_gem_-_secret_flight_deals
---


I just built my first CLI application to scrape flight deals from secretflying.com. Phew!

This gem:
1. scrapes flight deals departing from USA exluding the ads
2. lets user pick from the menu to view flight details
3. loads new page as the user continues to hit "next"
4. allows user to go back and forth between menu pages without reloading/scraping the data

You can find it here: https://github.com/jasmine1226/flight_deals

The coding part has been fun and it's great to integrate and utilize what we have learned so far. I became more familiar with Github and working with CLI/OO design paterns.
Scraping was more challenging than expected - I thought I picked an easy one to work with. What I didn't realize was that 1) it has an infinite scrolling page and 2) the flight deal page "appeared" to have consistent format but the texts were wrapped in different elements without class or id to identify them.

1) Infinite scrolling
When I loaded the page in my application, it only scraped 9 deals since the data isn't pulled until an user scrolls to the bottom of the page.
I turned to Google for help, and found[ this detailed article on how to scrape Instagram](http://www.diggernaut.com/blog/how-to-scrape-pages-infinite-scroll-extracting-data-from-instagram/) to be very helpful. It listed out exactly where to look for the information. While I was following the steps, I learned that secretflying.com was loading deals page by page, so all I needed to do was to scrape the deals from URLs with specific page numbers.

2) Scraping inconsistent page
I was able to get the information I need with my scrappy codes. There has to be a better way around this - maybe using a different scraping tool? Since there is no consistent css element, class, or id that I could use to identify where the information is, what I did right now is basically scraping all the content, extract the text as a string and split it into an array. Then I find the array element with the header (e.g. "AIRLINES"), and the element after it would be the information I need (e.g. "Eva Airlines").

Thing I'd like to learn more about after this project:
1. Adding deal filters to this gem
2. Learning about other scraping tools/methods
3. Working with json files and APIs




