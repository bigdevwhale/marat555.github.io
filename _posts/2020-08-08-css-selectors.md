---
layout: post
title: "Types of CSS Selectors"
tags:
- css
- css selectors
- front-end
thumbnail_path: blog/thumbs/types-of-css-selectors.png
add_to_popular_list: true
---

CSS selector is the part of a CSS rule set that actually selects the content you want to style. Let's look at all the different kinds of selectors 

{% include figure.html path=page.thumbnail_path %}

# Types of CSS Selectors

* [Universal Selector](#universal-selector)
* [Element Selector](#element-selector)
* [Id Selector](#id-selectore) 
* [Class Selector](#class-selector) 
* [Attribute Selector](#attribute-selector)
* [Descendant Combinator](#attribute-selector)
* [Child Combinator](#child-combinator)
* [General Sibling Combinator](#general-sibling-combinator)
* [Adjacent Sibling Combinator](#adjacent-sibling-combinator)
* [Pseudo-class](#pseudo-class)
* [Pseudo-element](#pseudo-element)

## Universal Selector

Universal selector selects all the elements on a webpage.

#### Example:

{% highlight css %}
* {
   margin: 0; 
   padding: 0; 
}
{% endhighlight %}

## Element Selector

Also referred to simply as a "type selector", this selector must match one or more HTML elements of the same name.

{% highlight css %}
p {
   font-family: arial, helvetica, sans-serif; 
}
{% endhighlight %}

## Id Selector

An ID selector is declared using a hash, or pound symbol (#) preceding a string of
characters. The stringof characters is defined bythe developer. This selector matches
any HTML element that has an ID attribute with the samevalue as thatof the selector,
but minus the hash symbol.

{% highlight css %}
#paragraph1 {
    margin: 0; 
}
{% endhighlight %}

## Class Selector

The class selector is the most useful of all CSS selectors. It's declared with a dot
preceding a string of one or more characters. Just as is the case with an ID selector,
this string of characters is defined by the developer. The class selector also matches
all elements on the page that have their class attribute set to the same value as the
class, minus the dot.

{% highlight css %}
.note {
   color: red; 
   background-color: yellow; 
   font-weight: bold; 
}
{% endhighlight %}

## Attribute Selector

The attribute selector targets elements based on the presence and/or value of HTML
attributes, and is declared using square brackets

{% highlight css %}
a[href="http://www.somesite.com"] {
   font-weight: bold; 
}
{% endhighlight %}

## Descendant Combinator

The class selector is the most useful of all CSS selectors. It's declared with a dot
preceding a string of one or more characters. Just as is the case with an ID selector,
this string of characters is defined by the developer. The class selector also matches
all elements on the page that have their class attribute set to the same value as the
class, minus the dot.

{% highlight css %}
div#paragraph1 p.note {
   color: green; 
}
{% endhighlight %}

## Child Combinator

A selector that uses the child combinator is similar to a selector that uses a descendant combinator, except it only targets immediate child elements

{% highlight css %}
p.note > b {
   color: blue; 
}
{% endhighlight %}

## General Sibling Combinator

A selector that uses a general sibling combinator matches elements based on sibling
relationships. That is to say, the selected elements are beside each other in the
HTML.


{% highlight css %}
h2 ~ p {
 margin-bottom: 20px;
}
{% endhighlight %}

## Adjacent Sibling Combinator

A selector that uses the adjacent sibling combinator uses the plus symbol (+), and
is almost the same as the general sibling selector. The difference is that the targeted
element must be an immediate sibling, not just a general sibling. 

{% highlight css %}
p + p {
 text-indent: 1.5em;
 margin-bottom: 0;
}
{% endhighlight %}

## Pseudo-class

A pseudo-class uses a colon character to identify a pseudo-state that an element
might be in—for example, the state of being hovered, or the state of being activated.

{% highlight css %}
a:active {
   color: blue;
}
{% endhighlight %}

## Pseudo-element

Finally, CSS has a selector referred to as a pseudo-element and, used appropriately,
it can be very useful.

{% highlight css %}
p::first-letter {
   font-size: 32px;
}
{% endhighlight %}
