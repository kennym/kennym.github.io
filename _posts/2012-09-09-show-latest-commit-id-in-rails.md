---
title: "Show latest commit ID in Rails"
date: "2012-09-09"
---

I find it useful to display the current git commit ID of my application:

[![](/assets/images/Screen-Shot-2012-09-08-at-5.04.19-PM.png "Screen Shot 2012-09-08 at 5.04.19 PM")](http://blog.kennymeyer.net/2012/09/08/show-latest-commit-id-in-rails/screen-shot-2012-09-08-at-5-04-19-pm/)

In your application’s _Gemfile_ add the **grit **gem, which is a Ruby wrapper for git, and run Bundler.

If you deploy your web application via Capistrano and git, then you can put the following in your _app/helpers/application\_helper.rb_:

\[gist id=3679774\]

Make sure the repo you are deploying to is non-bare. If you are not sure about that, in the project’s directory run:

\[gist id=”3679892”\]

If you see a SHA-1 Hash, life is good.

Now in your template you can add the helper anywhere you like – most people prefer to put that info in the footer:

\[gist id=”3679825”\]

How easy was that!
