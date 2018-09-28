---
layout: post
title: 'Development of PHP: from 5.0 to 7.2'
tags:
- PHP
- Programming
thumbnail_path: blog/thumbs/php.png
add_to_popular_list: true
---

In my work, I mainly use PHP 5.6 and 7.2. To learn more about the subject area, I decided to find out how the development of PHP was going from 5.0 to 7.2. 
 {% include figure.html path=page.thumbnail_path url=page.external_url %}

To structure the information, I decided to write this post about the most noticeable changes between versions. 
I hope this post will be useful  for someone too. All documentation links at the end. 
## Versions
#### PHP 5.0

In July 2004 PHP 5.0 was released. This version introduced with a set of cool changes. For me, the most important are enhancements for object-oriented programming. Thanks to this, object-oriented development has significantly increased its influence.

#### PHP 5.1.x and PHP 5.2.x

New features: 
* A complete rewrite of date handling code, with improved timezone support.
* Significant performance improvements compared to PHP 5.0.X.
* PDO extension is now enabled by default.
* New functions in various extensions and built-in functionality.

#### PHP 5.3.x

New features: 
* Support for [namespaces](http://php.net/manual/en/language.namespaces.php) has been added.
* Support for [Late Static Bindings](http://php.net/manual/en/language.oop5.late-static-bindings.php) has been added.
* Support for [jump labels](http://php.net/manual/en/control-structures.goto.php) (limited goto) has been added.
* Support for native [Closures](http://php.net/manual/en/functions.anonymous.php) (Lambda/Anonymous functions) has been added.
* There are two new magic methods, [__callStatic()](http://php.net/manual/en/language.oop5.overloading.php#object.callstatic) and [__invoke()](http://php.net/manual/en/language.oop5.magic.php#object.invoke).
* [Nowdoc](http://php.net/manual/en/language.types.string.php#language.types.string.syntax.nowdoc) syntax is now supported, similar to Heredoc syntax, but with single quotes.
* It is now possible to use Heredocs to initialize static variables and class properties/constants.
* [Heredocs](http://php.net/manual/en/language.types.string.php#language.types.string.syntax.heredoc) may now be declared using double quotes, complementing the Nowdoc syntax.
* [Constants](http://php.net/manual/en/language.constants.syntax.php) can now be declared outside a class using the const keyword.
* The [ternary](http://php.net/manual/en/language.operators.comparison.php#language.operators.comparison.ternary) operator now has a shorthand form: ?:.
* Dynamic access to static methods is now possible:
{% highlight php %}
<?php
<?php
class C {
   public static $foo = 123;
}

$a = "C";
echo $a::$foo;
?>
{% endhighlight %}
* [Exceptions](http://php.net/manual/en/language.exceptions.php) can now be nested:
{% highlight php %}
<?php
class MyCustomException extends Exception {}

try {
    throw new MyCustomException("Exceptional", 112);
} catch (Exception $e) {
    /* Note the use of the third parameter to pass $e
     * into the RuntimeException. */
    throw new RuntimeException("Rethrowing", 911, $e);
}
?>
{% endhighlight %}
* A [garbage collector](http://php.net/manual/en/features.gc.php) for circular references has been added, and is enabled by default.

#### PHP 5.4.x
New features: 
* Support for traits has been added.
* Short array syntax has been added, e.g. $a = [1, 2, 3, 4]; or $a = ['one' => 1, 'two' => 2, 'three' => 3, 'four' => 4];.
* Function array dereferencing has been added, e.g. foo()[0].
* Closures now support $this.
* <?= is now always available, regardless of the short_open_tag php.ini option.
* Class member access on instantiation has been added, e.g. (new Foo)->bar().

#### PHP 5.5.x
New features: 
* Support for generators has been added via the yield keyword. Generators provide an easy way to implement simple iterators without the overhead or complexity of implementing a class that implements the Iterator interface.

A simple example that reimplements the range() function as a generator (at least for positive step values):
{% highlight php %}
<?php
function xrange($start, $limit, $step = 1) {
    for ($i = $start; $i <= $limit; $i += $step) {
        yield $i;
    }
}

echo 'Single digit odd numbers: ';

/*
 * Note that an array is never created or returned,
 * which saves memory.
 */
foreach (xrange(1, 9, 2) as $number) {
    echo "$number ";
}

echo "\n";
?>
{% endhighlight %}
* finally keyword added
* foreach now supports list()
* empty() supports arbitrary expressions
* array and string literal dereferencing
* Function array dereferencing has been added, e.g. foo()[0].
* Closures now support $this.
* <?= is now always available, regardless of the short_open_tag php.ini option.
* Class member access on instantiation has been added, e.g. (new Foo)->bar().

#### PHP 5.6.x
New features:
* Constant expressions
* Variadic functions can now be implemented using the ... operator, instead of relying on func_get_args().

#### PHP 7.0.x
New features:
* Scalar type declarations 
* Return type declarations 
* Null coalescing operator
* Spaceship operator (<=>)

{% highlight php %}
<?php
// Integers
echo 1 <=> 1; // 0
echo 1 <=> 2; // -1
echo 2 <=> 1; // 1

// Floats
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1
 
// Strings
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1
?>
{% endhighlight %}

* Constant arrays using define() 
* Anonymous classes

{% highlight php %}
<?php
interface Logger {
    public function log(string $msg);
}

class Application {
    private $logger;

    public function getLogger(): Logger {
         return $this->logger;
    }

    public function setLogger(Logger $logger) {
         $this->logger = $logger;
    }
}

$app = new Application;
$app->setLogger(new class implements Logger {
    public function log(string $msg) {
        echo $msg;
    }
});

var_dump($app->getLogger());
?>
{% endhighlight %}

#### PHP 7.1.x
New features:
* Nullable types

Type declarations for parameters and return values can now be marked as nullable by prefixing the type name with a question mark. This signifies that as well as the specified type, NULL can be passed as an argument, or returned as a value, respectively.
* Void functions
* Symmetric array destructuring

The shorthand array syntax ([]) may now be used to destructure arrays for assignments (including within foreach), as an alternative to the existing list() syntax, which is still supported.
* Class constant visibility 
* iterable pseudo-type 
* Multi catch exception handling
* Support for keys in list() 
* Asynchronous signal handling 

#### PHP 7.2.x
New features:
* New object type
{% highlight php %}
<?php

function test(object $obj) : object
{
    return new SplQueue();
}

test(new StdClass());
{% endhighlight %}

* Abstract method overriding
* Password hashing with Argon2
* Extended string types for PDO
* Support for extended operations in LDAP

## Summary

In this article, I tried to describe all the most important changes that have been made with PHP from 5.0.

## See also

* [Migrating from PHP 4 to PHP 5.0.x](http://php.net/manual/en/migration5.php)
* [Migrating from PHP 5.0.x to PHP 5.1.x](http://php.net/manual/en/migration51.php)
* [Migrating from PHP 5.1.x to PHP 5.2.x](http://php.net/manual/en/migration52.php)
* [Migrating from PHP 5.2.x to PHP 5.3.x](http://php.net/manual/en/migration53.php)
* [Migrating from PHP 5.3.x to PHP 5.4.x](http://php.net/manual/en/migration54.php)
* [Migrating from PHP 5.4.x to PHP 5.5.x](http://php.net/manual/en/migration55.php)
* [Migrating from PHP 5.5.x to PHP 5.6.x](http://php.net/manual/en/migration56.php)
* [Migrating from PHP 5.6.x to PHP 7.0.x](http://php.net/manual/en/migration70.php)
* [Migrating from PHP 7.0.x to PHP 7.1.x](http://php.net/manual/en/migration71.php)
* [Migrating from PHP 7.1.x to PHP 7.2.x](http://php.net/manual/en/migration72.php)