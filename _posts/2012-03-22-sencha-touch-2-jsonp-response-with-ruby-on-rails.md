---
title: "Sencha Touch 2 JSONP response with Ruby on Rails"
date: "2012-03-22"
---

I had a hard time how to make JSONP response data render in a Sencha Touch 2 list view, **_but I made it. WOOT._**

Maybe, truth to be spoken, I had a harder time figuring out how JSONP actually worked. I had to understand that I had to wrap the JSON object into a JavaScript callback function and change the content type of the response to “text/javascript”. **( I still don’t get it. ![;)](images/icon_wink.gif) )**

So, here’s the code:

The last line of the the index method will be clear when you understand read the query string parameters which Sencha sends to the server:  
[![](images/Screen-Shot-2012-03-22-at-11.17.04-PM.png "Screen Shot 2012-03-22 at 11.17.04 PM")](http://blog.kennymeyer.net/2012/03/22/sencha-touch-json-p-request-with-ruby-on-rails/screen-shot-2012-03-22-at-11-17-04-pm/)

and, ultimately the Sencha Store object:

I wonder if you can write a more elegant method?
