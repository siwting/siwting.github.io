# 错误
``` SQL
[2016-07-26 19:22:12] [http-apr-8080-exec-9] (HealthController.java:78) ERROR sharejoy.api.action.cloudhealth.HealthController - 血压接口异常：【
### Error updating database.  Cause: java.sql.SQLException: This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
### The error may involve sharejoy.api.mapper.cloudhealth.BloodPressureMapper.insert-Inline
### The error occurred while setting parameters
### SQL: insert into db_bloodpressurexx (ID, USER_ID, YYYYMM_CODE, SBP_VAL, DBP_VAL, PULSE_VAL,        DEV_NO, USER_NO, RECEIVE_TIME, CREATE_TIME)     values (?, ?, ?,        ?, ?, ?,        ?, ?, ?,        now())
### Cause: java.sql.SQLException: This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)
'' ; uncategorized SQLException for SQL []; SQL state [HY000]; error code [1418]; This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable); nested exception is java.sql.SQLException: This function has none of DETERMINISTIC, NO SQL, or READS SQL DATA in its declaration and binary logging is enabled (you *might* want to use the less safe log_bin_trust_function_creators variable)】

```
# 原因

*开启了mysql的二进制日志*

``` config
log-bin=mysql-bin
binlog-ignore-db = mysql
binlog-ignore-db = test
binlog-ignore-db = performance_schema

```

# 解决办法

1. 执行如下语句

	``SET GLOBAL log\_bin\_trust\_function\_creators = 1;``

2. 修改mysql.ini配置文件，重启服务

	``log\_bin\_trust\_function\_creators = 1``

# 说明

参考：
[https://dev.mysql.com/doc/refman/5.7/en/stored-programs-logging.html]()

> 当创建存储过程、函数或触发器的时候，必须声明，要么是确定的或者它不会修改数据。否则，它可能是不安全的数据恢复或复制。

> 默认情况下，在CREATE FUNCTION、TRIGGER、PRODUCE语句被使用时，Deterministic，NO SQL或READS SQL DATA必须被明确指定至少一个。否则会出现错误
