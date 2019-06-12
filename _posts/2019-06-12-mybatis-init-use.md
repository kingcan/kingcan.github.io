---
title: Mybatis的初使用
layout: post
categories: Java
tags: mybatis
---
## 导包
   导包啊（我用的mybatis-3.4.6和mysql-connector-java 5.1.46，log4j的配置
）
为了更好的可视化MySQL，我推荐一个链接使用Navicat for MySQL https://blog.csdn.net/wypersist/article/details/79834490
在MySQL中新建一个ssm数据库，并且创建了一个customer表，字段如下pojo类所示
## 编写pojo类
   public class Customer {
   
    private Integer id ;//主键字段不应该有的，在数据库中为自增的主键
    
    private  String name;
    private String gender;
    private String telephone;
    private  String address;
    这里的字段就是和数据库中的字段要一一对应的。

## 配置mybatis

这是核心配置文件mybatis-config.xml，相当于总包，装很多映射的
   <?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!-- 加载属性文件 -->
    <properties resource="config.properties">
        <!--properties中还可以配置一些属性名和属性值 -->
        <!-- <property name="jdbc.driver" value=""/> -->
    </properties>

    <!-- 全局配置参数，需要时再设置 -->
    <!-- <settings> </settings> -->

    <typeAliases>
        <!-- 别名定义 -->
        <!-- 针对单个别名定义 type：类型的路径 alias：别名，类名不能写错
         别名可以随便起，但最好规范-->
        <typeAlias type="domain.Customer" alias="customer" />
        <!-- 批量别名定义 指定包名，mybatis自动扫描包中的po类，自动定义别名，别名就是类名（首字母大写或小写都可以） -->
        <package name="domain" />
    </typeAliases>

    <!-- 和spring整合后 environments配置将废除 -->
    <environments default="development">
        <environment id="development">
            <!-- 使用jdbc事务管理，事务控制由mybatis -->
            <transactionManager type="JDBC" />
            <!-- 数据库连接池，由mybatis管理 -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}" />
                <property name="url" value="${jdbc.url}" />
                <property name="username" value="${jdbc.username}" />
                <property name="password" value="${jdbc.password}" />
            </dataSource>
        </environment>
    </environments>


    <!-- 加载映射文件 -->
    <mappers>
        <!--通过resource方法一次加载一个映射文件 -->
        <!--注意这里的路径和xml文件 -->
        <mapper resource="mapper/Customer.xml" />

        <!-- 批量加载mapper 指定mapper接口的包名，mybatis自动扫描包下边所有mapper接口进行加载 -->
        <!-- 遵循一些规范：需要将mapper接口类名和mapper.xml映射文件名称保持一致，且在一个目录 -->
        <!-- 中上边规范的前提是：使用的是mapper代理方法
        <package name="...." />-->

    </mappers>

</configuration>

其中连接的配置 config.properties单独写，以便换数据库的
jdbc.driver = com.mysql.jdbc.Driver
jdbc.url = jdbc:mysql://192.168.74.130:3306/ssm?characterEncoding=UTF-8
jdbc.username = root
jdbc.password = qaz123

接下来就要写具体实体类的操作实现了
如上面核心配置文件包含的的Customer.xml。。简单说就是映射dao里的接口和SQL语句实现
<!--该文件编写mybatis中的mapper接口里面的方法提供对应的SQL语句 -->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="dao.CustomerMapper">

    <!-- 在映射文件中配置很多sql语句 -->
    <!-- 将sql语句封装到mappedStatement对象中，所以将id称为statement的id -->
    <!-- parameterType：指定输入参数的类型，这里指定int型 #{}表示一个占位符号 -->
    <!-- #{id}：其中的id表示接收输入的参数，参数名称就是id，如果输入 -->
    <!-- 参数是简单类型，#{}中的参数名可以任意，可以value或其它名称 -->
    <!-- resultType：指定sql输出结果的所映射的java对象类型，select指定resultType表示将单条记录映射成的java对象。 -->
    <!-- 表名要对，但是不区分大小写，resultType要写类名，同样不区分大小写 -->
    <!--该文件编写mybatis中的mapper接口里面的方法提供对应的SQL语句 -->
    <!-- 添加客户 -->
    <insert id="saveCustomer" parameterType="domain.Customer"> <!-- 这里绑定了dao里未实现的接口中的方法，后一个参数就是它要绑定的pojo类 -->
        INSERT INTO t_customer (name, gender, telephone, address) VALUES (#{name}, #{gender}, #{telephone}, #{address})
    </insert>

</mapper>
凡是涉及到类名引用的都要使用包名.类（全路径）

## 写一个测试类验证连接
import dao.CustomerMapper;
import domain.Customer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;

public class MybatisTest {
    @Test
    public  void test() throws IOException{
        //1.先创建 SqlSessionFactoryBuilder的构造器
        SqlSessionFactoryBuilder builder=new SqlSessionFactoryBuilder();
        //加载mybatis核心配置文件
        InputStream is = Resources.getResourceAsStream("resources/mybatis-config.xml");
        SqlSessionFactory factory=builder.build(is);
        //打开sqlSession
        SqlSession sqlSession=factory.openSession();
        //获取mapper接口的对象
        CustomerMapper customerMapper =sqlSession.getMapper(CustomerMapper.class);
        //操作
        Customer customer=new Customer();
        customer.setName("liuyan");
        customer.setAddress("wuhanCity");
        customer.setGender("女");
        customer.setTelephone("13256462");
        customerMapper.saveCustomer(customer);
        //提交事务
        sqlSession.commit();
        //关闭资源
        sqlSession.close();
    }
}

## 为啥要用mybatis

以前写dao层impl的时候是要实现具体的内容并且返回结果的
package com.sm.dao.impl;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import com.sm.bean.User;
import com.sm.dao.UserDao;
import com.sm.util.BaseDao;
import com.sm.util.Page;

public class UserDaoImpl implements UserDao {
  BaseDao bDao=new BaseDao();
	@Override
	public User login(String name, String pwd) {
		String sql="select *  from smbms_user where  userCode=? and userPassword=?";
		ResultSet rSet= bDao.query(sql,name,pwd);
		User user=new User();
		try {
			while (rSet.next()) {
				 user.setId(rSet.getInt(1));
				 user.setUserCode(rSet.getString(2));
				 user.setUserName(rSet.getString(3));
				 user.setUserPassWord(rSet.getString(4));
				 user.setGender(rSet.getInt(5));
				user.setBirthday(rSet.getDate(6));
				user.setPhone(rSet.getString(7));
				user.setAddress(rSet.getString(8));
				user.setUserRole(rSet.getInt(9));
				user.setCreatedBy(rSet.getInt(10));
				user.setCreationDate(rSet.getDate(11));
				user.setModifyBy(rSet.getInt(12));
				user.setModifyDate(rSet.getDate(13));
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
		
		
		return user;
	}
	@Override
	public List<User> queryAll(Page page,String userCode,String userRole) {
		String sql=null;
		ResultSet rSet=null;
		List<User> users=new ArrayList<User>();
	  if (("".equals(userCode)||userCode==null)&& ("".equals(userRole)||userRole==null)||userRole.equals("0")) {
		  sql="select * from smbms_user limit ?,?";
		  rSet= bDao.query(sql,page.getNowBySql(),page.getCount());
	}else if ((!"".equals(userCode)&&userCode!=null)&& ("".equals(userRole)||userRole==null||userRole.equals("0"))) {
		sql="select * from smbms_user where userCode like ? LIMIT ?,?";
	rSet=bDao.query(sql, "%"+userCode+"%",page.getNowBySql(),page.getCount());
	}else if (("".equals(userCode)||userCode==null)&& (!"".equals(userRole)&&userRole!=null&&!userRole.equals("0"))) {
		sql="select * from smbms_user where userRole =? LIMIT ?,?";
		rSet=bDao.query(sql,userRole,page.getNowBySql(),page.getCount());
	
	}else if ((!"".equals(userCode)&&userCode!=null)&& (!"".equals(userRole)&&userRole!=null&&!userRole.equals("0"))) {
		sql="select * from smbms_user where userCode like ? and userRole =? LIMIT ?,?";
		rSet=bDao.query(sql, "%"+userCode+"%",userRole,page.getNowBySql(),page.getCount());
	}
	 
		try {
			while (rSet.next()) {
				User user=new User();
				 user.setId(rSet.getInt(1));
				 user.setUserCode(rSet.getString(2));
				 user.setUserName(rSet.getString(3));
				 user.setUserPassWord(rSet.getString(4));
				 user.setGender(rSet.getInt(5));
				user.setBirthday(rSet.getDate(6));
				user.setPhone(rSet.getString(7));
				user.setAddress(rSet.getString(8));
				user.setUserRole(rSet.getInt(9));
				user.setCreatedBy(rSet.getInt(10));
				user.setCreationDate(rSet.getDate(11));
				user.setModifyBy(rSet.getInt(12));
				user.setModifyDate(rSet.getDate(13));
				users.add(user);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return users;
	}
	@Override
	public int queryByCount(String userCode,String userRole) {
		
		
		
		String sql=null;
		ResultSet rSet=null;
		  if (("".equals(userCode)||userCode==null)&& ("".equals(userRole)||userRole==null)||userRole.equals("0")) {
			  sql="select count(*) from smbms_user ";
			  rSet= bDao.query(sql);
		}else if ((!"".equals(userCode)&&userCode!=null)&& ("".equals(userRole)||userRole==null||userRole.equals("0"))) {
			sql="select count(*) from smbms_user where userCode like ? ";
		rSet=bDao.query(sql, "%"+userCode+"%");
		}else if (("".equals(userCode)||userCode==null)&& (!"".equals(userRole)&&userRole!=null&&!userRole.equals("0"))) {
			sql="select count(*) from smbms_user where userRole =? ";
			rSet=bDao.query(sql,userRole);
		
		}else if ((!"".equals(userCode)&&userCode!=null)&& (!"".equals(userRole)&&userRole!=null&&!userRole.equals("0"))) {
			sql="select count(*) from smbms_user where userCode like ? and userRole =? ";
			rSet=bDao.query(sql, "%"+userCode+"%",userRole);
		}
		  
		int count=0;//代表用户的数量
		 System.out.println("sql==>"+sql);
      	try {
			if (rSet.next()) {
			count=	rSet.getInt(1);
			}
		} catch (SQLException e) {
			e.printStackTrace();
		}
      	
		return count;
	}
	@Override
	public int addUser(User user,int id) {
		String sql="INSERT INTO `smbms_user`(`userCode`,`userName`,`userPassword`,`gender`,`birthday`,`phone`,`address`,`userRole`,`createdBy`,`creationDate`) VALUES (?,?,?,?,?,?,?,?,?,?);";
		int num= bDao.update(sql,user.getUserCode(),user.getUserName(),user.getUserPassWord(),user.getGender(),user.getBirthday(),user.getPhone(),user.getAddress(),user.getUserRole() ,id,new Date());
		
		return num;
	}
	@Override
	public int updateUser(User user) {
		String sql="update smbms_user set userName=? ,gender=? , birthday=? , phone=? , address=?, userRole=?, modifyBy=?, modifyDate=? where id=?";
		int num=bDao.update(sql,user.getUserName(),user.getGender(),user.getBirthday(),user.getPhone(),user.getAddress(),user.getUserRole(),user.getModifyBy(),user.getModifyDate(),user.getId());
		return num;
	}
	@Override
	public int deleteUser(String id) {
		String sql="delete  from smbms_user where id=?";
	int num=	bDao.update(sql,id);
		
		
		return num;
	}
	@Override
	public int updatePwd(int id, String pwd) {
		String sql="update smbms_user set userPassword=? where id=?";
		int num=bDao.update(sql,pwd,id);
		return num;
	}

}
这就显得结构混乱，Java混杂SQL语句，可读性差。其他的
1. 易于上手和掌握。

2. sql写在xml里，便于统一管理和优化。

3. 解除sql与程序代码的耦合。

4. 提供映射标签，支持对象与数据库的orm字段关系映射

5. 提供对象关系映射标签，支持对象关系组建维护

6. 提供xml标签，支持编写动态sql。
也有不好的
1. sql工作量很大，尤其是字段多、关联表多时，更是如此。

2. sql依赖于数据库，导致数据库移植性差。

3. 由于xml里标签id必须唯一，导致DAO中方法不支持方法重载。

4. 字段映射标签和对象关系映射标签仅仅是对映射关系的描述，具体实现仍然依赖于sql。（比如配置了一对多Collection标签，如果sql里没有join子表或查询子表的话，查询后返回的对象是不具备对象关系的，即Collection的对象为null）

5. DAO层过于简单，对象组装的工作量较大。

6.  不支持级联更新、级联删除。

7. 编写动态sql时,不方便调试，尤其逻辑复杂时。

8 提供的写动态sql的xml标签功能简单（连struts都比不上），编写动态sql仍然受限，且可读性低。

9. 若不查询主键字段，容易造成查询出的对象有“覆盖”现象。

10. 参数的数据类型支持不完善。（如参数为Date类型时，容易报没有get、set方法，需在参数上加@param）

11. 多参数时，使用不方便，功能不够强大。（目前支持的方法有map、对象、注解@param以及默认采用012索引位的方式）

12. 缓存使用不当，容易产生脏数据。
总结：

mybatis的优点其实也是mybatis的缺点，正因为mybatis使用简单，数据的可靠性、完整性的瓶颈便更多依赖于程序员对sql的使用水平上了。sql写在xml里，虽然方便了修改、优化和统一浏览，但可读性很低，调试也非常困难，也非常受限，无法像jdbc那样在代码里根据逻辑实现复杂动态sql拼接。mybatis简单看就是提供了字段映射和对象关系映射的jdbc，省去了数据赋值到对象的步骤而已，除此以外并无太多作为，不要把它想象成hibernate那样强大，简单小巧易用上手，方便浏览修改sql就是它最大的优点了。

mybatis适用于小型且程序员能力较低的项目和人群使用，对于中大型项目来说我并不推荐使用，如果觉得hibernate效率低的话（实际上也是使用不当所致，hibernate是实际上是不适用于拥有高负载的工程项目），还不如直接用spring提供的jdbc简单框架（Template），同样支持对象映射。
--------------------- 
作者：wangpeng047 
来源：CSDN 
原文：https://blog.csdn.net/wangpeng047/article/details/17039573 
版权声明：本文为博主原创文章，转载请附上博文链接！