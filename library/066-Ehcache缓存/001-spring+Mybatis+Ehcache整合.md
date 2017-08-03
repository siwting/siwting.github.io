1. 下载mybatis相关包与ehcache相关包

2. 在Map文件中打开echached效果，userMapper.xml文件内容如下：
```java
<?xml version="1.0" encoding="UTF-8" ?>  
    <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >  
    <mapper namespace="com.erpbase.dao.UserMapper">  
   <!-- 以下两个<cache>标签二选一,第一个可以输出日志,第二个不输出日志 -->  
        <cache type="org.mybatis.caches.ehcache.LoggingEhcache" />  
        <cache type="org.mybatis.caches.ehcache.EhcacheCache" />  
        ......  
</mapper>
```

3. 配置ehcache.xml

```config
<?xml version="1.0" encoding="UTF-8"?>    
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd">  
    <diskStore path="java.io.tmpdir"/>   
    <defaultCache      
            maxElementsInMemory="3000"      
            eternal="false"      
            timeToIdleSeconds="3600"      
            timeToLiveSeconds="3600"      
            overflowToDisk="true"      
            diskPersistent="false"      
            diskExpiryThreadIntervalSeconds="100"      
            memoryStoreEvictionPolicy="LRU"      
            />      
    <cache name="userCache"      
           maxElementsInMemory="3000"      
           eternal="false"      
           overflowToDisk="true"      
           timeToIdleSeconds="3600"      
           timeToLiveSeconds="3600"      
           memoryStoreEvictionPolicy="LFU"      
            />    
</ehcache>
```

 - 参数说明：

 ```config
 name: cache的名字，用来识别不同的cache，必须惟一。   

 maxElementsInMemory: 内存管理的缓存元素数量最大限值。   

 maxElementsOnDisk: 硬盘管理的缓存元素数量最大限值。默认值为0，就是没有限制。   

 eternal: 设定元素是否持久话。若设为true，则缓存元素不会过期。   

 overflowToDisk: 设定是否在内存填满的时候把数据转到磁盘上。

 timeToIdleSeconds： 设定元素在过期前空闲状态的时间，只对非持久性缓存对象有效。默认值为0,值为0意味着元素可以闲置至无限长时间。   

 timeToLiveSeconds: 设定元素从创建到过期的时间。其他与timeToIdleSeconds类似。   

 diskPersistent: 设定在虚拟机重启时是否进行磁盘存储，默认为false.(我的直觉，对于安全小型应用，宜设为true)。   

 diskExpiryThreadIntervalSeconds: 访问磁盘线程活动时间。   

 diskSpoolBufferSizeMB: 存入磁盘时的缓冲区大小，默认30MB,每个缓存都有自己的缓冲区。   

 memoryStoreEvictionPolicy: 元素逐出缓存规则。共有三种，Recently Used (LRU)最近最少使用，为默认。 First In First Out (FIFO)，先进先出。Less Frequently Used(specified as LFU)最少使用

```

4. 配置applicationContext-ehcache.xml

```config
<?xml version="1.0" encoding="UTF-8"?>  
<!-- /** * * 缓存配置 *  * */ -->  
<beans xmlns="http://www.springframework.org/schema/beans"  
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    xmlns:ehcache="http://ehcache-spring-annotations.googlecode.com/svn/schema/ehcache-spring"  
    xsi:schemaLocation="      
    http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd      
    http://ehcache-spring-annotations.googlecode.com/svn/schema/ehcache-spring    
    http://ehcache-spring-annotations.googlecode.com/svn/schema/ehcache-spring/ehcache-spring-1.1.xsd">  

    <!-- <ehcache:annotation-driven /> -->  

    <ehcache:annotation-driven cache-manager="ehcacheManager" />  

    <ehcache:config cache-manager="ehcacheManager">  
        <ehcache:evict-expired-elements interval="60" />  
    </ehcache:config>  

    <bean id="ehcacheManager"  
        class="org.springframework.cache.ehcache.EhCacheManagerFactoryBean">  
        <property name="configLocation" value="classpath:ehcache.xml" />  
    </bean>  
</beans>
```

5. 在spring-mvc.xml 中加入如下内容，将ehcache相关配置装配到spring容器中：

```config
   加载ehcache缓存配置文件     
   说明：在这里我遇到了这样一个问题，当使用@Service等注解的方式将类声明到配置文件中时，就需要将缓存配置import到主配置文件中，否则缓存会不起作用    
   如果是通过<bean>声明到配置文件中时，则只需要在web.xml的contextConfigLocation中加入applicationContext-ehcache.xml即可，不过还是推荐使用如下方式吧，因为这样不会有任何问题
```

```config
        <import resource="classpath:applicationContext-ehcache.xml"/>  
```

6. 在userServiceImpl.Java中加入通过注解进行配置：
```java
 @Cacheable(cacheName="userCache")  <strong>//这里的cacheName要跟ehcache.xml中保持一致</strong>  
public List<User> getUserList(User user, Map<String, Object> map) {  
   long l1 = new Date().getTime();  

   List<User> list = userMapper.getUserList(user);  
   Integer listSize = userMapper.getUserCount(user);  

   long l2 = new Date().getTime();  
   System.out.println("++++++++++++total time use: " + (l2-l1));  

   map.put("rows", list);  
   map.put("total", listSize);  

   return list;  
}  
```

```config
 到此spring+mybatis+EHCache配置完成。可以对比在加上@Cacheable(cacheName="userCache")和不加的两种情况下的(l2-l1)的时间，在我本地如果不加用时在40ms左右，加上之后第一次加载是40ms，第二次用时1ms，说明第一次加载的数据已经被放到缓存当中去，可见效率得到极大提升。

 拓展说明：

-对于清除缓存的方法，ehcache提供了两种，一种是在ehcache.xml中配置的时间过后自动清除，一种是在数据发生变化后触发清除。个人感觉第二种比较好。可以将
```

```java
@TriggersRemove(cacheName="userCache",removeAll=true)
@TriggersRemove(cacheName="userCache", when=When.AFTER_METHOD_INVOCATION, removeAll=true)
```
```config
 这句代码加到service里面的添加、删除、修改方法上。这样只要这几个方法有调用，缓存自动清除。
 对于Mybatis更简单，对不想缓存的sql结果，可以再后面添加useCache="false"即可：
```
```sql
     <select id="getLabelValueList" resultMap="BaseResultMap" parameterType="com.Product" useCache="false">  
     select Id, Name  
     from Product  
     where Enable = 1  
     <if test="shopid != null and shopid != 0 " >  
       AND Id not in (select Productid from shopproduct where shopid = #{shopid})   
     </if>  
     order by Id  
     </select>  
```
