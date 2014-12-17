#查询分组


我们来说说几种查询分组的方法



### 查找所有分组

```

$groups = Sentry::findAllGroups();

```

### 通过分组 id 查找一个分组

```
try
{
    $group = Sentry::findGroupById(1);
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '分组不存在';
}

```

### 通过分组名称 查找一个分组
```
try
{
    $group = Sentry::findGroupByName('admin');
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '分组不存在';
}

```

### 查找分组的权限

```
try
{
    $group = Sentry::findGroupById(1);

    // 获取用户的权限
    $groupPermissions = $group->getPermissions();
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '分组不存在';
}
```