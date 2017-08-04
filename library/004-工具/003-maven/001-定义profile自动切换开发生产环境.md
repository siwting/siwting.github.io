###001-定义profile自动切换开发生产环境



在POM.xml中配置3个profile，对应项目所处的3个不同的环境-dev, test, production, 可以在命令行通过mvn package –P${profileId}来让其运行。


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
