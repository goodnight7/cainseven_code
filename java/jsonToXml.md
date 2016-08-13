# 最简单的 json 与 xml 相互转化

[TOC]

### 说明
    直接使用 org.json 工具包来进行转化
    maven 
### maven 
* 加入依赖
```
    <dependency>
        <groupId>org.json</groupId>
        <artifactId>json</artifactId>
        <version>20160212</version>
    </dependency>
```

### java

* junit 测试文件

```
    //注意 导包 时，导入 org.json 的包
    @Test
    public void testJsonToXML(){
        InputStream in =  VelocityTest.class.getResourceAsStream("/jsonToXml.json");
        try {
            String string = IOUtils.toString(in);
            System.out.println(string);
            JSONObject jsob = new JSONObject(string);
            String xml = XML.toString(jsob);
            System.out.println(xml);
            JSONObject json = XML.toJSONObject(xml);
            System.out.println(json.toString());
            System.out.println(json);
        } catch (IOException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
    }
```

* 输出
```
{
  "page": 1,
  "rows": 0,
  "sidx": null,
  "sord": null,
  "startIndex": null,
  "number": null,
  "total": null,
  "records": null
}
<number>null</number><startIndex>null</startIndex><total>null</total><records>null</records><page>1</page><sord>null</sord><rows>0</rows><sidx>null</sidx>
{"number":null,"startIndex":null,"total":null,"records":null,"page":1,"sord":null,"rows":0,"sidx":null}
{"number":null,"startIndex":null,"total":null,"records":null,"page":1,"sord":null,"rows":0,"sidx":null}
```