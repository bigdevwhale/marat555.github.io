---
layout: post
title: "SOLID principles on examples."
tags:
- Programming
- Design patterns
- Generatng objects
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

* Generating Objects
  * [The Singleton Pattern](#the-singleton-pattern)
  * [Factory Method Pattern](#factory-method-pattern)
  * [Abstract Factory Pattern](#abstract-factory-pattern)
  * [Prototype](#prototype)
  * [Service Locator](#service-locator)
  * [Dependency Injection](#dependency-injection)
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
* [Database Patterns](https://it.badykov.com/blog/2018/10/28/database-patterns/){:target="_blank"}
  * [Data Mapper](https://it.badykov.com/blog/2018/10/28/database-patterns/#data-mapper){:target="_blank"}
  * [Identity map](https://it.badykov.com/blog/2018/10/28/database-patterns/#identity-map){:target="_blank"}
  * [Unit of Work](https://it.badykov.com/blog/2018/10/28/database-patterns/#unit-of-work){:target="_blank"}
  * [Lazy Load](https://it.badykov.com/blog/2018/10/28/database-patterns/#lazy-load){:target="_blank"}
  * [Domain Object Factory](https://it.badykov.com/blog/2018/10/28/database-patterns/#domain-object-factory){:target="_blank"}
  * [Identity Object](https://it.badykov.com/blog/2018/10/28/database-patterns/#identity-object){:target="_blank"}
  * [Domain Object Assembler](https://it.badykov.com/blog/2018/10/28/database-patterns/#domain-object-assembler){:target="_blank"}
  
## The Singleton Pattern 

<blockquote>
  <p>
  The singleton pattern is a software design pattern that restricts the instantiation of a class to one object.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/design-patterns/singleton-uml-diagram.png" caption="Class diagram exemplifying the singleton pattern" %}

#### Purpose
 A special class that generates one—and only one—object instance.
 
#### Example

Imagine a Settings class that holds application-level information. 

* A Settings object should be available to any object in your system.
* A Settings object should not be stored in a global variable, which can be
overwritten.
* There should be no more than one Preferences object in play in the system. 

#### Implementation 

{% highlight php %}
class Settings
{
    private $props = [];
    private static $instance;
    
    private function __construct()
    {
    }
    public static function getInstance()
    {
        if (empty(self::$instance)) {
            self::$instance = new Preferences();
        }
        return self::$instance;
    }
    public function setProperty(string $key, string $val)
    {
        $this->props[$key] = $val;
    }
    public function getProperty(string $key): string
    {
        return $this->props[$key];
    }
}
{% endhighlight %}

## Factory Method Pattern

<blockquote>
  <p>
  Factory method pattern is a creational pattern that uses factory methods to deal with the problem of creating objects without having to specify the exact class of the object that will be created.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/design-patterns/factory-method.png" caption="Class diagram exemplifying the factory method pattern" %}

#### Purpose

Building an inheritance hierarchy of creator classes.

#### Example

You need to create a variety of products of certain kinds.

#### Implementation 

{% highlight php %}

abstract class Product
{
    abstract public function getName(): string;
    abstract public function getSize(): int;
}

class ConcreteProduct extends Product
{
    protected $name;
    protected $size;
    
    public function __construct(string $name, int $size)
    {
        $this->name = $name;
        $this->size = $size;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function getSize(): int
    {
        return $this->size;
    }
}

abstract class ProductManager
{
    abstract public function getProduct($name, $size): Product;
}

class ConcreteProductManager extends ProductManager
{
    public function getProduct($name, $size): Product
    {
       return new ConcreteProduct($name, $size);
    }
}
{% endhighlight %}


## Abstract Factory Pattern

<blockquote>
  <p>
  The abstract factory pattern provides a way to encapsulate a group of individual factories that have a common theme without specifying their concrete classes.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/design-patterns/abstract-factory.png" caption="Class diagram exemplifying the abstract factory pattern" %}

#### Purpose

Grouping the creation of functionally related products

#### Example

You need to create a family of products of different types.

#### Implementation 

{% highlight php %}
class ProductA1 extends ProductA{...}
class ProductB1 extends ProductB{...}
class ProductA2 extends ProductA{...}
class ProductB2 extends ProductB{...}

abstract class ProductManager
{
   abstract function createProductA();
   abstract function createProductB();
}

class Product1Manager extends ProductManager
{
   public function createProductA() {
       return new ProductA1();
   }

    public function createProductB() {
        return new ProductB1();
    }
}

class Product2Manager extends ProductManager
{
    public function createProductA() {
        return new ProductA2();
    }

    public function createProductB() {
        return new ProductB2();
    }
}
{% endhighlight %}

## Prototype

<blockquote>
  <p>
   Prototype pattern is used when the type of objects to create is determined by a prototypical instance, which is cloned to produce new objects.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/design-patterns/prototype.png" caption="Class diagram exemplifying the prototype pattern" %}

#### Purpose

Using clone to generate objects.

#### Example

You need to create an object that is similar to an existing object.

#### Implementation 

{% highlight php %}
class Product
{
    protected $name;
    protected $size;
    
    public function __construct(string $name, int $size)
    {
        $this->name = $name;
        $this->size = $size;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function getSize(): int
    {
        return $this->size;
    }
}

$product = new Product('Nike', 56);
echo $product->getName(); // Nike
echo $product->getSize(); //56

// Clone and modify what is required
$cloned = clone $product;
$cloned->setName('Adidas');
echo $cloned->getName(); // Adidas
echo $cloned->getSize(); // 56
{% endhighlight %}
## Service Locator
<blockquote>
  <p>
  The service locator pattern is a design pattern used in software development to encapsulate the processes involved in obtaining a service with a strong abstraction layer.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/design-patterns/service-locator.jpg" caption="Diagram exemplifying the service locator pattern" %}

#### Purpose

Asking your system for objects. To implement a loosely coupled architecture in order to get better testable, maintainable and extendable code.

#### Implementation 

{% highlight php %}
class Settings
{
    static $DbType = 'PostgreSql';
}

class Foo
{
    protected $db;
    public function __construct()
    {
        $this->db = Settings::$DbType;
    }
}
{% endhighlight %}
## Dependency Injection

<blockquote>
  <p>
   Dependency injection is a technique whereby one object (or static method) supplies the dependencies of another object.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/design-patterns/dependency-injection.png" caption="Diagram exemplifying the dependency injection pattern" %}

#### Purpose

Letting your system give you objects. To implement a loosely coupled architecture in order to get better testable, maintainable and extendable code.

#### Implementation 

{% highlight php %}
class Computer
{
    protected $hardDrive;
    
    public function __construct($hardDrive)
    {
        $this->hardDrive = $hardDrive();
    }
}

$hardDrive = new HardDrive();
$computer = new Computer($hardDrive);
{% endhighlight %}

## Summary

This post covered some of the tricks that you can use to generate objects. 








