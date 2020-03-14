---
layout: post
title: "SOLID principles. How to create maintainable code."
tags:
- Programming
- Good practice
- SOLID
thumbnail_path: blog/thumbs/solid.png
add_to_popular_list: true
---

In a previous post, we looked at principles to avoid when writing code. You can read it here. In this post, we will look 
at SOLID design principles, programming standards that all developers must understand well to create good architecture.

{% include figure.html path=page.thumbnail_path %}

  SOLID is a mnemonic acronym for:
  
* [Single Responsibility](#single-responsibility)
* [Open-closed](#open-closed)
* [Liskov Substitution](#liskov-substitution)
* [Interface Segregation](#interface-segregation)
* [Dependency Inversion](#dependency-inversion)


## Single Responsibility

* Each object should be assigned one single responsibility.
* A specific class should solve a specific problem.

#### STUPID example

{% highlight php %}
class Order
{
    public function calculateTotalSum() {...}
    public function getItems() {...}
    public function getItemCount() {...}
    public function addItem() {...}
    public function deleteItem() {...}

    public function load() {...}
    public function save() {...}
    public function update() {...}
    public function delete() {...}

    public function printOrder() {...}
    public function showOrder() {...}
}
{% endhighlight %}

#### SOLID example

{% highlight php %}
class Order
{
    public function calculateTotalSum() {...}
    public function getItems() {...}
    public function getItemCount() {...}
    public function addItem() {...}
    public function deleteItem() {...}
}

class OrderRepository
{
    public function load() {...}
    public function save() {...}
    public function update() {...}
    public function delete() {...}
}

class OrderViewer
{
    public function printOrder() {...}
    public function showOrder() {...}
}
{% endhighlight %}

## Open-closed
* Software entities must be open for expansion but closed for modification.
* All classes, functions, etc. should be designed so that to change their behavior, it is not necessary to change their source code.

#### STUPID example

{% highlight php %}
class OrderRepository
{
    public function load($orderId)
    {
        $pdo = new PDO($this->config->getDsn(), $this->config->getDBUser(), $this->config->getDBPassword());
        $statement = $pdo->prepare('SELECT * FROM `orders` WHERE id=:id');
        $statement->execute(array(':id' => $orderId));
        return $query->fetchObject('Order');
    }
    public function save($order) {...}
    public function update($order) {...}
    public function delete($order) {...}
}
{% endhighlight %}

#### SOLID example

{% highlight php %}
{
    private $source;
    
    public function setSource(IOrderSource $source)
    {
        $this->source = $source;
    }
    
    public function load($orderId)
    {
        return $this->source->load($orderId);
    }

    public function save($order)
    {
        return $this->source->save($order);
    }

    public function update($order) {...};
    public function delete($order) {...};
}

interface IOrderSource
{
    public function load($orderId);
    public function save($order);
    public function update($order);
    public function delete($order);
}

class MySQLOrderSource implements IOrderSource
{
    public function load($orderId) {...};
    public function save($order) {...}
    public function update($order) {...}
    public function delete($order) {...}
}

class ApiOrderSource implements IOrderSource
{
    public function load($orderId) {...};
    public function save($order) {...}
    public function update($order) {...}
    public function delete($order) {...}
}
{% endhighlight %}
 
## Liskov substitution

* Objects in the program can be replaced by their inheritor without changing the properties of the program.
* When using the class inheritor, the result of code execution should be predictable and not change the properties of the method.
* It should be possible to substitute any subtype of the base type.
* Functions that use references to base classes should be able to use objects of derived classes without knowing it.

#### STUPID example

{% highlight php %}
class Rectangle
{
    protected $width;
    protected $height;

    public setWidth($width)
    {
        $this->width = $width;
    }

    public setHeight($height)
    {
        $this->height = $height;
    }

    public function getWidth()
    {
        return $this->width;
    }

    public function getHeight()
    {
        return $this->height;
    }
}

class Square extends Rectangle
{
    public setWidth($width)
    {
        parent::setWidth($width);
        parent::setHeight($width);
    }

    public setHeight($height)
    {
        parent::setHeight($height);
        parent::setWidth($height);
    }
}

function calculateRectangleSquare(Rectangle $rectangle, $width, $height)
{
    $rectangle->setWidth($width);
    $rectangle->setHeight($height);
    return $rectangle->getHeight * $rectangle->getWidth;
}

calculateRectangleSquare(new Rectangle, 4, 5); // 20
calculateRectangleSquare(new Square, 4, 5); // 25 ???
{% endhighlight %}

#### SOLID example

{% highlight php %}
class Rectangle
{
    protected $width;
    protected $height;

    public setWidth($width)
    {
        $this->width = $width;
    }

    public setHeight($height)
    {
        $this->height = $height;
    }

    public function getWidth()
    {
        return $this->width;
    }

    public function getHeight()
    {
        return $this->height;
    }
}

class Square
{
    protected $size;

    public setSize($size)
    {
        $this->size = $size;
    }

    public function getSize()
    {
        return $this->size;
    }
}
{% endhighlight %}


## Interface Segregation

* Many specialized interfaces are better than one universal.
* Compliance with this principle is necessary so that client classes using / implementing the interface are aware only of the methods that they use, which leads to a reduction in the amount of unused code.

#### STUPID example

{% highlight php %}
interface IItem
{
    public function applyDiscount($discount);
    public function applyPromocode($promocode);
    
    public function setColor($color);
    public function setSize($size);
    
    public function setCondition($condition);
    public function setPrice($price);
}
{% endhighlight %}

#### SOLID example

{% highlight php %}
interface IItem
{
    public function setCondition($condition);
    public function setPrice($price);
}

interface IClothes
{
    public function setColor($color);
    public function setSize($size);
    public function setMaterial($material);
}

interface IDiscountable
{
    public function applyDiscount($discount);
    public function applyPromocode($promocode);
}
{% endhighlight %}

## Dependency Inversion

* Dependencies within the system are based on abstractions. 
* Top-level modules are independent of lower-level modules. 
* Abstractions should not depend on the details. 
* Details should depend on abstractions.

#### STUPID example

{% highlight php %}
class Customer
{
    private $currentOrder = null;

    public function buyItems()
    {
        if (is_null($this->currentOrder)) {
            return false;
        }

        $processor = new OrderProcessor(); // !!!
        return $processor->checkout($this->currentOrder);
    }
    
    public function addItem($item)
    {
        if (is_null($this->currentOrder)) {
            $this->currentOrder = new Order();
        }

        return $this->currentOrder->addItem($item);
    }

    public function deleteItem($item)
    {
        if (is_null($this->currentOrder)) {
            return false;
        }

        return $this->currentOrder->deleteItem($item);
    }
}

class OrderProcessor
{
    public function checkout($order) {...}
}
{% endhighlight %}

#### SOLID example

{% highlight php %}
class Customer
{
    private $currentOrder = null;

    public function buyItems(IOrderProcessor $processor)
    {
        if (is_null($this->currentOrder)) {
            return false;
        }

        return $processor->checkout($this->currentOrder);
    }

    public function addItem($item){
        if (is_null($this->currentOrder)) {
            $this->currentOrder = new Order();
        }

        return $this->currentOrder->addItem($item);
    }

    public function deleteItem($item) {
        if (is_null($this->currentOrder)) {
            return false;
        }

        return $this->currentOrder->deleteItem($item);
    }
}

interface IOrderProcessor
{
    public function checkout($order);
}

class OrderProcessor implements IOrderProcessor
{
    public function checkout($order) {...}
}
{% endhighlight %}


## Summary

#### Single responsibility principle
<blockquote>
  <p>
One single responsibility should be assigned to each object.” To do this, we check how many reasons we have for changing the class - if more than one, then this class should be broken.
  </p>
</blockquote>

#### The principle of openness / closure (Open-closed)
<blockquote>
  <p>
Software entities must be open for expansion, but closed for modification." To do this, we present our class as a "black box" and see if we can change its behavior in this case.
  </p>
</blockquote>

#### The principle of substitution Barbara Liskov (Liskov substitution)
<blockquote>
  <p>
Objects in the program can be replaced by their heirs without changing the properties of the program." To do this, we check whether we have strengthened the preconditions and have weakened the postconditions. If this happens, then the principle is not respected.
  </p>
</blockquote>

#### Interface segregation
<blockquote>
  <p>
Many specialized interfaces are better than one universal. We check how much the interface contains methods and how different functions are superimposed on these methods, and if necessary, we break the interfaces.
</p>
</blockquote>

#### Dependency Inversion Principle
<blockquote>
  <p>
Dependencies should be built on abstractions, not details.” We check whether the classes depend on some other classes (directly instantiate objects of other classes, etc.) and if this dependence takes place, we replace it with a dependence on abstraction.
</p>
</blockquote>









