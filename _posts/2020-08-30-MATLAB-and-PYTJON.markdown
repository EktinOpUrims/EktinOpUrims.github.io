---
layout: post
title:  "Matlab and Python"
date:   2020-08-29 23:31:07 +0200
categories: jekyll update
katex: true
---
In this post we will employ Matlab and Python to solve numerically a partial differential equation, and will compare the results.
We will treat the following equation:

$$u_t + \alpha (u^p)_x + \beta u_{xxx} =0, $$

where $$\alpha, \beta$$ are real numbers, and $$p=1,2,3,\ldots$$ is an integer.
There are two important cases of the above equation.
<ul>
<li>
$$p=2, \alpha=3,\beta=1.$$
In this case we have the Korteweg-de Vries equation (KdV)

$$u_t+6uu_x+u_{xxx}=0.$$

That equation has an exact solution (called soliton) of the form

$$u_{solKdV}(x,t)=\frac{2c^2}{\cosh^2(c(x-4c^2t))}.$$

</li>
<li>
$$p=3,\alpha=2,\beta=1.$$
In this case the equation becomes the (`plus') modified Korteweg-de Vries equation ((+)MKdV) (to distinguish with ``minus'' KdV, which corresponds to $$\alpha=-2$$)

$$u_t+6u^2u_x+u_{xxx}=0.$$

The MKdV has an exact solution (also called soliton) of the form

$$u(x,t)_{solMKdV}=\frac{2c}{\cosh(2c(x-4c^2t))}.$$

</li>
</ul>


<!--<h1>A</h1>
<h2>A</h2>
<h3>A</h3>
#<h4>A</h4>
<h5>A</h5>
<h6>A</h6>
-->

<hr> <!-- a horizontal line -->
<hr>

## The method

### Fast Fourier transform.
The method to solve the equation is the Fourier method, using the Fast Fourier Transform method. It is recommended first to look at the usual <a href="">Fourier Transform</a>, and <a href="">Fast Fourier Trnsform</a>. At first site it is not clear how the two are related. We will see this now. So we assume that our solution $$u(x,t)$$ is very small outside of a finite interval $$[A,B].$$ For a function $$ u(x) $$ (periodic with period $$B-A$$) define its Fourier Transform by the formula 

$$ \widehat{u}(k) = \frac{1}{B-A}\int_{A}^{B}u(x)e^{-2\pi i\frac{x-A}{B-A}k}d x \approx
\sum\limits_{j=0}^{n-1}u(x_j)e^{-2\pi i \frac{jk}{n}} \frac{1}{n} = \frac{1}{n}fft(u).$$

We see that on the right-hand-side (rhs) we got exactly the fast fourier transform. Here we discretized $$x_j=A+(B-A)\frac{j}{n},\quad j=0,1,\ldots, n-1.$$
The $$k$$ is an integer ranging from `0` to `n-1`, or, alternatively, from `0` to `n/2` and then from `-n/2 +1` to `-1`, to have a more symmetric situation. Note that the fft works better for powers of 2, so for us `n=2^{N}`.

We have the following important property:
$$\widehat{u'} = i \mathbb{k} \widehat{u}, $$
where $$\mathbb{k} = \frac{2\pi}{B-A}k.$$

In what follows we drop the bold sign at $$\mathbb{k},$$ and will write it just as $$k.$$

<hr />

### Application of FFT to the equation.
We apply the fft to 

`u_t +\alpha (u^p)_x + u_{xxx}=0, `

to obtain

`(^u)_t +\alpha i k (^(u^p)) + (ik)^3 ^u=0, `

and, multiplying by `e^{-i\beta k^3t}` to combine the first and the third term,

` (^u e ^{-i \beta k^3 t} )_t  = -\alpha i k ^(u^p) e^{-i \beta k^3 t}.`

<hr />

### Runge-Kutta 4th order method (RG4).

We arrive at the following situation:

``\frac{d y}{d t} = F(t, y).``

The <a href="">RG4</a> is as follows: if we have $y(t_n),$ then to obtain $y(t_{n+1})$ we do the following:

``k_1 = F(t_n, y_n)``
<br> <!-- break the line -->
``k_2 = F(t_n + dt/2, y_n + k1 dt/2)``
<br>
``k_3 = F(t_n + dt/2, y_n + k2 dt/2)``
<br />
``k_4 = F(t_n + dt, y_n + k3 dt)``
<br />
and then we combine all of them
<br>
``y(t_{n+1}) = y(t_n) + dt/6 (k_1 + 2k2 + 2k_3 + k_4).``

Don't mix up all different $k$s from this post.

<hr />

### Application of RG4 to our case.

So, we have `y = ^u e^{-i\beta k^3 t},`
and our equation writes in terms of `y` as follows:
(we denote fft(u) by u^, and ifft(uhat) by uhat*)

``y_t = -\alpha i k ([(y e^{i \beta k^3 t})*]^p)^ e^{-i \beta k^3 t}. ``

Then the k_1, k_2, k_3, k_4 from RG4 scheme take the following form:

$$k_1 = -\alpha i k ([IFFT{(y e^{i \beta k^3 t})}]$$

$$ k_1 = -\alpha i k ([(y e^{i \beta k^3 t})*]^p)^ e^{-i \beta k^3 t}, $$

``k_1 = -\alpha i k ([(y e^{i \beta k^3 t})*]^p)^ e^{-i \beta k^3 t}, ``

``k_2 = -\alpha i k ([((y + dt/2 k_1) e^{i \beta k^3 ( t + dt/2 )})*]^p)^ e^{-i \beta k^3 ( t + dt/2 )}, ``

``k_3 = -\alpha i k ([((y + dt/2 k_2) e^{i \beta k^3 ( t + dt/2 )})*]^p)^ e^{-i \beta k^3 ( t + dt/2 )}, ``

``k_4 = -\alpha i k ([((y + dt k_3) e^{i \beta k^3 ( t + dt )})*]^p)^ e^{-i \beta k^3 ( t + dt )}, ``

and then

``y_{n+1} = y_n + dt/6 (k_1 + 2k_2 + 2k_3 + k_4).``

What is written so far needs to be refined. Indeed, for us, k will be a vector ``k = dx * [-n/2+1, ..., 0, n/2]``.
A typical value for n will be 2^10 = 1024. Then we will take k^3 -- to obtain very large numbers, of the order 2^30. And they will be multiplied by `t`, which is small only in the beginning. To resolve this issue, we define
`l_j = k_j e^{i\beta k^3t},` then the above equations for `k_j` are rewritten for `l_j` as follows:

``l_1 = -\alpha i k ([(u^ })*]^p)^ , ``

``l_2 = -\alpha i k ([((u^ + dt/2 l_1) e^{i \beta k^3 (  dt/2 )})*]^p)^ e^{-i \beta k^3 ( dt/2 )}, ``

``l_3 = -\alpha i k ([((u^ + dt/2 k_2) e^{i \beta k^3 (  dt/2 )})*]^p)^ e^{-i \beta k^3 ( dt/2 )}, ``

``l_4 = -\alpha i k ([((u^ + dt k_3) e^{i \beta k^3 (  dt )})*]^p)^ e^{-i \beta k^3 ( dt )}, ``

and in the end we obtain a very simple formula for updating the fft(u):

``(u^)_{n+1} = [(u^)_n + dt/6 (l_1 + 2l_2 + 2l_3 + l_4) ] e^{i \beta k^3 dt}.``

The situation for $l_j$ is better than for $k_j$, since now $k^3$ is multiplied by $dt,$ which does not accumulate. We will need to keep $k^3 dt$ resonably small. That is, $2^{n/2} dt$ must not be too big.

<hr />
The numerical scheme.

So, now we will be given an initial function $u(x).$
We will need to perform the following steps:
<ol>
<li> `u \mapsto uhat=fft(u) ` </li>
<li> `` define l_j, j=1,2,3,4, and update uhat``</li>
<li> uhat \mspto u,  and plot u </li>
</ol>
Repeat the steps 2,3 in a loop.

<hr />
<hr />

## Implementation in MATLAB
{% highlight ruby %}

A=-10, B=10

{% endhighlight %}
The code can be downloaded here



## Implementation in Python (numpy)
{% highlight ruby %}

import numpy as np
import time

class Param:
    n = 3


A = -10, B=10

{% endhighlight %}
The code can be downloaded here

[sdf][sdf]

`hi`

'hi'

``hellow``

"hello"

You’ll find this post in your `_posts` directory. Go ahead and edit it and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `jekyll serve`, which launches a web server and auto-regenerates your site when a file is updated.

$$ x+x $$

[Jekyll docs][jekyll-docs]

Jekyll requires blog post files to be named according to the following format:

`YEAR-MONTH-DAY-title.MARKUP`

Where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file. After that, include the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
