---
layout: post
title: 5 best practices for writing good SQL queries
tags:
- sql
- database
- best practices
thumbnail_path: blog/thumbs/sql-best-practices.jpg
add_to_popular_list: true
---

Performance and fast response of the web and mobile applications largely depends on the quality of writing SQL queries. 
Most of the junior developers with whom I worked SQL queries are a weak points. So I decided to write a post about the best practices for writing SQL queries, 
which good SQL developers uses every day.

{% include figure.html path=page.thumbnail_path %}

## Good practices

Obviously, these practices are much larger, but I decided to gather in this post the most used queries and not overload with rare cases.

#### Define all the necessary fields for SELECT instead of using SELECT *

<b>Bad practice</b>

As it seems to me, this is the most common mistake to get all available columns using *.

{% highlight sql %}
SELECT *
FROM users;
{% endhighlight %}

<b>Good practice</b>

With a large number of records and rows in the table, defining all the necessary fields will greatly speed up your query.

{% highlight sql %}
SELECT name, description, address
FROM users;
{% endhighlight %}

#### Use WHERE to filter records instead of HAVING

<b>Bad practice</b>

{% highlight sql %}
SELECT name
FROM users
GROUP BY name
HAVING age > 25
{% endhighlight %}

HAVING should only be used when filtering on an aggregated field. 

<b>Good practice</b>

If the goal is to filter records based on the condition, then a better solution cannot be found.

{% highlight sql %}
SELECT name
WHERE age > 25
FROM users;
{% endhighlight %}

#### Select more fields rather tha use DISTINCT

<b>Bad practice</b>

{% highlight sql %}
SELECT DISTINCT name, age
FROM users
{% endhighlight %}

<b>Good practice</b>

{% highlight sql %}
SELECT name, age, address
FROM users
{% endhighlight %}

#### Use INNER JOIN instead of WHERE

<b>Bad practice</b>

{% highlight sql %}
SELECT users.name, address.street
FROM users.id = address.user_id
{% endhighlight %}

<b>Good practice</b>

{% highlight sql %}
SELECT users.name, address.street
FROM users
    INNER JOIN address
    ON users.id = address.user_id
{% endhighlight %}

#### Use IN and EXIST in the right situations

<b>IN</b> is efficient when most of the filter criteria is in the subquery. 

<b>EXISTS</b> is efficient when most of the filter criteria is in the main query.

<b>Bad practice</b>

{% highlight sql %}
Select * from users u
where id IN 
(select user_id from address)
{% endhighlight %}

<b>Good practice</b>

{% highlight sql %}
Select * from users u 
where EXISTS (select * from address a
where a.user_id = u.user_id)
{% endhighlight %}

## Summary

Best practices fro writing SQL queries:
* Define all the necessary fields for <b>SELECT</b> instead of using <b>SELECT *</b>
* Use <b>WHERE</b> to filter records instead of <b>HAVING</b>
* Select more fields rather tha use <b>DISTINCT
* Use <b>INNER JOIN</b> instead of <b>WHERE</b>
* Use <b>IN</b> and <b>EXIST</b> in the right situations







