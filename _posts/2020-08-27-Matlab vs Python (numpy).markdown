---
layout: post
title:  "Matlab vs Python"
date:   2020-08-27 15:25:47 +0200
categories: jekyll update
---

<!-- Load jQuery -->
      
      
        
        <script src="//code.jquery.com/jquery-1.11.1.min.js"></script>
      
      
        
        <!-- Load KaTeX -->
      
      
        
        <link rel="stylesheet" href="//cdnjs.cloudflare.com/ajax/libs/KaTeX/0.1.1/katex.min.css">
      
      
        
        <script src="//cdnjs.cloudflare.com/ajax/libs/KaTeX/0.1.1/katex.min.js"></script>

{% highlight ruby %}
	def
{% endhighlight %}


$("script[type='math/tex']").replaceWith(
      
      
        
          function(){
      
      
        
            var tex = $(this).text();
      
      
        
            return "<span class=\"inline-equation\">" + 
      
      
        
                   katex.renderToString(tex) +
      
      
        
                   "</span>";
      
      
        
        });
      
      
        
        

      
      
        
        $("script[type='math/tex; mode=display']").replaceWith(
      
      
        
          function(){
      
      
        
            var tex = $(this).text();
      
      
        
            return "<div class=\"equation\">" + 
      
      
        
                   katex.renderToString("\\displaystyle "+tex) +
      
      
        
                   "</div>";
      
      
        
        });