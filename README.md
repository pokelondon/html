# POKE HTML Style Guide

*A mostly reasonable approach to HTML fundamentals*


## Table of Contents

1. [Formatting](#formatting)
1. [Doctype](#doctype)
1. [Getting Meta](#going-meta)
1. [OldIE Helper Classes](#oldie-helper-classes)
1. [Semantic Markup](#semantic-markup)
1. [Performance](#performance)
1. [Accessibility](#accessibility)


## Formatting

- 4-space tabs
- Quote attributes with single-quotes
- No slash in self-closing tags (`/>`)
- Escape `<`, `>` and `&` entities ([source](http://stackoverflow.com/a/25612313/258794))
- Indent after opening a block-level tag unless its contents are minimal and will fit on one short line:

**Bad**

```html
<div class='Masthead'>
    <p>Nulla vitae elit libero, a pharetra augue. Nullam id dolor id nibh ultricies vehicula ut id elit. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Sed posuere consectetur est at lobortis. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Nullam id dolor id nibh ultricies vehicula ut id elit. Curabitur blandit tempus porttitor.</p>
</div>
```

**Good**

```html
<div class='Masthead'>
    <p>
        Nulla vitae elit libero, a pharetra augue. Nullam id dolor id nibh ultricies vehicula ut id elit. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Sed posuere consectetur est at lobortis. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Nullam id dolor id nibh ultricies vehicula ut id elit. Curabitur blandit tempus porttitor.
    </p>
</div>

<div class='Masthead'>
    <h3>Nulla vitae elit libero</h3>
</div>
```


## Doctype

- We should always use the HTML5 doctype:

```html
<!DOCTYPE html>
```

- And a sensible default `html` tag:

```html
<html lang='en' dir='ltr'>
```

- If using [Modernizr](https://modernizr.com/), you can add a `no-js` class to the `html` tag which will be replaced with a `js` class when Modernizr has loaded. This is useful for targeting JS-dependent behaviours or presentation.

```html
<html lang='en' dir='ltr' class='no-js'>
```


## Getting Meta

- A sensible set of base `meta` tags:

```html
<meta http-equiv='X-UA-Compatible' content='IE=edge,chrome=1'>
<meta charset='utf-8'>
<meta http-equiv='imagetoolbar' content='no'>
<meta name='viewport' content='width=device-width, initial-scale=1.0'>
```

- This encourages IE into its most modern rendering mode where possible, declares the document encoding to be UTF-8, suppresses the UI-polluting image handling contextual toolbar in older versions of IE, and sets up a decent default viewport on mobile devices.

- The HTML5 Boilerplate project [notes](https://github.com/h5bp/html5-boilerplate/issues/1187) that your `<title>` tag should appear *after* the charset declaration.


## OldIE Helper Classes

- Wrapping the `body` tag in conditional comments allows the use of helper classes to target different versions of IE in CSS:

```html
<!--[if lt IE 7 ]> <body class='ie6'> <![endif]-->
<!--[if IE 7 ]>    <body class='ie7'> <![endif]-->
<!--[if IE 8 ]>    <body class='ie8'> <![endif]-->
<!--[if IE 9 ]>    <body class='ie9'> <![endif]-->
<!--[if (gt IE 9)|!(IE)]><!--> <body class='not-ie'> <!--<![endif]-->
```

- Previous standard behaviour was to use this technique on the `html` tag, but this messes with IE's handling of `X-UA-Compatible` as described [here](https://github.com/h5bp/html5-boilerplate/issues/1187).

- This technique will not detect IE10, IE11 or Edge. Further deviousness (starting point [here](http://stackoverflow.com/questions/9900311/how-do-i-target-only-internet-explorer-10-for-certain-situations-like-internet-e)) is required for those more modern versions.


## Semantic Markup

- Use HTML tags appropriate to the role of the content they describe.
- Your documents should be readable when your CSS is removed, leaving only browser defaults. This can be achieved by sensible selection of HTML tags.
- Semantic markup improves document readability for assistive technologies and contributes to your SEO. It also helps to ensure your content can still be read if your CSS should fail to load, or if CSS is not supported by the UA being used to view your pages.
- For a more in-depth look see this [Wikipedia article](https://en.wikipedia.org/wiki/Semantic_HTML).

** Bad **

```html
<div class='Nav-outer'>
    <div class='Nav-inner'>
        <a href='#foo'>Home</a>
        <a href='#foo'>About Us</a>
        <a href='#foo'>Contact</a>
    </div>
</div>
```

** Good **

```html
<nav class='Nav-outer'>
    <ul class='Nav-inner'>
        <li>
            <a href='#foo'>Home</a>
        </li>
        <li>
            <a href='#foo'>About Us</a>
        </li>
        <li>
            <a href='#foo'>Contact</a>
        </li>
    </ul>
</nav>
```

** Bad **

```html
<div class='Sidebar'>
    <div class='Sidebar-quote'>
        Integer posuere erat a ante venenatis dapibus posuere velit aliquet.
    </div>
    <div class='Sidebar-author'>
        John Smith
    </div>
</div>
```

** Good **

```html
<aside class='Sidebar'>
    <blockquote class='Sidebar-quote'>
        Integer posuere erat a ante venenatis dapibus posuere velit aliquet.
    </blockquote>
    <cite class='Sidebar-author'>
        John Smith
    </cite>
</aside>
```


## Performance

- Very small JS files, particularly those for polyfills or feature detection, e.g. Modernizr or Picturefill, should be inlined in the `head` of the document where possible so the benefits they provide are available immediately without an additional HTTP round-trip. Note that inline scripts aren't cached, so use this technique sparingly.
- If your performance budget requires it, you can inline critical CSS in the document `head` and load the bulk of your CSS further down the document.
- Consider using [`async` and `defer`](http://www.html5rocks.com/en/tutorials/speed/script-loading/) to speed up non-blocking script loading.
- Our current convention is to place most behavioural scripts at the bottom of the `body` tag so the DOM will always be ready to manipulate – however, this often means a slight delay before the scripts are executed and events bound, etc.


## Accessibility

- Use semantic markup
- Use the `tabindex` attribute to ensure a sensible tab order in your UI
- Use ARIA roles on key elements to aid document navigation and the outlining features of assistive technologies
- Give inline images useful `alt` attributes that describe the content of the image where possible
