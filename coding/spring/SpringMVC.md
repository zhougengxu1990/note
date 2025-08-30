# SpringMVC
## 注意问题
@RequestParams(required=false)指的是参数若没传,会给参数赋值为null.注意参数接收类型不要为基本数据类型,否则会报错因为其不能接受null值