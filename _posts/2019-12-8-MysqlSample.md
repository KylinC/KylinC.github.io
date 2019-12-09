---
dlayout:    post
title:      A Complete Example for Handle MySQL
subtitle:   一个MySQL完整使用样例（从ER到代码、触发器）
date:       2019-12-8
author:     Kylin
header-img: img/bg-database.jpg
catalog: true
tags:
    - MySQL
    - Database
---



# An Example to Handle MySQL



## 1-1、E-R关系构建

- 为实现3NF，我们删除了如下标成红色的信息：

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.24.44.png)

- 构建的E-R图如下:

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.40.00.png)

模型的sql文件参考附件 **extra/extra1/model.sql**.



## 2-1、使用触发器实现业务逻辑

### 分析解答

- Condition1: 在订单详情表插入前，需要检查每件商品的数据量和库存的关系。如果商品数量超过库存，则跳过该商品继续下单。否则，更新商品库存，并删除购物车详情中的商品信息。

- Condition2: 在订单信息插入后，检查购物车是否清空。若清空，则删除购物车有关信息。否则保留购物车中信息。

> Trigger:

```mysql
DELIMITER $
CREATE TRIGGER trigger1 AFTER INSERT ON OrderDetail FOR EACH ROW
BEGIN
DECLARE tmp INT;
IF NEW.product_num > (select stock from Product where product_id = NEW.product_id) THEN
    Signal sqlstate '45000' set message_text = 'incalid data';
ELSE 
    UPDATE Product SET stock = stock - NEW.product_num WHERE product_id = NEW.product_id;
    DELETE FROM CartDetail WHERE (cart_id = (select cart_id from AllOrder WHERE order_id = NEW.order_id) AND product_id = NEW.product_id AND product_num = NEW.product_num);
    IF (select count(*) from(SELECT * FROM CartDetail WHERE (cart_id = (select cart_id from AllOrder WHERE order_id = NEW.order_id)))AS temp)=0 THEN
        DELETE FROM Cart WHERE cart_id = (select cart_id from AllOrder WHERE order_id = NEW.order_id);
    END IF;
END IF;
END $
DELIMITER ;
```

该 trigger脚本已附加在提交文件 **trigger.sql** 内，关于其运行，可以顺序运行 **extra/extra1** 内的 **data_init.sql** 、**test_procedure.sql** 脚本进行测试，在测试中可以看到该trgger对于超库存订单的无报错忽略、cart_detail的链接删除、清空购物车的自动删除等作业要求等操作。

### 测试样例输出：

```mysql
mysql> SELECT "TEST CONDITION1:";
+------------------+
| TEST CONDITION1: |
+------------------+
| TEST CONDITION1: |
+------------------+
1 row in set (0.00 sec)

mysql> 
mysql> -- before:
mysql> 
mysql> SELECT "BEFORE:";
+---------+
| BEFORE: |
+---------+
| BEFORE: |
+---------+
1 row in set (0.01 sec)

mysql> 
mysql> select * from Product;
+------------+--------------+---------------+-------+
| product_id | product_name | product_price | stock |
+------------+--------------+---------------+-------+
|          1 | coco         |            50 |     3 |
|          2 | cocopro      |           100 |     1 |
|          3 | littlecoco   |            10 |     2 |
+------------+--------------+---------------+-------+
3 rows in set (0.00 sec)

mysql> select * from CartDetail;
+---------------+---------+------------+-------------+---------------------+--------------+
| cartdetail_id | cart_id | product_id | product_num | cart_time           | Cart_cart_id |
+---------------+---------+------------+-------------+---------------------+--------------+
|             1 |       1 |          1 |           1 | 2019-12-08 18:53:50 |            1 |
|             2 |       1 |          2 |           2 | 2019-12-08 18:53:50 |            1 |
|             3 |       1 |          3 |           1 | 2019-12-08 18:53:50 |            1 |
+---------------+---------+------------+-------------+---------------------+--------------+
3 rows in set (0.00 sec)

mysql> select * from Cart;
+---------+---------+------------+--------------+
| cart_id | user_id | cart_price | User_user_id |
+---------+---------+------------+--------------+
|       1 |       1 |        260 |            1 |
+---------+---------+------------+--------------+
1 row in set (0.00 sec)

mysql> 
mysql> INSERT INTO OrderDetail(orderdetail_id,order_id,product_id,product_num,AllOrder_order_id) VALUES(1,1,1,1,1);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO OrderDetail(orderdetail_id,order_id,product_id,product_num,AllOrder_order_id) VALUES(2,1,2,2,1);
ERROR 1644 (45000): incalid data
mysql> 
mysql> SELECT "AFTER 1 useful and un-useful:";
+-------------------------------+
| AFTER 1 useful and un-useful: |
+-------------------------------+
| AFTER 1 useful and un-useful: |
+-------------------------------+
1 row in set (0.00 sec)

mysql> 
mysql> select * from Product;
+------------+--------------+---------------+-------+
| product_id | product_name | product_price | stock |
+------------+--------------+---------------+-------+
|          1 | coco         |            50 |     2 |
|          2 | cocopro      |           100 |     1 |
|          3 | littlecoco   |            10 |     2 |
+------------+--------------+---------------+-------+
3 rows in set (0.00 sec)

mysql> select * from CartDetail;
+---------------+---------+------------+-------------+---------------------+--------------+
| cartdetail_id | cart_id | product_id | product_num | cart_time           | Cart_cart_id |
+---------------+---------+------------+-------------+---------------------+--------------+
|             2 |       1 |          2 |           2 | 2019-12-08 18:53:50 |            1 |
|             3 |       1 |          3 |           1 | 2019-12-08 18:53:50 |            1 |
+---------------+---------+------------+-------------+---------------------+--------------+
2 rows in set (0.00 sec)

mysql> select * from Cart;
+---------+---------+------------+--------------+
| cart_id | user_id | cart_price | User_user_id |
+---------+---------+------------+--------------+
|       1 |       1 |        260 |            1 |
+---------+---------+------------+--------------+
1 row in set (0.00 sec)

mysql> 
mysql> SELECT "AFTER Clear Cart details:";
+---------------------------+
| AFTER Clear Cart details: |
+---------------------------+
| AFTER Clear Cart details: |
+---------------------------+
1 row in set (0.00 sec)

mysql> DELETE FROM CartDetail WHERE (cartdetail_id = 2);
Query OK, 1 row affected (0.00 sec)

mysql> INSERT INTO OrderDetail(orderdetail_id,order_id,product_id,product_num,AllOrder_order_id) VALUES(2,1,3,1,1);
Query OK, 1 row affected (0.00 sec)

mysql> 
mysql> select * from Product;
+------------+--------------+---------------+-------+
| product_id | product_name | product_price | stock |
+------------+--------------+---------------+-------+
|          1 | coco         |            50 |     2 |
|          2 | cocopro      |           100 |     1 |
|          3 | littlecoco   |            10 |     1 |
+------------+--------------+---------------+-------+
3 rows in set (0.00 sec)

mysql> select * from CartDetail;
Empty set (0.00 sec)

mysql> select * from Cart;
Empty set (0.00 sec)
```

### 样例截图

- 初始化数据（Product、CartDetails、Cart）

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.07.32.png)

- Condition1测试（关于Condition参考上文）

>  第一条测试是有效的购物信息（在购物车内、没有超过库存），第二条信息是无效信息（超过库存）

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.10.22.png)

> 执行结果表明系统忽略了无效信息，执行了有效信息，而且cart_detail内执行了对应删除。Product内的对应库存减少了购买量。

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.14.37.png)

- Condition2测试

> 首先删除第二条订单，否则无法清空1号购物车的购物详情。然后执行添加OrderDetail操作，之后1号购物车的OrderDetail被清空。

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.10.44.png)

> 可以看到，1号购物车在清空后于Cart表中被删除。Product内的对应库存减少了购买量。

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.20.25.png)

> 测试代码包含在 extra/extra1中，包括 **model.sql**（模型构建文件）、**test1.sql**(测试数据初始化文件)、**test2.sql**（测试脚本文件）



## 2-2、使用触发器实现约束定义

- Constraint: 用户密码必须保护字母大小写、数字；用户性别必须为male、female之一。



> Trigger

```mysql
drop trigger if exists user_insert_trigger;
DELIMITER $
create trigger user_insert_trigger after insert on user 
for each row
begin
	if (new.user_gender != "male" and new.user_gender != "female") then 
        signal sqlstate '45000' set message_text = 'user_gender: male or female';
	elseif (new.user_password not regexp '[A-Z]' or new.user_password not regexp '[a-z]' or new.user_password not regexp '[0-9]') then 
		signal sqlstate '45000' set message_text = 'user_password must contain capital, lowercase and number';
    end if;
end $
DELIMITER ;
```

> 测试:

> 从测试中看出，不满足条件信息会出现提示，并且不会执行插入。

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%887.44.01.png)

Trigger代码整合入附件 **trigger.sql**



## 3、使用 Java API 复现 Trigger 实现的逻辑

### 脚本

测试代码查看 **JAVA/Main.java**(执行文件) 以及 **JAVA/Sqlconn.java**（自己封装的数据库连接包）

运行时把两个文件放入一个project后，执行 **Main.java** 即可

### 创新点

- 封装的连接类

```java
package Sqlconn;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class Sqlconn {
	private static String url="jdbc:mysql://localhost:3306/mydb?useSSL=false";
    private static String user="admin";
    private static String password="admin";
    private static String driver="com.mysql.jdbc.Driver";
    
    public Connection getCon() throws Exception{
        Class.forName(driver);
        Connection con=DriverManager.getConnection(url, user, password);
        return con;
    }

    public void close(Statement stmt,Connection con) throws Exception{
        if(stmt!=null){
            stmt.close();
            if(con!=null){
                con.close();
            }
        }
            
    }
}
```

- 自定义Java数据体(以User定义为例)

```java
class User{
	private int user_id;
	private String user_name;
	private String user_password;
	private String user_gender;
	private String user_address;
	
	public boolean hasDigit(String content) {  
		boolean flag = false;  
		Pattern p = Pattern.compile(".*\\d+.*");  
		Matcher m = p.matcher(content);  
		if (m.matches())  
		flag = true;  
		return flag;  
	} 
	
	public boolean hasLow(String content) {
		boolean flag = false;  
		Pattern p = Pattern.compile(".*[a-z].*");  
		Matcher m = p.matcher(content);  
		if (m.matches())  
		flag = true;  
		return flag; 
	}
	
	public boolean hasCapital(String content) {
		boolean flag = false;  
		Pattern p = Pattern.compile(".*[A-Z].*");  
		Matcher m = p.matcher(content);  
		if (m.matches())  
		flag = true;  
		return flag; 
	}
	
	public int get_user_id() {
		return user_id;
	}
	public String get_user_name() {
		return user_name;
	}
	public String get_user_password() {
		return user_password;
	}
	public String get_user_gender() {
		return user_gender;
	}
	public String get_user_address() {
		return user_address;
	}
	
	public User(int user_id, 
			String user_name, 
			String user_password,
			String user_gender,
			String user_address)throws WrongInputException{
		super();
		if(user_gender!="male" & user_gender!="female") {
        	throw new WrongInputException("user_gender must be 'male' or 'female'!");
        }
		if(!hasDigit(user_password) |
				!hasLow(user_password) |
				!hasCapital(user_password)) {
        	throw new WrongInputException("password must contains Low/Capital letter and digital!");
        }
		this.user_id = user_id;
		this.user_name=user_name;
		this.user_password = user_password;
		this.user_gender = user_gender;
		this.user_address = user_address;
	}
}
```

- 自定义异常抛出（实现约束）

```java
class WrongInputException extends Exception { 
    WrongInputException(String s) {
        super(s);
    }
}
```



### 测试样例

- 超过库存异常报错

>  先依据 **extra/extra3** 中的 **model.sql** 和 **test1.sql** 初始化数据库及数据，则有数据库中 Product表

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.30.11.png)

> 然后定义一条订单要求购买product_id=3的商品30件（超过库存），则抛出自定义异常"Stock is too low！"

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.29.45.png)

- 下单成功删除对应库存及购物车详情

> 首先初始化数据中存在三个购物车信息，和对应库存

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.46.34.png)

> 执行Java处的下单，对应购物车详情3，即对littlecoco下单1个

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.46.14.png)

> java处无异常抛出，查看数据库发现对应littlecoco库存减少1，对应购物车信息被删除

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.46.42.png)

- 购物车清空后自动删除

> 初始化数据中，CartDetail还剩一个条目，对应Cart中有对应一个购物车，对应下单库存充足

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.58.47.png)

> 执行最后一个购物车详情下单，之后Java显示下单成功

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.57.46.png)

> 之后查看数据库，可以发现，对应购物车与购物车详情被删除，库存减少

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%889.58.59.png)

- 完整性约束

> 在Java中，我们完整书写了各类完整性异常抛出，如用户性别异常抛出：

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%8810.04.12.png)

> 密码必须包含字母大小写以及数字异常抛出：

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%8810.04.30.png)

> 除此之外，还定义了下单数量判断等约束，用Java的异常抛出是优雅的



## 4、使用存储过程实现业务逻辑

### 问题一（批量购买）分析解答

- 问题一：假设某用户购买了该系统中所有价格不超过100元的商品X件，其中X是存储过程的参数之一。需要修改商品表中满足条件商品的剩余库存量。判断X是否小于0，若是，使用SIGNAL SQLSTATE ‘45000’实现报错。若商品库存不小于X，对商品库存进行修改；否则，使用SIGNAL SQLSTATE ‘45000’实现报错。调用该存储过程，验证其正确性。

>Procedure

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

直接使用判断句进行判断即可。

### 测试样例

> 从测试中可以看出，procedure进行了判断，在发现不超过100元的商品"cocopro"后，判断其库存超过x=100，之后执行了库存更新。

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%888.51.35.png)

代码整合入 **procedure.sql**,测试整合入附件 **extra/extra2/problem1test.sql**.



- 问题二：商家需要知道某个时间点Y（DATETIME）后的平均订单金额，其中Y是存储过程的参数之一。调用该存储过程，验证其正确性。

> Procedure

```mysql
delimiter 
CREATE PROCEDURE timelineya(IN x datetime,OUT a double)
BEGIN
    declare b double;
    declare c double;
    set b = (select sum(order_price) from ALLOrder where order_time>x);
    set c = (select count(*) from ALLOrder where order_time>x);
    set a=b/c;
END
```

直接求和求平均，或用avg()函数。

### 测试样例

> 从测试中可以看出，所求值为输入datetime之后订单的平均值

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-08-%E6%88%AA%E5%B1%8F2019-12-08%E4%B8%8B%E5%8D%889.25.00.png)

代码整合入 **procedure.sql**,测试整合入附件 **extra/extra2/problem2test.sql**.



## 5、附加题

### 第一部分

- 订单评价机制

> 设计一个满足3nf的带评价表模型

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%8810.28.06.png)

- 退货机制

这部分只需将OrderDetail还原为库存与CartDetail，即之前所做工作的逆过程，在**AdditionProblem/Main.java** 和 **AdditionProblem/Sqlconn.java** 中实现。

- 会员打折机制

通过在User中定义对应的会员折扣，在JAVA中定义update_order方法对Order进行更新

```java
private static int update_order(int user_id)
		throws Exception{
		Connection con=db.getCon();
		Statement stmt=con.createStatement();
		String getorderid = "Select * from AllOrder Where user_id="+user_id;
		ResultSet rs =  stmt.executeQuery(getorderid);
		int order_id = 0;
        if(rs.next()) {
        	order_id = rs.getInt("order_id");
        }
        String getcount = "Select * from User where user_id="+user_id;
        rs =  stmt.executeQuery(getcount);
		float count = 0;
        if(rs.next()) {
        	count = rs.getFloat("count");
        }
		String getsum = "Select * from OrderDetail where order_id="+order_id;
		rs =  stmt.executeQuery(getsum);
		int product_id = 0;
		int product_num =0;
        if(rs.next()) {
        	product_id = rs.getInt("product_id");
        	product_num = rs.getInt("product_num");
        }
		String getprice = "Select * from Product where product_id="+product_id;
//		System.out.print(getprice+"\n");
		rs =  stmt.executeQuery(getprice);
		int product_price =0;
        if(rs.next()) {
        	product_price = rs.getInt("product_price");
        }
//        System.out.println(count+"\n");
//        System.out.println(product_price+"\n");
//        System.out.println(product_num+"\n");
		float num_price = count*product_price*product_num;
		
		String updatesql = "UPDATE AllOrder SET order_price="+num_price+" Where user_id="+user_id;
		int result=stmt.executeUpdate(updatesql);
		db.close(stmt,con);
	    return result;
	}
```

> 我们可以在Java IDE中执行更新脚本

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%8811.46.58.png)

> 显示更新成功后查看数据库，发现已经打了6折

![](http://kylinhub.oss-cn-shanghai.aliyuncs.com/2019-12-09-%E6%88%AA%E5%B1%8F2019-12-09%E4%B8%8B%E5%8D%8811.48.35.png)

所有附加题文件已放入 **AdditionProblem**



### 第二部分

对于改进后的数据库，已经可以实现基本现实操作，即在前面的Trigger已经完整定义了具有现实意义的约束关系。

