---
title: Jekyll Related Posts
layout: post
tags:
- code
---

Spent some time this weekend trying to implement 'Related Posts' onto my blog. Wasn't hard, but also wasn't as straight forward as I expected.

[Jekyll's documentation](http://jekyllrb.com/docs/configuration/) is pretty light and wasn't really helpful for related posts. Also, [general sentiment](http://charliepark.org/tags-in-jekyll/) [on the internet](http://techtinkering.com/2011/08/17/improving-related-posts-on-jekyll/) is that Jekyll's existing related posts function is subpar.

Most signs point to [Lawrence Woodman's library](https://github.com/LawrenceWoodman/related_posts-jekyll_plugin) for overwriting Jekyll's existing related posts function and replacing it with a better one. 

While it's not bad, I prefer using tags instead of categories. Thankfully I found [David Lynch's blog](http://davidlynch.org/blog/2011/10/jekyll/) and he had an improved version of Lawrence Woodman's library to do exactly that. 

Add a file to the ```_plugins``` directory called ```related_posts.rb``` and paste in the code below. Thanks David!

```
require 'jekyll/post'

module BetterRelatedPosts

  # Used to remove #related_posts so that it can be overridden
  def self.included(klass)
    klass.class_eval do
      remove_method :related_posts
    end
  end

  # Calculate related posts.
  #
  # Returns [<Post>]
  def related_posts(posts)
    return [] unless posts.size > 1
    highest_freq = Jekyll::Post.tag_freq(posts).values.max
    related_scores = Hash.new(0)
    posts.each do |post|
      post.tags.each do |tag|
        if self.tags.include?(tag) && post != self
          cat_freq = Jekyll::Post.tag_freq(posts)[tag]
          related_scores[post] += (1+highest_freq-cat_freq)
        end
      end
    end

    Jekyll::Post.sort_related_posts(related_scores)
  end

  module ClassMethods
    # Calculate the frequency of each tag.
    #
    # Returns {tag => freq, tag => freq, ...}
    def tag_freq(posts)
      return @tag_freq if @tag_freq
      @tag_freq = Hash.new(0)
      posts.each do |post|
        post.tags.each {|tag| @tag_freq[tag] += 1}
      end
      @tag_freq
    end

    # Sort the related posts in order of their score and date
    # and return just the posts
    def sort_related_posts(related_scores)
      related_scores.sort do |a,b|
        if a[1] < b[1]
          1
        elsif a[1] > b[1]
          -1
        else
          b[0].date <=> a[0].date
        end
      end.collect {|post,freq| post}
    end
  end

end

module Jekyll
  class Post
    include BetterRelatedPosts
    extend BetterRelatedPosts::ClassMethods
  end
end
```

You can use tags in your front matter, ie

```
---
title: Jekyll Related Posts
layout: post
tags:
 - jekyll
---
```

And use this in one of your layouts file

```
{% for rel in site.related_posts limit:5 %}
  <li><a href="{{ rel.url }}">{{ rel.title }}</a></li>
{% endfor %}
```