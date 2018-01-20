[]()
<div id="home">

<div id="main">

<div id="mainContent">

<div class="forFlow">

<div id="post_detail">

<div id="topics">

<div class="post">

[ORM----hibernate入门Demo(无敌详细版)](http://www.cnblogs.com/hckblogs/p/7866630.html)
======================================================================================================================

<div class="clear">

</div>

<div class="postBody">

<div id="cnblogs_post_body" class="blogpost-body">

一.Hibernate（开放源代码的对象关系映射框架）简介:

Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，它将POJO与数据库表建立映射关系，是一个全自动的orm框架，hibernate可以自动生成SQL语句，自动执行，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。
Hibernate可以应用在任何使用JDBC的场合，既可以在Java的客户端程序使用，也可以在Servlet/JSP的Web应用中使用，最具革命意义的是，Hibernate可以在应用EJB的[J2EE](https://baike.baidu.com/item/J2EE)架构中取代CMP，完成[数据持久化](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E6%8C%81%E4%B9%85%E5%8C%96)的重任。

<div class="lemma-summary">

<div class="para">

二.Hibernate语言特点

</div>

<div class="para">

1.将对数据库的操作转换为对Java对象的操作，从而简化开发。通过修改一个“持久化”对象的属性从而修改数据库表中对应的记录数据。

</div>

<div class="para">

2.提供线程和进程两个级别的缓存提升应用程序性能。

</div>

<div class="para">

3.有丰富的映射方式将Java对象之间的关系转换为数据库表之间的关系。

</div>

<div class="para">

4.屏蔽不同数据库实现之间的差异。在Hibernate中只需要通过“方言”的形式指定当前使用的数据库，就可以根据底层数据库的实际情况生成适合的SQL语句。

</div>

<div class="para">

5.非侵入式：Hibernate不要求持久化类实现任何接口或继承任何类，POJO即可。

</div>

</div>

<div class="configModuleBanner">

三.ORM原理

</div>

<div class="configModuleBanner">

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120124933883-1092983689.png)
四.hibernate入门Demo

该项目使用到的jar包

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120150355696-1661792915.png)

 

1.我使用的是mysql关系型数据库，先建立一个数据库，并且创建一个customer(客户信息)表

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120125229993-2053020876.png)

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120125416915-1653366551.png)

 

 2.创建一个Customer实体类（持久化类）

持久化类是应用程序中的业务实体类,这里的持久化是指类的对象能够被持久化保存到数据库中.

Hibernate 使用普通的JAVA对象（POJO），即POJO的编程模式来进行持久化.

该类中包含的是与数据库表相对应的各个属性，这些属性通过getter/setter方法来访问,对外部隐藏了内部的实现细节.

通常持久化类的编写应嘎遵循一些规则：

1).持久化类中必须提供无参数public构造器（如果没有提供任何构造方法，虚拟机会自动提供默认构造方法，但是如果提供了其他有参数的构造方法的话

虚拟机不再提供默认构造方法，必须手动编写无参构造方法）.

2).持久化类中所有属性使用private修饰，提供public的setter/getter方法

3).必须提供标识属性OID,与数据库表中逐渐对应,例如下面Customer类id属性

4).持久化类属性应尽量使用基本数据类型的包装类型,例如int换成Integer,long换成Long,目的是为了与数据库表的字段默认值null一致。（例如int是区分不开null跟0的，不赋值的int类型默认值为0）

5).持久化类不要用final修饰,使用final修饰将无法生成代理对象进行优化.

<div class="cnblogs_Highlighter">

``` {.brush:java;gutter:true;}
public class Customer {
    private Integer id;    //主键id
    private String name;   //客户姓名
    private Integer age;   //客户年龄
    private String sex;    //客户性别
    private String city;   //客户地址
    /**
     * @return the id
     */
    public Integer getId() {
        return id;
    }
    /**
     * @param id the id to set
     */
    public void setId(Integer id) {
        this.id = id;
    }
    /**
     * @return the name
     */
    public String getName() {
        return name;
    }
    /**
     * @param name the name to set
     */
    public void setName(String name) {
        this.name = name;
    }
    /**
     * @return the age
     */
    public Integer getAge() {
        return age;
    }
    /**
     * @param age the age to set
     */
    public void setAge(Integer age) {
        this.age = age;
    }
    /**
     * @return the sex
     */
    public String getSex() {
        return sex;
    }
    /**
     * @param sex the sex to set
     */
    public void setSex(String sex) {
        this.sex = sex;
    }
    /**
     * @return the city
     */
    public String getCity() {
        return city;
    }
    /**
     * @param city the city to set
     */
    public void setCity(String city) {
        this.city = city;
    }
    //重写toString()方法
    @Override
    public String toString() {
        return "Customer [id=" + id + ", name=" + name + ", age=" + age + ", sex=" + sex + ", city=" + city + "]";
    }
    
    
}
```

</div>

3.编写映射文件Customer.hbm.xml

 
实体类Customer目前还不具备持久化操作的能力,而Hibernate需要知道实体类Customer映射到数据库
Hibernate中的哪个表,以及类中的哪个属性

  对应数据库表中的哪一个字段，这些都需要在映射文件中配置.

 
在实体类Customer所在的包中,创建一个名称为Customer.hbm.xml的映射文件，在该文件中定义了实体类Customer的属性是如何映射到customer表的列上的

开始编写xml文件,该文件头部信息可以通过下面的步骤找到：

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120135503211-1340007294.png)

 

  ![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120135451321-787965045.jpg)

 

<div class="cnblogs_code">

    <?xml version="1.0" encoding="UTF-8"?>
      <!DOCTYPE hibernate-mapping PUBLIC 
           "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
           "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
      <hibernate-mapping>
          <!-- name代表的是实体类名，table代表的是表名 -->
          <class name="com.hck.entity.Customer" table="customer">
              <!-- name=id代表的是customer类中属性  column=id代表的是table表中的字段 -->
              <id name="id" column="id">
                 <!-- 主键生成策略 -->
                 <generator class="native"/>
              </id>
              <!-- 其他属性使用property标签来映射 -->
              <property name="name" column="name" type="string"/>
              <property name="age"  column="age"  type="integer"/>
              <property name="sex"  column="sex"  type="string"/>
              <property name="city" column="city" type="string"/>
          </class>
      </hibernate-mapping>
     

</div>

4.编写核心配置文件hibernate.cfg.xml

  Hibernate的映射文件反映了持久化类和数据库表的映射信息,

 
而Hibernate的配置文件则主要用来配置数据库连接以及Hibernate运行时所需要的各个属性的值.

  在项目的src目录下创建一个名称为hibernate.cfg.xml的文件

 xml的头部编写可以按下图所以查找复制

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120141127055-2023344067.png)

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120141155571-123125601.png)

编写hibernate.cfg.xml文件

<div class="cnblogs_Highlighter">

``` {.brush:java;gutter:true;}
<?xml version="1.0" encoding="UTF-8"?>
  <!DOCTYPE hibernate-configuration PUBLIC
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
  <hibernate-configuration>
         <session-factory>
              <!--指定方言 -->
              <property name="hibernate.dialect">
                org.hibernate.dialect.MySQLDialect
              </property>
              <!-- 数据库驱动 -->
              <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
              <!-- 连接数据库的url -->
              <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/tb_test</property>
              <!-- 数据库用户名 -->
              <property name="hibernate.connection.username">root</property>
              <!-- 数据库密码 -->
              <property name="hibernate.connection.password">123456</property>
              <!-- 其他配置 -->
              <!-- 格式化sql -->
              <property name="format_sql">true</property>
              <!-- 用来关联hbm配置文件 -->
              <mapping resource="com/hck/entity/Customer.hbm.xml"/>
         </session-factory>
  </hibernate-configuration>
```

</div>

　5.编写单元测试类代码如下：

   1).解释一下

``` {.brush:java;gutter:true;}
  config=new Configuration().configure();是如何读取src目录下的hibernate.cfg.xml
  跟踪代码可以看到：
```

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120170957805-874108537.png)
 

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120171043493-2114995544.png)

默认读取src目录下的hibernate.cfg.xml文件,所以hibernate.cfg.xml文件放在src是基于约定优于配置原理的

 

``` {.brush:java;gutter:true;}
```

<div class="cnblogs_Highlighter">

``` {.brush:java;gutter:true;}
public class HibernateTestDemo {
   //定义变量
    Configuration config;
    SessionFactory sessionFactory;
    Session session;
    Transaction transaction;
    //before表示在方法执行前执行
    @Before
    public void setUp()
    {
      //1.加载hibernate.cfg.xml配置
      config=new Configuration().configure();
      //2.获取SessionFactory
      sessionFactory=config.buildSessionFactory();  
     //3.获得一个session
      session=sessionFactory.openSession();
      //4.开始事务
      transaction=session.beginTransaction();
    }
    //添加操作
    @Test
    public void insert()
    {    
      //5.操作
      Customer customer=new Customer();
      customer.setId(1);
      customer.setName("zhangsan");
      customer.setAge(20);
      customer.setSex("m");
      customer.setCity("guangzhou");
      session.save(customer);
    }
    //删除操作
    @Test
     public void delete()
     {
        //先查询
        Customer customer=(Customer)session.get(Customer.class, 1);
        //再删除
        session.delete(customer);
     }
    //查询操作
    @Test
    public void select()
    {
        Customer customer=(Customer)session.get(Customer.class, 1);
        System.out.println(customer);   
    }
    //更新操作
    @Test
    public void update()
    {    
      Customer customer=new Customer();
      customer.setId(1);
      customer.setName("zhangsan");
      customer.setAge(20);
      customer.setSex("m");
      //修改地址为beijing
      customer.setCity("beijing");
      //存在就更新，不存在就执行插入操作
      session.saveOrUpdate(customer);
    }
    //After表示在方法执行结束后执行
    @After
    public void closeTransaction()
    {
      //6.提交事务
      transaction.commit();
      //7.关闭资源
      session.close();
      sessionFactory.close();
    }
}
```

</div>

1)执行插入操作后,打开mysql使用select \* from customer;得到的结果：

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120153911243-1087599878.png)

2)执行查询操作(查看控制台输出)

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120154011774-1755315045.png)

3.执行更新操作

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120154132883-198477345.png)

4.删除操作

![](https://images2017.cnblogs.com/blog/1261992/201711/1261992-20171120154322930-155305762.png)

五.总结

超级完整版hibernate入门demo完成了,如有不懂或者问题请留言\~

 

 

 

</div>

</div>

<div id="MySignature">

</div>

<div class="clear">

</div>

<div id="blog_post_info_block">

<div id="BlogPostCategory">

</div>

<div id="EntryTag">

</div>

<div id="blog_post_info">

</div>

<div class="clear">

</div>

<div id="post_next_prev">

</div>

</div>

</div>

</div>

</div>

</div>

[]()
<div id="blog-comments-placeholder">

</div>

<div id="comment_form" class="commentform">

[]()
<div id="divCommentShow">

</div>

<div id="comment_form_container">

</div>

<div id="ad_text_under_commentbox" class="ad_text_commentbox">

</div>

<div id="ad_t2">

</div>

<div id="opt_under_post">

</div>

<div id="cnblogs_c1" class="c_ad_block">

</div>

<div id="under_post_news">

</div>

<div id="cnblogs_c2" class="c_ad_block">

</div>

<div id="under_post_kb">

</div>

<div id="HistoryToday" class="c_ad_block">

</div>

</div>

</div>

</div>

<div id="sideBar">

<div id="sideBarMain">

<div id="blog-calendar" style="display:none">

</div>

<div id="leftcontentcontainer">

<div id="blog-sidecolumn">

</div>

</div>

</div>

</div>

<div class="clear">

</div>

</div>

<div class="clear">

</div>

</div>

</div>
