2016 7 22 [刘纯彰]()

[TOC]
# 说明
kibana 是 elasticsearch 的可视化管理平台，marvel 和 sense 是 kibana 上的两个应用
所以安装的顺序是：
 * elasticsearch
 * kibana
 * sense
 * marvel

# 环境搭建
* ## elasticsearch 安装
1. [下载elasticsearch安装包](https://www.elastic.co/downloads/elasticsearch) 

       进入下载页面后，按要求操作即可
   1. 下载安装包 并解压至你想放到的目录内
        ![](https://static-www.elastic.co/assets/blt114dd05890650309/download-es-step1.png?q=935)
   1. 进入解压后的目录 运行 bin/elasticsearch 
可手动点开，也可终端运行 
```./bin/elasticsearch ```
        ![](https://static-www.elastic.co/assets/blt7d5d73bf582d34cd/download-es-step2.png?q=935)
   2.  在终端输入
```curl -X GET http://localhost:9200/  ``` 
  ![](https://static-www.elastic.co/assets/bltf20bf9db4d4b20a4/download-es-step3.png?q=935)

* ## kibana
1. mac 下终端直接运行
```brew install kibana ```
2. 也可去 [kibana 下载页](https://www.elastic.co/downloads/kibana) 下载安装包 解压即可
3. 在浏览器中进入kibana 服务端口
```http://localhost:5601```
* ## sense
1. 安装sense 在终端中进行 kibana 目录 
并运行
```./bin/kibana plugin --install elastic/sense ```
2. 运行kibana
```./bin/kibana```
3. 在浏览器中打开
```http://localhost:5601/app/sense```

* ## marvel 2.0 +
1. install marvel into elasticsearch
终端进入elasticsearch目录
```./bin/plugin install license
    ./bin/plugin install marvel-agent```
2. install marvel into kibana
终端进入kibana目录
```./bin/kibana plugin --install elasticsearch/marvel/latest```
3. start kibana and elasticsearch
```./bin/kibana
    ./bin/elasticsearch```
4. open in browser
```http://localhost:5601/app/marvel```