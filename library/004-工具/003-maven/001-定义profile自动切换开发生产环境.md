###001-定义profile自动切换开发生产环境



在POM.xml中配置3个profile，对应项目所处的3个不同的环境-dev, test, production, profile的id属性即为每个环境赋予一个唯一的标示，<properties>元素的内容则是以key-value的形式出现的键值对，如我们定义了一个变量<env>，其值在不同的环境下(不同id)被赋予了不同的值(dev, production, test),要激活不同的环境打包，我们可以在命令行通过mvn package –P${profileId}来让其运行，为了开发便利，默认激活的是dev开发环境，即开发人员不需要通过命令行手动输入-p参数也能运行dev环境的打包。

``` python 
<!-- 不同的打包环境 -->
 <profiles>
    <!-- 开发环境，默认激活 -->
    <profile>
        <id>dev</id>
        <properties>
           <env>dev</env>
        </properties>
        <activation>
           <activeByDefault>true</activeByDefault><!--默认启用的是dev环境配置-->
        </activation>
    </profile>
    <!-- 生产环境 -->
    <profile>
        <id>production</id>
        <properties>
           <env>production</env>
        </properties>
    </profile>
    <!-- 测试环境 -->
    <profile>
        <id>test</id>
        <properties>
           <env>test</env>
        </properties>
    </profile>
 </profiles>


 ```
