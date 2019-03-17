---
layout: post
title: "SQL Injection Prevention"
tags:
- Sql
- Sql injection
thumbnail_path: blog/thumbs/sql-injection.jpg
add_to_popular_list: true
---

{% include figure.html path="blog/sql-injection/sql-injection.jpg" %}
SQL injection is one of the most accessible ways to hack a site. The essence of such injections is the injection of arbitrary
 SQL code into the data (transmitted via GET, POST requests or Cookie values). If the site is vulnerable and performs such 
 injections, then, in fact, there is an opportunity to execute any SQL query to the database.

## How to detect a vulnerability that allows introducing SQL injection?

Pretty easy. For example, there is a test site testsite.com. The site displays a list of news, with the possibility of a detailed view. The address of the page with a detailed description of the news is as follows: test.ru/?detail=1. Ie, through the GET request, the detail variable passes the value 1 (which is the identifier of the entry in the news table).

Let's change the GET request to ?detail=1' or ?detail=1". Next, we will try to send these requests to the server, that is, we go to testsite.com/?detail=1' or to testsite.com/?detail=1".

If an error occurs on entering these pages, then the site is vulnerable to SQL injections.

## Protection against SQL injection (SQL implementations)

Protection against hacking comes down to the basic rule of "trust but verify." You need to check everything - numbers, strings, dates, data in special formats. Next we look at the main ways to protect.


#### Prepared Statements (with Parameterized Queries)

Prepared statement is part of the functionality of the SQL database, designed to separate the request data and the actual SQL query. For example, we have a query:

{% highlight sql %}
insert into someTable (name) values ​​('George');
{% endhighlight %}

What can we notice just by looking at it? Firstly, the insert request itself is usually static and does not change in different requests, in 90% of cases it is simply hard-coded into the code or generated using some ORM; the data value (in this case, 'George') changes constantly and is set from the outside - from user input or from other sources. Bind variables allow you to specify a request separately, and then transfer data to it separately, something like this (pseudo-code):

{% highlight sql %}
request = sql_prepare('insert into table(name) values(:1)');
sql_execute(request, Array('George'));
sql_execute(request, Array('John'));
sql_execute(request, Array('Richard'));
sql_execute(request, Array('Frank'));
{% endhighlight %}

There are several advantages to using prepared statements:
1. The obvious advantage - the same prepared query can be used several times for different data, thereby reducing the code.
2. Requests with bindable variables are better cached by the server, reducing the time to parse.
3. Requests with bound variables have ready built-in protection against SQL injections.

#### Stored Procedures

You can use stored procedures. Unlike prepared expressions, stored procedures are stored in the database, 
but in both cases, the SQL query is first defined, and parameters are passed to it.

#### White List Input Validation

The overwhelming majority of articles on injections, completely overlook this moment. But the reality is that in it we are faced with the need to substitute in the query not only data but also other elements - identifiers (field and table names) and even syntax elements, keywords. Even insignificant such as DESC or AND, but the safety requirements for such substitutions should still be no less strict!

Let us examine a rather banal case.
We have a database of goods, which is displayed to the user in the form of an HTML table. The user can sort this table by one of the fields, in any direction.
That is, at least from the user's side, the name of the column and the direction of sorting come to us.
Substitute them in the request directly - guaranteed injection. The usual formatting methods will not help here. Prepared expressions with neither identifiers nor keywords will lead to nothing but an error message.
The only solution is whitelisting.
This, of course, is not Newton's binomial, and many developers easily implement this paradigm along the way, faced for the first time with the need to substitute the field name into a query. However, the article on protection from injections without this rule will be incomplete, and the defense itself will be full of holes.

The essence of the method lies in the fact that all possible choices must be strictly written in our code, and only they should be included in the request, based on user input.

Example:

{% highlight php %}
$order   = isset($_GET['order']) ? $_GET['order'] : ''; // just to complete the code
$sort    = isset($_GET['sort'])  ? $_GET['sort']  : '';

$allowed = array("name", "price", "qty"); // enumerate options
$key     = array_search($sort,$allowed); // look for the passed parameter among them
$orderby = $allowed[$key]; // choose the found (or, due to the type conversion - the first) element.
$order   = ($order == 'DESC') ? 'DESC' : 'ASC'; // determine the direction of sorting
$query   = "SELECT * FROM `table` ORDER BY $orderby $order"; // request is 100% secure
{% endhighlight %}

#### Escaping All User-Supplied Input

This technique should only be used as a last resort, when none of the above are feasible. Input validation is probably a better choice as this methodology is frail compared to other defenses and we cannot guarantee it will prevent all SQL Injection in all situations.

## See more

SQL Injection Attack Cheat Sheets

The following articles describe how to exploit different kinds of SQL Injection Vulnerabilities on various platforms that this article was created to help you avoid:

* [SQL Injection Cheat Sheet](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)
* Bypassing WAF's with SQLi - [SQL Injection Bypassing WAF](https://www.owasp.org/index.php/SQL_Injection_Bypassing_WAF)

Description of SQL Injection Vulnerabilities

* OWASP article on [SQL Injection](https://www.owasp.org/index.php/SQL_Injection) Vulnerabilities
* OWASP article on [Blind_SQL_Injection](https://www.owasp.org/index.php/Blind_SQL_Injection) Vulnerabilities

How to Avoid SQL Injection Vulnerabilities

* [OWASP Developers Guide](https://www.owasp.org/index.php/:Category:OWASP_Guide_Project) article on how to avoid SQL injection vulnerabilities
* OWASP Cheat Sheet that provides [numerous language specific examples of parameterized queries using both Prepared Statements and Stored Procedures](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Query_Parameterization_Cheat_Sheet.md)
* [The Bobby Tables site (inspired by the XKCD webcomic) has numerous examples in different languages of parameterized Prepared Statements and Stored Procedures](http://bobby-tables.com/)

How to Review Code for SQL Injection Vulnerabilities

* [OWASP Code Review Guide](https://www.owasp.org/index.php/Category:OWASP_Code_Review_Project) article on how to [Review Code for SQL Injection](https://www.owasp.org/index.php/Reviewing_Code_for_SQL_Injection) Vulnerabilities

How to Test for SQL Injection Vulnerabilities

* [OWASP Testing Guide](https://www.owasp.org/index.php/:Category:OWASP_Testing_Project) article on how to [Test for SQL Injection](https://www.owasp.org/index.php/Testing_for_SQL_Injection_(OWASP-DV-005)) Vulnerabilities


