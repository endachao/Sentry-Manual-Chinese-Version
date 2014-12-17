#删除分组


我们依旧简单的删除分组

```

try
{
    // 通过 分组ID 查找分组
    $group = Sentry::findGroupById(1);

    // 删除分组
    $group->delete();
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '分组不存在';
}


```
