---
layout: post
title: Jekyll Tutorial Learning Notes(Part I)
categories:
- Jekyll
tags:
- Jekyll
---

## Introduction
I decided to move my posts from Wordpress to github. The host fee is too expensive to afford, and the control panel for WP is so bad. I want a more geek blog. Jekyll is my final choice. This tutorial is  based on the [Youtube video tutorial](http://youtu.be/jVeNnHy65Rs) by Thomas Bradley. It is highly recommanded to have a look at his tutorial, clear and easy to understand. I write posts only for memo and learning. This is *part I* for what I have learnt today(video 1 to 15).


## Installation
Jekyll is a command line tool used in terminal. We consider you have installed command line tool in your mac, if not, you need to go apple's developer site or use your Xcode to download and install before you strat.  

Since Jekyll is based on Ruby, you need install gem. Open your terminal, check and update the latest version of gem:

    $ sudo gem update --system

Then use this command line to install Jekyll:

    $ sudo gem install jekyll
    
## Set up your Github pages

Here Github Pages server is great place to host your Jekyll. Easy to control, and free to go. Here you need have a github account and a github application on mac is necessary (otherwise you need to do following things by using the command line). 

 - Create a new public repository with a name and description, check the README initialization option
 - Clone this repository to your local directory
 - Create and publish a new branch
 - Set this new branch to default and delete the master branch
 - Go to your directory and create the *index.html* with simple content

 ```
 <!DOCTYPE html>
 <html lang="en-ca">
 <head>
 <meta charset="utf-8">
 <title>first github site</title>
 </head>	
 <body>
 <h1>First Jekyll site</h1>
 <p>Hello every one, this is my first Jekyll site!</p>
 </body>
 </html>	
 ```

 - Give a commit to this change and then sync Your site
 - Go to your site and say hello world (e.g. [username.github.com/your-respository-name](username.github.com/your-respository-name))

##Set up a Jekyll project
We start to create a configuration file: *_config.yml*. Add these content to this file. 

```
    Markdown: redcarpet
    baseurl: /your-respository-name
    exclude: ['README.md']
```

Notice the space between words. Commit and sync this change on your github application. 
