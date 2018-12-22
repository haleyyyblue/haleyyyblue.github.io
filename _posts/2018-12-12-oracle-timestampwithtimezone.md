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

## Tips
Oracle's timestamp with timezone data type can be inserted and updated with various timezone. and there are several ways to determine timezone of the timestamp data. Your timestamp with timezone data will be inserted as following condition.

### About timezone
1) If user inserted NULL and default is SYSTIMESTAMP, then timezone will be DB Server's timezone.
2) If App uses some tool(i.e Hibernate), then timezone will be dependent on the tool's behavior.
3) If user executes alter command for timezone before inserting timestamp with timezone, that will be the timezone.

### About return value
1) If user only selects HH:MI:SS, then there is a risk we can't compare time exactly in case different timezone data exists.

## Sample SQL
### Create Test Table

Let's make test table which has timestamp with time zone data type.


{% highlight sql %}
create table TEST (
ID_TEST varchar2(20) not null,
CREATED timestamp with time zone DEFAULT SYSTIMESTAMP NOT NULL,
UPDATED timestamp with time zone DEFAULT SYSTIMESTAMP NOT NULL,
constraint PK_TEST primary key ( ID_TEST )
);
{% endhighlight %}

### Insert 1 row by default (systimestamp) which uses db server's timezone

{% highlight sql %}
set linesize 300
col id_test for a20
col created for a40
col updated for a40

insert into TEST (ID_TEST) values ('A');
{% endhighlight %}

### Insert 1 row by specifying UTC offset
{% highlight sql %}
insert into TEST (ID_TEST,CREATED) values('B','12-DEC-18 06.52.00.000000 PM +00:00');
{% endhighlight %}

### Insert after setting time_zone for the session
{% highlight sql %}
alter session set time_zone='+08:00';
insert into TEST (ID_TEST,CREATED) values('C',12-DEC-18 06.52.00.000000 PM +00:00');
{% endhighlight %}

### Check Result
{% highlight sql %}
select id_test,created from test;
ID_TEST        CREATED
-------------  --------------------------------
A              2018-12-12 05:21:42 PM +09:00
B              2018-12-12 04:51:13 PM +00:00
C              2018-12-12 04:51:13 PM -08:00
{% endhighlight %}
We can see different timestamp data has been inserted with different timezone.

### Select only timestamp
{% highlight sql %}
select id_test,to_char(created,'yyyy-mm-dd hh24:mi:ss') as created from test ;
ID_TEST        CREATED
-------------  --------------------------
A              2018-12-12 17:21:42
B              2018-12-12 16:51:13
C              2018-12-12 16:51:13
{% endhighlight %}
Here we can see we might misunderstand exact time of each row because B and C are in different timezone, even if their timestamp is same.

### Select with timezone
{% highlight sql %}
select id_test,to_char(created,'yyyy-mm'dd hh24:mi:ss tzh:tzm') as created from test ;
ID_TEST        CREATED
-------------  --------------------------------
A              2018-12-12 17:21:42 PM +09:00
B              2018-12-12 16:51:13 PM +00:00
C              2018-12-12 16:51:13 PM -08:00
{% endhighlight %}

so you may want to convert timezone so that we can assure every row is in same timezone.
Let's update records to different timezone

### Update records to different timezone
{% highlight sql %}
update test set created = created at time zone '+09:00' ;
select created from test ;
ID_TEST        CREATED
-------------  --------------------------------
A              2018-12-12 08:21:42 AM +09:00
B              2018-12-12 04:51:13 PM +09:00
C              2018-12-12 12:51:13 AM +09:00
{% endhighlight %}
