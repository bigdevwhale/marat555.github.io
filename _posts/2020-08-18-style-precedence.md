---
layout: post
title: "CSS Precedence"
tags:
- css
- css selectors
- front-end
- css precedence
thumbnail_path: blog/thumbs/css-precedence.png
add_to_popular_list: true
---

In a single HTML document possible that some CSS rules will conflict with one another. CSS uses a mechanism called the **cascade** to resolve any such conflicts. 

{% include figure.html path=page.thumbnail_path %}

Before describing how the cascade works, we need to consider two of its main components - **specificity** and **inheritance**.

## Specificity

{% include figure.html path=page.thumbnail_path path="blog/css-precedence/specificity.jpg" %}

Specificity refers to the relative weights of various rules. It determines which styles apply to an element when more than one rule could apply. 
Table shows how much each part of a selector contributes to the total specificity of that selector.

| Selector type | Example | Specificity |
| --- | --- | --- |
| Universal selector | * | 0,0,0,0 |
| Combinator | * | 0,0,0,0 |
| Element identifier | div | 0,0,0,**1** |
| Pseudo-element identifier | ::first-line | 0,0,0,**1** |
| Class identifier | .error | 0,0,**1**,0 |
| Pseudo-class identifier | :hover | 0,0,**1**,0 |
| Attribute identifier | [name="email"] | 0,0,**1**,0 |
| ID identifier | #elem | 0,**1**,0,0 |
| Inline style attribute | style="color: blue;" | **1**,0,0,0 |

<br />
Specificity values are cumulative; thus, a selector containing two attribute identifiers and a class identifier (e.g., 
**[name="email"][type="checkbox"].hello**) has a specificity of
**0,0,3,0**. Specificity values are sorted from right to left; thus, a selector containing 4 element identifiers (**0,0,0,4**) has a lower specificity than a selector containing just a single class identifier (**0,0,1,0**).

The !important directive gives a declaration more weight than nonimportant
declarations. The declaration retains the specificity of its selectors and is used
only in comparison with other important declarations

If you want to learn more about css selectors, you can read my [last article](https://it.badykov.com/blog/2020/08/08/css-selectors/) about css selectors.

## Inheritance

{% include figure.html path="blog/css-precedence/inheritance.jpg" %}

When no value for an inherited property has been specified on an element, the element gets the computed value of that property on its parent element. Only the root element of the document gets the initial value given in the property's summary.

Examples of non-inherited properties are **padding**, **border**, **margin**, and **background**.

## The Cascade

{% include figure.html path="blog/css-precedence/cascade.jpeg" %}

The cascade is how CSS resolves conflicts between styles; in other words, it
is the mechanism by which a user agent decides, for example, what color to
make an element when two different rules apply to it and each one tries to set
a different color. Here’s how the cascade works:
1. Find all rules with a selector that matches a given element.
2. Sort all declarations applying to the given element by explicit weight.
Those rules that are marked !important have a higher explicit weight
than those that are not.
3. Sort all declarations applying to the given element by origin. There are
three basic origins: author, reader, and user agent. Under normal
circumstances, the author’s styles win out over the reader’s styles.
Howerver, !important reader styles are stronger than any other styles,
including !important author styles. Both author and reader styles
override the user agent’s default styles.
4. Sort all declarations applying to the given element by specificity. Those
elements with a higher specificity have more weight than those with
lower specificity.
5. Sort all declarations applying to the given element by order. The later a
declaration appears in the stylesheet or document, the more weight it is
given. Declarations that appear in an imported stylesheet are considered
to come before all declarations within the stylesheet that imports them.

## See Also

[Types of CSS Selectors](https://it.badykov.com/blog/2020/08/08/css-selectors/)

