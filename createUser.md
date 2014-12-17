#创建用户




#### 创建用户 之 createUser 方法


** 创建用户并分配用户组 **

我们创建一个用户，并且为它分配用户组，他会继承自用户组的权限

```
try
{
    // 创建一个用户 login字段是必须的 默认是 email 可配置，参考 [2.2 配置登陆字段](setlogin.md)
    $user = Sentry::createUser(array(
        'email'     => 'yccphp@163.com',
        'password'  => '123456',
        'activated' => true,
    ));

    // 查找用户组 where 为 组id ＝ 1
    $adminGroup = Sentry::findGroupById(1);

    // 把 用户加到 用户组
    $user->addGroup($adminGroup);
}
catch (Cartalyst\Sentry\Users\LoginRequiredException $e)
{
    echo 'login 字段是必须的';
}
catch (Cartalyst\Sentry\Users\PasswordRequiredException $e)
{
    echo '没有提供密码';
}
catch (Cartalyst\Sentry\Users\UserExistsException $e)
{
    echo '用户已经存在';
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '用户组不存在';
}

```

** 创建一个用户 单独设置权限**

上面代码中，我们创建了一个用户，并且为他分配了用户组，继承了用户组的权限

但是，我们想一个用户，不加入用户组，但是又有自己的权限，sentry 可以帮我们实现吗？

答案是，必须的！

```
try
{
    // 创建一个用户
    $user = Sentry::createUser(array(
        'email'       => 'yccphp@163.com',	// 邮箱
        'password'    => '123456',			// 密码
        'activated'   => true,				// 是否允许登陆
        'permissions' => array(				// 权限组
            'user.create' => -1,			// 拒绝访问  user.create
            'user.delete' => -1,			// 拒绝访问  user.delete
            'user.view'   => 1,				// 允许访问  user.view
            'user.update' => 1,				// 允许访问  user.update
        ),
    ));
}
catch (Cartalyst\Sentry\Users\LoginRequiredException $e)
{
    echo 'login 字段是必须的';
}
catch (Cartalyst\Sentry\Users\PasswordRequiredException $e)
{
    echo '密码字段不存在'
}
catch (Cartalyst\Sentry\Users\UserExistsException $e)
{
    echo '创建的用户已经存在';
}

````


可以看到我们对 permissions 字段进行了赋值，这个字段正是表明了 这个用户有什么权限，我们看下用户的权限码

```
-1 : 拒绝
 1 : 允许
 0 : 继承

```

我们可以限制用户，可以访问什么，不可以访问什么，权限控制的是不是很灵活呢 ？

-1 ，1 我们可以很轻易的理解，但是这个 0 权限继承是什么意思呢？

0 表示继承，继承自 分组！就是说，我们 设置权限 1 或者 -1 的时候，其实是覆盖了分组的权限，但是我们设置为 0  的时候，是继承自分组的权限哦！



#### 创建用户 之 register 注册用户

上面我们讲解了 创建用户，本节我们讲解下 注册用户。

需求如下：
	用户通过 表单注册用户，初始注册的 用户是不可以登录的，必须通过邮件激活用户，这个需求我们怎么做呢？
	
其实，在 sentry 中，为我们提供了 一条龙的 用户体系服务，这些 在 sentry 中都变的很简单

我们先讲解 注册用户，产生激活码吧


```
try
{
    // 从表单收集的数据 注册用户
    $user = Sentry::register(array(
        'email'    => 'yccphp@163.com',
        'password' => '123456',
    ));

    // 获取此用户的 激活码
    $activationCode = $user->getActivationCode();

    // 你可以继续 ，把激活码通过短信，或者 email 发送给 用户
}
catch (Cartalyst\Sentry\Users\LoginRequiredException $e)
{
    echo 'login 字段是必须的';
}
catch (Cartalyst\Sentry\Users\PasswordRequiredException $e)
{
    echo '没有提供密码';
}
catch (Cartalyst\Sentry\Users\UserExistsException $e)
{
    echo '用户已存在';
}

```
我们看上面的代码，首先我们调用 register 注册了一个新用户，这个 register 的参数如下

Param        | Required | Default | Type    | Description
------------ | -------- | ------- | ------- | -----------------------------------
$credentials | true     | null    | array   | 用户的资料。
$activate    | false    | false   | boolean | 是否手动激活


我们看 第二个参数 activate ，默认是 false ，就是需要会员手动来激活。

我们上面的代码，就是需要会员手动激活，然后我们调用了 getActivationCode ，产生了一个 激活码，来作为激活用户的凭证


如果我们不需要会员手动激活，只需要要 把 第二个参数设置为 true ,就可以跳过此步骤



好了，我们已经完成了，注册用户，产生激活码的步骤了，至于怎么激活用户呢？请看 [3.5激活用户](actUser.md)