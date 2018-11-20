---
layout: post
title: "Ways of Creation Custom Composer Dependency"
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
Or if we are using a certain library for our project and we decide to change something in the library, we will want our project to use the patched version.

And we are looking for the best way to create and connect this dependency module using composer.


## Local files

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


## VCS repository

VCS stands for version control system. This includes versioning systems like git, svn, fossil or hg. Composer has a repository type for installing packages from these systems.

#### Loading a package from a public VCS repository

Example assuming you patched monolog to fix a bug in the bugfix branch:

```plain
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/igorw/monolog"
        }
    ],
    "require": {
        "monolog/monolog": "dev-bugfix"
    }
}
```

#### Using private VCS repositories

Exactly the same solution allows you to work with your private repositories at GitHub and BitBucket:
```plain
{
    "require": {
        "vendor/my-private-repo": "dev-master"
    },
    "repositories": [
        {
            "type": "vcs",
            "url":  "git@bitbucket.org:vendor/my-private-repo.git"
        }
    ]
}
```

## Packagist

You can submit your package to [Packagist](https://packagist.org/) and then anyone who wants to use your package can add your package to their composer.json file like this:

```plain
"require": {
        "your/module": "master"
    },
```

## Summary

In this post, we looked at ways to use a custom module in composer project.