---
layout: post
title: "Design patterns. Short and clear. Part 2: Flexible Object Programming."
tags:
- Programming
- Design patterns
- Flexible object programming
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
* Patterns for Flexible Object Programming
  * [The Composite Pattern](#the-composite-pattern)
  * [The Decorator Pattern](#the-decorator-pattern)
  * [The Facade Pattern](#the-facade-pattern)
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
  
## The Composite Pattern 

<blockquote>
  <p>
 The composite pattern describes a group of objects that is treated the same way as a single instance of the same type of object. 
 The intent of a composite is to "compose" objects into tree structures to represent part-whole hierarchies. Implementing the composite pattern lets clients treat individual objects and compositions uniformly.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/flexible-object-programming/composite-pattern-uml-diagram.png" caption="Class diagram exemplifying the composite pattern" %}

#### Purpose
 Composing structures in which groups of objects can be used as if they were individual objects.

#### Implementation 

{% highlight php %}
class ComponentException extends Exception {};
abstract class Component
{
    protected $_children = array();
    abstract public function Add(Component $Component);
    abstract public function Remove($index);
    abstract public function GetChild($index);
    abstract public function GetChildren();
    abstract public function Operation();
}
class Composite extends Component
{
    public function Add(Component $Component)
    {
        $this -> _children[] = $Component;
    }
    public function GetChild($index)
    {
        if (! isset($this -> _children[$index]))
        {
            throw new ComponentException("Child not exists");
        }
        return $this -> _children[$index];
    }
    public function Operation()
    {
        print "I am composite. I have " . count($this -> GetChildren()) . " children\n";
        foreach($this -> GetChildren() as $Child)
        {
            $Child -> Operation();
        }
    }
    public function Remove($index)
    {
        if (! isset($this -> _children[$index]))
        {
            throw new ComponentException("Child not exists");
        }
        unset($this -> _children[$index]);
    }
    public function GetChildren()
    {
        return $this -> _children;
    }
}
class Leaf extends Component
{
    public function Add(Component $Component)
    {
        throw new ComponentException("I can't append child to myself");
    }
    public function GetChild($index)
    {
        throw new ComponentException("Child not exists");
    }
    public function Operation()
    {
        print "I am leaf\n";
    }
    public function Remove($index)
    {
        throw new ComponentException("Child not exists");
    }
    public function GetChildren()
    {
        return array();
    }
}
{% endhighlight %}

## The Decorator Pattern

<blockquote>
  <p>
   The decorator pattern is a design pattern that allows behavior to be added to an individual object, dynamically, without affecting the behavior of other objects from the same class.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/flexible-object-programming/decorator-pattern-diagram.png" caption="Class diagram exemplifying the decorator pattern" %}

#### Purpose

 A flexible mechanism for combining objects at runtime to extend functionality.

#### Implementation 

{% highlight php %}
abstract class Component
{
    abstract public function Operation();
}
class ConcreteComponent extends Component
{
    public function Operation()
    {
        return 'I am component';
    }
}
abstract class Decorator extends Component
{
    protected
        $_component = null;
    public function __construct(Component $component)
    {
        $this -> _component = $component;
    }
    protected function getComponent()
    {
        return $this -> _component;
    }
    public function Operation()
    {
        return $this -> getComponent() -> Operation();
    }
}
class ConcreteDecoratorA extends Decorator
{
    public function Operation()
    {
        return '<a>' . parent::Operation() . '</a>';
    }
}
class ConcreteDecoratorB extends Decorator
{
    public function Operation()
    {
        return '<strong>' . parent::Operation() . '</strong>';
    }
}
// Example
$Element = new ConcreteComponent();
$ExtendedElement = new ConcreteDecoratorA($Element);
$SuperExtendedElement = new ConcreteDecoratorB($ExtendedElement);

{% endhighlight %}


## The Facade Pattern

<blockquote>
  <p>
  The facade pattern is a software-design pattern commonly used with object-oriented programming. Analogous to a facade in architecture, a facade is an object that serves as a front-facing interface masking more complex underlying or structural code.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/flexible-object-programming/facade-pattern-uml.jpg" caption="A sample UML class and sequence diagram for the Facade design pattern." %}

#### Purpose

 Creating a simple interface to complex or variable systems.


#### Implementation 

In this implementation, the creation and initialization of subsystems are hard wired into the code; this can be avoided by entering the [factory pattern](https://it.badykov.com/blog/2018/10/07/generating-objects/#factory-method-pattern) or using a 
parameterized constructor for the facade.
{% highlight php %}
/**
 * SystemA
 */
class Bank
{
    public function OpenTransaction() {}
    public function CloseTransaction() {}
    public function transferMoney($amount) {}
}
/**
 * SystemB
 */
class Client
{
    public function OpenTransaction() {}
    public function CloseTransaction() {}
    public function transferMoney($amount) {}
}
/**
 * SystemC
 */
class Log
{
    public function logTransaction() {}
}
class Facade
{
    public function transfer($amount)
    {
        $Bank = new Bank();
        $Client = new Client();
        $Log = new Log();
        $Bank->OpenTransaction();
        $Client->OpenTransaction();
        $Log->logTransaction('Transaction open');
        $Bank->transferMoney(-$amount);
        $Log->logTransaction('Transfer money from bank');
        $Client->transferMoney($amount);
        $Log->logTransaction('Transfer money to client');
        $Bank->CloseTransaction();
        $Client->CloseTransaction();
        $Log->logTransaction('Transaction close');
    }
}
// Client code
$Transfer = new Facade();
$Transfer->transfer(1000);
{% endhighlight %}

## Summary

In this post, I looked at a few of the ways that classes and objects can be organized in a system. 








