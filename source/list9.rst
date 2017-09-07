SpringData MongoDB
==================
整合MongoDB的框架还有Morphia，有兴趣的可以上网看看，网上也有别的乱七八糟框架的，这里简单地说下SpringData。

项目git地址：https://code.csdn.net/qq_16313365/demo-springdata-mongo/tree/master。

官方文档：https://docs.spring.io/spring-data/mongodb/docs/1.7.0.RELEASE/reference/html/。

spring Data MongoDB 项目提供与mongodb文档数据库的集成。Spring Data MongoDB POJO的关键功能区域为中心的模型与MongoDB的DBCollection轻松地编写一个存储库交互数据访问。

【pom.xml】
::
	1.	<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
	2.	    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
	3.	    <modelVersion>4.0.0</modelVersion>  
	4.	  
	5.	    <groupId>com.jastar</groupId>  
	6.	    <artifactId>demo</artifactId>  
	7.	    <version>0.0.1-SNAPSHOT</version>  
	8.	    <packaging>jar</packaging>  
	9.	  
	10.	    <name>demo</name>  
	11.	    <url>http://www.jastar-wang.tech</url>  
	12.	  
	13.	    <!-- 版本配置 -->  
	14.	    <properties>  
	15.	        <spring.version>4.1.4.RELEASE</spring.version>  
	16.	        <spring.data.version>1.7.0.RELEASE</spring.data.version>  
	17.	        <log4j.version>1.2.17</log4j.version>  
	18.	    </properties>  
	19.	  
	20.	    <dependencies>  
	21.	        <!-- 单元测试包 -->  
	22.	        <dependency>  
	23.	            <groupId>junit</groupId>  
	24.	            <artifactId>junit</artifactId>  
	25.	            <version>4.10</version>  
	26.	            <scope>test</scope>  
	27.	        </dependency>  
	28.	  
	29.	        <!-- spring核心包 -->  
	30.	        <dependency>  
	31.	            <groupId>org.springframework</groupId>  
	32.	            <artifactId>spring-core</artifactId>  
	33.	            <version>${spring.version}</version>  
	34.	        </dependency>  
	35.	        <dependency>  
	36.	            <groupId>org.springframework</groupId>  
	37.	            <artifactId>spring-context</artifactId>  
	38.	            <version>${spring.version}</version>  
	39.	        </dependency>  
	40.	        <dependency>  
	41.	            <groupId>org.springframework</groupId>  
	42.	            <artifactId>spring-beans</artifactId>  
	43.	            <version>${spring.version}</version>  
	44.	        </dependency>  
	45.	        <dependency>  
	46.	            <groupId>org.springframework</groupId>  
	47.	            <artifactId>spring-tx</artifactId>  
	48.	            <version>${spring.version}</version>  
	49.	        </dependency>  
	50.	        <dependency>  
	51.	            <groupId>org.springframework</groupId>  
	52.	            <artifactId>spring-webmvc</artifactId>  
	53.	            <version>${spring.version}</version>  
	54.	        </dependency>  
	55.	        <dependency>  
	56.	            <groupId>org.springframework</groupId>  
	57.	            <artifactId>spring-aop</artifactId>  
	58.	            <version>${spring.version}</version>  
	59.	        </dependency>  
	60.	        <dependency>  
	61.	            <groupId>org.springframework</groupId>  
	62.	            <artifactId>spring-test</artifactId>  
	63.	            <version>${spring.version}</version>  
	64.	        </dependency>  
	65.	  
	66.	        <!-- 关系型数据库整合时需配置 如hibernate jpa等 -->  
	67.	        <dependency>  
	68.	            <groupId>org.springframework</groupId>  
	69.	            <artifactId>spring-orm</artifactId>  
	70.	            <version>${spring.version}</version>  
	71.	        </dependency>  
	72.	  
	73.	        <!-- spring aop 关联 -->  
	74.	        <dependency>  
	75.	            <groupId>org.aspectj</groupId>  
	76.	            <artifactId>aspectjweaver</artifactId>  
	77.	            <version>1.8.7</version>  
	78.	        </dependency>  
	79.	  
	80.	        <!-- log4j -->  
	81.	        <dependency>  
	82.	            <groupId>log4j</groupId>  
	83.	            <artifactId>log4j</artifactId>  
	84.	            <version>${log4j.version}</version>  
	85.	        </dependency>  
	86.	  
	87.	        <!-- spring整合MongoDB -->  
	88.	        <dependency>  
	89.	            <groupId>org.springframework.data</groupId>  
	90.	            <artifactId>spring-data-mongodb</artifactId>  
	91.	            <version>${spring.data.version}</version>  
	92.	        </dependency>  
	93.	  
	94.	    </dependencies>  
	95.	  
	96.	    <repositories>  
	97.	        <repository>  
	98.	            <id>spring-milestone</id>  
	99.	            <name>Spring Maven MILESTONE Repository</name>  
	100.	            <url>http://repo.spring.io/libs-milestone</url>  
	101.	        </repository>  
	102.	    </repositories>  
	103.	  
	104.	    <build>  
	105.	        <plugins>  
	106.	            <plugin>  
	107.	                <groupId>org.apache.maven.plugins</groupId>  
	108.	                <artifactId>maven-compiler-plugin</artifactId>  
	109.	                <configuration>  
	110.	                    <source>1.7</source>  
	111.	                    <target>1.7</target>  
	112.	                </configuration>  
	113.	            </plugin>  
	114.	        </plugins>  
	115.	    </build>  
	116.	</project>  
实体类【UserInfo.java】
::
	1.	package com.jastar.demo.entity;  
	2.	  
	3.	import java.io.Serializable;  
	4.	import java.sql.Timestamp;  
	5.	  
	6.	import org.springframework.data.annotation.Id;  
	7.	import org.springframework.data.mongodb.core.index.IndexDirection;  
	8.	import org.springframework.data.mongodb.core.index.Indexed;  
	9.	import org.springframework.data.mongodb.core.mapping.Document;  
	10.	import org.springframework.data.mongodb.core.mapping.Field;  
	11.	  
	12.	/** 
	13.	 * 用户实体类 
	14.	 * <p> 
	15.	 * ClassName: UserInfo 
	16.	 * </p> 
	17.	 * <p> 
	18.	 * Description:本类用来展示MongoDB实体类映射的使用 
	19.	 * </p> 
	20.	 * <p> 
	21.	 * Copyright: (c)2017 Jastar·Wang,All rights reserved. 
	22.	 * </p> 
	23.	 *  
	24.	 * @author Jastar·Wang 
	25.	 * @date 2017年4月12日 
	26.	 */  
	27.	@Document(collection = "coll_user")  
	28.	public class UserInfo implements Serializable {  
	29.	  
	30.	    /** serialVersionUID */  
	31.	    private static final long serialVersionUID = 1L;  
	32.	  
	33.	    // 主键使用此注解  
	34.	    @Id  
	35.	    private String id;  
	36.	  
	37.	    // 字段使用此注解  
	38.	    @Field  
	39.	    private String name;  
	40.	  
	41.	    // 字段还可以用自定义名称  
	42.	    @Field("myage")  
	43.	    private int age;  
	44.	  
	45.	    // 还可以生成索引  
	46.	    @Indexed(name = "index_birth", direction = IndexDirection.DESCENDING)  
	47.	    @Field  
	48.	    private Timestamp birth;  
	49.	  
	50.	    public String getId() {  
	51.	        return id;  
	52.	    }  
	53.	  
	54.	    public void setId(String id) {  
	55.	        this.id = id;  
	56.	    }  
	57.	  
	58.	    public String getName() {  
	59.	        return name;  
	60.	    }  
	61.	  
	62.	    public void setName(String name) {  
	63.	        this.name = name;  
	64.	    }  
	65.	  
	66.	    public int getAge() {  
	67.	        return age;  
	68.	    }  
	69.	  
	70.	    public void setAge(int age) {  
	71.	        this.age = age;  
	72.	    }  
	73.	  
	74.	    public Timestamp getBirth() {  
	75.	        return birth;  
	76.	    }  
	77.	  
	78.	    public void setBirth(Timestamp birth) {  
	79.	        this.birth = birth;  
	80.	    }  
	81.	  
	82.	}  
附录：
 * @Id - 用于字段级别，标记这个字段是一个主键，默认生成的名称是“_id”
 * @Document - 用于类，以表示这个类需要映射到数据库，您也可以指定映射到数据库的集合名称
 * @DBRef - 用于字段，以表示它将使用com.mongodb.DBRef进行存储。
 * @Indexed - 用于字段，表示该字段需要如何创建索引
 * @CompoundIndex - 用于类，以声明复合索引
 * @GeoSpatialIndexed - 用于字段，进行地理位置索引
 * @TextIndexed - 用于字段，标记该字段要包含在文本索引中
 * @Language - 用于字段，以设置文本索引的语言覆盖属性。
 * @Transient - 默认情况下，所有私有字段都映射到文档，此注解将会去除此字段的映射
 * @PersistenceConstructor - 标记一个给定的构造函数，即使是一个protected修饰的，在从数据库实例化对象时使用。构造函数参数通过名称映射到检索的DBObject中的键值。
 * @Value  这个注解是Spring框架的一部分。在映射框架内，它可以应用于构造函数参数。这允许您使用Spring表达式语言语句来转换在数据库中检索的键值，然后再用它来构造一个域对象。为了引用给定文档的属性，必须使用以下表达式：@Value("#root.myProperty")，root要指向给定文档的根。
 * @Field - 用于字段，并描述字段的名称，因为它将在MongoDB BSON文档中表示，允许名称与该类的字段名不同。
 * @Version - 用于字段锁定，保存操作时检查修改。初始值是0，每次更新时自动触发。
【db.properties】
::
	1.	###---The mongodb settings---  
	2.	mongo.dbname=demo  
	3.	mongo.host=localhost  
	4.	mongo.port=27017  
	5.	mongo.connectionsPerHost=8  
	6.	mongo.threadsAllowedToBlockForConnectionMultiplier=4  
	7.	mongo.connectTimeout=1000  
	8.	mongo.maxWaitTime=1500  
	9.	mongo.autoConnectRetry=true  
	10.	mongo.socketKeepAlive=true  
	11.	mongo.socketTimeout=1500  
	12.	mongo.slaveOk=true  
	13.	mongo.writeNumber=1  
	14.	mongo.writeTimeout=0  
	15.	mongo.writeFsync=true  
【spring-mgo.xml】
::
	1.	<?xml version="1.0" encoding="UTF-8"?>  
	2.	<beans xmlns="http://www.springframework.org/schema/beans"  
	3.	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
	4.	    xmlns:context="http://www.springframework.org/schema/context"  
	5.	    xmlns:mongo="http://www.springframework.org/schema/data/mongo"  
	6.	    xsi:schemaLocation="http://www.springframework.org/schema/context  
	7.	          http://www.springframework.org/schema/context/spring-context-3.0.xsd  
	8.	          http://www.springframework.org/schema/data/mongo   
	9.	          http://www.springframework.org/schema/data/mongo/spring-mongo-1.0.xsd  
	10.	          http://www.springframework.org/schema/beans  
	11.	          http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">  
	12.	  
	13.	    <!-- 读取属性文件 -->  
	14.	    <context:property-placeholder location="classpath:db.properties" />  
	15.	  
	16.	    <!-- 启用注解支持 -->  
	17.	    <context:annotation-config />  
	18.	  
	19.	    <!-- 扫描组件包 -->  
	20.	    <context:component-scan base-package="com.jastar.demo" />  
	21.	  
	22.	    <!-- SpringData类型转换器 -->  
	23.	    <mongo:mapping-converter id="mongoConverter">  
	24.	        <mongo:custom-converters>  
	25.	            <mongo:converter>  
	26.	                <bean class="com.jastar.demo.converter.TimestampConverter" />  
	27.	            </mongo:converter>  
	28.	        </mongo:custom-converters>  
	29.	    </mongo:mapping-converter>  
	30.	  
	31.	    <!--   
	32.	        MongoDB配置部分   
	33.	        1.mongo：连接配置   
	34.	        2.db-factory：相当于sessionFactory   
	35.	        3.mongoTemplate：与数据库接口交互的主要实现类   
	36.	    -->  
	37.	    <mongo:mongo host="${mongo.host}" port="${mongo.port}">  
	38.	        <mongo:options   
	39.	            connections-per-host="${mongo.connectionsPerHost}"  
	40.	            threads-allowed-to-block-for-connection-multiplier="${mongo.threadsAllowedToBlockForConnectionMultiplier}"  
	41.	            connect-timeout="${mongo.connectTimeout}"   
	42.	            max-wait-time="${mongo.maxWaitTime}"  
	43.	            auto-connect-retry="${mongo.autoConnectRetry}"   
	44.	            socket-keep-alive="${mongo.socketKeepAlive}"  
	45.	            socket-timeout="${mongo.socketTimeout}"   
	46.	            slave-ok="${mongo.slaveOk}"  
	47.	            write-number="${mongo.writeNumber}"   
	48.	            write-timeout="${mongo.writeTimeout}"  
	49.	            write-fsync="${mongo.writeFsync}" />  
	50.	    </mongo:mongo>  
	51.	  
	52.	    <mongo:db-factory id="mongoDbFactory" dbname="${mongo.dbname}" mongo-ref="mongo" />  
	53.	  
	54.	    <bean id="mongoTemplate" class="org.springframework.data.mongodb.core.MongoTemplate">  
	55.	        <constructor-arg name="mongoDbFactory" ref="mongoDbFactory" />  
	56.	        <constructor-arg name="mongoConverter" ref="mongoConverter" />  
	57.	    </bean>  
	58.	  
	59.	</beans>  
说明：
 * mongo:options - 用于配置一些数据库连接设置信息
 * mongo:db-factory - 相当于Hibernate中的SessionFactory
 * mongoTemplate - 非常重要，整个与数据库的交互操作全是靠他，相当于Hibernate的HibernateTemplate
另外，以上配置中有一个类型转换器，因为Spring Data MongoDB本身默认时间类型是java.util.Date，如果实体字段含有java.sql.Timestamp类型，需要自定义转换器进行转换，否则后续操作会报错

【BaseDaoImpl.Java】
::
	1.	package com.jastar.demo.dao.impl;  
	2.	  
	3.	import static org.springframework.data.mongodb.core.query.Criteria.where;  
	4.	  
	5.	import java.io.Serializable;  
	6.	import java.lang.reflect.Field;  
	7.	import java.lang.reflect.Method;  
	8.	import java.lang.reflect.Modifier;  
	9.	import java.util.ArrayList;  
	10.	import java.util.HashMap;  
	11.	import java.util.List;  
	12.	import java.util.Map;  
	13.	  
	14.	import org.springframework.beans.factory.annotation.Autowired;  
	15.	import org.springframework.data.annotation.Id;  
	16.	import org.springframework.data.domain.Sort;  
	17.	import org.springframework.data.domain.Sort.Direction;  
	18.	import org.springframework.data.domain.Sort.Order;  
	19.	import org.springframework.data.mongodb.core.MongoTemplate;  
	20.	import org.springframework.data.mongodb.core.query.Query;  
	21.	import org.springframework.data.mongodb.core.query.Update;  
	22.	  
	23.	import com.jastar.demo.dao.IBaseDao;  
	24.	import com.jastar.demo.util.EmptyUtil;  
	25.	import com.jastar.demo.util.PageModel;  
	26.	  
	27.	/** 
	28.	 * 基本操作接口MongoDB数据库实现类 
	29.	 * <p> 
	30.	 * ClassName: BaseDaoImpl 
	31.	 * </p> 
	32.	 * <p> 
	33.	 * Description:本实现类适用于MongoDB数据库，以下代码仅供参考，本人水平有限，可能会存在些许问题（如有更好方案可告知我，一定虚心学习）， 
	34.	 * 再次提醒，仅供参考！！ 
	35.	 * </p> 
	36.	 * <p> 
	37.	 * Copyright: (c)2017 Jastar·Wang,All rights reserved. 
	38.	 * </p> 
	39.	 *  
	40.	 * @author Jastar·Wang 
	41.	 * @date 2017年4月12日 
	42.	 */  
	43.	public abstract class BaseDaoImpl<T> implements IBaseDao<T> {  
	44.	  
	45.	    protected abstract Class<T> getEntityClass();  
	46.	  
	47.	    @Autowired  
	48.	    protected MongoTemplate mgt;  
	49.	  
	50.	    @Override  
	51.	    public void save(T entity) {  
	52.	        mgt.save(entity);  
	53.	    }  
	54.	  
	55.	    @Override  
	56.	    public void update(T entity) {  
	57.	  
	58.	        // 反向解析对象  
	59.	        Map<String, Object> map = null;  
	60.	        try {  
	61.	            map = parseEntity(entity);  
	62.	        } catch (Exception e) {  
	63.	            e.printStackTrace();  
	64.	        }  
	65.	  
	66.	        // ID字段  
	67.	        String idName = null;  
	68.	        Object idValue = null;  
	69.	  
	70.	        // 生成参数  
	71.	        Update update = new Update();  
	72.	        if (EmptyUtil.isNotEmpty(map)) {  
	73.	            for (String key : map.keySet()) {  
	74.	                if (key.indexOf("{") != -1) {  
	75.	                    // 设置ID  
	76.	                    idName = key.substring(key.indexOf("{") + 1, key.indexOf("}"));  
	77.	                    idValue = map.get(key);  
	78.	                } else {  
	79.	                    update.set(key, map.get(key));  
	80.	                }  
	81.	            }  
	82.	        }  
	83.	        mgt.updateFirst(new Query().addCriteria(where(idName).is(idValue)), update, getEntityClass());  
	84.	    }  
	85.	  
	86.	    @Override  
	87.	    public void delete(Serializable... ids) {  
	88.	        if (EmptyUtil.isNotEmpty(ids)) {  
	89.	            for (Serializable id : ids) {  
	90.	                mgt.remove(mgt.findById(id, getEntityClass()));  
	91.	            }  
	92.	        }  
	93.	  
	94.	    }  
	95.	  
	96.	    @Override  
	97.	    public T find(Serializable id) {  
	98.	        return mgt.findById(id, getEntityClass());  
	99.	    }  
	100.	  
	101.	    @Override  
	102.	    public List<T> findAll() {  
	103.	        return mgt.findAll(getEntityClass());  
	104.	    }  
	105.	  
	106.	    @Override  
	107.	    public List<T> findAll(String order) {  
	108.	        List<Order> orderList = parseOrder(order);  
	109.	        if (EmptyUtil.isEmpty(orderList)) {  
	110.	            return findAll();  
	111.	        }  
	112.	        return mgt.find(new Query().with(new Sort(orderList)), getEntityClass());  
	113.	    }  
	114.	  
	115.	    @Override  
	116.	    public List<T> findByProp(String propName, Object propValue) {  
	117.	        return findByProp(propName, propValue, null);  
	118.	    }  
	119.	  
	120.	    @Override  
	121.	    public List<T> findByProp(String propName, Object propValue, String order) {  
	122.	        Query query = new Query();  
	123.	        // 参数  
	124.	        query.addCriteria(where(propName).is(propValue));  
	125.	        // 排序  
	126.	        List<Order> orderList = parseOrder(order);  
	127.	        if (EmptyUtil.isNotEmpty(orderList)) {  
	128.	            query.with(new Sort(orderList));  
	129.	        }  
	130.	        return mgt.find(query, getEntityClass());  
	131.	    }  
update方法不像关系型数据库那样，给个实体类就能更新，需要自己去想办法搞定。（所以MongoDB都是用来读写的，存储一些信息的）

【TestUseService.java】
::
	1.	package com.jastar.test;  
	2.	  
	3.	import java.sql.Timestamp;  
	4.	import java.util.List;  
	5.	  
	6.	import org.junit.Test;  
	7.	import org.junit.runner.RunWith;  
	8.	import org.springframework.beans.factory.annotation.Autowired;  
	9.	import org.springframework.test.context.ContextConfiguration;  
	10.	import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;  
	11.	  
	12.	import com.jastar.demo.entity.UserInfo;  
	13.	import com.jastar.demo.service.UserService;  
	14.	import com.jastar.demo.util.PageModel;  
	15.	  
	16.	@RunWith(SpringJUnit4ClassRunner.class)  
	17.	@ContextConfiguration(locations = "classpath:spring-mgo.xml")  
	18.	public class TestUserService {  
	19.	  
	20.	    @Autowired  
	21.	    private UserService service;  
	22.	  
	23.	    @Test  
	24.	    public void save() {  
	25.	        UserInfo user = new UserInfo();  
	26.	        user.setName("张三");  
	27.	        user.setAge(25);  
	28.	        user.setBirth(Timestamp.valueOf("2017-4-12 16:52:00"));  
	29.	        service.save(user);  
	30.	        System.out.println("已生成ID:" + user.getId());  
	31.	    }  
	32.	  
	33.	    @Test  
	34.	    public void find() {  
	35.	        UserInfo user = service.find("58edf1b26f033406394a8a61");  
	36.	        System.out.println(user.getName());  
	37.	    }  
	38.	  
	39.	    @Test  
	40.	    public void update() {  
	41.	        UserInfo user = service.find("58edf1b26f033406394a8a61");  
	42.	        user.setAge(18);  
	43.	        service.update(user);  
	44.	    }  
	45.	  
	46.	    @Test  
	47.	    public void delete() {  
	48.	        service.delete("58edef886f03c7b0fdba51b9");  
	49.	    }  
	50.	  
	51.	    @Test  
	52.	    public void findAll() {  
	53.	        List<UserInfo> list = service.findAll("age desc");  
	54.	        for (UserInfo u : list) {  
	55.	            System.out.println(u.getName());  
	56.	        }  
	57.	    }  
	58.	  
	59.	    @Test  
	60.	    public void findByProp() {  
	61.	        List<UserInfo> list = service.findByProp("name", "张三");  
	62.	        for (UserInfo u : list) {  
	63.	            System.out.println(u.getName());  
	64.	        }  
	65.	    }  
	66.	  
	67.	    @Test  
	68.	    public void findByProps() {  
	69.	        List<UserInfo> list = service.findByProps(new String[] { "name", "age" }, new Object[] { "张三", 18 });  
	70.	        for (UserInfo u : list) {  
	71.	            System.out.println(u.getName());  
	72.	        }  
	73.	    }  