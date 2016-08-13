[TOC]

## 文件上传
        spring mvc 中文件上传的实现
* ### spring 配置文件
```
<!-- 配置SpringMVC的上传文件 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="defaultEncoding" value="${springmvc.defaultEncoding}"/>
    <property name="resolveLazily" value="true"/>
    <property name="maxUploadSize" value="104857600"/>
</bean>
```
* ### 页面 
        javascript 里,用的是 formdata 包装上传文件 采用 ajax 页面无刷新上传
        这里面特别需要注意的是 processData contentType 一定要都是 false 才可以
        方法选 post
```
var userName = $("#userName").val();
var idNo = $("#idNo").val();
var reason = $("#reason").val();
var idType = $("#idType").val();

var singleData = new FormData();
singleData.append("userName", userName);
singleData.append("idType", idType);
singleData.append("idNo", idNo);
singleData.append("reason", reason);
singleData.append("authPaper", $("#inputfile1")[0].files[0]);
singleData.append("idPaper", $("#inputfile2")[0].files[0]);

$.ajax({
    url: '${pageContext.request.contextPath}/report/single/query',
    processData: false,
    contentType: false,
    async: false,
    data: singleData,
    type: 'POST',
    success: function (data) {
        if (data.status == 1) {
            bootbox.alert({
                buttons: {
                    ok: {
                        label: '确定',
                        className: 'btn-sm btn-danger'
                    }
                },
                message: data.message,
                callback: function () {
                    closeWindow();
                }
            });
        } else if (data.status == 0) {
            bootbox.alert(data.message);
        } else {
            bootbox.alert("无法获取响应信息");
        }
    },
    error: function (data, e, status) {
        bootbox.alert("操作异常" + e);
    }
});
```

* ### controller
        前台传过来的参数 通过 multipartfile 类来接收 然后用 getInputStream() 得到流,再对流进行处理即可
        
```
@RequestMapping("single/query")
@ResponseBody
public BaseResponse querySingle(@RequestParam("userName") String userName,
                                @RequestParam("idType") String idType,
                                @RequestParam("idNo") String idNo,
                                @RequestParam("reason") String reason,
                                @RequestParam(value = "authPaper", required = false) MultipartFile authPaper,
                                @RequestParam(value = "idPaper", required = false) MultipartFile idPaper) {
    try {
        logger.info("单条查询操作 : " + userName + idNo + reason);

    } catch (ManagerException e) {
        logger.error(e.getMessage(), e);
        return BaseResponse.getFailInstance(e);
    } catch (Exception e) {
        logger.error(e.getMessage(), e);
        return BaseResponse.getFailInstance(e);
    }
    return BaseResponse.getSuccessInstance("");
}
```

## 文件下载

* ### spring mvc 配置文件
        这里是为了解决 response 的字符编码的乱码的问题
```
<!-- 设置json和response的字符编码 -->
<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter">
   <property name="messageConverters">
      <list>
         <bean class="org.springframework.http.converter.ByteArrayHttpMessageConverter" />
         <ref bean="stringHttpMessageConverter" />
      </list>
   </property>
</bean>
<bean id="stringHttpMessageConverter" class="org.springframework.http.converter.StringHttpMessageConverter">
   <property name="supportedMediaTypes">
      <list>
         <value>text/plain;charset=UTF-8</value>
      </list>
   </property>
</bean>
```

* ### controller
        将fileName 重新再写了一遍的原因是为了解决文件名中文显示乱码的问题 (dfileName)

```
@RequestMapping("download/template")
public ResponseEntity<byte[]> download(@RequestParam("fileName") String fileName) throws IOException {
    File file = new File(getRootPath() + "resources/filesForDownload/" + fileName);
    String dfileName = new String(fileName.getBytes("gb2312"), "iso8859-1");
    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.APPLICATION_OCTET_STREAM);
    headers.setContentDispositionFormData("attachment", dfileName);
    return new ResponseEntity<byte[]>(FileUtils.readFileToByteArray(file), headers, HttpStatus.CREATED);
}
```
* ### page
        page 这块直接根据 controller 写就可以了,没有多余的要求
