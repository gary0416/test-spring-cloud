# spring cloud config
## server例子
启动服务,访问http://localhost:7060/config-client/dev可以看到已经支持远程配置

类似的还有:
- http://localhost:7060/config-client-dev.yml
- http://localhost:7060/config-client-dev.json

程序启动时可以看到都支持哪些访问方式(label为git的):
```
2018-05-12 16:22:49.414  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}-{profiles}.properties],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.properties(java.lang.String,java.lang.String,boolean) throws java.io.IOException
2018-05-12 16:22:49.414  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}-{profiles}.yml || /{name}-{profiles}.yaml],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.yaml(java.lang.String,java.lang.String,boolean) throws java.lang.Exception
2018-05-12 16:22:49.415  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{label}/{name}-{profiles}.json],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.labelledJsonProperties(java.lang.String,java.lang.String,java.lang.String,boolean) throws java.lang.Exception
2018-05-12 16:22:49.416  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{label}/{name}-{profiles}.properties],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.labelledProperties(java.lang.String,java.lang.String,java.lang.String,boolean) throws java.io.IOException
2018-05-12 16:22:49.416  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{label}/{name}-{profiles}.yml || /{label}/{name}-{profiles}.yaml],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.labelledYaml(java.lang.String,java.lang.String,java.lang.String,boolean) throws java.lang.Exception
2018-05-12 16:22:49.417  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}-{profiles}.json],methods=[GET]}" onto public org.springframework.http.ResponseEntity<java.lang.String> org.springframework.cloud.config.server.environment.EnvironmentController.jsonProperties(java.lang.String,java.lang.String,boolean) throws java.lang.Exception
2018-05-12 16:22:49.417  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profiles}/{label:.*}],methods=[GET]}" onto public org.springframework.cloud.config.environment.Environment org.springframework.cloud.config.server.environment.EnvironmentController.labelled(java.lang.String,java.lang.String,java.lang.String)
2018-05-12 16:22:49.418  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profiles:.*[^-].*}],methods=[GET]}" onto public org.springframework.cloud.config.environment.Environment org.springframework.cloud.config.server.environment.EnvironmentController.defaultLabel(java.lang.String,java.lang.String)
2018-05-12 16:22:49.422  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profile}/{label}/**],methods=[GET],produces=[application/octet-stream]}" onto public synchronized byte[] org.springframework.cloud.config.server.resource.ResourceController.binary(java.lang.String,java.lang.String,java.lang.String,javax.servlet.http.HttpServletRequest) throws java.io.IOException
2018-05-12 16:22:49.422  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profile}/{label}/**],methods=[GET]}" onto public java.lang.String org.springframework.cloud.config.server.resource.ResourceController.retrieve(java.lang.String,java.lang.String,java.lang.String,javax.servlet.http.HttpServletRequest,boolean) throws java.io.IOException
2018-05-12 16:22:49.424  INFO 7416 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/{name}/{profile}/**],methods=[GET],params=[useDefaultLabel]}" onto public java.lang.String org.springframework.cloud.config.server.resource.ResourceController.retrieve(java.lang.String,java.lang.String,javax.servlet.http.HttpServletRequest,boolean) throws java.io.IOException
```
接下来看config-client项目

## bus-refresh
client众多,都手动太麻烦,可通过server里的refresh解决.

启动eureka server,config server,config client,post方式访问http://localhost:7060/actuator/bus-refresh.
通过console可以看到server和client都已触发.

如果想灰度,可以http://localhost:7060/actuator/bus-refresh/config-client:\*\*,
表示名字为config-client的所有service,注意这里是/actuator/bus-refresh/{destination}格式,
也不是网上大部分或官网写的/bus/refresh?destination=customers:\*\*.