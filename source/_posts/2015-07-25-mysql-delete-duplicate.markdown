---
layout: post
title: "MySQL删除重复记录"
date: 2015-07-25 01:14:32 +0800
comments: true
categories: [mysql] 
---

###准备数据
```
CREATE TABLE `test_01` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(45) NOT NULL,
  `value` varchar(45) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
 
insert into test_01 (name, value) values ('a','pp');
insert into test_01 (name, value) values ('a','pp');
insert into test_01 (name, value) values ('b','iii');
insert into test_01 (name, value) values ('b','pp');
insert into test_01 (name, value) values ('b','pp');
insert into test_01 (name, value) values ('c','pp');
insert into test_01 (name, value) values ('c','pp');
insert into test_01 (name, value) values ('c','iii');
```
数据准备完成后，结果集如下：

```
mysql> select * from test_01;
+----+------+-------+
| id | name | value |
+----+------+-------+
|  1 | a    | pp    |
|  2 | a    | pp    |
|  3 | b    | iii   |
|  4 | b    | pp    |
|  5 | b    | pp    |
|  6 | c    | pp    |
|  7 | c    | pp    |
|  8 | c    | iii   |
+----+------+-------+
8 rows in set (0.00 sec)
```

要求删除重复元素，并保留id较小的元素，期望得到的结果集如下：
```
mysql> select * from test_01;
+----+------+-------+
| id | name | value |
+----+------+-------+
|  1 | a    | pp    |
|  3 | b    | iii   |
|  4 | b    | pp    |
|  6 | c    | pp    |
|  8 | c    | iii   |
+----+------+-------+
5 rows in set (0.00 sec)
```

###方案

方案1：
```
DELETE FROM test_01
WHERE
    id NOT IN (SELECT
        min_id
    FROM
        (SELECT
            MIN(id) AS min_id
        FROM
            test_01
        GROUP BY name , value) AS tmp);
```

方案2：
```
DELETE a FROM test_01 a
        LEFT JOIN
    (SELECT
        MIN(id) AS id
    FROM
        test_01
    GROUP BY name , value) AS b ON a.id = b.id
WHERE
    b.id IS NULL; 
```




