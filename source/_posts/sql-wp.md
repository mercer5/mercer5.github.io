---
title: sql-wp
date: 2020-02-13 20:39:04
categories: sql
tags: 
- sql
---

# sql wp

## 网址

http://xuesql.cn/



## tip

服务SELECT查询语法

```sql
SELECT column, another_column, … 
FROM mytable 
WHERE condition(s)
ORDER BY column ASC/DESC 
LIMIT num_limit OFFSET num_offset;
```



用INNER JOIN 连接表的语法

```sql
SELECT column, another_table_column, … 
FROM mytable （主表） 
INNER/LEFT/RIGHT/FULL JOIN another_table （要连接的表）    
	ON mytable.id = another_table.id (主键连接，两个相同的连成1条) 
WHERE condition(s) 
ORDER BY column, … ASC/DESC 
LIMIT num_limit OFFSET num_offset;
```



在查询条件中处理 NULL

```sql
SELECT column, another_column, … 
FROM mytable 
WHERE column IS/IS NOT NULL
AND/OR another_condition 
AND/OR …;
```



属性列和表取别名的例子

```sql
SELECT column AS better_column_name, … 
FROM a_long_widgets_table_name AS mywidgets 
INNER JOIN widget_sales
	ON mywidgets.id = widget_sales.widget_id;
```



对全部结果数据做统计

```sql
SELECT AGG_FUNC(\column_or_expression\) AS aggregate_description, … 
FROM mytable 
WHERE constraint_expression;
```



用分组的方式统计

```sql
先对数据做WHERE，后对结果做分组
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, … 
FROM mytable 
WHERE constraint_expression 
GROUP BY column;
```

| Function                    | Description                                                  |
| --------------------------- | ------------------------------------------------------------ |
| COUNT(*****), COUNT(column) | 计数！COUNT(*) 统计数据行数，COUNT(column) 统计column非NULL的行数. |
| MIN(column)                 | 找column最小的一行.                                          |
| MAX(column)                 | 找column最大的一行.                                          |
| AVG(column)                 | 对column所有行取平均值.                                      |
| SUM(column)                 | 对column所有行求和.                                          |



用HAVING进行筛选

```sql
可以对分组之后的数据再做SELECT筛选
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, … 
FROM mytable 
WHERE condition 
GROUP BY column HAVING \group_condition\;
```



完整的SELECT查询

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), … 
FROM mytable    
JOIN another_table      
	ON mytable.column = another_table.column    
WHERE constraint_expression   
GROUP BY column    
HAVING constraint_expression    
ORDER BY column ASC/DESC    
LIMIT count OFFSET COUNT;
```



## lesson 0

照着抄就行了



## lesson 1 select

1. 找到所有电影的名称`title`

   `SELECT title FROM movies;`

2. 找到所有电影的导演

   `SELECT director FROM movies;`

3. 找到所有电影的名称和导演

   `SELECT title,director FROM movies;`

4. 找到所有电影的名称和上映年份

   `SELECT title,year FROM movies;`

5. 找到所有电影的所有信息

   `SELECT * FROM movies;`

6. 找到所有电影的名称,Id和播放时长

   `SELECT title,id,length_minutes FROM movies;`

7. 请列出所有电影的ID,名称和出版国(即美国)

   `SELECT title,id,'美国' as country FROM movies;`



## lesson 2 condition

1. 找到`id`为6的电影

   ```sql
   SELECT * FROM movies
   where id = 6;
   ```

2. 找到在2000-2010年间`year`上映的电影

   ```sql
   SELECT * FROM movies
   where year between 2000 and 2010;
   ```

3. 找到不是在2000-2010年间`year`上映的电影

   ```sql
   SELECT * FROM movies
   where year not between 2000 and 2010;
   ```

4. 找到头5部电影

   ```sql
   SELECT * FROM movies
   where id<6;
   ```

5. 找到2010（含）年之后的电影里片长小于两个小时的片子

   ```sql
   SELECT * FROM movies
   where year >=2010
       and length_minutes<120;
   ```

6. 找到99年和09年的电影,只要列出年份和片长看下

   ```sql
   SELECT year,length_minutes FROM movies
   where year=1999 or year=2009;
   ```

   

## lesson 3 condition

1. 找到所有`Toy Story`系列电影 

   ```sql
   SELECT * FROM movies
   where title like 'toy story%'
   ```

2. 找到所有`John Lasseter`导演的电影

   ```sql
   SELECT * FROM movies
   where director like 'john lasseter'
   ```

3. 找到所有不是`John Lasseter`导演的电影

   ```sql
   SELECT * FROM movies
   where director not like 'john lasseter'
   ```

4. 找到所有电影名为` "WALL-" `开头的电影

   ```sql
   SELECT * FROM movies
   where title like 'wall-%'
   ```

5. 有一部98年电影中文名《虫虫危机》请给我找出来

   ```sql
   SELECT * FROM movies
   where year = 1998
   ```

6. 找出所有Pete导演的电影,只要列出电影名,导演名和年份就可以

   ```sql
   SELECT title,director,year FROM movies
   where director like 'pete%'
   ```

7. John Lasseter导演了两个系列，一个Car系列一个Toy Story系列,请帮我列出这John Lasseter导演两个系列千禧年之后的电影

   ```sql
   SELECT * FROM movies
   where director like 'john lasseter'
       and year>2000
   ```



## lesson 4 filtering and sorting

1. 按导演名`排重`列出所有电影，并按导演名正序排列

   ```sql
   SELECT distinct director FROM movies
   ```

2. 列出按上映年份`最新`上线的4部电影

   ```sql
   SELECT * FROM movies
   order by year desc
   limit 4
   ```

3. 按电影名字母序`升序`排列，列出前5部电影

   ```sql
   SELECT * FROM movies
   order by title asc
   limit 5
   ```

4. 按电影名字母序升序排列，列出上一题`之后`的5部电影

   ```sql
   SELECT * FROM movies
   order by title asc
   limit 5 offset 5
   ```

5. 如果按片长排列，John Lasseter导演导过片长第3长的电影是哪部，列出名字即可

   ```sql
   SELECT title FROM movies
   where director like 'john lasseter'
   order by length_minutes asc
   limit 1 offset 2
   ```

6. 按导演名字母升序,如果导演名相同按年份降序,取前10部电影给我

   ```sql
   SELECT * FROM movies
   order by director asc,year desc
   limit 10
   ```



## lesson 5 review

1. 列出所有加拿大人的`Canadian`信息(包括所有字段)

   ```sql
   SELECT * FROM north_american_cities
   where country like 'canada'
   ```

2. 列出所有美国`United States`的城市按纬度从北到南排序(包括所有字段)

   ```sql
   SELECT * FROM north_american_cities
   where country like 'united states'
   ```

3. 列出所有在`Chicago`西部的城市，从西到东排序(包括所有字段)

   ```sql
   SELECT * FROM north_american_cities
   where Longitude < -87.629798
   order by Longitude asc
   ```

4. 用人口数`population`排序,列出墨西哥`Mexico`最大的2个城市(包括所有字段)

   ```sql
   SELECT * FROM north_american_cities
   where country like 'mexico'
   order by population desc
   limit 2 
   ```

5. 列出美国`United States`人口3-4位的两个城市和他们的人口(包括所有字段)

   ```sql
   SELECT * FROM north_american_cities
   where country like 'united states'
   order by population desc
   limit 2 offset 2
   ```

6. 北美所有城市,请按国家名字母序从A-Z再按人口从多到少排列看下前10位的城市(包括所有字段)

   ```sql
   SELECT * FROM north_american_cities
   order by country asc ,population desc
   limit 10
   ```

   

## lesson 6 inner join

1. 找到所有电影的线下`Domestic_sales`和线上销售额

   ```sql
   SELECT * FROM movies
   inner join Boxoffice 
       on movies.id = boxoffice.movie_id
   ```

2. 找到所有线上销售额比线下销售大的电影

   ```sql
   SELECT * FROM movies
   inner join Boxoffice 
       on movies.id = boxoffice.movie_id
   where International_sales>Domestic_sales
   ```

3. 找出所有电影按市场占有率`rating`倒序排列

   ```sql
   SELECT * FROM movies
   inner join Boxoffice 
       on movies.id = boxoffice.movie_id
   order by rating desc
   ```

4. 每部电影按线上销售额比较,排名最靠前的导演是谁,线上销量多少

   ```sql
   SELECT director,International_sales FROM movies
   inner join Boxoffice 
       on movies.id = boxoffice.movie_id
   order by International_sales desc
   limit 1
   ```



## lesson 7 outer join

1. 找到所有有雇员的办公室(`buildings`)名字 

   ```sql
   SELECT distinct building 
   FROM employees
   left join buildings
       on employees.building = buildings.building_name
   ```

2. 找到所有办公室和他们的最大容量

   ```sql
   SELECT Building_name,capacity
   FROM  Buildings
   order by capacity desc
   ```

3. 找到所有办公室里的所有角色（包含没有雇员的）,并做唯一输出(`DISTINCT`)

   ```sql
   select distinct building_name,role 
   from Buildings  
   left join employees  
       on Buildings.building_name =employees.building;
   ```

4. 找到所有有雇员的办公室(`buildings`)和对应的容量

   ```sql
   select distinct building,capacity 
   from employees  
   left join buildings  
       on employees.building = buildings.building_name 
   where building is not null
   ```



## lesson 8 null

1. 找到雇员里还没有分配办公室的(列出名字和角色就可以)

   ```sql
   SELECT name,role FROM employees
   where building is null
   ```

2. 找到还没有雇员的办公室

   ```sql
   SELECT distinct building_name FROM buildings
   left join employees
       on employees.building=buildings.building_name
   where name is null
   ```



## Lesson 9 表达式

1. 列出所有的电影ID,名字和销售总额(以百万美元为单位计算)

   ```sql
   SELECT id,title,(Domestic_sales+International_sales)/1000000 as total FROM movies as mov
   inner join boxoffice as box
       on mov.id = box.movie_id
   ```

2. 列出所有的电影ID,名字和市场指数(`Rating`的10倍为市场指数)

   ```sql
   SELECT id,title,rating*10 as point FROM movies as mov
   inner join boxoffice as box
       on mov.id = box.movie_id
   ```

3. 列出所有偶数年份的电影，需要电影ID,名字和年份

   ```sql
   SELECT id,title,year as point FROM movies as mov
   inner join boxoffice as box
       on mov.id = box.movie_id
   where year%2==0
   ```

4. John Lasseter导演的每部电影每分钟值多少钱,告诉我最高的3个电影名和价值就可以

   ```sql
   SELECT title,(Domestic_sales+International_sales)/length_minutes as price_per_min FROM movies as mov
   inner join boxoffice as box
       on mov.id = box.movie_id
   where director like 'john lasseter'
   order by price_per_min desc
   limit 3
   ```

5. 电影名最长的3部电影和他们的总销量是多少

   ```sql
   SELECT title,length(title),Domestic_sales+International_sales as total FROM movies as mov
   inner join boxoffice as box
       on mov.id = box.movie_id
   order by length(title) desc
   limit 3
   ```



## lesson 10 统计1

1. 找出就职年份最高的雇员(列出雇员名字+年份）

   ```sql
   SELECT name,years_employed as x FROM employees
   order by x desc
   limit 1
   ```

2. 按角色(`Role`)统计一下每个角色的平均就职年份

   ```sql
   SELECT role,avg(years_employed) FROM employees 
   group by role
   ```

3. 按办公室名字总计一下就职年份总和

   ```sql
   SELECT sum(years_employed),building FROM employees
   group by building
   ```

4. 每栋办公室按人数排名,不要统计无办公室的雇员

   ```sql
   SELECT building,count() as count FROM employees 
   where building is not null 
   group by building
   ```

5. 就职1,3,5,7年的人分别占总人数的百分比率是多少(给出年份和比率"50%" 记为 50)

   ==这题没做出来: (,不知道错哪里==
   
   ```sql
SELECT Years_employed,round(count(Years_employed)/15.0*100,0)
   FROM employees
   where years_employed in (1,3,5,7)
   group by Years_employed
   ```
   
   ```sql
   round(数值,小数点保留的位数)
   ```
   
   

## lesson 11 统计2

1. 统计一下Artist角色的雇员数量

   ```sql
   SELECT count(role) FROM employees
   group by role
   having role like 'artist'
   ```

2. 按角色统计一下每个角色的雇员数量

   ```sql
   SELECT role,count(role) FROM employees
   group by role
   ```

3. 算出Engineer角色的就职年份总计

   ```sql
   SELECT sum(Years_employed) FROM employees 
   where role like 'engineer'
   ```

4. 按角色分组算出每个角色按有办公室和没办公室的统计人数(列出角色，数量，有无办公室,注意一个角色如果部分有办公室，部分没有需分开统计）

   ```sql
   SELECT role,count(building) as number,"yes" FROM employees a 
   group by role 
   union 
   select role,count(name),"no" from employees b 
   where building is null 
   group by role
   
   ```

5. 按角色和就职年份统计人数,年份按0-3，3-6，6-9这种阶梯分组，最后按角色+阶梯分组排序

   ```sql
   
   ```



## lesson 12 

1. 统计出每一个导演的电影数量（列出导演名字和数量）

   ```sql
   SELECT director,count(*) FROM movies
   group by director
   ```

2. 统计一下每个导演的销售总额(列出导演名字和销售总额)

   ```sql
   SELECT director,sum(International_sales)+sum(domestic_sales) FROM movies
   inner join boxoffice
       on movies.id = boxoffice.movie_id
   group by director
   ```

3. 按导演分组计算销售总额,求出平均销售额冠军（统计结果过滤掉只有单部电影的导演，列出导演名，总销量，电影数量，平均销量)

   ```sql
   SELECT director,sum(Domestic_sales+International_sales)as sales,count(director),sum(Domestic_sales+International_sales)/count(director) as avgsales
   FROM movies as m
   left join Boxoffice as b
   on b.movie_id=m.id
   group by director
   having count(Director)>1
   order by Avgsales desc
   limit 1
   ```

4. 找出每部电影和单部电影销售冠军之间的销售差，列出电影名，销售额差额

   ```sql
   SELECT title,(select max(Domestic_sales+International_sales) from Boxoffice)-(Domestic_sales+International_sales) as sales
   FROM movies as m
   left join Boxoffice as b
   on b.movie_id=m.id
   ```

   

