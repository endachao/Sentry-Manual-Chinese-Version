#激活用户


在 [3.1创建用户](createUser.md) 中，我们注册了一个用户，并且产生了激活码，这个时候，我们得到激活码，怎么激活用户呢？

其实很简单，看代码

```
try
{
    // 根据 userid 查询用户
    $user = Sentry::findUserById(1);

    // 使用激活码激活用户
    if ($user->attemptActivation('产生的激活码'))
    {
        // 用户激活成功
    }
    else
    {
        // 用户激活失败
    }
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}
catch (Cartalyst\Sentry\Users\UserAlreadyActivatedException $e)
{
    echo '用户已经被激活，不能重复激活';
}

```

很简单的，我们就可以实现激活用户的功能啦！