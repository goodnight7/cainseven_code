[TOC]

## 说明
   1. 环境：maven spring 
      在maven spring 环境下进行配置 
   2. 数据源：数据库
   3. 网上有很多直接从文件读取模板的，这里就不写了
## maven 依赖
   在 maven 中添加依赖
```
<dependency>
   <groupId>org.apache.velocity</groupId>
   <artifactId>velocity</artifactId>
   <version>1.7</version>
</dependency>
```

## spring bean 文件配置

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">

    <bean id="velocityEngine"
          class="org.springframework.ui.velocity.VelocityEngineFactoryBean">
        <property name="velocityProperties">
            <map>
                //指定模板从数据库输入
                <entry key="resource.loader" value="ds"/>
                //模板所在的表
                <entry key="ds.resource.loader.resource.table" value="template_test"/>
                //模板表的唯一主键，对应为 templateLocation
                <entry key="ds.resource.loader.resource.keycolumn" value="tid"/>
                //模板    
                <entry key="ds.resource.loader.resource.templatecolumn" value="template"/>
                //时间戳 数据库中类型必须为 timestampcolumn
                <entry key="ds.resource.loader.resource.timestampcolumn" value="update_time"/>
                //数据源loader 实例 从这里指定数据库
                <entry key="ds.resource.loader.instance" value-ref="dataSourceLoader"/>
                //开启缓存 间隔时间为300以下就直接读取缓存 
                <entry key="ds.resource.loader.cache" value="true"/>
                <entry key="ds.resource.loader.modificationCheckInterval" value="300"/>
            </map>
        </property>
    </bean>


    <bean id="dataSourceLoader"
          class="org.apache.velocity.runtime.resource.loader.DataSourceResourceLoader">
        <property name="dataSource" ref="baseDataSource"/>
    </bean>

</beans>
```

## 数据库
* 表结构

```
CREATE TABLE `template_test` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `tid` varchar(255) NOT NULL COMMENT '模板ID',
  `template` varchar(255) CHARACTER SET utf8 COLLATE utf8_bin NOT NULL,
  `update_time` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  UNIQUE KEY `tid` (`tid`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8
```
* 数据
|id|tid|template|update_time|
|-----------|------------|-------------|-----------|
|1| simple  |hello $name ,this is first velocity|   2016-07-29 12:51:13|

## java 测试文件

* junit 测试代码
```
//spring 测试环境配置
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("/spring/spring-config.xml")
//@ContextConfiguration("classpath:spring/spring-config.xml")
//@ContextConfiguration(locations = {"classpath:spring/spring-config.xml"})
public class VelocityTest {
@Autowired
private VelocityEngine velocityEngine;
    @Test
    public void testVelocity(){
        Map<String, Object> map = new HashMap<String, Object>();
        map.put("name", "xiaoming");
       //mergeTemplateIntoString(VelocityEngine velocityEngine, String templateLocation, String encoding, Map<String, Object> model) 
       //如果是文件 templateLocation就直接是路径，如果是数据库 templateLocation 就是spring配置文件中的 keyColumn
        System.out.println(VelocityEngineUtils.mergeTemplateIntoString(velocityEngine, "simple","utf-8", map));
    }
}
```
* 输出
```
hello xiaoming ,this is first velocity
```