# SpringSecurity快速入门

Spring Security 是针对Spring项目的安全框架，也是Spring Boot底层安全模块默认的技术选型，他可以实现强大的Web安全控制，对于安全控制，我们仅需要引入 `spring-boot-starter-security` 模块，进行少量的配置，即可实现强大的安全管理。

需要记住下面几个类：

1. WebSecurityConfigurerAdapter：自定义Security策略
2. AuthenticationManagerBuilder：自定义认证策略
3. @EnableWebSecurity：开启WebSecurity 模式

SpringSecurity的最主要的两个目标是“认证”和“授权”（访问控制）

1. 认证：验证凭据，如用户名密码等信息，以验证登录者身份。通常都是用户名密码方式，有时候会与身份验证因素结合使用
2. 授权：授权发生在系统成功验证您的身份之后，最后会授予您访问资源（如信息、文件、数据库、资金、位置等等几乎任何内容）





