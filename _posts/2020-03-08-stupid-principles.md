---
layout: post
title: "STUPID principles. What you should avoid when writing code."
tags:
- Programming
- Bad practice
- STUPID
thumbnail_path: blog/thumbs/stupid.jpg
add_to_popular_list: true
---

If you want to write understandable, flexible and maintainable code, you need to know what bad code looks like first. In this post, I will discuss bad practices that should be avoided when writing code.

{% include figure.html path=page.thumbnail_path %}

  STUPID is a mnemonic acronym for:
  
* [Singleton](#singleton)
* [Tight Coupling](#tight-coupling)
* [Untestability](#untestability)
* [Premature Optimization](#premature-optimization)
* [Indescriptive Naming](#indescriptive-naming)
* [Duplication](#duplication)


## Singleton

* Programs that use global state are very difficult to test;
* Programs that depend on global status hide their dependencies.
* Often you can replace using a singleton with something better.
* Avoiding everything static is very important to prevent strong coupling.

## Tight Coupling
* Tight coupling is a generalization of the singleton problem.
* Coupling is a measure of how related routines or modules are.
* If making a change in one module in your application requires you to change another module, then coupling exists. For example, you instantiate objects in the class of your constructor instead of passing instances as parameters. This is bad because it does not allow further changes, such as replacing an instance with an instance of a subclass, a mock object, or whatever.
* Strongly coupled modules are difficult to reuse, and also difficult to test.
 
## Untestability

In most cases, the impossibility of testing is caused by a tight coupling.

## Premature Optimization 

Donald Ervin Knuth said: 
<blockquote>
  <p>
Premature optimization is the root of all evil. Only costs alone, and no good. 
</p>
</blockquote>
Optimized systems are much more complex than just writing a loop or using pre-increment instead of post-increment. You will end up with unreadable code. That is why system optimization is much harder.

There are two rules for optimizing an application: Do not do this; (only for professionals!) do not optimize this yet.

## Indescriptive Naming

* Name your classes, methods, attributes, and variables appropriately.
* Do not abbreviate them!
* Write code for people, not for machines.
* Computers only understand 0 and 1.

## Duplication

Duplicated code is inefficient.

## Summary

In this post, I tried to briefly and clearly explain what principles you should avoid when writing code.







