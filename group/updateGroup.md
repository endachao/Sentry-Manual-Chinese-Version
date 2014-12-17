#修改分组


我们上节中，几行代码创建了分组，在这节中，我们依旧几行代码来修改我们的分组

```

try
{
    // 通过 分组ID 查找分组
    $group = Sentry::findGroupById(1);

    // 更新分组详情
    $group->name = 'Users';
    $group->permissions = array(
        'admin.index' => 0,
        'users.index' => 0,
    );

    // 更新分组
    if ($group->save())
    {
        // 分组信息已被更新
    }
    else
    {
        // 分组信息未被更新
    }
}
catch (Cartalyst\Sentry\Groups\NameRequiredException $e)
{
    echo '分组名称必须存在';
}
catch (Cartalyst\Sentry\Groups\GroupExistsException $e)
{
    echo '分组已经存在';
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '分组不存在';
}

```

好了，简简单单的，我们已经修改了一个分组啦

