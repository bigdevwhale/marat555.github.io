---
layout: post
title: "6 Simple and Useful PostgreSQL Features that I wish I knew when I started "
tags:
- programming
- PostgreSQL
- SQL
thumbnail_path: blog/simple-and-usefull-postgresql-features/elephant.png
add_to_popular_list: true
---


{% include figure.html path="blog/simple-and-usefull-postgresql-features/elephant.png" %}
I use PostgreSql a lot in my working time. So recently, I spent some time refreshing and deepening my knowledge of PostgreSQL to improve my skills and experience in using it (writing and optimizing queries, creating new objects, etc.). And I found many awesome features and syntax sugar constructions that can tremendously ease your daily routine and eliminate problems that may appear while resolving sophisticated tasks. In this article, I will try to review 6 PostgreSql traits that seem to me the most important and easy-usable in a clear and brief way.

## Features

* [Identity](#identity)
* [COALESCE + NULLIF](#coalesce--nullif)
* [Grouping set, rollup, cube](#grouping-set-rollup-cube)
* [Common Table Expression](#common-table-expression)
* [Domains](#domains)
* [USING keyword](#using-keyword)

## Identity

Serial is the "old" implementation of auto-generated unique values that has been part of Postgres for ages. However that is not part of the SQL standard.

To be more compliant with the SQL standard, Postgres 10 introduced the syntax using generated as identity.

The underlying implementation is still based on a sequence, the definition now complies with the SQL standard. One thing that this new syntax allows is to prevent an accidental override of the value.

Consider the following tables:
{% highlight sql %}
create table t1 (id serial primary key);
create table t2 (id integer primary key generated always as identity);
{% endhighlight %}

Now when you run:
{% highlight sql %}
insert into t1 (id) values (1);
{% endhighlight %}

The underlying sequence and the values in the table are not in sync any more. If you run another
{% highlight sql %}
insert into t1 default_values;
{% endhighlight %}

You will get an error because the sequence was not advanced by the first insert, and now tries to insert the value 1 again.

With the second table however,
{% highlight sql %}
insert into t2 (id) values (1);
{% endhighlight %}

Results in:
<blockquote>
ERROR: cannot insert into column "id"
Detail: Column "id" is an identity column defined as GENERATED ALWAYS.
</blockquote>

So you can't accidentally "forget" the sequence usage. You can still force this, using the override system value option:

{% highlight sql %}
insert into t2 (id) overriding system value values (1);
{% endhighlight %}

which still leaves you with a sequence that is out-of-sync with the values in the table, but at least you were made aware of that.

identity columns also have another advantage: they also minimize the grants you need to give to a role in order to allow inserts.

While a table using a serial column requires the INSERT privilege on the table and the USAGE privilege on the underlying sequence this is not needed for tables using an identity columns. Granting the INSERT privilege is enough.

It is recommended to use the new identity syntax rather than serial.

For more information, you can refer to the official documentation

## COALESCE + NULLIF

As everybody knows the COALESCE function accepts an unlimited number of arguments and returns the first argument that is not null. If all arguments are null, the COALESCE function will return null.
Here is how it interprets multiple null values: 

{% highlight sql %}
postgres=# SELECT COALESCE(null,null, 1, 2, 3, null, 4);

  COALESCE 
        1

(1 row)
{% endhighlight %}

But what if you want similar to COALESCE function for zero instead of null. In that case you can combine NULLIF with COALESCE:
The NULLIF function returns a null value if argument_1 equals to argument_2, otherwise it returns argument_1.

See the following example:

{% highlight sql %}
SELECT
	NULLIF (1, 1); - return NULL
SELECT
	NULLIF (1, 0); - return 1
SELECT
	NULLIF ('A', 'B'); - return A
{% endhighlight %}

So in the example proposed above you to make COALESCE return first non-zero value, you can simply replace all COALESCE arguments with such call: NULLIF(arg, 0) 

{% highlight sql %}
SELECT COALESCE(NULLIF(arg_1, 0), NULLIF(arg_2, 0), NULLIF(arg_3, 0))
{% endhighlight %}

## Grouping set, rollup, cube

grouping set, rollup, cube are irreplaceable tools for reporting and performance purposes. 

### Grouping set
GROUP BY will turn every distinct entry in a column into a group. Sometimes you might want to do more grouping at once. Why is that necessary? Suppose you are processing a 10 TB table. Clearly, reading this data is usually the limiting factor in terms of performance. So reading the data once and producing more results at once is appealing. That’s exactly what you can do with GROUP BY GROUP SETS. Suppose we want to produce two results at once:

- GROUP BY country
- GROUP BY product_name

Here’s how it works: 

{% highlight sql %}
test=# SELECT  country, product_name, sum(amount_sold)
       FROM    t_sales
       GROUP BY GROUPING SETS ((1), (2))
       ORDER BY 1, 2;
 country   | product_name | sum
-----------+--------------+-----
 Argentina |              | 137
 Germany   |              | 104
 USA       |              | 373
           | Hats         | 323
           | Shoes        | 291
(5 rows)
{% endhighlight %}

### Rollup

When creating reports, you will often need the “bottom line” which sums up what has been shown in the table. The way to do that in SQL is to use “GROUP BY ROLLUP”:

{% highlight sql %}
test=# SELECT  country, product_name, sum(amount_sold)
       FROM    t_sales
       GROUP BY ROLLUP (1, 2)
       ORDER BY 1, 2;
 country   | product_name | sum
-----------+--------------+-----
 Argentina | Hats         | 111
 Argentina | Shoes        |  26
 Argentina |              | 137
 Germany   | Hats         |  41
 Germany   | Shoes        |  63
 Germany   |              | 104
 USA       | Hats         | 171
 USA       | Shoes        | 202
 USA       |              | 373
           |              | 614
(10 rows)
{% endhighlight %}

PostgreSQL will inject a couple of rows into the result. As you can see, “Argentina” returns 3 and not just 2 rows. The “product_name = NULL” entry was added by ROLLUP. It contains the sum of all argentinian sales (116 + 27 = 137). Additional rows are injected for both other countries. Finally, a row is added for the overall sales worldwide. 

### CUBE

{% highlight sql %}
ROLLUP is useful if you want to add the “bottom line”. However, you often want to see all combinations of countries and products. GROUP BY CUBE will do exactly that:

test=# SELECT  country, product_name, sum(amount_sold)
       FROM    t_sales
       GROUP BY CUBE (1, 2)
       ORDER BY 1, 2;
 country   | product_name | sum
-----------+--------------+-----
 Argentina | Hats         | 111
 Argentina | Shoes        |  26
 Argentina |              | 137
 Germany   | Hats         |  41
 Germany   | Shoes        |  63
 Germany   |              | 104
 USA       | Hats         | 171
 USA       | Shoes        | 202
 USA       |              | 373
           | Hats         | 323
           | Shoes        | 291
           |              | 614
(12 rows)
{% endhighlight %}

In this case, we’ve got all the combinations. Technically, it’s the same as: GROUP BY country + GROUP BY product_name + GROUP BY country_product_name + GROUP BY (). We could do that using more than just one statement, but doing it at once is easier – and a lot more efficient. 

Again, NULL values have been added to indicate various aggregation levels. 

## Common Table Expression

A common table expression is a temporary result set which you can reference within another SQL statement including SELECT, INSERT, UPDATE or DELETE.

Common Table Expressions are temporary in the sense that they only exist during the execution of the query.

The following shows the syntax of creating a CTE:

{% highlight sql %}
WITH cte_name (column_list) AS (
    CTE_query_definition 
)
statement;
{% endhighlight %}

Common Table Expressions or CTEs are typically used to simplify complex joins and subqueries in PostgreSQL.

The following statement illustrates how to join a CTE with a table:

{% highlight sql %}
WITH cte_rental AS (
    SELECT staff_id,
        COUNT(rental_id) rental_count
    FROM   rental
    GROUP  BY staff_id
)
SELECT s.staff_id,
    first_name,
    last_name,
    rental_count
FROM staff s
    INNER JOIN cte_rental USING (staff_id); 
{% endhighlight %}

In this example:

First, the CTE returns a result set that includes staff id and the number of rentals.
Then, join the staff table with the CTE using the staff_id column.

## Domains

In my opinion, this is a very useful feature for creating a custom type if you have many columns that need to be restricted by several of the same constraints, have same data type and/or same default value.
Let's create, for example, a VARCHAR domain that should have a not null constraint and a default value of 'N/A'.

{% highlight sql %}
CREATE DOMAIN addr VARCHAR(90) NOT NULL DEFAULT 'N/A';
{% endhighlight %}

## USING keyword

The USING clause is a shorthand that allows you to take advantage of the specific situation where both sides of the join use the same name for the joining column(s). It takes a comma-separated list of the shared column names and forms a join condition that includes an equality comparison for each one. For example, joining T1 and T2 with USING (a, b) produces the join condition ON T1.a = T2.a AND T1.b = T2.b.

## Conclusion

This post covers simple and helpful features of Postgresql.
For more info, you can always refer to official docs.
