## mybatis联合查询
       在sql开发过程中偶尔会遇到因为一个字段就需要关联另一张表，或者是这个字段需要进行复杂查询，如果不想写成一大段很丑的
    sql，该怎么办？mybatis提供了一种解决方案，可以用<association/>，例：
``` sql
  <resultMap id="SleepInfoResultMap" type="iguard.measurementType.entity.SleepInfo">
    <result column="sleep_time" property="sleepTime" jdbcType="REAL"/>
    <association property="goodDays" column="goodDays" select="findGoodDays"/>
  </resultMap>

  <select id="findGoodDays" parameterType="java.util.Date"
          resultType="int">
  </select>
```
       看到了吗，在SleepInfo实体中有个goodDays属性，这是需要通过统计算出来的，通过这种方式避免了写出来很糗的sql，是不是
    很好用啊，哈哈。
