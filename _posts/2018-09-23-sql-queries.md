---
layout: post
title: 5 best practices for writing good SQL queries
tags:
- sql
- sql queries
- best practices
thumbnail_path: blog/thumbs/sql-best-practices.jpg
add_to_popular_list: true
---

Performance and fast response of the web and mobile applications largely depend on the quality of writing SQL queries. 
For most of the junior developers with whom I have worked SQL queries are weak points. So I decided to write a post about best practices for writing SQL queries, 
which good SQL developers uses every day.

{% include figure.html path=page.thumbnail_path %}

## Good practices

Obviously, these practices are much larger, but I decided to gather in this post the most used queries and not overload with rare cases.

#### Define all the necessary fields for SELECT instead of using SELECT *

As it seems to me, this is the most common mistake to get all available columns using *.
With a large number of records and rows in the table, defining all the necessary fields will greatly speed up your query.

<b><i>Bad practice</i></b>

{% highlight sql %}
SELECT *
FROM users;
{% endhighlight %}

<b><i>Good practice</i></b>

{% highlight sql %}
SELECT name, description, address
FROM users;
{% endhighlight %}

#### Use WHERE to filter records instead of HAVING

<b>HAVING</b> should only be used when filtering on an aggregated field.
 If the goal is to filter records based on the condition, then a better solution than <b>WHERE</b> cannot be found.

<b><i>Bad practice</i></b>

{% highlight sql %}
SELECT name
FROM users
GROUP BY name
HAVING age > 25
{% endhighlight %}

<b><i>Good practice</i></b>

{% highlight sql %}
SELECT name
WHERE age > 25
FROM users;
{% endhighlight %}

#### Select more fields rather tha use DISTINCT

<b>SELECT DISTINCT</b> query is performed using the grouping of all fields in the query, which is significantly more resource-intensive than using the usual <b>SELECT</b>.

<b><i>Bad practice</i></b>

{% highlight sql %}
SELECT DISTINCT name, age
FROM users
{% endhighlight %}

<b><i>Good practice</i></b>

{% highlight sql %}
SELECT name, age, address
FROM users
{% endhighlight %}

#### Use INNER JOIN instead of WHERE

If the query optimizer is doing its job right, there should be no difference between those queries. But <b>INNER JOIN</b> makes query more readable.

<b><i>Bad practice</i></b>

{% highlight sql %}
SELECT users.name, address.street
FROM users.id = address.user_id
{% endhighlight %}

<b><i>Good practice</i></b>

{% highlight sql %}
SELECT users.name, address.street
FROM users
    INNER JOIN address
    ON users.id = address.user_id
{% endhighlight %}

#### Use IN and EXIST in the right situations

<b>IN</b> is efficient when most of the filter criteria is in the subquery. 

<b>EXISTS</b> is efficient when most of the filter criteria is in the main query.

<b><i>Bad practice</i></b>

{% highlight sql %}
Select * from users u
where id IN 
(select user_id from address)
{% endhighlight %}

<b><i>Good practice</i></b>

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







