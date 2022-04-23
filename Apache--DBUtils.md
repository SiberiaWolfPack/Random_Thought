### Apache--DBUtils
1. 如何引出使用它的？
* 关闭connection后，resultSet无法使用，也就无法复用，但是如果不及时关闭，别的程序来连接他时，就会推迟，我们肯定想要尽量早点关闭它。
* 只能用一次resultSet，因此它不利于管理数据。
* resultSet使用不方便，各种接口其实比较来说不是所见即所得。

2. JavaBean（PoJO，Domain）
* 让数据库表中的属性和类的成员一一对应，这样就可以把结果集数据封装到一个类对象中，多次结果集就可以封装到一个ArrayList中。

3. ApDBUtils查询
* QueryRunner类：封装了SQL的执行，是线程安全的。可以实现增删改查和批处理。
* ResultSetHandler接口：该接口用于处理结果集，将数据按要求转换为另一种形式。
* 测试程序
```java
package JDBC;

import Utils.JDBCUtilsByDruid;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.testng.annotations.Test;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

// 使用dbutils + druid
@Test
public class DBUtils_USE {

    // 返回结果是多行的情况
    public void testQueryMany() throws SQLException {
        // 1. 得到连接
        Connection connection = JDBCUtilsByDruid.getConnection();
        // 2. 使用DBUtils类和接口，先引入jar文件
        // 3. 创建QueryRunner
        QueryRunner queryRunner = new QueryRunner();
        // 4. 执行sql，返回ArrayList结果集
        String sql = "select * from actor where id >= ?";

        // query方法就是执行sql语句，得到resultset，封装到Arraylist集合中
        // new BeanListHandler<>(Actor.class)，传入Actor.class要进行反射机制获取Actor类的属性
        // 1就是给sql语句中的？赋值的
        List<Actor> list = queryRunner.query(connection, sql, new BeanListHandler<>(Actor.class), 1);

        System.out.println("输出集合的信息：");
        // 5. do something
        for (Actor actor : list) {
            System.out.println(actor);
        }

        // 6. 释放资源
        JDBCUtilsByDruid.close(null,null,connection);
    }
}
```
* 实体类
```java
package JDBC;


import java.sql.Date;
import java.time.LocalDateTime;

public class Actor { // JavaBean
    private Integer id; // 最好用Integer，对象对应对象
    private String name;
    private String sex;
    private LocalDateTime borndate; // 这里有个转换错误，utils类中的Date无法去转换到mysql中的datetime类型，按照提示，需要使用localdatetime
    private String phone;

    public  Actor(){} // 一定要有一个无参构造函数，底层反射需要
    public Actor(int id, String name, String sex, LocalDateTime borndate, String phone) {
        this.id = id;
        this.name = name;
        this.sex = sex;
        this.borndate = borndate;
        this.phone = phone;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public LocalDateTime getBorndate() {
        return borndate;
    }

    public void setBorndate(LocalDateTime borndate) {
        this.borndate = borndate;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    @Override
    public String toString() {
        return "Actor{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", sex='" + sex + '\'' +
                ", borndate=" + borndate +
                ", phone='" + phone + '\'' +
                '}';
    }
}
```
4. ApDBUtils源码分析

5. ApDBUtilsDML