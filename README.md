sqlparser
=========

This is a toy and simple sql parser, LALR(1) code generated by lex and bison. 

Currently, partially DML semtanics covered, remnant (mostly DDL and administrator command) would fulfill in future.


## code

Core structure is `Stmt` and `Item` in sql.h

## compile

	make

## use

### format sql
	./format your_sql_file

### create index for this sql
	./index your_sql_file

### example

	hoterran@hoterran-laptop:~/Projects/sqlparser$ cat test/test_groupby_orderby.sql 
	select a.id, b.id, c.name from a, b where id = name group by xxx, zzz, ddd 
	having count(*) > 1 order by a desc,b asc ,c desc limit 1;

	hoterran@hoterran-laptop:~/Projects/sqlparser$ ./format test/test_groupby_orderby.sql 
	SQL parse worked
	================
	SELECT
		a.id,
		b.id,
		c.name
	FROM
		a,
		b
	WHERE
		id = name    
	GROUPBY
		xxx,
		zzz,
		ddd
	HAVING 
		func(*) > 1
	ORDER BY
		a DESC,
		b,
		c DESC
	LIMIT
		1


	hoterran@hoterran-laptop:~/Projects/sqlparser$ cat test/test_in_query.sql 
	select a.z,c.d from t1 a, t2 b where 
	a.id = 111 and (b.id = 222 or z.id = 333) and b.id = kkk
	and b.id in (select c.id from c);

	hoterran@hoterran-laptop:~/Projects/sqlparser$ ./format test/test_in_query.sql 
	SQL parse worked
	================
	SELECT
		a.z,
		c.d
	FROM
		t1 AS a,
		t2 AS b
	WHERE
		(
			(
				(
					a.id = 111
				)
				AND
				(
					(
						b.id = 222
					)
					OR
					(
						z.id = 333
					)
				)
			)
			AND
			(
				b.id = kkk
			)
		)
		AND
		(
			b.id
			IN (
				 SELECT
					c.id
				FROM
					c
			)
		)

	hoterran@hoterran-laptop:~/Projects/sqlparser$ cat test/test_index.sql 
	select * from aaa a, bbb b where a.id = 1 and a.name = "aaaa" and a.kkk = "zzz" and b.id = a.id;
	hoterran@hoterran-laptop:~/Projects/sqlparser$ ./index test/test_index.sql 
	create index index_aaa_id_name_kkk on aaa(id, name, kkk);
	create index index_bbb_id on bbb(id);



## TODO

    * out,left,right join
    * union
    * 
