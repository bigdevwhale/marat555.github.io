---
layout: post
title: "Design patterns. Short and clear. Part 5: Database Patterns."
tags:
- Programming
- Design patterns
- Database patterns
thumbnail_path: blog/thumbs/design-patterns.png
add_to_popular_list: true
---

Design patterns are guidelines for solving repetitive problems.

{% include figure.html path=page.thumbnail_path %}
[Quote](https://en.wikipedia.org/wiki/Software_design_pattern) from Wikipedia:
<blockquote>
  <p>
  Software design pattern is a general, reusable solution to a commonly occurring problem within a given context in software design.
  </p>
</blockquote>

## Types of Design Patterns

* [Generating Objects](https://it.badykov.com/blog/2018/10/07/generating-objects){:target="_blank"}
  * [The Singleton Pattern](https://it.badykov.com/blog/2018/10/07/generating-objects/#the-singleton-pattern){:target="_blank"}
  * [Factory Method Pattern](https://it.badykov.com/blog/2018/10/07/generating-objects/#factory-method-pattern){:target="_blank"}
  * [Abstract Factory Pattern](https://it.badykov.com/blog/2018/10/07/generating-objects/#abstract-factory-pattern){:target="_blank"}
  * [Prototype](https://it.badykov.com/blog/2018/10/07/generating-objects/#prototype){:target="_blank"}
  * [Service Locator](https://it.badykov.com/blog/2018/10/07/generating-objects/#service-locator){:target="_blank"}
  * [Dependency Injection](https://it.badykov.com/blog/2018/10/07/generating-objects/#dependency-injection){:target="_blank"}
* [Patterns for Flexible Object Programming](https://it.badykov.com/blog/2018/10/14/flexible-object-programming){:target="_blank"}
  * [The Composite Pattern](https://it.badykov.com/blog/2018/10/14/flexible-object-programming/#the-composite-pattern){:target="_blank"}
  * [The Decorator Pattern](https://it.badykov.com/blog/2018/10/14/flexible-object-programming/#the-decorator-pattern){:target="_blank"}
  * [The Facade Pattern](https://it.badykov.com/blog/2018/10/14/flexible-object-programming/#the-facade-pattern){:target="_blank"}
* [Performing and Representing Tasks](https://it.badykov.com/blog/2018/10/21/performing-and-representing-tasks){:target="_blank"}
  * [The Interpreter pattern](https://it.badykov.com/blog/2018/10/21/performing-and-representing-tasks/#the-interpreter-pattern){:target="_blank"}
  * [The Strategy pattern](https://it.badykov.com/blog/2018/10/21/performing-and-representing-tasks/#the-strategy-pattern){:target="_blank"}
  * [The Observer pattern](https://it.badykov.com/blog/2018/10/21/performing-and-representing-tasks/#the-observer-pattern){:target="_blank"}
  * [The Visitor pattern](https://it.badykov.com/blog/2018/10/21/performing-and-representing-tasks/#the-visitor-pattern){:target="_blank"}
  * [The Command pattern](https://it.badykov.com/blog/2018/10/21/performing-and-representing-tasks/#the-command-pattern){:target="_blank"}
  * [The Null Object pattern](https://it.badykov.com/blog/2018/10/21/performing-and-representing-tasks/#the-null-object-pattern){:target="_blank"}
* [Enterprise Patterns](https://it.badykov.com/blog/2018/10/28/enterprise-patterns/){:target="_blank"}
  * [Front Controller](https://it.badykov.com/blog/2018/10/28/enterprise-patterns/#front-controller){:target="_blank"}
  * [Application Controller](https://it.badykov.com/blog/2018/10/28/enterprise-patterns/#application-controller){:target="_blank"}
  * [Template View](https://it.badykov.com/blog/2018/10/28/enterprise-patterns/#template-view){:target="_blank"}
  * [Page Controller](https://it.badykov.com/blog/2018/10/28/enterprise-patterns/#page-controller){:target="_blank"}
  * [Transaction Script](https://it.badykov.com/blog/2018/10/28/enterprise-patterns/#transaction-script){:target="_blank"}
  * [Domain Model](https://it.badykov.com/blog/2018/10/28/enterprise-patterns/#domain-model){:target="_blank"}
* Database Patterns
  * [Data Mapper](#data-mapper)
  * [Identity map](#identity-map)
  * [Unit of Work](#unit-of-work)
  * [Lazy Load](#lazy-load)
  * [Domain Object Factory](#domain-object-factory)
  * [Identity Object](#identity-object)
  * [Domain Object Assembler](#domain-object-assembler)
  
## Data Mapper

<blockquote>
  <p>
 The data mapper pattern is an architectural pattern. It was named by Martin Fowler in his 2003 book Patterns of Enterprise Application Architecture. The interface of an object conforming to this pattern would include functions such as Create, Read, Update, and Delete, that operate on objects that represent domain entity types in a data store.  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/database-patterns/datamapper.png" caption="Class diagram exemplifying the Data Mapper pattern" %}

#### Purpose

 Create specialist classes for mapping Domain Model objects to and from relational databases.
 
#### Example

{% highlight php %}
<?php
class NewsMapper
{
    ....
    public function save(News $news) { ... }
    public function getById($id) { ... }
    public function findLatestNews() { ... }
}

$mapper = new NewsMapper($pdo);
$someNews = $mapper->getById(10);
$someNews->title = 'New title'; 
$mapper->save($someNews);

$newNews = new News();
$newNews->title = 'News title!';
$newNews->text = 'Text of news';
$mapper->save($newNews);
{% endhighlight %}


## Identity Map

<blockquote>
  <p>
 The identity map pattern is a database access design pattern used to improve performance by providing a context-specific, in-memory cache to prevent duplicate retrieval of the same object data from the database.  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/database-patterns/identitymap-sequence-diagram.png" caption="Class diagram exemplifying the Identity Map pattern" %}

#### Purpose

Keep track of all the objects in your system to prevent duplicate instantiations and unnecessary trips to the database.

#### Example 

By using Data-Mapper pattern without an identity map, you can easily run into problems because you may have more than one object that references the same domain entity.
The identity map solves this problem by acting as a registry for all loaded domain instances.
{% highlight php %}
   $userMapper = new UserMapper($pdo);
 
   $user1 = $userMapper->find(1); // creates new object
   $user2 = $userMapper->find(1); // returns same object
 
   echo $user1->getNickname(); // joe123
   echo $user2->getNickname(); // joe123
 
   $user1->setNickname('bob78');
 
   echo $user1->getNickname(); // bob78
   echo $user2->getNickname(); // bob78 -> yes, much better
{% endhighlight %}

## Unit of Work

<blockquote>
  <p>
 The Unit of Work pattern is used to group one or more operations (usually database operations) into a single transaction or “unit of work”, so that all operations either pass or fail as one.  </p>
</blockquote>

#### Purpose

 Automate the process by which objects are saved to the database, ensuring that only objects that have been changed are updated, and only those that have been newly created are inserted.

#### Example 
A lightweight interface of a UOW might look like this:

{% highlight php %}
<?php
namespace ModelRepository;
use ModelEntityInterface;

interface UnitOfWorkInterface
{
    public function fetchById($id);
    public function registerNew(EntityInterface $entity);
    public function registerClean(EntityInterface $entity);
    public function registerDirty(EntityInterface $entity);
    public function registerDeleted(EntityInterface $entity);
    public function commit();
    public function rollback();
    public function clear();
}

{% endhighlight %}

## Lazy Load

<blockquote>
  <p>
  Lazy loading is a design pattern commonly used in computer programming to defer initialization of an object until the point at which it is needed. It can contribute to efficiency in the program's operation if properly and appropriately used. The opposite of lazy loading is eager loading. 
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/database-patterns/lazy-load.gif" caption="Class diagram exemplifying the Lazy Load pattern" %}

#### Purpose

Defer object creation, and even database queries, until they are actually needed.

## Domain Object Factory

<blockquote>
  <p>
  Lazy loading is a design pattern commonly used in computer programming to defer initialization of an object until the point at which it is needed. It can contribute to efficiency in the program's operation if properly and appropriately used. The opposite of lazy loading is eager loading. 
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/database-patterns/DomainObjectFactory_Seq.gif" caption="Class diagram exemplifying the Domain Object Factory pattern" %}

#### Purpose

Populates domain objects based on query results.

## Identity Object

<blockquote>
  <p>
  Object identity is a fundamental object orientation concept. With object identity, objects can contain or refer to other objects. Identity is a property of an object that distinguishes the object from all other objects in the application.
  </p>
</blockquote>

#### Purpose

Encapsulate the logic for constructing SQL queries. Allow clients to construct query criteria without reference to the underlying database. 

## Domain Object Assembler

<blockquote>
  <p>
  Domain Object Assembler constructs a controller that manages the high-level process of data storage and retrieval.
  </p>
</blockquote>

#### Purpose

Populates, persists, and deletes domain objects using a uniform factory framework.

## Summary

In this post, we looked at the following database patterns:

* Data Mapper
* Identity map
* Unit of Work
* Lazy Load
* Domain Object Factory
* Identity Object
* Domain Object Assembler






