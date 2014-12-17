# 删除用户

前面我们说了创建用户，修改用户，本节我们说下删除用户！

看你不爽，delete 掉你！就是这么任性！

** delete 删除用户**

```
try
{
    // 根据 user id 查询用户信息
    $user = Sentry::findUserById(1);

    // 删除用户
    $user->delete();
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在！';
}

```

sentry 暂时就提供 delete 方法来删除用户，所以那句 

>看你不爽，delete 掉你

可不是说白话哦