---
layout: post
title: "Ways of creation custom composer dependency"
tags:
- Composer
- Dependency management
thumbnail_path: blog/thumbs/composer-dependency.png
add_to_popular_list: true
---

{% include figure.html path=page.thumbnail_path %}
Сomposer is awesome. It allows us to manage dependencies and required libraries for the application we are developing. 


But what if the dependency we need does not exist in the composer default repository. Then we as inventors (maybe not) implement it ourselves. 


In this post I will discuss the basic techniques of how this can be done.

## How does Composer work?

{% include figure.html path="blog/composer-dependency/composer.png" caption="The following graph shows how Composer works when using Packagist as a central repository" %}


Unless we explicitly indicate that we want to use a different repository, Composer makes use of the default repository: Packagist. 

Composer will ask Packagist for information about all packages required in the composer.json file, as well as dependencies required by those packages. 

When Composer gets all that information, it solves the dependency graph using a SAT solver and generates the composer.lock file with the specific packages that must be installed in order to fulfil the requirements defined in the composer.json file. Finally, Composer downloads those packages from different sources: GitHub, Bitbucket, PEAR or any GIT/SVN/Mercurial repository.

## Problem

We implement the system, which in turn consists of several applications. And each of these applications requires a specific software module to work.

And we are looking for the best way to create and connect this dependency module using composer.

## Ways of solution

Ways will go from worst to best for understanding bad and good practices.

#### Local files

You can move your module to the application folder and use your application namespace in your module.

Example app's namespace definition in composer's psr-4 key.
 
```plain
   "psr-4": {
           "App\\": "app/",
       }
```

Example of your module's namespace:

```plain
    namespace App\Module;
```

This method is perfect if you have one project with one dependency, but for several projects, methods described below will be more convenient.


#### R 

