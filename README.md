# 使用 SOFATracer 远程汇报数据到Jaeger

本示例是演示上报Span数据到Jaeger中的Demo，使用的是官方的上报Zipkin的例子。Demo依赖的相关jar包可以在`resource`目录下的`lib`中找到



## 配置文件

在工程的 `application.properties` 文件下添加一个 SOFATracer 要使用的参数

### 使用http方式上传

```properties
# Application Name
spring.application.name=SOFATracerJaegerPerformanceTest
# logging path
logging.path=./logs
server.port=8082

com.alipay.sofa.tracer.jaeger.baseUrl=http://localhost:9411
com.alipay.sofa.tracer.jaeger.enabled=true
com.alipay.sofa.tracer.jaeger.gzipped=true
```

### 使用UDP方式上传Jaeger Agent

在工程的 `application.properties` 文件下添加一个 SOFATracer 要使用的参数

```properties
# Application Name
spring.application.name=SOFATracerJaegerPerformanceTest
# logging path
logging.path=./logs
server.port=8082
com.alipay.sofa.tracer.jaeger.agent.enabled=true
com.alipay.sofa.tracer.jaeger.agent.host=127.0.0.1
com.alipay.sofa.tracer.jaeger.agent.port=6831
com.alipay.sofa.tracer.jaeger.agent.maxPacketSize=0
```

在`sofa.tracer.properties`中配置：

```properties
com.alipay.sofa.tracer.jaeger.agent.flushInterval=100
com.alipay.sofa.tracer.jaeger.agent.maxQueueSize=20000
com.alipay.sofa.tracer.jaeger.agent.closeEnqueueTimeout=2000
```



## 启动Jaeger服务端

Jaeger服务端的启动可以使用`jaegertracing/all-in-one:1.23`docker镜像。

## 运行

可以将工程导入到 IDE 中运行生成的工程里面中的 `main` 方法启动应用，也可以直接在该工程的根目录下运行 `mvn spring-boot:run`，将会在控制台中看到启动日志：

```
2018-05-12 13:12:05.868  INFO 76572 --- [ost-startStop-1] o.s.b.w.servlet.FilterRegistrationBean   : Mapping filter: 'SpringMvcSofaTracerFilter' to urls: [/*]
2018-05-12 13:12:06.543  INFO 76572 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/zipkin]}" onto public java.util.Map<java.lang.String, java.lang.Object> com.alipay.sofa.tracer.examples.zipkin.controller.SampleRestController.zipkin(java.lang.String)
2018-05-12 13:12:07.164  INFO 76572 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)
```

可以通过在浏览器中输入 [http://localhost:8080/helloZipkin](http://localhost:8080/helloJaeger 来访问 REST 服务，结果类似如下：

```json
{
	content: "Hello, SOFATracer Jaeger Remote Report!",
	id: 1,
	success: true
}
```

##  查看链路数据

打开浏览器中访问Jaeger UI界面就可以看到链路信息

