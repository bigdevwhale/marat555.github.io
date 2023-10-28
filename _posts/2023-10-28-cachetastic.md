---
layout: post
title: "Supercharge Your Laravel Caching with Cachetastic!"
tags:
- laravel
- cache
- php
- github
thumbnail_path: blog/cachetastic/cachetastic.png
add_to_popular_list: true
---

{% include figure.html path="blog/cachetastic/cachetastic.png" %}

Are you tired of slow API responses and sluggish database queries in your Laravel application? 
Do you wish you could sprinkle some magic dust and make your application's performance go 
*poof* lightning fast? Well, we've got a secret weapon for you, and it's called **Cachetastic**.

## The Need for Speed

As Laravel developers, we've all been there. You've built a fantastic application, but when it comes to 
those pesky external API calls or complex database queries, things can get a bit sluggish. 
Your users are tapping their fingers, and your server is working overtime. 

Or sometimes, your code for caching can become so complex that 
it's hard to maintain and difficult to read. 

Cachetastic simplifies the process, letting you focus on 
building your application while it handles the heavy lifting of caching. It's time to supercharge your caching game.

## Introducing Cachetastic

Cachetastic is not just another caching package; it's your secret weapon for optimizing caching in Laravel applications. 
It's designed to make caching method results a breeze, and it's your ticket to faster responses, reduced latency, and 
happier users.

## What Makes Cachetastic Awesome?

Cachetastic doesn't just sprinkle some caching magic; it packs a punch with a range of features that set it apart:

#### Method-Level Caching

Cachetastic lets you cache the results of any method. Whether you're dealing with external APIs, 
complex database queries, or computationally intensive tasks, Cachetastic has got you covered.

#### Give Up Complex Cache Code

Have you ever wrestled with complex cache generation code? The days of generating intricate cache keys are over! 
Cachetastic automatically generates cache keys based on the method name and parameters. 
Say goodbye to complex code and embrace simplicity.

#### Flexible Cache Management

Cachetastic puts you in control. You can force-refresh and update the cache with new values on-demand. When things change, 
simply clear the cache, and Cachetastic will handle the rest.

#### Laravel Integration

If you're a Laravel developer, you'll love this. Cachetastic seamlessly integrates with Laravel's caching system. 
It feels like it's part of the family.

## Getting Started

1. Install Cachetastic using Composer:
    ``` bash
    composer require bigdevwhale/cachetastic
    ```

2. Configure the default [cache driver](https://laravel.com/docs/10.x/cache) in your Laravel application.
3. Start caching method results with Cachetastic. You can cache regular methods, or take it up a notch and cache static methods!
    ``` php
    use Cachetastic\Cachetastic;
    use YourApiService;
   
    // Create an instance of Cachetastic to cache the result of a regular method
    $cacheService = new Cachetastic(
        new YourApiService(), // The service or object to call the method on.
        'fetchData',          // The name of the method to call on the service.
        [1, 2]               // An array of parameters to pass to the method.
    );
   
    // Customize the cache duration (optional)
    $cacheService->setCacheDuration(60);
   
    // Cache the result of your API call, whether it's a regular method
    $result = $cacheService->retrieveOrCache();
   
    // Create an instance of Cachetastic to cache the result of a static method
    $cacheServiceStatic = new Cachetastic(
        YourApiService::class, // The class with the static method.
        'fetchDataStatic',    // The name of the static method to call.
        [1, 2]                // An array of parameters to pass to the static method.
    );
   
    // Cache the result of your API call, whether it's a static method
    $resultStatic = $cacheServiceStatic->retrieveOrCache();
    ```
4. Customize caching, force clear the cache when needed, and enjoy the speed of Cachetastic!

#### Overwriting Cache Keys

One thing to keep in mind: if two methods are executed in the same class with only array parameters, 
they will overwrite each other's cache value since only scalar parameters are used for cache key generation. 
In this case, consider using the setCustomCacheKey method for fine-grained control.

## Join the Cachetastic Party
Cachetastic is open-source software licensed under the MIT License. We also welcome contributions from the open-source community. 
Feel free to submit bug reports, feature requests, or pull requests on our [GitHub repository](https://github.com/bigdevwhale/cachetastic).

So why wait? Supercharge your Laravel application with Cachetastic and turbocharge your caching game! 
Give it a try and experience the benefits for yourself. Happy caching! 💨✨

