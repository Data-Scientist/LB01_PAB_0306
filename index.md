---
layout: page
title: Hello,Guys of Team2!
tagline: hello from Blan, don't hit me plz.... 
---
{% include JB/setup %}
# Caution !! #

## Do not read just after a meal ##


Sorry for coming late!
This is a test of our page and we'll edit it in the future.

It's just a copy of our organization page, for testing only.


If anyone want to test your md files which are converted from rmd files which are generated in RStudio, just try to put your md files in folder _post in github,gh-pages branch.

Cause I'm using a Ubuntu system, I think I'd better not telling you guys how to use github in your system,  google knows it.

All you need to do is to put your md files in _post, don't forget to rename it in the format year-month-day-Title.md such as 2014-03-11-HelloFromBlan.md.

If there's any question, please send me a mail or leave me a message, my telephone number is .... sorry, my email address is blangero1987@gmail.com.

Finally, sorry for my poor English....

See you~


Read [Jekyll Quick Start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

Complete usage and documentation available at: [Jekyll Bootstrap](http://jekyllbootstrap.com)

## Update Author Attributes

In `_config.yml` remember to specify your own data:
    
    title : My Blog =)
    
    author :
      name : Name Lastname
      email : blah@email.test
      github : username
      twitter : username

The theme should reference these variables whenever needed.
    
## Sample Posts

This blog contains sample posts which help stage pages and blog data.
When you don't need the samples anymore just delete the `_posts/core-samples` folder.

    $ rm -rf _posts/core-samples

Here's a sample "posts list".

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


