#### fastjson工具库

* 将json字符串转换为java对象，将java对象转换为json字符串的一个类库

* 通过`JSON.toJSONString(object);`方法将对象转换为JSON字符串

~~~java
String s = JSONObject.toJSONString(obj);
~~~



* 通过`List<String> s = JSON.parseObject(jsonString, String.class);`将JSON字符串转换为对象，一般需要先将对象进行合理的封装.

~~~java
//通过直接调用JSONObject类传入json字符串和需要转换的类的实例
Object obj = JSONObject.parseObject(JSONString, Object.class);
//若JSON中有多个对象集合，则使用List<Object>接收json对象
~~~

#### Jackson

* 将java对象序列化为json字符串，也可以将json字符串反向序列化为java对象的一个框架
* 通过readValue()和writeValue()方法来实现对象和字符串的转换

