---
layout: single
title:  "Enabling KaTeX With Minimal Mistakes To Render LaTeX Equations On Github Pages"
date:   2019-06-20 21:27:00 -0700
categories: jekyll
katex: true
---

# Why KaTeX and not MathJax?

While building my blog, I knew I definitely wanted to include LaTeX support so
that I can share theoretical insights. That said, the main choice appeared to be
MathJax, but I didn't really like it because it was slow. I found an alternative
in [KaTeX](https://katex.org/), which is [much faster than
MathJax](https://www.intmath.com/cg5/katex-mathjax-comparison.php). 

KaTeX is relatively new compared to MathJax, and so it is not as full-featured
as MathJax. However, I find that it does just as much, if not more than, what
you might find in a typical RMarkdown or Jupyter Notebook, so that's good enough
for me. If you're in doubt, you can check out [KaTeX's currently supported
functions on their documentation](https://katex.org/docs/supported.html).

# Setting Up KaTeX With Minimal Mistakes

I really like the Minimal Mistakes Jekyll theme, so I'll base my guide on
that. Minimal Mistakes comes shipped with a folder called `_includes/`, which
has a `_includes/script.html` that most people edit.

If you initialized your Jekyll blog with `jekyll new` like I did, then chances
are you don't have Minimal Mistake's `_includes/script.html`. You'll want to
copy that file from the [Minimal Mistakes
GitHub repository](https://github.com/mmistakes/minimal-mistakes) and place it
in your root directory at the same place (i.e. `_includes/script.html`) to avoid
breaking the website by creating a blank `scripts.html` (which I naively did,
whoops). 

Anyways, once you copied `_includes/script.html`, add the following to your
`_includes/scripts.html`:

{% raw %}
```liquid
{% if page.katex %}
    {% include katex.html %}
{% endif %}
```
{% endraw %}

Next, create a file `_include/katex.html`:

```html
<!DOCTYPE html>
<!-- KaTeX requires the use of the HTML5 doctype. Without it, KaTeX may not render properly -->
<html>
  <head>
    <!-- Load jQuery -->
    <script src="//code.jquery.com/jquery-1.11.1.min.js"></script>
    <!-- Load KaTeX -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/katex.min.css" integrity="sha384-yFRtMMDnQtDRO8rLpMIKrtPCD5jdktao2TV19YiZYWMDkUR5GQZR/NOVTdquEx1j" crossorigin="anonymous">
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/katex.min.js" integrity="sha384-9Nhn55MVVN0/4OFx7EE5kpFBPsEMZxKTCnA+4fqDmg12eCTqGi6+BB2LjY8brQxJ" crossorigin="anonymous"></script>
    <script defer src="https://cdn.jsdelivr.net/npm/katex@0.10.2/dist/contrib/auto-render.min.js" integrity="sha384-kWPLUVMOks5AQFrykwIup5lo0m3iMkkHrD0uJ4H5cjeGihAutqP0yW0J6dpFiVkI" crossorigin="anonymous"></script>
    <!-- Auto render inline and display equations -->
    <script>
        document.addEventListener("DOMContentLoaded", function() {
            renderMathInElement(document.body, {'delimiters' : [
                {left: "$$", right: "$$", display: true},
                {left: "\\[", right: "\\]", display: true},
                {left: "$", right: "$", display: false},
                {left: "\\(", right: "\\)", display: false}
            ]});
        
            document.querySelectorAll("script[type='math/tex; mode=display']").forEach(function(el) {
                el.outerHTML = katex.renderToString(el.textContent.replace(/%.*/g, ''), { displayMode: true });
            });
        });
    </script>
  </head>
</html>
```

Now, to enable KaTeX, simply add `katex: true` to the YAML of any page you want
to use it on:

```yaml
---
layout: single
title:  "My Title"
date:   2019-06-19 19:10:33 -0700
katex: true
---
```

And that's it! Now you're ready to write cool equations that load fast. Write
equations the same way you would in LaTeX by using `$` for inline or `$$` for
display, such as `$$\theta = (X^{T} X)^{-1} X^{T} y$$`:

$$\theta = (X^{T} X)^{-1} X^{T} y$$

One small annoying note. If you're like me and prefer to use `\(\)` for inline
and `\[\]` for display, you'll need to escape the backslashes to get the 
equations to render properly, as in `\\[ ax^2 + bx + c = 0 \\]`.

\\[ ax^2 + bx + c = 0 \\]

# Acknowledgements

Shoutouts to Steven Keras, [whose code I borrowed for
katex.html](https://github.com/stevenkaras/stevenkaras.github.com/blob/master/_includes/js/katex.js).
