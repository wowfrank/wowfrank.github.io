---
layout: post
title: 3 Simple steps to setup Jekyll Categories and Tags
date: 2019-10-31 00:00:00 +0300
description: 3 Simple steps to setup Jekyll Categories and Tags (optional)
img: jekyll-categories.png # Add image post (optional)
fig-caption: 3 Simple steps to setup Jekyll Categories and Tags # Add figcaption (optional)
tags: [Jekyll,Categories]
categories: ['Jekyll']
---


## Why Jekyll Categories or Tags?

Imagine you have a blog where you discuss very different things all together. Many bloggers post their personal experiences along with some professional posts. Curating information is very important to make users browse through your website with ease. What if New York Times had no categories like Politics, Business, Tech etc..? How hard would it be to track what happened to last night’s football game? There should be a Sports category to make readers’ life easy.

One more thing I want to clear right away is that tags are nothing but categories in Jekyll. And if you want your post links to be /category/title/ or /tag/title/ then you can do that in _config.yml file by using this line

```yaml
permalink: /:categories/:title/
```

### How to implement Jekyll Categories?

Before implementing Jekyll categories, see if your content can be divided into certain domains. For example on my blog, I have sorted my posts into Jekyll, Web-Design, Github, etc. You will see the categories at the end of the article. On clicking them, you will be redirected to a tags page where all blog posts are categorized under certain topics. One post may contain more than one category if it is dealing with more than one topic.

Let’s see how we can implement it.

### Add Jekyll Categories to Front matter

This is the first step in any method to implement Jekyll Categories or Jekyll Tags. Decide what categories you want. It is better to have fewer categories. As shown in the example below, add categories: front matter to all the posts according to their content.

Let’s imagine there are 3 posts in your blog. One is personal, one is tech and another is a mixed post. Here you can organize them into two categories - personal and tech. Now, add these categories to all the posts.

Post 1:

```yaml
---
layout: post
title: My ways to live life.
categories: Personal
---
```

Post 2:

```yaml
---
layout: post
title: Top innovations in technology.
categories: Tech
---
```

Post 3:

```yaml
---
layout: post
title: New tech to organize your daily tasks.
categories: [Tech, Personal]
---
```

### Create a Category page

This is the page which will be shown when someone clicks on any category. Something like my jekyll tags page. Copy below code and paste it into a new file and name it categories.html.

{% highlight html %}
---
layout: page
permalink: /categories/
title: Categories
---


<div id="archives">
{% for category in site.categories %}
    <div class="archive-group">
        {% capture category_name %}{{ category | first }}{% endcapture %}
        <div id="#{{ category_name | slugize }}"></div>
        <p></p>

        <h3 class="category-head">{{ category_name }}</h3>
        <a name="{{ category_name | slugize }}"></a>
        {% for post in site.categories[category_name] %}
            <article class="archive-item">
                <h4><a href="{{ site.baseurl }}{{ post.url }}">{{post.title}}</a></h4>
            </article>
        {% endfor %}
    </div>
{% endfor %}
</div>
```
{% endhighlight %}

The page will look like this. It will have all the categories listed out. One article can be listed in many categories. This happens when you use more than one category for a post.

If you want a simple list of all posts inside a certain category then use the below code. A better example for this would be this website.

#### Personal

```html
{% for post in site.categories.Personal %}
	<li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
```

#### Tech

```html
{% for post in site.categories.Tech %}
	<li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
```

Display categories on Jekyll posts.

It is better to show the categories to which the current article belongs to. And upon clicking any of the categories, users should land on the page we created in the previous step.

This can be done with the following liquid syntax. Copy this to post layout wherever you want to show the categories.

```html
<div class="post-categories">
	{% if post %}
		{% assign categories = post.categories %}
	{% else %}
		{% assign categories = page.categories %}
	{% endif %}
	{% for category in categories %}
	<a href="{{site.baseurl}}/categories/#{{category|slugize}}">{{category}}</a>
	{% unless forloop.last %}&nbsp;{% endunless %}
	{% endfor %}
</div>
```

This can also be done using a simple piece of code if you just want to show categories without any link.

```liquid
{{page.categories | capitalize | join: ', '}}
```

or

```liquid
{{page.tags | capitalize | join: ', '}}
```

This is how we can implement categories. If we want tags instead of categories then we should replace category with tag and categories with tags everywhere.

### Conclusion

It is good to categorize items if there is a lot of content on varying topics. But if the content is just on one subject then there is no need to categorize it. Using category slug in the URL helps users to figure out what they will encounter on opening the link. Use Jekyll Categories where you think is necessary. If you think your content doesn’t belong to any category then try Jekyll Collections.