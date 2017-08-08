
### 数据转换

``` java

  //方案一:
  double get_double = (double)(Math.round(result_value*100)/100.0)
  //方案二:
  DecimalFormat df = new DecimalFormat("#.##");
  double get_double = Double.ParseDouble(df.format(result_value));
  //方案三:
  double get_double = Double.ParseDouble(String.format("%.2f",result_value));
  //方案四:
  BigDecimal bd = new BigDecimalresult_value();
  BigDecimal  bd2 = bd.setScale(2,BigDecimal  .ROUND_HALF_UP);
  double get_double = Double.ParseDouble(bd2.ToString());

```
