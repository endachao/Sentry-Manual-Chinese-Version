#配置登陆字段

sentry 中，默认的登陆凭证是 email ,但是老板是要求，用户要通过手机号登录，这个时候，怎么办呢？

其实很好解决，sentry 灵活的配置，帮你排忧解难

我们打开 sentry 的配置文件，

打开 **verdor/cartalyst/sentry/src/config/config.php**

找到  

>login_attribute

我们把他修改为

```

'login_attribute' => 'phone',

```

这个时候，去你的数据库 user 表里面，新加一个字段，取名为 phone ,就可以啦

如果有 问题 ，或者不理解的地方，欢迎加入 QQ群：365969825 

或者 新浪微博 [@袁超](http://www.weibo.com/28ex)