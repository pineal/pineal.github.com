---
layout: post
title: Jekyll Tutorial Learning Notes(Part I)
categories:
- Jekyll
tags:
- Jekyll
---

>I decided to move my posts from Wordpress to Github. The host fee is too expensive to afford, and the control panel for WP is so bad. I want a more geek blog. Jekyll is my final choice. This tutorial is  based on the [Youtube video tutorial](http://youtu.be/jVeNnHy65Rs) by Thomas Bradley. It is highly recommended to have a look at his tutorial, clear and easy to understand. I write posts only for memo and learning. This is *part I* for what I have learnt today(video 1 to 15).


## Installation
Jekyll is a command line tool used in terminal. We consider you have installed command line tool in your mac, if not, you need to go apple's developer site or use your Xcode to download and install before you start.  

Since Jekyll is based on Ruby, you need install gem. Open your terminal, check and update the latest version of gem:

    $ sudo gem update --system

Then use this command line to install Jekyll:

    $ sudo gem install jekyll
    
## Set up your Github pages

Here Github Pages server is great place to host your Jekyll. Easy to control, and free to go. Here you need have a Github account and a Github application on mac is necessary (otherwise you need to do following things by using the command line). 

 - Create a new public repository with a name and description, check the README initialisation option
 - Clone this repository to your local directory
 - Create and publish a new branch
 - Set this new branch to default and delete the master branch
 - Go to your directory and create the *index.html* with simple content

 ```
 <!DOCTYPE html>
<html lang="en-ca">
<head>
	<meta charset="utf-8">
	<title>first Github site</title>
</head>	
<body>
    <h1>First Jekyll site</h1>
    <p>Hello every one, this is my first Jekyll site!</p>
</body>
</html>	
 ```

 - Give a commit to this change and then sync Your site
 - Go to your site and say hello world (*e.g. username.github.com/your-respository-name*)

##Set up a Jekyll project
We start to create a configuration file: *_config.yml*. Add these content to this file. 

```
    Markdown: redcarpet
    baseurl: /your-respository-name
    exclude: ['README.md']
```

Notice the space between words. Commit and sync this change on your github application. 

##Run Jekyll locally
Open your terminal, use this command:

    $ jekyll serve --watch --baseurl ""
    
You will see server running in terminal. Open your browser, and enter *localhost:4000* to access your website locally. Keep your server running when you develop locally. If the change is not shown on browser, stop the server (Control + C) and restart it.

##_Site Folder
This folder will generated automatically as a final version of no matter HTML or markdown files in your local site directory. But it is kind of temporary one. What we need to do is go to Github application, and in setting panel, add *_site* into ignore files(.gitignore), then commit it. 

##Layout
This is very as similar as Wordpress. It just like a frame that can contain different modules. 

To achieve this, create a directory in your site, which is called **_layout**. Create a new HTML file in this folder. We name it **default.html**.

For example, the **index.html** mentioned previously now can be simplified to body content:

```
    <h1>First Jekyll site</h1>
    <p>Hello every one, this is my first Jekyll site!</p>
```

Other codes are moved into the layout page  **default.html**,
For example, the **index.html** mentioned previously now can be simplified to body content:

```
<!DOCTYPE html>
<html lang="en-ca">
<head>
	<meta charset="utf-8">
	<title>first github site</title>
</head>	
<body>
    {{content}}
</body>
</html>	
```

The main body is replaced by the line labeled by **{{content}}**. 

But the computer can not logically combine the layout and content together, Jekyll provides a label put the content to the layout. For instance, the **index.html** needs to declare which layout will be used like:

```
---
layout:default
---
    <h1>First Jekyll site</h1>
    <p>Hello every one, this is my first Jekyll site!</p>
```

##Create a navigation module
Most blog are unlikely single page. We need hyperlinks to connected all pages. A navigation bar is helpful and important. We create a new page called **news.html**. Then in the default layout, we put a navigation in header.

```
<header>
    <h1>First Jekyll site</h1>
    <nav>
    <ul>
    <li><a href="index.html">Home</a></li>
    <li><a href="news.html">News</a></li>
    </ul>
    </nav>
</header>
```

##Site baseurl
The hyperlink shown above may cause problem when it publishes globally. To fixed it we need to add baseurl as **config.yml** set: 

```
    <li><a href="{{site.baseurl}}/index.html">Home</a></li>
    <li><a href="{{site.baseurl}}/news.html">News</a></li>
```

 Commit this change on Github, refresh your site and check the source code. 
 
##Include
In real developing process, some modules will be shared in many pages, commonly like header and footer. **Include** is just like C/C++ programming, which to add some modules in. 

In Jekyll, we have a folder called **_include** to contain these modules. 

For example, we can extract the navigation as a single page **nav.html** in **_include** folder:

```
<nav>
    <ul>
    <li><a href="{{site.baseurl}}/index.html">Home</a></li>
    <li><a href="{{site.baseurl}}/news.html">News</a></li>
    </ul>
</nav>
```
Then in the **default** layout, we replace this part by the include label like:

```
<header><h1>First Jekyll site</h1>
</header>
```

You can also put the navigation in the footer.

##Add CSS
CSS is used for decorating the HTML pages. In Jekyll, we need create a folder **CSS**. We add some basic CSS style for example: 

```
html {
	margin: 0;
	padding: 0;
	color: blue;
	font: normal 100%/1.4 Georgia, serif;
}
```

Apply CSS style in your layout: add line 5 to your head part of HTML file. Remember to add the baseurl in sure it works well no matter locally or globally. 

```
...
<head>
	<meta charset="utf-8">
	<title>first github site</title>
	<link rel="stylesheet" type="text/css" href="{{site.baseurl}}/css/common.css">
</head>	
...
```

##Highlight navigation
Here we are going to study some application of CSS. A new CSS class is created:

```
.current {
	background-color: #000531;
	color: #fff;
}
```
Then apply it to the navigation like:

```
    <li>
    <a href="{{site.baseurl}}/index.html" class=“current”>Home</a>
    </li>
```
However we just need to apply this effect in the homepage, now the module is shared by every page. A if statement is the solution:

```
	<li>
	<a href="{{site.baseurl}}/index.html" class=“{%if page.url == 'index.html' %}current{% endif %}”>Home</a>
	</li>
```

##Include parameters
If we want to apply a secondary CSS class to different modules, an include parameter is useful. For example, the navigation module used in header and footer will apply the different CSS style nav-top and nav-buttom.

Then in the nav.html, we need to add:

```
<nav>
    <ul class"nav {{include.navclass}}>
    <li><a href="{{site.baseurl}}/index.html">Home</a></li>
    <li><a href="{{site.baseurl}}/news.html">News</a></li>
    </ul>
</nav>
```

In the CSS sheet, we can style both of these like:

```
.nav {
    margin : 0;
    padding : 0;
    list-style-type :none;
}

.nav > li {
    display: inline-block;
}

.nav-top {
	color: red;
}
.nav-bottom {
	color: green;
}
```

##Lopping over posts
A folder **_posts** could contain all posts. The name convention for each post is very strict, e.g.

    2013-06-23-A-beautiful-love-with-claire.md

The posts are common markdown format. 

```
---
layout: news-article
title: Curiosity rover makes big water discovery in Mars dirt
meta: Future Mars explorers may be able to get all the water they need out of the red dirt beneath their boots.
source: http://www.space.com
category: news
---

Future Mars explorers may be able to get all the water they need out of the red dirt beneath their boots, a new study suggests.
```

You can also add a limit number of posts you want to show on the page. All title will be looped by using **news.title** The expert is extracted by **news.meta** label.

The posts can be displayed by categories by modifying the for loop like:

```
 	{% for news in site.categories.news limit:2 %}
	<li>
		<a href="">{{news.title}}</a>
		<p>{{news.meta}}</p>
	</li>
	{% endfor %}
```
The categories are declared very top in every markdown post, and generated in **_site** folder.

##Summary
Here we have done almost half of this tutorial, and this is the first time I use markdown to write post. I feel so good. By markdown and Latex, I can say goodbye to word forever! The Jekyll is as cool as I thought. Since I have some basic knowledge of Github/HTML/CSS/Wordpress, it seems not difficult for me to learn. I will continue Part II tomorrow.

# Jekyll Tutorial Learning Notes -- Part II
These notes are for video 16 to 32.
##Nested layouts