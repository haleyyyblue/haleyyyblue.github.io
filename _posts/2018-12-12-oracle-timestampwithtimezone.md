---
layout: post
title:  "Work with Oracle Timestamp with timezone"
date:   2018-12-12
excerpt: "Oracle has timestamp with time zone data type and Here's some examples to understand difference between default, UTC offset, other time zone value "
tag:
- Oracle 
- timestampwithtimezone
comments: true
--- 

## Create Test Table

Let's make test table which has timestamp with time zone data type.


{% highlight sql %}
create table TEST (
ID_TEST varchar2(20) not null,
CREATED timestamp with time zone DEFAULT SYSTIMESTAMP NOT NULL,
UPDATED timestamp with time zone DEFAULT SYSTIMESTAMP NOT NULL,
constraint PK_TEST primary key ( ID_TEST )
);
{% endhighlight %}

## Insert 1 row by default (systimestamp) which uses db server's timezone

{% highlight sql %}
set linesize 300
col id_test for a20
col created for a40
col updated for a40

insert into TEST (ID_TEST) values ('A');
{% endhighlight %}

## Insert 1 row by specifying UTC offset
{% highlight sql %}
insert into TEST (ID_TEST,CREATED) values('B','12-DEC-18 06.52.00.000000 PM +00:00');
{% endhighlight %}

## Insert after setting time_zone for the session
{% highlight sql %}
alter session set time_zone='+08:00';
insert into TEST (ID_TEST,CREATED) values('C',12-DEC-18 06.52.00.000000 PM +00:00');
{% endhighlight %}

## Check Result
{% highlight sql %}
select id_test,created from test;
{% endhighlight %}

{% highlight sql %}
{% endhighlight %}
