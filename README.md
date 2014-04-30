#友盟WindowsPhone SDK  json 解析库

修改自 www.json.org 的 开源代码

## 使用说明
提供decode和encode 方法，对基本数据类型到json字串的转化

* json array -> List<string,object>
* json object -> Dictionary<string,object>
* valueType -> double or long

eg:<br>

```
var json = "{\"abc\":\"abc is abc\"}";
var dic = JSON.JsonDecode(json)

var jsonStr = JSON.JsonEncode(dic);

```

## 数值类型解析

对任何数值类型(int,short,long,float,double...)都会被解析成long或者double类型的对象。
<br>比如:<br>

```
int a = 12;
Dictionary<string,object> dic = new Dictionary<string,object>();
dic.put("abc",a);
dic.put("bbc","cbb");

string s = JSON.JsonEncode(dic);//s = {"abc":12,"bbc":"cbb"}

Dictionary<string,object> dicc = JSON.JsonDecode(s);

int aa = (long)dicc["abc"];
string bbc = (string)dicc["bbc"];
```

注意这一句： `int aa = (long)dicc["abc"];` 并不是 `int aa = (int)dicc["abc"];`这是因为，
在解析  `{"abc":12,"bbc":"cbb"}`这一句时，无法确定 `12` 是long型还是 int型数据，为了不丢失
数据，使用long.parse(...)做解析,会产生一个long型的对象，而long（object）型的对象是不能强转成 int （valueType）型数据的
，所以要先强转成  long 型数据，在转成 int 型，这涉及到了 C# 中对 valueType <-> object 的装箱机制。

如果有兴趣可以修改代码，提供类似 getInt(String),getLong(String)之类的方法，做动态解析
