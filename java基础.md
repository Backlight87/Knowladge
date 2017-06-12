#### 只保留两位小数

```

//方案一:这种方法有个问题：如果你是0.467（小于1），你的结果会是0.00
double d = 13.4324;
d=((int)(d*100))/100;
//方案二:
DecimalFormat df = new DecimalFormat("#.##");
get_double = Double.ParseDouble(df.format(result_value));

```
