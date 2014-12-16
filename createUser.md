#创建用户




#### 创建用户 之 createUser 方法


** 创建用户并分配用户组 **

我们创建一个用户，并且为它分配用户组，他会继承自用户组的权限

```
try
{
    // 创建一个用户 下面字段是必须的
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
    echo '必须的字段不全';
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
    echo '必须字段不全';
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



