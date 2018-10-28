---
layout: post
title: "Design patterns. Short and clear. Part 4: Enterprise Patterns."
tags:
- Programming
- Design patterns
- Enterprise patterns
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
* Enterprise Patterns
  * [Front Controller](#front-controller)
  * [Application Controller](#application-controller)
  * [Template View](#template-view)
  * [Page Controller](#page-controller)
  * [Transaction Script](#transaction-script)
  * [Domain Model](#domain-model)
* Database Patterns
  
## Front Controller

<blockquote>
  <p>
 The front controller design pattern is listed in several pattern catalogs and related to the design of web applications. It is "a controller that handles all requests for a website", which is a useful structure for web application developers to achieve the flexibility and reuse without code redundancy.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/enterprise-patterns/front-controller.gif" caption="Class diagram exemplifying the Front Controller pattern" %}

#### Purpose
 Use this for larger systems in which you know that you will need as much flexibility as possible in managing many different views and commands.

#### Implementation 

{% highlight php %}
<?php
class FrontController
{
    const DEFAULT_ACTION     = "index";
    const DEFAULT_CONTROLLER = "Index";
    const BASE_PATH = '';
    
    protected $controller    = self::DEFAULT_CONTROLLER;
    protected $action        = self::DEFAULT_ACTION;
    protected $params        = array();
    
    public function __construct(array $options = array()) {
        if (empty($options)) {
           $this->parseUri();
        }
        else {
            if (isset($options["controller"])) {
                $this->setController($options["controller"]);
            }
            if (isset($options["action"])) {
                $this->setAction($options["action"]);     
            }
            if (isset($options["params"])) {
                $this->setParams($options["params"]);
            }
        }
    }
    
    protected function parseUri() {
        $path = trim(parse_url($_SERVER["REQUEST_URI"], PHP_URL_PATH), "/");
        if($path === self::BASE_PATH){
        	$this->setController($this->controller);
        	$this->setAction($this->action);
        }else{
			if(self::BASE_PATH != ''){
				$path = trim(str_replace(self::BASE_PATH, "", $path), "/");
			}
        	@list($controller, $action, $params) = explode("/", $path, 3);
        	if (isset($controller)){
        	    $this->setController($controller);
        	}
        	if (isset($action)){
        	    $this->setAction($action);
        	}
        	if (isset($params)){
        	    $this->setParams(explode("/", $params));
        	}
        } 
    }
    
    public function setController($controller) {
        $controller = ucfirst(strtolower($controller)) . "Controller";
        $controller = "Controller\\" . $controller;
        if (!class_exists($controller)) {
        	header("HTTP/1.0 404 Not Found");
        	echo "<h1>404 Not Found</h1>";
        	echo "Página não encontrada. Ir para <a href='/produtos'>Home</a>";
        	exit();
    	}
        $this->controller = $controller;
        return $this;
    }
    
    public function setAction($action) {
        $reflector = new \ReflectionClass($this->controller);
        if (!$reflector->hasMethod($action)) {
            header("HTTP/1.0 404 Not Found");
            echo "<h1>404 Not Found</h1>";
            echo "Página não encontrada. Ir para <a href='/produtos'>Home</a>";
            exit();
    	}
        $this->action = $action;
        return $this;
    }
    
    public function setParams(array $params) {
        $this->params = $params;
        return $this;
    }
    
    public function run() {
        call_user_func_array(array(new $this->controller, $this->action), $this->params);
    }
}
{% endhighlight %}

## Application Controller

<blockquote>
  <p>
   The application controller pattern is the pattern that permits the centralization of all view logic and promotes a unique process to define the flow of pages. 
  </p>
</blockquote>

#### Purpose

  Create a class to manage view logic and command selection.
 
####  Example

Here I'll present a small, high-level sample of an Application Controller that manages a form compilation in multiple steps.

In Zend Framework, it can be thought of as homogeneous to Action Controllers (so that it can be easily put in an existing application's infrastructure.) It usually does not take advantage of automated mechanisms like the ViewRenderer plugin to achieve a finer control. In fact, it does not map to a single domain action or a single view: the view is chosen on the fly and the domain model is seen through a Facade.

#### Implementation

{% highlight php %}
<?php

class ApplicationController extends Zend_Controller_Action
{
    const LAST = 4;

    public function init()
    {
        // ... set up the various resources, like forms or domain entities
    }

    public function executeAction()
    {
        // other view variables...

        // keep partial compilations
        $this->session->merge($this->request->getPost());

        // this is our View change
        $step = $this->_request->getParam('step', 1);
        if ($step == self::LAST) { 
            return $this->render('final.phtml');
        } else {
            $this->view->form = $this->forms[$step];
        }

        // the script is always the same but the form rendered changes
        $this->render('appcontroller.phtml');
    }
}
{% endhighlight %}

## Template View

<blockquote>
  <p>
  In Template View, static HTML pages are defined and markers are used as placeholders within them for dynamic content on the page. When the page is requested, the markers are replaced with the dynamic content (ie, from a database or similar) by the underlying web server or framework.
  </p>
</blockquote>
{% include figure.html path=page.thumbnail_path path="blog/enterprise-patterns/template-view.png" caption="Class diagram exemplifying the Template view pattern." %}

#### Purpose

 Create pages that manage display and user interface only, incorporating dynamic information into display markup with as little raw code as possible.

#### Implementation 

{% highlight php %}
<?php
$this->headTitle($this->object);
$this->headTitle($this->methodName);
if ($this->result) {
    echo "<p>Result: $this->result</p>";
}
if ($this->form) {
    echo $this->form;
}
{% endhighlight %}

## Page Controller

<blockquote>
  <p>
  Page controller is an alternative to front controller in MVC model.
  </p>
</blockquote>

#### Purpose

 Lighter weight but less flexible than Front Controller, Page Controller addresses the same need. Use this pattern to manage requests and handle view logic if you want fast results and your system is unlikely to grow substantially in complexity.
 
#### Example
 
Zend framework Page Controller example

#### Implementation 

{% highlight php %}
class FooController extends Zend_Controller_Action
{
    public function barAction()
    {
        // assign a variable to the view, with the value coming from somewhere
        $this->view->variable1 = ...
        $this->render('viewName'); // executes views/scripts/viewName.php
    }
       
    public function bazAction()
    {
        // other action related to the former
    }
}
{% endhighlight %}

## Transaction Script

<blockquote>
  <p>
  Transaction Script (TS) is the simplest domain logic pattern we can find. It needs less work to implement than other domain logic patterns and therefore it's perfect fit for smaller applications that doesn't need big architecture behind them.
  </p>
</blockquote>

#### Purpose

  When you want to get things done fast, with minimal up-front planning, fall back on procedural library code for your application logic. This pattern does not scale well.
  
#### Implementation 

{% highlight php %}
<?php
// we are forced to mock a request in production code:
$_GET['id'] = $someRow['id];
$_GET['orderField'] = 'name';
include 'list.php';
// or even capturing the response:
ob_start();
$_GET['id'] = $someRow['id];
$_GET['orderField'] = 'name';
include 'list.php';
$content = ob_get_content();
ob_end_clean();
{% endhighlight %}

## Domain Model

<blockquote>
  <p>
 At its worst business logic can be very complex. Rules and logic describe many different cases and slants of behavior, and it's this complexity that objects were designed to work with. A Domain Model creates a web of interconnected objects, where each object represents some meaningful individual, whether as large as a corporation or as small as a single line on an order form.
  </p>
</blockquote>

#### Purpose

At the opposite pole from Transaction Script, use this pattern to build object-based models of your business participants and processes.

#### Example

 Webmail System.

#### Implementation 

{% highlight php %}
<?php
/**
 * Let's suppose we're developing a webmail application, and we 
 * want to create a Domain Model which encapsulates all the logic
 * of sending and receiving mails, creating them, forwarding, managing
 * replies and their visualization, and so on.
 * The most important class in this picture is an Entity which 
 * we'll give the name Email.
 */
class Email // does not extend anything
{
    /**
     * We probably want to introduce a class to manage addresses.
     * The modelling phase make decisions such as this, deciding on the issue of
     * primitive variable vs. wrapper class for object fields.
     * For now, we will leave them as simple strings. Subsequent
     * refactoring may introduce wrapper classes or interfaces.
     * @var string
     */
    private $_sender;
    private $_recipient;

    private $_subject;
    private $_text;
    
    /**
     * @return string
     */
    public function getSender()
    {
        return $this->_sender;
    }

    /**
     * Do we need setters and getters? Every field should be 
     * analyzed. If we can keep it private and inaccessible,
     * it's usually better.
     */
    public function setSender($sender)
    {
        $this->_sender = $sender;
    }

    /**
     * @return string
     */
    public function getRecipient()
    {
        return $this->_recipient;
    }

    public function setRecipient($recipient)
    {
        $this->_recipient = $recipient;
    }

    /**
     * @return string
     */
    public function getSubject()
    {
            return $this->_subject;
    }

    public function setSubject($subject)
    {
        $this->_subject = $subject;
    }

    /**
     * @return string
     */
    public function getText()
    {
        return $this->_text;
    }

    public function setText($text)
    {
        $this->_text = $text;
    }

    public function __toString()
    {
        return $this->_subject . ' > ' . substr($this->_text, 0, 20) . '...';
    }

    public function reply()
    {
        $reply = new Email();
        $reply->setRecipient($this->_sender);
        $reply->setSender($this->_recipient);
        $reply->setSubject('Re: ' . $this->_subject);
        $reply->setText($this->_sender . " wrote:\n" . $this->_text);
        return $reply;
    }
}

/**
 * Interface for a service. This is part of the Domain Model,
 * implementations will be plugged in depending on the environment.
 */
interface EmailRepository
{
    /**
     * @return array
     * @TypeOf(Email)
     */
    public function getEmailsFor($recipient);
}

// client code
$mail = new Email();
$mail->setSender("alice@example.com");
$mail->setRecipient("bob@example.com");
$mail->setSubject('Hello');
$mail->setText('This is a test of an Email object, which is part of our Domain Model.');
echo $mail, "\n";
$reply = $mail->reply();
echo $reply, "\n";
{% endhighlight %}

## Summary

In this post, we looked at the following enterprise patterns:

* Front Controller
* Application Controller
* Template View
* Page Controller
* Transaction Script
* Domain Model







