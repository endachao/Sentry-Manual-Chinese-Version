# 修改用户

前面我们说过了 创建一个用户，本节我们学习如何修改一个用户

首先，我们弄清楚，我们的需求是什么

1、更新用户信息

2、更新用户信息，并且更新用户组

这些在 sentry 中也是非常简单的，我们先说更新用户信息

**仅更新用户信息**

```

try
{
    // 根据id 查询用户
    $user = Sentry::findUserById(1);

    // 更新用户的属性
    $user->email = 'yccphp@163.com';
    $user->first_name = 'Yuan';

    // 执行更新
    if ($user->save())
    {
        // 更新成功
    }
    else
    {
        // 更新失败
    }
}
catch (Cartalyst\Sentry\Users\UserExistsException $e)
{
    echo '必要字段不全';
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}


```

先查询用户，查询出来后修改用户的属性，最后调用 save 方法进行保存，是不是很简单呢？


好了，我们接着说第二种需求，更新用户时，把用户组也更新了，

也就是，这个用户本来是 超级 vip 会员组，给他更新资料的时候，顺带把他的组更新为 普通会员 用户组

** 更新用户信息顺带更新绑定的用户组**

```
try
{
    // 查询用户
    $user = Sentry::findUserById(1);

    // 查出 新的用户组
    $adminGroup = Sentry::findGroupById(1);

    // 把 用户 与 用户组绑定
    if ($user->addGroup($adminGroup))
    {
        // 绑定成功
    }
    else
    {
        // 绑定失败
    }

    // 更新用户信息
    $user->email = 'john.doe@example.com';
    $user->first_name = 'John';

    // 执行保存动作
    if ($user->save())
    {
        // 保存成功
    }
    else
    {
        // 保存失败
    }
}
catch (Cartalyst\Sentry\Users\UserExistsException $e)
{
    echo '必要字段不全';
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '用户组不存在';
}

```

是不是很简单，就是根据 用户组 id 查出需要绑定的用户组 ，

根据 用户id 查出 绑定的用户

然后调用 addGroup ，就更新了它绑定的用户组啦

更新用户，就这么多，非常简单是吧？