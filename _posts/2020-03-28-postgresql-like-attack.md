---
layout: post
title: "PostgreSQL-like attack (similar to DDoS)"
tags:
- PostgreSQL
- DDoS
thumbnail_path: blog/thumbs/postresql-attack.png
add_to_popular_list: true
---

{% include figure.html path=page.thumbnail_path %}


## Background

After the client requests to connect to the database, the client will be prompted to enter the user password. If the client does not enter the password, the database server will disconnect after a timeout period.

In other words, before the server actively disconnects, the connection needs to occupy a slot, which is one of max_connection.

https://www.postgresql.org/docs/9.6/static/runtime-config-connection.html#RUNTIME-CONFIG-CONNECTION-SECURITY

<blockquote>
  <p>
    authentication_timeout (integer)  
  </p>
  
<p>
    Maximum time to complete client authentication, in seconds.   
</p>

<p>
If a would-be client has not completed the authentication protocol in this much time, the server closes the connection.
</p>

   <p>
This prevents hung clients from occupying a connection indefinitely.   
 </p>

<p>
The default is one minute (1m). This parameter can only be set in the postgresql.conf file or on the server command line.  
</p>
</blockquote>

The other parameter is the client connection timeout parameter, which has nothing to do with the attack. I just took it out here for everyone to understand.

https://www.postgresql.org/docs/9.6/static/libpq-connect.html#LIBPQ-CONNECT-CONNECT-TIMEOUT


<blockquote>
  <p>
    connect_timeout  
  </p>
  
<p>
Maximum wait for the connection, in seconds (write as a decimal integer string).
</p>

<p>
If a would-be client has not completed the authentication protocol in this much time, the server closes the connection.
</p>

<p>
Zero or not specified means wait indefinitely.
 </p>

<p>
It is not recommended to use a timeout of less than 2 seconds.
</p>
</blockquote>

An attacker can use this rule to initiate a large number of connection requests concurrently, but without providing a password, waiting for the server to time out, so that all the max_connection connections can be occupied.

## Phenomenon

If the user is full, the connection initiated by other normal requests will encounter an insufficient connection error.

53300 too_many_connections

At the same time, the count (*) queried in pg_stat_activity is less than the actual max_connection configuration.
Because a successful connection without authentication does not yet appear in the pg_stat_activity session.

## Precautions
1. Do not expose the listening port.

2. If the listening port must be exposed, it is recommended to use source IP authentication to filter illegal IP to avoid most attacks.

3. Configure pg_hba.conf, authenticate IP, DB, USER, and avoid most attacks.

4. Configure user, DB-level connection restrictions, even if attacked, can also guarantee that some connections can be used. Unless the attacker knows all the database names and user names, they attack them. Otherwise, all connections will not be taken up.

##  Related questions
Even if it is not a DDoS attack, users may encounter similar problems.

For example, when the database is very busy, the user's request-response becomes slow, and the user's application piles up a lot of requests. These requests need to establish a new connection to the database. Due to the very busy database itself, the response is slow and the explosive high concurrent connection requests make the PG The fork operation on the server become slower, and the auth message packet is returned after this (so causing a similar phenomenon).

Therefore, if the user encounters such a weird problem, the number of connections displayed in pg_stat_activity is smaller than max_connection, and it is indeed impossible to connect to the database (reporting an error of too_many_connections), then you can see if there is no DDoS attack or the database is busy.












