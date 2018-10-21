---
layout: post
title: "Design patterns. Short and clear. Part 3: Performing and Representing Tasks."
tags:
- Programming
- Design patterns
- Performing and Representing Tasks
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

## Introduction

In this post, we get active. We look at patterns that help us to get things done, whether interpreting a mini-language or encapsulating an algorithm.

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
* Performing and Representing Tasks
  * [The Interpreter pattern](#the-interpreter-pattern)
  * [The Strategy pattern](#the-strategy-pattern)
  * [The Observer pattern](#the-observer-pattern)
  * [The Visitor pattern](#the-visitor-pattern)
  * [The Command pattern](#the-command-pattern)
  * [The Null Object pattern](#the-null-object-pattern)
* Enterprise Patterns
* Database Patterns
  
## The Interpreter pattern 

<blockquote>
  <p>
 The interpreter pattern is a design pattern that specifies how to evaluate sentences in a language. 
 The basic idea is to have a class for each symbol (terminal or nonterminal) in a specialized computer language. The syntax tree of a sentence in the language is an instance of the composite pattern and is used to evaluate (interpret) the sentence for a client.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/performing-and-representing-tasks/interpreter.png" caption="Class diagram exemplifying the Interpreter pattern" %}

#### Purpose
 Building a mini-language interpreter that can be used to create scriptable applications.
 
#### Example 

Unit converter.

#### Implementation 

{% highlight php %}
<?php
interface Converter
{
    public function show();
}
// === A Simple Converter ===
// That convert Gallon to Litre
class GallonToLitre implements Converter
{
    private $gallon;
    public function __construct($gallon)
    {
        $this->gallon = $gallon;
    }
    public function show()
    {
        return round($this->gallon * 3.79);
    }
}
// === A Simple Converter ===
// That convert Mile to Kilometer
class MileToKilometer implements Converter
{
    private $mile;
    public function __construct($mile)
    {
        $this->mile = $mile;
    }
    public function show()
    {
        return round($this->mile * 1.6);
    }
}
// === An Intepreter ===
// That interpret to Kilometer per Litre by using existing converters.
// Consider this as structuring a sentence using grammers (converters).
class MpgToKml implements Converter
{
    private $g2l;
    private $m2k;
    public function __construct(Converter $g2l, Converter $m2k)
    {
        $this->g2l = $g2l;
        $this->m2k = $m2k;
    }
    public function show()
    {
        echo  $this->g2l->show() . "l/" . $this->m2k->show() . "k\n";
    }
}

$mpg2kml = new MpgToKml(new GallonToLitre(1), new MileToKilometer(20));
$mpg2kml->show();
// Output: 4l/32k
{% endhighlight %}

## The Strategy pattern

<blockquote>
  <p>
   The strategy pattern (also known as the policy pattern) is a software design pattern that enables selecting an algorithm at runtime. 
   Instead of implementing a single algorithm directly, code receives run-time instructions as to which in a family of algorithms to use.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/performing-and-representing-tasks/strategy.png" caption="Class diagram exemplifying the Strategy pattern" %}

#### Purpose

 Identifying algorithms in a system and encapsulating them into their own types.
 
####  Example

Car selection algorithm based on the day of the week.

#### Implementation example

{% highlight php %}
<?php
interface Car
{
    public function pick();
}
class Fit implements Car
{
    public function pick()
    {
        echo "Picking Fit for today.\n";
    }
}
class Vitz implements Car
{
    public function pick()
    {
        echo "Picking Vitz for today.\n";
    }
}
// === CarPickerStrategy ===
// That decide which car object to use based on the situation
// The benefit is that we can add more strategies later,
// clients doesn't have to know or change their implementation
class CarPickerStrategy
{
    public $today;
    public function __construct($today)
    {
        $this->today = $today;
    }
    public function pick()
    {
        if( $this->today == "Monday" ) {
            $car = new Vitz();
        } else {
            $car = new Fit();
        }
        $car->pick();
    }
}
// ---
$carpicker = new CarPickerStrategy("Sunday");
$carpicker->pick();
// Output: Picking Fit for today.
$carpicker->today = "Monday";
$carpicker->pick();
// Output: Picking Vitz for today.
{% endhighlight %}


## The Observer pattern

<blockquote>
  <p>
  The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/performing-and-representing-tasks/observer.png" caption="Class diagram exemplifying the Observer design pattern." %}

#### Purpose

 Creating hooks for alerting disparate objects about system events.
 
#### Example
 
 You have a list. When there is a change, you need invoke update() on each element in a list.

#### Implementation 

{% highlight php %}
<?php
// === Subject ===
// Have a observer list. When there is a change,
// iterate through observer list and invoke update()
// on each observer.
class Car
{
    protected $phones = []; // observer list
    protected $status;
    public function attach(Phone $phone)
    {
        $this->phones[] = $phone;
    }
    public function getStatus()
    {
        return $this->status;
    }
    public function setStatus($status)
    {
        $this->status = $status;
        $this->notify();
    }
    public function notify() {
        foreach($this->phones as $phone) {
            $phone->update();
        }
    }
}
// === Observer ===
// Which add itself to observer-list of subject,
// by invoking attach() method of the subject.
class Phone
{
    private $car;
    public $name;
    public function __construct(Car $car, $name)
    {
        $this->name = $name;
        $this->car = $car;
        $this->car->attach($this);
    }
    public function update()
    {
        echo $this->name . " - Car is " . $this->car->getStatus() . "\n";
    }
}
// ---
$car = new Car();
$iphone = new Phone( $car, "iPhone" );
$nexus = new Phone( $car, "Nexus" );
$car->setStatus("driving");
// Output:
// iPhone - Car is driving
// Nexus - Car is driving
{% endhighlight %}

## The Visitor pattern

<blockquote>
  <p>
   The visitor design pattern is a way of separating an algorithm from an object structure on which it operates. A practical result of this separation is the ability to add new operations to existent object structures without modifying the structures. 
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/performing-and-representing-tasks/visitor.png" caption="Class diagram exemplifying the Visitor design pattern." %}

#### Example

 Applying an operation to all the nodes in a tree of objects.
 

#### Implementation 

{% highlight php %}
<?php
class Car
{
    public $color = "Silver";
    // accepting Enhancer as visitor
    public function visitor(Enhancer $enhancer)
    {
        $this->color = $enhancer->enhance( $this );
    }
}
// === A visitor ===
// That can visit into Car object change it
// The benefit is, Car object is now seperated from
// detail algorithm of how it can be enhanced
class Enhancer
{
    // visiting into Car
    public function enhance(Car $car)
    {
        return "Matelic $car->color";
    }
}
// ---
$car = new Car();
echo $car->color . "\n";
// Output: Silver
$car->visitor(new Enhancer());
echo $car->color . "\n";
// Output: Matelic Silver
{% endhighlight %}

## The Command pattern

<blockquote>
  <p>
  The command pattern is a software design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/performing-and-representing-tasks/command.png" caption="A sample UML class diagram for the Command design pattern." %}

#### Purpose

 Creating a simple interface to complex or variable systems.
 
#### Example
 
On and off the lamp.

#### Implementation 

{% highlight php %}
<?php
class Lamp
{
    public function open()
    {
        echo "Lamp is opened.\n";
    }
    public function close()
    {
        echo "Lamp is closed.\n";
    }
}
interface Button
{
    public function execute();
}
// === A command that can be executed on Lamp ===
class SwitchUp implements Button
{
    public $lamp;
    public function __construct(Lamp $lamp)
    {
        $this->lamp = $lamp;
    }
    public function execute()
    {
        $this->lamp->open();
    }
}
// === A command that can be executed on Lamp ===
class SwitchDown implements Button
{
    public $lamp;
    public function __construct(Lamp $lamp)
    {
        $this->lamp = $lamp;
    }
    public function execute()
    {
        $this->lamp->close();
    }
}
// === A Collection ===
// That accept and execute commands
class Commands
{
    public $commands = [];
    public function add(Button $button)
    {
        $commands[] = $button;
        $button->execute();
    }
}
// ---
$lamp = new Lamp();
$up = new SwitchUp($lamp);
$down = new SwitchDown($lamp);
$commands = new Commands();
$commands->add($up);
// Output: Lamp is opened.
$commands->add($down);
// Output: Lamp is closed.
{% endhighlight %}

## The Null Object pattern

<blockquote>
  <p>
  In object-oriented computer programming, a null object is an object with no referenced value or with defined neutral ("null") behavior. The null object design pattern describes the uses of such objects and their behavior (or lack thereof). 
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/performing-and-representing-tasks/null-object.png" caption="Class diagram exemplifying The Null Object design pattern." %}

#### Example

 Using non-operational objects in place of null values.

#### Implementation 

{% highlight php %}
<?php
interface Car
{
    public function drive();
}
class Fit implements Car
{
    public function drive()
    {
        echo "Driving Fit...\n";
    }
}
class Vitz implements Car
{
    public function drive()
    {
        echo "Driving Vitz...\n";
    }
}
// === Null implementation ===
// A blank implementation similar to real one
class NullCar implements Car
{
    public function drive()
    {
        //
    }
}
$name = null;
switch($name) {
    case "Fit":
        $car = new Fit(); break;
    case "Vitz":
        $car = new Vitz(); break;
    default:
        $car = new NullCar();
}
// ---
$car->drive();
// Output: [nothing]
// We avoided `Call to a memeber function on null` error here.
// Without NullCar object the code will look like following
// to avoid such error.
//
// if( isset($car) and $car instanceof Car ) {
//     $car->drive();
// }
{% endhighlight %}

## Summary

In this post, we looked at patterns that help us to get things done. 








