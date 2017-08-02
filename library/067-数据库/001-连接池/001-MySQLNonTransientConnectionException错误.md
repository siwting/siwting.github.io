### MySQLNonTransientConnectionException 解决方法

#### 修改配置文件
1. druid.maxWait=28800
2. druid.timeBetweenEvictionRunsMillis=18000
3. druid.minEvictableIdleTimeMillis=28800

#### 查看druid版本

我们现在用的是druid-1.0.18.jar换成最新的版本：如druid-1.1.0.jar
