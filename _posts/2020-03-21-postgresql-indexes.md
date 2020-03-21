---
layout: post
title: "Types of PostgreSQL Indexes. Short and clear."
tags:
- PostgreSQL
- Database
- Indexes
thumbnail_path: blog/thumbs/database-index.jpg
add_to_popular_list: true
---

There are several types of indexes in PostgreSQL. Beginners or those who have little to do with the databases use the index that PostgreSQL creates by default (B-tree) or do not create them at all. It is necessary to know what indexes are used in which situations to speed up the execution of queries. In this post, I will try to examine this briefly and simply, without going into details of the internal structure of indexes.

{% include figure.html path=page.thumbnail_path %}

  PostgreSQL Indexes:
  
* [Hash](#hash)
* [B-tree](#b-tree)
* [GiST](#gist) 
* [SP-GiST](#sp-gist) 
* [GIN](#gin)
* [RUM](#rum) 
* [BRIN](#brin)
* [Bloom](#bloom)


## Hash

The hash index stores the hash value of the indexed field VALUE, and only supports equal-value queries.

#### Usage

The hash index is particularly suitable for scenarios where the field VALUE is very long (not suitable for b-tree indexing, because a PAGE of b-tree needs to store at least 3 ENTRY, so it does not support particularly long VALUE), such as very long strings, and users Only equivalent search is required, and hash index is recommended.

#### Example

{% highlight bash %}
postgres=# create table t_hash (id int, info text);  
CREATE TABLE  
postgres=# insert into t_hash select generate_series(1,100), repeat(md5(random()::text),10000);  
INSERT 0 100  
  
-- Using b-tree index will give an error, because the index page length exceeds 1/3
postgres=# create index idx_t_hash_1 on t_hash using btree (info);  
ERROR:  index row size 3720 exceeds maximum 2712 for index "idx_t_hash_1"  
HINT:  Values larger than 1/3 of a buffer page cannot be indexed.  
Consider a function index of an MD5 hash of the value, or use full text indexing.  
  
postgres=# create index idx_t_hash_1 on t_hash using hash (info);  
CREATE INDEX  
  
postgres=# set enable_hashjoin=off;  
SET  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_hash where info in (select info from t_hash limit 1);  
                                                             QUERY PLAN                                                                
-------------------------------------------------------------------------------------------------------------------------------------  
 Nested Loop  (cost=0.03..3.07 rows=1 width=22) (actual time=0.859..0.861 rows=1 loops=1)  
   Output: t_hash.id, t_hash.info  
   Buffers: shared hit=11  
   ->  HashAggregate  (cost=0.03..0.04 rows=1 width=18) (actual time=0.281..0.281 rows=1 loops=1)  
         Output: t_hash_1.info  
         Group Key: t_hash_1.info  
         Buffers: shared hit=3  
         ->  Limit  (cost=0.00..0.02 rows=1 width=18) (actual time=0.012..0.012 rows=1 loops=1)  
               Output: t_hash_1.info  
               Buffers: shared hit=1  
               ->  Seq Scan on public.t_hash t_hash_1  (cost=0.00..2.00 rows=100 width=18) (actual time=0.011..0.011 rows=1 loops=1)  
                     Output: t_hash_1.info  
                     Buffers: shared hit=1  
   ->  Index Scan using idx_t_hash_1 on public.t_hash  (cost=0.00..3.02 rows=1 width=22) (actual time=0.526..0.527 rows=1 loops=1)  
         Output: t_hash.id, t_hash.info  
         Index Cond: (t_hash.info = t_hash_1.info)  
         Buffers: shared hit=6  
 Planning time: 0.159 ms  
 Execution time: 0.898 ms  
(19 rows)  
{% endhighlight %}

## B-tree

#### Usage

B-tree is suitable for all data types, supports sorting, and supports searches greater than, less than, equal to, greater than or equal to, less than or equal to.

The combination of indexing and recursive queries also enables fast sparse retrieval.

#### Example

{% highlight bash %}
postgres=# create table t_btree(id int, info text);  
CREATE TABLE  
postgres=# insert into t_btree select generate_series(1,10000), md5(random()::text) ;  
INSERT 0 10000  
postgres=# create index idx_t_btree_1 on t_btree using btree (id);  
CREATE INDEX  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_btree where id=1;  
                                                          QUERY PLAN                                                             
-------------------------------------------------------------------------------------------------------------------------------  
 Index Scan using idx_t_btree_1 on public.t_btree  (cost=0.29..3.30 rows=1 width=37) (actual time=0.027..0.027 rows=1 loops=1)  
   Output: id, info  
   Index Cond: (t_btree.id = 1)  
   Buffers: shared hit=1 read=2  
 Planning time: 0.292 ms  
 Execution time: 0.050 ms  
(6 rows)  
{% endhighlight %}

## GIN
Gin is an inverted index that stores the VALUE or VALUE element of the indexed field, and the list or tree of the row number.

#### Usage

* When you need to search for VALUE in a multi-valued type, it is suitable for multi-valued types, such as arrays, full-text search, TOKEN. (Depending on the type, search for intersect, contain, greater than, left, right, etc.)
* When the user's data is sparse, if you want to search for a value of VALUE, you can adapt the type supported by btree_gin to support ordinary btree. (Supports btree operators)
* When the user needs to search by any column, gin supports multi-column expansion to separately establish index fields, and at the same time supports bitmapAnd and bitmapOr merge of internal multi-domain indexes to quickly return the requested data by any column.

#### Example. Multi-value type search.

{% highlight bash %}
postgres=# create table t_gin1 (id int, arr int[]);  
CREATE TABLE  
  
postgres=# do language plpgsql $$  
postgres$# declare  
postgres$# begin  
postgres$#   for i in 1..10000 loop  
postgres$#     insert into t_gin1 select i, array(select random()*1000 from generate_series(1,10));  
postgres$#   end loop;  
postgres$# end;  
postgres$# $$;  
DO  
postgres=# select * from t_gin1 limit 3;  
 id |                    arr                      
----+-------------------------------------------  
  1 | {128,700,814,592,414,838,615,827,274,210}  
  2 | {284,452,824,556,132,121,21,705,537,865}  
  3 | {65,185,586,872,627,330,574,227,827,64}  
(3 rows)  
  
postgres=# create index idx_t_gin1_1 on t_gin1 using gin (arr);  
CREATE INDEX  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_gin1 where arr && array[1,2];  
                                                       QUERY PLAN                                                          
-------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_gin1  (cost=8.93..121.24 rows=185 width=65) (actual time=0.058..0.207 rows=186 loops=1)  
   Output: id, arr  
   Recheck Cond: (t_gin1.arr && '{1,2}'::integer[])  
   Heap Blocks: exact=98  
   Buffers: shared hit=103  
   ->  Bitmap Index Scan on idx_t_gin1_1  (cost=0.00..8.89 rows=185 width=0) (actual time=0.042..0.042 rows=186 loops=1)  
         Index Cond: (t_gin1.arr && '{1,2}'::integer[])  
         Buffers: shared hit=5  
 Planning time: 0.208 ms  
 Execution time: 0.245 ms  
(10 rows)  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_gin1 where arr @> array[1,2];  
                                                     QUERY PLAN                                                        
---------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_gin1  (cost=7.51..9.02 rows=1 width=65) (actual time=0.022..0.022 rows=0 loops=1)  
   Output: id, arr  
   Recheck Cond: (t_gin1.arr @> '{1,2}'::integer[])  
   Buffers: shared hit=5  
   ->  Bitmap Index Scan on idx_t_gin1_1  (cost=0.00..7.51 rows=1 width=0) (actual time=0.020..0.020 rows=0 loops=1)  
         Index Cond: (t_gin1.arr @> '{1,2}'::integer[])  
         Buffers: shared hit=5  
 Planning time: 0.116 ms  
 Execution time: 0.044 ms  
(9 rows)  
{% endhighlight %}

#### Example. Single value sparse data search.

{% highlight bash %}
postgres=# create extension btree_gin;  
CREATE EXTENSION  
postgres=# create table t_gin2 (id int, c1 int);  
CREATE TABLE  
postgres=# insert into t_gin2 select generate_series(1,100000), random()*10 ;  
INSERT 0 100000  
postgres=# create index idx_t_gin2_1 on t_gin2 using gin (c1);  
CREATE INDEX  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_gin2 where c1=1;  
                                                         QUERY PLAN                                                            
-----------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_gin2  (cost=84.10..650.63 rows=9883 width=8) (actual time=0.925..3.685 rows=10078 loops=1)  
   Output: id, c1  
   Recheck Cond: (t_gin2.c1 = 1)  
   Heap Blocks: exact=443  
   Buffers: shared hit=448  
   ->  Bitmap Index Scan on idx_t_gin2_1  (cost=0.00..81.62 rows=9883 width=0) (actual time=0.867..0.867 rows=10078 loops=1)  
         Index Cond: (t_gin2.c1 = 1)  
         Buffers: shared hit=5  
 Planning time: 0.252 ms  
 Execution time: 4.234 ms  
(10 rows)  
{% endhighlight %}
 
#### Example. Multi-column arbitrary search.

{% highlight bash %}
postgres=# create table t_gin3 (id int, c1 int, c2 int, c3 int, c4 int, c5 int, c6 int, c7 int, c8 int, c9 int);  
CREATE TABLE  
postgres=# insert into t_gin3 select generate_series(1,100000), random()*10, random()*20, random()*30, random()*40, random()*50, random()*60, random()*70, random()*80, random()*90;  
INSERT 0 100000  
postgres=# create index idx_t_gin3_1 on t_gin3 using gin (c1,c2,c3,c4,c5,c6,c7,c8,c9);  
CREATE INDEX  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_gin3 where c1=1 or c2=1 and c3=1 or c4=1 and (c6=1 or c7=2) or c8=9 or c9=10;  
                                                                                              QUERY PLAN                                                                                                 
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_gin3  (cost=154.03..1364.89 rows=12286 width=40) (actual time=1.931..5.634 rows=12397 loops=1)  
   Output: id, c1, c2, c3, c4, c5, c6, c7, c8, c9  
   Recheck Cond: ((t_gin3.c1 = 1) OR ((t_gin3.c2 = 1) AND (t_gin3.c3 = 1)) OR (((t_gin3.c4 = 1) AND (t_gin3.c6 = 1)) OR ((t_gin3.c4 = 1) AND (t_gin3.c7 = 2))) OR (t_gin3.c8 = 9) OR (t_gin3.c9 = 10))  
   Heap Blocks: exact=834  
   Buffers: shared hit=867  
   ->  BitmapOr  (cost=154.03..154.03 rows=12562 width=0) (actual time=1.825..1.825 rows=0 loops=1)  
         Buffers: shared hit=33  
         ->  Bitmap Index Scan on idx_t_gin3_1  (cost=0.00..83.85 rows=9980 width=0) (actual time=0.904..0.904 rows=10082 loops=1)  
               Index Cond: (t_gin3.c1 = 1)  
               Buffers: shared hit=6  
         ->  Bitmap Index Scan on idx_t_gin3_1  (cost=0.00..9.22 rows=172 width=0) (actual time=0.355..0.355 rows=164 loops=1)  
               Index Cond: ((t_gin3.c2 = 1) AND (t_gin3.c3 = 1))  
               Buffers: shared hit=8  
         ->  BitmapOr  (cost=21.98..21.98 rows=83 width=0) (actual time=0.334..0.334 rows=0 loops=1)  
               Buffers: shared hit=13  
               ->  Bitmap Index Scan on idx_t_gin3_1  (cost=0.00..7.92 rows=42 width=0) (actual time=0.172..0.172 rows=36 loops=1)  
                     Index Cond: ((t_gin3.c4 = 1) AND (t_gin3.c6 = 1))  
                     Buffers: shared hit=6  
               ->  Bitmap Index Scan on idx_t_gin3_1  (cost=0.00..7.91 rows=41 width=0) (actual time=0.162..0.162 rows=27 loops=1)  
                     Index Cond: ((t_gin3.c4 = 1) AND (t_gin3.c7 = 2))  
                     Buffers: shared hit=7  
         ->  Bitmap Index Scan on idx_t_gin3_1  (cost=0.00..14.38 rows=1317 width=0) (actual time=0.124..0.124 rows=1296 loops=1)  
               Index Cond: (t_gin3.c8 = 9)  
               Buffers: shared hit=3  
         ->  Bitmap Index Scan on idx_t_gin3_1  (cost=0.00..12.07 rows=1010 width=0) (actual time=0.102..0.102 rows=1061 loops=1)  
               Index Cond: (t_gin3.c9 = 10)  
               Buffers: shared hit=3  
 Planning time: 0.272 ms  
 Execution time: 6.349 ms  
(29 rows)  
{% endhighlight %}

## GIST
GiST stands for Generalized Search Tree.

It is a balanced, tree-structured access method, that acts as a base template in which to implement arbitrary indexing schemes.

B-trees, R-trees and many other indexing schemes can be implemented in GiST.

#### Usage

GiST is a universal indexing interface. You can use GiST to implement b-tree, r-tree, and other index structures.

Different types support different index retrievals. E.g:

* Geometry type, support location search (include, intersect, up, down, left, etc.), sort by distance.
* range type, support location search (include, intersect, in left and right, etc.).
* IP type, support location search (include, intersect, left and right, etc.).
* Spatial type (PostGIS), support location search (include, intersect, up, down, left, etc.), sorted by distance.
* scalar type, supports sorting by distance

#### Example. Geometry type retrieval.

{% highlight bash %}
postgres=# create table t_gist (id int, pos point);  
CREATE TABLE  
postgres=# insert into t_gist select generate_series(1,100000), point(round((random()*1000)::numeric, 2), round((random()*1000)::numeric, 2));  
INSERT 0 100000  
postgres=# select * from t_gist  limit 3;  
 id |       pos         
----+-----------------  
  1 | (325.43,477.07)  
  2 | (257.65,710.94)  
  3 | (502.42,582.25)  
(3 rows)  
postgres=# create index idx_t_gist_1 on t_gist using gist (pos);  
CREATE INDEX  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_gist where circle '((100,100) 10)'  @> pos;  
                                                       QUERY PLAN                                                         
------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_gist  (cost=2.55..125.54 rows=100 width=20) (actual time=0.072..0.132 rows=46 loops=1)  
   Output: id, pos  
   Recheck Cond: ('<(100,100),10>'::circle @> t_gist.pos)  
   Heap Blocks: exact=41  
   Buffers: shared hit=47  
   ->  Bitmap Index Scan on idx_t_gist_1  (cost=0.00..2.53 rows=100 width=0) (actual time=0.061..0.061 rows=46 loops=1)  
         Index Cond: ('<(100,100),10>'::circle @> t_gist.pos)  
         Buffers: shared hit=6  
 Planning time: 0.147 ms  
 Execution time: 0.167 ms  
(10 rows)  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_gist where circle '((100,100) 1)' @> pos order by pos <-> '(100,100)' limit 10;  
                                                              QUERY PLAN                                                                 
---------------------------------------------------------------------------------------------------------------------------------------  
 Limit  (cost=0.28..14.60 rows=10 width=28) (actual time=0.045..0.048 rows=2 loops=1)  
   Output: id, pos, ((pos <-> '(100,100)'::point))  
   Buffers: shared hit=5  
   ->  Index Scan using idx_t_gist_1 on public.t_gist  (cost=0.28..143.53 rows=100 width=28) (actual time=0.044..0.046 rows=2 loops=1)  
         Output: id, pos, (pos <-> '(100,100)'::point)  
         Index Cond: ('<(100,100),1>'::circle @> t_gist.pos)  
         Order By: (t_gist.pos <-> '(100,100)'::point)  
         Buffers: shared hit=5  
 Planning time: 0.092 ms  
 Execution time: 0.076 ms  
(10 rows)  
{% endhighlight %}

#### Example. Sort by scalar type

{% highlight bash %}
postgres=# create extension btree_gist;  
CREATE EXTENSION  
  
postgres=# create index idx_t_btree_2 on t_btree using gist(id);  
CREATE INDEX  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_btree order by id <-> 100 limit 1;  
                                                                QUERY PLAN                                                                   
-------------------------------------------------------------------------------------------------------------------------------------------  
 Limit  (cost=0.15..0.19 rows=1 width=41) (actual time=0.046..0.046 rows=1 loops=1)  
   Output: id, info, ((id <-> 100))  
   Buffers: shared hit=3  
   ->  Index Scan using idx_t_btree_2 on public.t_btree  (cost=0.15..408.65 rows=10000 width=41) (actual time=0.045..0.045 rows=1 loops=1)  
         Output: id, info, (id <-> 100)  
         Order By: (t_btree.id <-> 100)  
         Buffers: shared hit=3  
 Planning time: 0.085 ms  
 Execution time: 0.076 ms  
(9 rows) 
{% endhighlight %}

## SP-GIST
SP-GiST is an abbreviation for space-partitioned GiST.

SP-GiST supports partitioned search trees, which facilitate development of a wide range of different non-balanced data structures, such as quad-trees, k-d trees, and radix trees (tries).

The common feature of these structures is that they repeatedly divide the search space into partitions that need not be of equal size.

Searches that are well matched to the partitioning rule can be very fast.

SP-GiST is similar to GiST and is a general indexing interface, but SP-GIST uses the method of spatial partitioning, making SP-GiST better support unbalanced data structures, such as quad-trees, k-d tree, radis tree.

#### Usage

* Geometry type, support location search (include, intersect, up, down, left, etc.), sort by distance.
* Range type, support location search (include, intersect, in left and right, etc.).
* IP type, support location search (include, intersect, left and right, etc.).

#### Example. Range type search.

{% highlight bash %}
postgres=# create table t_spgist (id int, rg int4range);  
CREATE TABLE  
  
postgres=# insert into t_spgist select id, int4range(id, id+(random()*200)::int) from generate_series(1,100000) t(id);  
INSERT 0 100000  
postgres=# select * from t_spgist  limit 3;  
 id |   rg      
----+---------  
  1 | [1,138)  
  2 | [2,4)  
  3 | [3,111)  
(3 rows)  
  
postgres=# set maintenance_work_mem ='32GB';  
SET  
postgres=# create index idx_t_spgist_1 on t_spgist using spgist (rg);  
CREATE INDEX  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_spgist where rg && int4range(1,100);  
                                                       QUERY PLAN                                                          
-------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_spgist  (cost=2.55..124.30 rows=99 width=17) (actual time=0.059..0.071 rows=99 loops=1)  
   Output: id, rg  
   Recheck Cond: (t_spgist.rg && '[1,100)'::int4range)  
   Heap Blocks: exact=1  
   Buffers: shared hit=6  
   ->  Bitmap Index Scan on idx_t_spgist_1  (cost=0.00..2.52 rows=99 width=0) (actual time=0.043..0.043 rows=99 loops=1)  
         Index Cond: (t_spgist.rg && '[1,100)'::int4range)  
         Buffers: shared hit=5  
 Planning time: 0.133 ms  
 Execution time: 0.111 ms  
(10 rows)  
  
postgres=# set enable_bitmapscan=off;  
SET  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_spgist where rg && int4range(1,100);  
                                                             QUERY PLAN                                                                
-------------------------------------------------------------------------------------------------------------------------------------  
 Index Scan using idx_t_spgist_1 on public.t_spgist  (cost=0.28..141.51 rows=99 width=17) (actual time=0.021..0.051 rows=99 loops=1)  
   Output: id, rg  
   Index Cond: (t_spgist.rg && '[1,100)'::int4range)  
   Buffers: shared hit=8  
 Planning time: 0.097 ms  
 Execution time: 0.074 ms  
(6 rows)  
{% endhighlight %}

## BRIN
BRIN index is a block-level index. Unlike B-TREE and other indexes, BRIN records do not record the index details in units of line numbers but record the statistical information of each data block or each continuous data block. Therefore, the BRIN index space occupation is particularly small, and the impact on data writing, updating, and deleting is also small.

#### Usage

BRIN is a LOSSLY index. When the value of the indexed column is strongly related to physical storage, the BRIN index works very well.

For example, time-series data, creating a BRIN index on a time or sequence field, and performing an equivalent value and range query are very effective.

#### Example. Range type search.

{% highlight bash %}
postgres=# create table t_brin (id int, info text, crt_time timestamp);  
CREATE TABLE  
postgres=# insert into t_brin select generate_series(1,1000000), md5(random()::text), clock_timestamp();  
INSERT 0 1000000  
  
postgres=# select ctid,* from t_brin limit 3;  
 ctid  | id |               info               |          crt_time            
-------+----+----------------------------------+----------------------------  
 (0,1) |  1 | e48a6cd688b6cc8e86ee858fa993b31b | 2017-06-27 22:50:19.172224  
 (0,2) |  2 | e79c335c679b0bf544e8ba5f01569df7 | 2017-06-27 22:50:19.172319  
 (0,3) |  3 | b75ec6db320891a620097164b751e682 | 2017-06-27 22:50:19.172323  
(3 rows)  
  
  
postgres=# select correlation from pg_stats where tablename='t_brin' and attname='id';  
 correlation   
-------------  
           1  
(1 row)  
  
postgres=# select correlation from pg_stats where tablename='t_brin' and attname='crt_time';  
 correlation   
-------------  
           1  
(1 row)  
  
postgres=# create index idx_t_brin_1 on t_brin using brin (id) with (pages_per_range=1);  
CREATE INDEX  
postgres=# create index idx_t_brin_2 on t_brin using brin (crt_time) with (pages_per_range=1);  
CREATE INDEX  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_brin where id between 100 and 200;  
                                                       QUERY PLAN                                                          
-------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_brin  (cost=43.52..199.90 rows=74 width=45) (actual time=1.858..1.876 rows=101 loops=1)  
   Output: id, info, crt_time  
   Recheck Cond: ((t_brin.id >= 100) AND (t_brin.id <= 200))  
   Rows Removed by Index Recheck: 113  
   Heap Blocks: lossy=2  
   Buffers: shared hit=39  
   ->  Bitmap Index Scan on idx_t_brin_1  (cost=0.00..43.50 rows=107 width=0) (actual time=1.840..1.840 rows=20 loops=1)  
         Index Cond: ((t_brin.id >= 100) AND (t_brin.id <= 200))  
         Buffers: shared hit=37  
 Planning time: 0.174 ms  
 Execution time: 1.908 ms  
(11 rows)  
  
postgres=# explain (analyze,verbose,timing,costs,buffers) select * from t_brin where crt_time between '2017-06-27 22:50:19.172224' and '2017-06-27 22:50:19.182224';  
                                                                                       QUERY PLAN                                                                                          
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on public.t_brin  (cost=59.63..4433.67 rows=4474 width=45) (actual time=1.860..2.603 rows=4920 loops=1)  
   Output: id, info, crt_time  
   Recheck Cond: ((t_brin.crt_time >= '2017-06-27 22:50:19.172224'::timestamp without time zone) AND (t_brin.crt_time <= '2017-06-27 22:50:19.182224'::timestamp without time zone))  
   Rows Removed by Index Recheck: 2  
   Heap Blocks: lossy=46  
   Buffers: shared hit=98  
   ->  Bitmap Index Scan on idx_t_brin_2  (cost=0.00..58.51 rows=4494 width=0) (actual time=1.848..1.848 rows=460 loops=1)  
         Index Cond: ((t_brin.crt_time >= '2017-06-27 22:50:19.172224'::timestamp without time zone) AND (t_brin.crt_time <= '2017-06-27 22:50:19.182224'::timestamp without time zone))  
         Buffers: shared hit=52  
 Planning time: 0.091 ms  
 Execution time: 2.884 ms  
(11 rows)  
{% endhighlight %}

## RUM
Rum is an indexing plug-in, open-source by Postgrespro, suitable for full-text retrieval, and belongs to an enhanced version of GIN.

Enhancements include:

1. In the RUM index, the position information of lexeme is stored, so when ranking is calculated, there is no need to return to the table query (and GIN needs to return to the table query).

2. RUM supports phrase search, but GIN cannot.

3. In a RUM index, users are allowed to store fields VALUE other than ctid (line number) in the posting tree, such as timestamps.

This makes RUM not only support the full-text search supported by GIN, but also supports the calculation of text similarity values, sorting by similarity, etc. At the same time support position matching, for example (speed and passion, you can use "speed" <2> "passion" to match, but GIN index can not do it)


#### Example

{% highlight bash %}
postgres=# create table rum_test(c1 tsvector);  
CREATE TABLE  
  
postgres=# CREATE INDEX rumidx ON rum_test USING rum (c1 rum_tsvector_ops);  
CREATE INDEX  
  
$ vi test.sql  
insert into rum_test select to_tsvector(string_agg(c1::text,',')) from  (select (100000*random())::int from generate_series(1,100)) t(c1);  
  
$ pgbench -M prepared -n -r -P 1 -f ./test.sql -c 50 -j 50 -t 200000  
  
postgres=# explain analyze select * from rum_test where c1 @@ to_tsquery('english','1 | 2') order by c1 <=> to_tsquery('english','1 | 2') offset 19000 limit 100;  
                                                               QUERY PLAN                                                                  
-----------------------------------------------------------------------------------------------------------------------------------------  
 Limit  (cost=18988.45..19088.30 rows=100 width=1391) (actual time=58.912..59.165 rows=100 loops=1)  
   ->  Index Scan using rumidx on rum_test  (cost=16.00..99620.35 rows=99749 width=1391) (actual time=16.426..57.892 rows=19100 loops=1)  
         Index Cond: (c1 @@ '''1'' | ''2'''::tsquery)  
         Order By: (c1 <=> '''1'' | ''2'''::tsquery)  
 Planning time: 0.133 ms  
 Execution time: 59.220 ms  
(6 rows)  
  
postgres=# create table test15(c1 tsvector);  
CREATE TABLE  
postgres=# insert into test15 values (to_tsvector('jiebacfg', 'hello china, i''m digoal')), (to_tsvector('jiebacfg', 'hello world, i''m postgresql')), (to_tsvector('jiebacfg', 'how are you, i''m digoal'));  
INSERT 0 3  
postgres=# select * from test15;  
                         c1                            
-----------------------------------------------------  
 ' ':2,5,9 'china':3 'digoal':10 'hello':1 'm':8  
 ' ':2,5,9 'hello':1 'm':8 'postgresql':10 'world':3  
 ' ':2,4,7,11 'digoal':12 'm':10  
(3 rows)  
postgres=# create index idx_test15 on test15 using rum(c1 rum_tsvector_ops);  
CREATE INDEX  
postgres=# select *,c1 <=> to_tsquery('hello') from test15;  
                         c1                          | ?column?   
-----------------------------------------------------+----------  
 ' ':2,5,9 'china':3 'digoal':10 'hello':1 'm':8     |  16.4493  
 ' ':2,5,9 'hello':1 'm':8 'postgresql':10 'world':3 |  16.4493  
 ' ':2,4,7,11 'digoal':12 'm':10                     | Infinity  
(3 rows)  
postgres=# explain select *,c1 <=> to_tsquery('postgresql') from test15 order by c1 <=> to_tsquery('postgresql');  
                                   QUERY PLAN                                     
--------------------------------------------------------------------------------  
 Index Scan using idx_test15 on test15  (cost=3600.25..3609.06 rows=3 width=36)  
   Order By: (c1 <=> to_tsquery('postgresql'::text))  
(2 rows) 
{% endhighlight %}

## Bloom
The bloom index interface is an index interface constructed by PostgreSQL based on bloom filters. It belongs to the loss index and can converge the result set (exclude results that absolutely do not meet the conditions, and then select the results that meet the conditions from the remaining results). Supports equivalent query for any combination of columns.

Bloom stores signatures. The larger the signature, the more space it consumes, but the more accurate the exclusion. There are pros and cons.

{% highlight bash %}
CREATE INDEX bloomidx ON tbloom USING bloom (i1, i2, i3)
        WITH (length = 80, col1 = 2, col2 = 2, col3 = 4);

Signature length 80 bit, maximum allowed 4096 bits
col1-col32, specify the bits of each column respectively, the default length is 2, the maximum allowed 4095 bits.
{% endhighlight %}

Bloom provides an index access method based on Bloom filters.

A Bloom filter is a space-efficient data structure that is used to test whether an element is a member of a set. In the case of an index access method, it allows fast exclusion of non-matching tuples via signatures whose size is determined at index creation.

This type of index is most useful when a table has many attributes and queries test arbitrary combinations of them.

## Usage

The bloom index is suitable for multiple columns and any combination of queries.

#### Example

{% highlight bash %}
=# CREATE TABLE tbloom AS  
   SELECT  
     (random() * 1000000)::int as i1,  
     (random() * 1000000)::int as i2,  
     (random() * 1000000)::int as i3,  
     (random() * 1000000)::int as i4,  
     (random() * 1000000)::int as i5,  
     (random() * 1000000)::int as i6  
   FROM  
  generate_series(1,10000000);  
SELECT 10000000  
=# CREATE INDEX bloomidx ON tbloom USING bloom (i1, i2, i3, i4, i5, i6);  
CREATE INDEX  
=# SELECT pg_size_pretty(pg_relation_size('bloomidx'));  
 pg_size_pretty  
----------------  
 153 MB  
(1 row)  
=# CREATE index btreeidx ON tbloom (i1, i2, i3, i4, i5, i6);  
CREATE INDEX  
=# SELECT pg_size_pretty(pg_relation_size('btreeidx'));  
 pg_size_pretty  
----------------  
 387 MB  
(1 row)  
  
=# EXPLAIN ANALYZE SELECT * FROM tbloom WHERE i2 = 898732 AND i5 = 123451;  
                                                        QUERY PLAN  
---------------------------------------------------------------------------------------------------------------------------  
 Bitmap Heap Scan on tbloom  (cost=178435.39..178439.41 rows=1 width=24) (actual time=76.698..76.698 rows=0 loops=1)  
   Recheck Cond: ((i2 = 898732) AND (i5 = 123451))  
   Rows Removed by Index Recheck: 2439  
   Heap Blocks: exact=2408  
   ->  Bitmap Index Scan on bloomidx  (cost=0.00..178435.39 rows=1 width=0) (actual time=72.455..72.455 rows=2439 loops=1)  
         Index Cond: ((i2 = 898732) AND (i5 = 123451))  
 Planning time: 0.475 ms  
 Execution time: 76.778 ms  
(8 rows)  
{% endhighlight %}

## Summary

In this post, we looked at the following PostgreSql indexes:

* [Hash](#hash)
* [B-tree](#b-tree)
* [GiST](#gist) 
* [SP-GiST](#sp-gist) 
* [GIN](#gin)
* [RUM](#rum) 
* [BRIN](#brin)
* [Bloom](#bloom)


 











