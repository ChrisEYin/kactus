---
title: Youtube Videos in Jekyll
layout: post
tags:
 - code
 - youtube
 - jekyll
---

Was trying to embed Youtube videos onto my blog (Jekyll / Markdown), and [a lot](http://stackoverflow.com/questions/10529859/how-to-include-video-in-jekyll-markdown-blog) [of the ways](http://dev.pibby.com/jekyll-youtube-plugin/)/[plugins](https://gist.github.com/vanto/1455726) [out there](http://blog.omgmog.net/post/video-integration-with-jekyll/) weren't working for me. 

Regardless what I did, I always hit this:

``` 
Maruku cannot parse this block of HTML/XML: 
# <iframe 
     width="532" 
     height="400" 
     src="//www.youtube.com/embed/iqUG2_cmZ6I" 
     frameborder="0" 
     allowfullscreen></iframe> 
```

--

After trying a bunch, what eventually what did work was 2 things:

1. Make sure that there's a space between the opening and closing iframe tags.
2. And change ```allowfullscreen``` to ```allowfullscreen="allowfullscreen"```.

Now the above should look like below. Thanks internet.

```
# <iframe 
     width="532" 
     height="400" 
     src="//www.youtube.com/embed/iqUG2_cmZ6I" 
     frameborder="0" 
     allowfullscreen="allowfullscreen"> </iframe>
```