#重置密码


我们已经讲解完了，如果创建，注册用户，激活，修改，删除用户了，看似已经全了，但是我们还缺了一点

那就是为用户提供重置密码！ 这个是一个网站必不可少的功能之一吧

我们重置密码，在 sentry 中分成 两步

1、发送 重置代码给用户
2、验证 重置代码，成功则修改密码


首先我们看 第一步

**获取重置密码的代码**


```
try
{
    // 根据 email 查找用户
    $user = Sentry::findUserByLogin('yccphp@163.com');

    // 获取重置代码
    $resetCode = $user->getResetPasswordCode();

    //你可以把 重置代码，通过 email 或者 短信 发送给用户
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

哈哈，是不是和我们激活用户很像呢 ？

第二步，

**验证重置代码**

```
try
{
    // 查询用户
    $user = Sentry::findUserById(1);

    // 检查重置密码的代码是否有效
    if ($user->checkResetPasswordCode(重置代码))
    {
        // 重置用户密码
        if ($user->attemptResetPassword(重置密码,新密码))
        {
            
            // 密码重置通过
        }
        else
        {
            // 密码重置失败
        }
    }
    else
    {
        // 所提供的密码重置代码是无效的
    }
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在.';
}

```



简单两步，重置密码，就如同简单两步实现激活用户，sentry 的魅力，就是体现在这里