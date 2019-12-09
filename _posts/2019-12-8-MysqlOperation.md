---
dlayout:     post
title:      Basic Operations in MySQL
subtitle:   MySQL数据库操作方式
date:       2019-12-8
author:     Kylin
header-img: img/MySQL-OpenShift.jpg
catalog: true
tags:
    - MySQL
    - Database

---



# Basic Operations in MySQL



### Terminal 登陆

```bash
mysql -u admin -p
```



### 查看数据库列表并选中

```bash
show databases;

use testdb;
```



### 查看当前数据库Table列表

```bash
show tables;
```



### 显示表简介与显示表内容

```bash
desc some_table;


select * from some_table;
```



### Trigger 使用

> root下使用

```bash
SET GLOBAL log_bin_trust_function_creators = 1;
```

> 两个实例表定义

```
CREATE TABLE `users` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8mb4 DEFAULT NULL,
  `add_time` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `name` (`name`(250)) USING BTREE
) ENGINE=MyISAM AUTO_INCREMENT=1000001 DEFAULT CHARSET=latin1;

CREATE TABLE `logs` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `log` varchar(255) DEFAULT NULL COMMENT '日志说明',
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT='日志表';
```

> 触发器定义

```
DELIMITER $
CREATE TRIGGER user_log AFTER INSERT ON users FOR EACH ROW
BEGIN
DECLARE s1 VARCHAR(40)character set utf8;
DECLARE s2 VARCHAR(20) character set utf8;#后面发现中文字符编码出现乱码，这里设置字符集
SET s2 = " is created";
SET s1 = CONCAT(NEW.name,s2);     #函数CONCAT可以将字符串连接
INSERT INTO logs(log) values(s1);
END $
DELIMITER ;
```

> 验证

```
 INSERT INTO users(name,add_time) VALUES("kylin",11233);
```

> 查看Trigger

```
show triggers;
```

> 删除Trigger

```
drop trigger user_log;#删除触发器
```

> 拒绝插入的Trigger

```mysql
DELIMITER $
CREATE TRIGGER test1 AFTER INSERT ON User FOR EACH ROW
BEGIN
DELETE FROM User WHERE user_id = NEW.user_id;
END $
DELIMITER ;
```



### Procedure 使用

- Procedure 定义

```sql
delimiter 
CREATE PROCEDURE buy100ya(IN x INTEGER)
BEGIN
    if ((select count(*) from (select stock from Product where product_price<=100)AS temp where stock<x)!=0) THEN
        signal sqlstate '45000' set message_text = 'Stack too Low';
	else
        update Product set stock=stock-x where product_price<=100;
    end if;
END
```

- 调用

```mysql
call procedure_name(parameters);
```



### Java 接口进行SQL数据操作(在Eclipse中操作为例)

附上Java这部分的原始博客: [https://www.cnblogs.com/biehongli/p/5988246.html](https://www.cnblogs.com/biehongli/p/5988246.html)

- 添加 sql-connect 到 Buildpath的Libraries/Moudulepath

下载链接: 

- 封装 API 接口(可以跳过)

```java
package Sqlconn;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class Sqlconn {
	private static String url="jdbc:mysql://localhost:3306/testdb?useSSL=false";//生命数据库的url(地址)
    private static String user="xxxxx";//数据库账号
    private static String password="xxxxx";//数据库密码
    private static String driver="com.mysql.jdbc.Driver";//数据库的驱动
    
    /**
     * 
     * @return
     * @throws Exception
     */
    public Connection getCon() throws Exception{
        Class.forName(driver);//加载数据库驱动
        Connection con=DriverManager.getConnection(url, user, password);
        return con;
    }
    
    /**
     * 
     * @param con
     * @throws Exception
     */
    public void close(Connection con) throws Exception{
        if(con!=null){
            con.close();
        }    
    }
}
```

- 执行简单 sql 操作（确保 mysql 处于打开状态）

```java
package hello;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.awt.List;
import java.sql.Connection;
import java.sql.DriverManager; 
import java.sql.Statement;
import java.util.Arrays;

import Sqlconn.Sqlconn;

import java.sql.ResultSet;

public class Main {  
	public static void main(String args[]) throws Exception {  
    	Sqlconn db=new Sqlconn();
        String sql="insert into test values(\"5\",\"Ding\",\"90\");";//生成一条sql语句
        Connection con=db.getCon();//获取数据库的连接
        Statement stmt=con.createStatement();//创建一个Statement连接
        int result=stmt.executeUpdate(sql);//执行sql语句
        System.out.println("执行了"+result+"数据");
        stmt.close();//关闭顺序是先关闭小的，后关闭大的
        con.close();
	}
}  
```

- 自定义结构体（用于java内的完整性约束判断）

```java
class Person {
    private int id;
    private String name;
    private float score;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public float getScore() {
        return score;
    }
    public void setScore(float score) {
        this.score = score;
    }
    
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    
    public Person(int id, String name, float score) {
        super();
        this.id = id;
        this.name = name;
        this.score = score;
    } 
}
```

- 自定义异常抛出（用于约束性判断截止）

```java
class WrongInputException extends Exception { 
    WrongInputException(String s) {
        super(s);
    }
}
```

