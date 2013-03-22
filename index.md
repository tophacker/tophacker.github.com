---
layout: page
title: Hacker World!
tagline: Supporting tagline
---
{% include JB/setup %}

## Hacker Ethic

    # 1. Access to computers--and anything that might teach you something about the way the world works--should be unlimited and total. Always yield to the Hands-On Imperative!
    # 2. All information should be free.
    # 3. Mistrust Authority--Promote Decentralization.
    # 4. Hackers should be judged by their hacking, not bogus criteria such as degrees, age, race, or position.
    # 5. You can create art and beauty on a computer.
    # 6. Computers can change your life for the better.

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


