# week 3

*2021-12-18 yuheng wu*

[TOC]

这周主要是延续上周的内容，内容依然是和查询相关，但是我们在查询中可能会遇到一些时候，我们想要了解到每一个有共同特性部分的一些属性，这时如果还像之前一样查询就会非常繁琐，举个粒子:

## function(聚合函数)

由此我们引入另一种更方便的机制，类似于编程语言中的函数（function），sql语言一样拥有自己的函数，通过直接使用这些函数，我们能方便的执行一些我们最常使用的一些操作：

- COUNT()
- MIN()
- MAX()
- AVG()
- SUM()

以上函数常和DISTINCT一起使用来排除一些重复的情况，比如我们如果想查询选修了课程的学生人数，这是我们到E表中去查询

`select count(xh) from E`

这就带来了一个问题，一个学生选了多门课，记录都存在于E中，这就会把同一个学生重复算入其中，采取DISTINCT就能避免重复的问题

`select count(distinct xh) from E`

 ## GROUP BY

sql标准中 **select有三种情况**

1. select语句中没有使用分组和聚合，那么select是对查询结果做一个投影（projection）

   `select name from S where sex = 'male'`

2. select语句中没有使用分组但是使用了聚合函数，此时是对查询结果做聚合操作

   `select count(*) count_male, avg(age) avg_age from S where sex = 'male'`

3. select语句使用了分组子句和聚合操作，此时是对每一个分组做聚合操作

   `select cno, count(*) from SC group by cno` （此处要投影的列有一些注意点如下）

   - 一般使用了分组(group)就会使用聚合，但是聚合不一定会有分组(group)
   - 同一个聚合操作有多个值，必须使用分组（group)





***

## Quiz

1. 查询学生的总人数

   ```sql
   select count(xh)
   from S;
   ```

2. 查询选修了课程的学生人数

   ```sql
   select count(distinct xh)
   from E;
   ```

3. 计算1号课程的学生的平均春季

   ```sql
   select avg(zpcj)
   from E
   where kh = 08305001;
   ```

4. 求各个课程号及相应的选课人数

   ```sql
   select kh, count(xh)
   from E
   group by kh;
   ```

5. 求各个课程号及相应的课程成绩在90分以上的学生人数

   ```sql
   select kh, count(xh)
   from E
   where zpcj >= 90
   group by kh;
   ```

6. 选修了2门及以上的学生学号

   ```sql
   select xh
   from E
   group by xh
   having count(kh) >=2;
   ```

7. 查询选修了2门及以上课程的学生学号及选修门数，查询结果按门数降序排列，若门数相同，按学号升序排列

   ```sql
   select xh, count(kh) as num
   from E
   group by xh
   having count(kh) >=2
   order by num DESC, xh ASC;
   ```

8. 查询有3门以上课程在90分以上的学生的学号及90分以上的课程数

   ```sql
   select xh, count(kh)
   from E
   where zpcj >= 90
   group by xh
   having count(kh) >= 3;
   ```

9. 所有成绩在80以上的学生的学号

   ```sql
   select xh
   from E
   group by xh
   having min(zpcj) >= 80;
   ```

10. 有成绩在90以上的学生的学号

    ```sql
    select xh
    from E
    group by xh
    having min(zpcj) >= 90;
    ```

    