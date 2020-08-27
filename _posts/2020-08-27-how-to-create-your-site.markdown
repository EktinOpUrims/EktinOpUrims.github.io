---
layout: post
title:  "How to create your site, and host it on github"
date:   2020-08-27 15:25:47 +0200
categories: jekyll update
katex: True
---

<h3> If you want to host your site on github, </h3>

<p> Do this:</p>
<ul>
	<li>Install <a href="https://git-scm.com/downloads">git</a>, then go <a href="https://pages.github.com/"> here to host your page</a> </li>
	<li><a href="https://jekyllrb.com/"> Read this fantastic how-to-do pages</a></li>
	<li><a href="https://pages.github.com/"> how to host on github </a> 
		<ul> 
			<li>git add --all</li>
			<li>git commit -m "commit"</li>
			<li>git push -u origin master</li>
			<li>Note that it takes 1-2 minutes for your changes to appear online on "https://yourname/github.io" </li>
		</ul>
	</li>
	<li><a href="https://xuc.me/blog/katex-and-jekyll/">to incorporate Latex</a></li>
	Based and combines two other posts 
	<a href="https://web.archive.org/web/20170117172154/http://willdrevo.com/latex-equation-rendering-in-javascript-with-jekyll-and-katex/"> Katex </a>
	and <a href="https://www.intmath.com/blog/mathematics/katex-a-new-way-to-display-math-on-the-web-9445"> Katex 2 </a>
	
	<li> <a href="https://gendignoux.com/blog/2020/05/23/katex.html"> Another instruction on Latex usage in Jekyll </a>
	
		<a href="https://github.com/KaTeX/KaTeX/releases"> Download Katex sources </a>
	
	</li>
	
	<li><a href="https://programminghistorian.org/en/lessons/building-static-sites-with-jekyll-github-pages#preparing-for-installation-">
		Another detailed instruction to set up jekyll and github </a>
	</li>
	
</ul>

<p> So, to set up katex in jekyll website, do the following. </p>
<ul>
	<li>
		In _config.yml file, write
		{% highlight ruby %}
markdown: kramdown
kramdown:
	math_engine: mathjax
		{% endhighlight %}
	</li>
	<li>
		download katex from <a href=""></a>
		unzip it, and move the folder katex in /assets/plugins/, so that you have for instance the following path: /assets/plugins/katex/katex.min.css
	</li>
	<li> in _includes/head.html file, add the lines (without space between { and % (I can not write it here, because jekyll process that as a code ))
		{% highlight ruby%}
<head>
    ...
    {nospace% if page.katex %nospace}
    <!-- KaTeX -->
    <link rel="stylesheet" href="/assets/plugins/katex/katex.min.css">
    {nospace% endif %nospace}
</head>			
		{% endhighlight %}
	then in each markdown page, write the following in the top:
	{% highlight ruby %}
---
layout: post
title: My awesome blog post
katex: True
---

Insert blog post content here...
Did you know that $$1 + 1 = 2$$?	
	{% endhighlight %}
	</li>
	$$ 2+2 $$
	
	$$x^2$$
</ul>

<ul>
	<li><a href="https://docs.github.com/en/github/working-with-github-pages/setting-up-a-github-pages-site-with-jekyll"> Jekyll and Github tutorials </a></li>
</ul>

<ul>
	<li>
		after you managed to host a simple page on git-hub, run the following in your terminal (to run terminal on windows, press quadropule-key and type in "terminal". To run terminal on mac, press quadropule-key and space-key, and type in "terminal")
	
	{% highlight ruby %}
		jekyll new JekyllDemo,
	{% endhighlight %}
	
		then copy what is inside the folder to your folder "yourname/github/io"
	</li>
</ul>

