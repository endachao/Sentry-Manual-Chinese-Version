#创建分组


创建一个分组，是非常简单的，我们来学习下吧

```
try
{
    // 创建分组
    $group = Sentry::createGroup(array(
        'name'        => 'admin',
        'permissions' => array(
            'admin.index' => 1,
            'users.index' => 1,
        ),
    ));
}
catch (Cartalyst\Sentry\Groups\NameRequiredException $e)
{
    echo '分组名称必须存在';
}
catch (Cartalyst\Sentry\Groups\GroupExistsException $e)
{
    echo '分组已经存在';
}

```

值得注意的是，permissions 的值是一个数组哦～

创建分组篇完毕，简单吧？