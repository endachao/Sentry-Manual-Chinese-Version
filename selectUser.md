#各种查询用户的方法集合

具体的检查用户是否具有某个权限，我们单开权限篇讲解，本来给用户规划了一个权限篇的，但是分组的权限，和用户的权限，其实都是一样的，所以我们单开一个权限篇讲解

[2.3 权限篇](priLogin.md)


单独开出一节，讲解通过这种方法查询用户的方法


### 查询当前登陆用户

```
// 获得当前登陆的用户
$user = Sentry::getUser();

$user->first_name;


```


### 查询所有用户

```
$users = Sentry::findAllUsers();

```

### 查找具有指定权限的用户

```

// 当仅指定一个权限时，数组参数，只要数组元素中的权限码，用户具有一个 就可以查出
$users = Sentry::findAllUsersWithAccess(array('admin', 'other'));

// 查询 具有 admin 权限的用户
$users = Sentry::findAllUsersWithAccess('admin');

```

### 查找分组下的所有用户

```
// 查出分组
$group = Sentry::findGroupByName('admin');

// 查出分组下的所有用户
$users = Sentry::findAllUsersInGroup($group);

```


### 根据资料，查询用户

根据凭证数组查找一个用户，其中必须包含登录字段。

```
try
{
    $user = Sentry::findUserByCredentials(array(
        'email'      => 'yccphp@163.com',
        'password'   => '101058',
        'first_name' => 'yuan',
    ));
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

### 根据用户id 查找用户

```

try
{
    $user = Sentry::findUserById(1);
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

### 通过登录字段查找一个用户

登录字段，默认为 email ,可配置，详解 [2.2配置登陆字段](setlogin.md)

```
try
{
    $user = Sentry::findUserByLogin('yccphp@163.com');
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

### 通过激活码查找一个用户

我们注册的时候，有发送给用户一个激活码，这个方法就是根据激活码来查找这个用户

```

try
{
    $user = Sentry::findUserByActivationCode(激活码);
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在.';
}
```

### 通过密码重置码 查找一个用户

对应的，我们在用户重置密码的时候，也给用户发送了一个 重置码 ，我们可以通过它，来查找用户

```
try
{
    $user = Sentry::findUserByResetPasswordCode(重置码);
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

### 验证密码

这个功能有点鸡肋，先查询出用户，在验证密码，不知道在哪些地方有用，所以归类到 查询方法集合

```
try
{
    // 查询用户
    $user = Sentry::findUserById(1);

    if($user->checkPassword('密码'))
    {
        echo '密码正确';
    }
    else
    {
        echo '密码错误';
    }
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}


```

### 查询用户的分组

```
try
{
    // 查询用户
    $user = Sentry::findUserByID(1);

    // 获取用户的分组
    $groups = $user->getGroups();
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```


### 获取用户的权限

** 注意：只返回用户的权限，不包括所在分组的权限**
```
try
{
    // 查询一个用户
    $user = Sentry::findUserByID(1);

    // 获取用户的权限
    $permissions = $user->getPermissions();
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在.';
}

```

### 获取用户分组 ＋ 个人的权限
** 它合并了 用户的分组权限 和 个人权限**

```

try
{
    // 查询用户
    $user = Sentry::getUserProvider()->findById(1);

    // 获得合并后的权限信息
    $permissions = $user->getMergedPermissions();
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在.';
}

```

