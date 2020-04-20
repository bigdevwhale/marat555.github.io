---
layout: post
title: "Problems and solutions that I encountered while installing and configuring simplesamlphp"
tags:
- saml
- simplesamlphp
thumbnail_path: blog/saml/authentication-vs-authorization.png
add_to_popular_list: true
---

{% include figure.html path=page.thumbnail_path %}

## Background

Last week I needed to implement authorization via SAML

<blockquote>
  <p>
  Security Assertion Markup Language (SAML) is an open standard for exchanging authentication and authorization data between parties, in particular, between an identity provider and a service provider. SAML is an XML-based markup language for security assertions (statements that service providers use to make access-control decisions).
  </p>
</blockquote>

The project I'm working on is written in PHP, so the choice of a library for saml is obvious, it's simplesamlphp.
In the process of working on the task, a couple of unpleasant problems arose related to simplesamlphp. Today I would like to tell about them. I hope this article will make life easier for someone.


## Problem 1. Undefined classes

I installed the library through the composer command and made all the necessary settings indicated on the official documentation.
But immediately an error occurred:
<blockquote>
  <p>
  'SimpleSAML\\Auth\\Source' not found in /vendor/simplesamlphp/simplesamlphp/modules/saml/www/sp/saml2-acs.php on line 12, referer:
  </p>
</blockquote>

This error was in the source code file of the library itself, so I had to see what was there.

### Solution
Having studied the problem, I realized that namespaces are not recognized in all files in this directory
<blockquote>
  <p>
  \vendor\simplesamlphp\simplesamlphp\modules\saml\www\sp\
  </p>
</blockquote>

The easiest solution was to include a vendor autoload in these files. In my case, added the following line

{% highlight php %}
require_once dirname(dirname(__FILE__)). "/../../../../../../vendor/autoload.php";
{% endhighlight %}

## Problem 2. Poor location of configuration files

Config files are located directly in the directory with all composer dependencies, so the configs will be deleted when the command is executed to update the composer dependencies under certain conditions. 
For example, a new version of the simplesamlphp library will be released. 

### Solution

You need to set an environment variable with the location of your config directory. in my case, I added the following line in the apache config of my web application:

<blockquote>
  <p>
  SetEnv SIMPLESAMLPHP_CONFIG_DIR /var/simplesamlphp/config
  </p>
</blockquote>


## Problem 3. Sometimes authorization via saml does not work

After all the settings made from the saml specified in the documentation and above, authorization through simplesamplnp began to work. But all of a sudden, authorization stopped working on one of the servers from time to time, and I had to dig into the logs.
First, I changed the logging option to debug in the simplesaml config:

{% highlight php %}
'logging.level' => SimpleSAML\Logger::DEBUG,
{% endhighlight %}

This is what I then found in the logs:

<blockquote>
  <p>
  samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Responder"/>
  </p>
</blockquote>

This means that the problem is on the side of the identity provider. 

That's what was in the logs on the side of the identity provider:

<blockquote>
  <p>
 SSO Configuration Microsoft.IdentityServer.Service.SecurityTokenService.RevocationValidationException: 
  </p>
</blockquote>


### Solution

The problem was solved by disabling the SigningCertificateRevocationCheck option on the side of the identity provider.



## Summary

Today I examined the problems that I encountered while working with the simplesamlphp library.
