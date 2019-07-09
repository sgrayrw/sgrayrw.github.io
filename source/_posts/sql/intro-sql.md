---
title: Intro SQL
date: 2019-07-09
tags: SQL
---

## SELECT

{% codeblock lang:sql %}
-- individual column
select col from table; -- `;` is optional in single statement

-- multiple columns
select col1, col2, col3
from table;

-- all columns
select * from table;

-- distinct rows
select distinct col from table;

-- limit results
select col from table
limit 3, 4; -- start from row 3 (4th row), show 4 rows
{% endcodeblock %}

<!-- more -->

## ORDER BY

`order by` should generally be the last clause in a statement.

{% codeblock lang:sql %}
-- sort column
select col from table order by col;

-- order by multiple cols
select col1, col2, col3
from table
order by col2, col3; -- first by col2, then col3

-- order by col position
select col1, col2, col3
from table
order by 2, 3; -- first by col2, then col3

-- sort direction
select col1, col2, col3
from
order by col2 desc, col3; -- first by col2 descending, then col3 ascending (default)
{% endcodeblock %}

## WHERE

Better perform data filtering in DB rather than client to increase performance.

{% codeblock lang:sql %}
select col1, col2
from table
where col2 = x; -- operators including `>`, `<=`, `between`, `is null`, etc.
				-- e.g.
				-- where `col2` between 5 and 10;

-- null rows are not returned when checking !=
-- to return null rows, use
select col from table where col != x or col is null;

-- `and`, `or`
select col1, col2
from table
where col1 = x and col2 = y;

-- `in`
select col1, col2
from table
where col1 in (x, y);

-- `not`
select col1, col2
from table
where not col1 = x;
{% endcodeblock %}

## LIKE
Use `like` for search patterns including wildcards (通配符).

`%` matches 0-n chars
{% codeblock lang:sql %}
select col from table
where col like 'ray%'; -- start by `ray`

select email from persons
where email like 'a%@rayrw.com'; -- matches `a@rayrw.com`, `a123@rayrw.com`.

-- `%` does NOT match null rows
-- sometimes need to add `%` to the end of pattern as DBMS might add whitespace to the end of the field to fill the row.
{% endcodeblock %}

`_` matches 1 char

`[]` matches a charset
{% codeblock lang:sql %}
select col from table
where col like '[AB]%'; -- matches `Ahh`, `Bye`.

-- `[^AB]` matches any char other than `A` and `B`.
{% endcodeblock %}

## Calculated Fields

Better format data in DB rather than client to increase performance.

{% codeblock lang:sql %}
-- concatenate
select concat(col1, '(', col2, ')') from table
-- output: `col1 (col2)`

-- `as`: alias
select concat(col1, '(', col2, ')')
as my_alias
from table

-- math calculation
select col1, col2, col3, col1*col2*col3 as result
from table


{% endcodeblock %}