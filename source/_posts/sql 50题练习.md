---
title: sql 50题练习
date: 2021-11-03 21:13:38
tags: [sql]
categories: [数据库]
---
__表结构如下：__
<!--more-->
{% spoiler "点击显/隐内容" %}
```sql
create table Student(SId varchar(10),Sname varchar(10),Sage datetime,Ssex varchar(10));
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-12-20' , '男');
insert into Student values('04' , '李云' , '1990-12-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-01-01' , '女');
insert into Student values('07' , '郑竹' , '1989-01-01' , '女');
insert into Student values('09' , '张三' , '2017-12-20' , '女');
insert into Student values('10' , '李四' , '2017-12-25' , '女');
insert into Student values('11' , '李四' , '2012-06-06' , '女');
insert into Student values('12' , '赵六' , '2013-06-13' , '女');
insert into Student values('13' , '孙七' , '2014-06-01' , '女');


create table Course(CId varchar(10),Cname nvarchar(10),TId varchar(10));
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');

create table Teacher(TId varchar(10),Tname varchar(10));
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');
select week(CURDATE())+1;

create table SC(SId varchar(10),CId varchar(10),score decimal(18,1));
insert into SC values('01' , '01' , 80);
insert into SC values('01' , '02' , 90);
insert into SC values('01' , '03' , 99);
insert into SC values('02' , '01' , 70);
insert into SC values('02' , '02' , 60);
insert into SC values('02' , '03' , 80);
insert into SC values('03' , '01' , 80);
insert into SC values('03' , '02' , 80);
insert into SC values('03' , '03' , 80);
insert into SC values('04' , '01' , 50);
insert into SC values('04' , '02' , 30);
insert into SC values('04' , '03' , 20);
insert into SC values('05' , '01' , 76);
insert into SC values('05' , '02' , 87);
insert into SC values('06' , '01' , 31);
insert into SC values('06' , '03' , 34);
insert into SC values('07' , '02' , 89);
insert into SC values('07' , '03' , 98);
```
{% endspoiler %}

1.sql语句的执行顺序为
>当一个查询语句同时出现了where,group by,having,order by的时候，执行顺序和编写顺序是：
1.执行where xx对全表数据做筛选，返回第1个结果集。
2.针对第1个结果集使用group by分组，返回第2个结果集。
3.针对第2个结果集中的数据执行select xx返回第3个结果集。
4.针对第3个结集执行having xx进行筛选，返回第4个结果集。
5.针对第4个结果集排序,获得最终结果。

2.SUM(),MIN(),Max()这类的，我们称作是聚合函数。那么我们不能在where子句中使用这些函数，为什么呢？
> 聚集函数也叫列函数，它们都是基于整列数据进行计算的，而where子句则是对数据行进行过滤的(这里过滤是在一个记录里边过滤的,基于"行")，在筛选过程中依赖“基于已经筛选完毕的数据得出的计算结果”是一种悖论，这是行不通的。更简单地说，因为聚集函数要对全列数据时行计算，因而使用它的前提是：结果集已经确定！

而where子句还处于“确定”结果集的过程中，因而不能使用聚集函数。

1.查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数
```sql
select student.*,r.* from Student,  ( 
select t1.sid,t1.score as c1 ,t2.score as c2 from 
    (select SId, score  from sc where sc.CId = '01') as t1, 
    (select SId ,score  from sc where sc.CId = '02') as t2
  where t1.score > t2.score  and t1.sid = t2.sid) as r where  Student.SId = r.SId ;
```
2.查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩
```sql
select student.sid, sname ,r.avger from student join ( 
  select sid ,avg(score) as avger  from sc group by sc.Sid  having avg(score)>  60 )as r on 
  student.sid = r.sid; 
```
3.查询在 SC 表存在成绩的学生信息
```sql
select DISTINCT student.*#DISTINCT去重
from student,sc
where student.SId=sc.SId
```
4.查询所有同学的学生编号、学生姓名、选课总数、所有课程的成绩总和
```sql
select student.sid ,student.sname ,r.s1,r.s2  from student join  
(select sid,count(sid) as s1 ,sum(score)as s2 from sc group by sc.sid ) as r  
on  student.sid  = r.sid ;
```
5.查询「李」姓老师的数量
 ```sql 
 select count(tid) from teacher where tname like "李%";
```
6.查询学过「张三」老师授课的同学的信息
```sql
select student.* ,s.CId from student,
(select sc.sid ,sc.cid from sc  where  sc.CId = '02'  group by sc.SId)as s 
where student.sid = s.sid;
```
7.查询有成绩学生且选课总数大于等于2的同学的信息
```sql
SELECT STUDENT.* FROM STUDENT,
(select ID.SID FROM  (SELECT SC.SID, count(score) as total  from sc group by sc.sid )AS ID ,
(SELECT count(course.cid) as tcid from course)  as t1 WHERE ID.TOTAL < T1.TCID)AS D WHERE STUDENT.SID = D.SID;
```
8.查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息
```sql
select * from student 
where student.sid in (
    select sc.sid from sc 
    where sc.cid in(
        select sc.cid from sc 
        where sc.sid = '01'
    )
);
```
9.查询没学过"张三"老师讲授的任一门课程的学生姓名
```sql
select student.sname from student where sid not in(
select sid  from sc ,
(select cid  from course ,
(select tid from teacher where tname = '张三') as id
where course.TId = id.tid) as cid1 where sc.cid = cid1.cid);
```
10.查询学生的总成绩，并进行排名
```sql
select student.sid,student.sname,r.s from student ,
(select sid,sum(score) as s  from sc group by sid)  as r 
where student.sid = r.sid order by r.s desc;
```
11.查询各科目优秀人数，分数在85分以上的人数为优秀
```sql
select r.cid, r.cname, sum(case r.score when  r.score >= 85 and r.score <= 100 then 1 else 0 end )as "优秀"
from 
 (select *  from sc  join course
on sc.cid = course.cid) as r group by r.cid
 ;
 ```
 
 12.查询语文成绩前三名
 ``` sql
select student.Sname, sc.sid ,sc.score from student ,sc,
(select cid from co where cname = '语文') as b 
where (sc.cid = b.cid and student.sid = sc.sid) order by score desc limit 3;
```
13.查询各科成绩最高分、最低分和平均分：
```sql
select max(score)as '最高分',min(score) as '最低分',avg(score) as '平均分' from sc group by sc.cid;
```
14.查询每门课程被选修的学生数
```sql
select sc.cid,sum(sc.sid) from sc group by sc.CId ;
```

15.查询男生数、女生数量
```sql
select ssex ,count(sid) from student group by ssex;
```
16.查询名字含有风的学生的信息
```sql
select  * from student where sname  like '%风%';
```
17.查询同名学生数量
```sql 
select r.sname,r.num from (select  student.*,count(sname) as num  from student group by sname) as r
where r.num >1;
```
17.嵌套查询列出同名的全部学生的信息
```sql
select * from student
where sname in (
select sname from student
group by sname
having count(*)>1
);
```
18.查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩
 ```sql
 select student .*,r.avger from 
 (select   sid, avg(score)as avger  from sc group by sid)as r,student   
 where student.sid = r.sid and r.avger >= 85;
 ```
 ```sql
 select student.sid, student.sname, AVG(sc.score) as aver from student, sc
where student.sid = sc.sid
group by sc.sid
having aver > 85;
```
19.查询课程编号为 01 且课程成绩在 80 分及以上的学生的学号和姓名
 ```sql 
 select student.sid,student.sname,r.* from (select  sid ,score ,sc.cid from sc where sc.cid = '01' and sc.score >= 80)as r,student where student.sid = r.sid;
 ```
 ```sql
 select student.sid,student.sname ,sc.score
from student,sc
where cid="01"
and score>=80
```
20.查询同名学生名单，并统计同名人数
```sql
select b.*,c.num from (select student.* from student join student as a on
 student.sid != a.sid 
 and student.sname = a.sname)as b join (select  student.sname,count(student.sname) as num from student group by 
 student.sname) as c on b.sname = c.sname; 
```
21.查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息
```sql
select student.* from (select sc.sid from ((select sc.sid, sc.cid from  sc  where sc.SId = '01')as r  
join sc on sc.sid != r.sid and sc.CId in  (r.cid)))as a 
,student  where student.sid = a.sid group by  a.sid ;

select * from student 
 where student.sid in (
    select sc.sid from sc 
    where sc.cid in(
        select sc.cid from sc 
        where sc.sid = '01'
    )
);
```
22.查询每门功成绩最好的前两名
```sql
select *
from sc as t1
where (select count(*) from sc as t2 where t1.CId=t2.CId and t2.score >t1.score)<2
ORDER BY t1.CId
```


23.查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
```sql
 select sc.sid,sc.cid,sc.score from sc ,sc as t where sc.sid =  t.sid and t.CId != sc.CId and sc.score = t.score group by sc.cid; 
 ```
24.成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
 ```sql
 select student.*,r.cid,r.score from student, (select * from  sc where sc.CId = 
 (select co. cid from  co where co.TId = (select teacher.tid from teacher  where teacher.Tname = '张三'))
 order by sc.score desc limit 1 )as r where  student.sid = r.sid;
```
25.查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
 ```sql
 select t1.* from sc as t1,sc as t2 where 
  t1.SId=t2.SId and t1.CId!=t2.CId and t1.score =t2.score group by t1.cid ;
  ```