### 一个MySQL约束的建表小练习
```sql
-- 商店售货微型表设计 - 使用MySQL约束

CREATE DATABASE shop_db; # 建库

DROP TABLE goods
CREATE TABLE goods( # 建立商品表
	goods_id INT PRIMARY KEY, # id作为主键
	goods_name VARCHAR(64) NOT NULL DEFAULT '',
	unitprice DECIMAL(10,2) NOT NULL DEFAULT 0 CHECK(unitprice <= 9999.99), # check约束
	category INT NOT NULL DEFAULT 0,
	provider VARCHAR(64) NOT NULL DEFAULT ''
);

CREATE TABLE customer( # 建立客户表
	customer_id VARCHAR(64) PRIMARY KEY, # id作为主键
	`name` VARCHAR(64) NOT NULL DEFAULT '',
	address VARCHAR(64) NOT NULL DEFAULT '',
	email VARCHAR(64) UNIQUE NOT NULL DEFAULT '', # email不能重复用unique约束
	sex ENUM ('男','女') NOT NULL ,# 枚举类型
	card_Id CHAR(18)	
);

CREATE TABLE purchase(
	order_id INT UNSIGNED PRIMARY KEY,
	customer_id VARCHAR(64) NOT NULL DEFAULT '', # 订单的客户编号需要有外键约束
	goods_id INT NOT NULL DEFAULT 0, # 订单的商品编号需要有外键约束
	nums INT NOT NULL DEFAULT 0,
	
	FOREIGN KEY (customer_id) REFERENCES customer(customer_id),
	FOREIGN KEY (goods_id) REFERENCES goods(goods_id)
);





```