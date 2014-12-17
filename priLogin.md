#权限篇

首先，我们需要知道，用户组权限和用户权限是一样的

给用户组分配权限，无非就是给用户组的 permissions 字段赋值 

给用户分配权限，无非就是给 用户 permissions 字段赋值

用户的权限优先级高于 用户组，所以相同的权限，定义在 用户 permissions 里面，会覆盖用户组的权限

然后我们看下

### 权限是什么？

权限，就是定义 你可以干什么，不可以干什么

比如：

我们的一个学生管理系统，分下列角色

一：管理组

	1.录入员 （只可以录入学生信息）
	2.教师  （只可以查看学生的基本信息）
	3.校长	（可以删除学生信息，查看学生相册，看私房照）
	

二：用户组

	1.学生 （只可以查询自己的信息）
	
	

我们分析上面所有动作

1.录入学生信息
2.查看学生信息
3.删除学生信息
4.查看学生相册

可以清晰的看到，管理组特征是 都拥有 查看学生的信息 这个权限，用户组 只有 查看信息的权限


权限对应为

```
// 录入员
$admin_1 = array(
	'create.xuesheng'=>1, // 允许 创建学生
	'xuesheng.chakan'=>0, // 拒绝 查看学生 
	'xuesheng.zhaopian'=>0, // 拒绝 看学生私房照

);

// 教师
$admin_2 = array(
	'xuesheng.chakan'=>1, // 允许 查看学生
	'create.xuesheng'=>0, // 拒绝 创建学生
	'xuesheng.zhaopian'=>0, //拒绝  看学生私房照

);

// 校长
$admin_3 = array(
	'create.xuesheng'=>1, // 允许 创建学生
	'xuesheng.chakan'=>1, // 允许 查看学生 
	'xuesheng.zhaopian'=>1, //允许 看学生私房照
);

// 学生
$user = array(
	'xuesheng.chakan'=>1, //允许 查看学生 
	'create.xuesheng'=>0, //拒绝 创建学生
	'xuesheng.zhaopian'=>0, //拒绝 看学生私房照

);

```

上面的 0 1  代表什么意思呢？

在用户组权限中

```
0 : 拒绝
1 : 允许

```

在 用户权限中

```
-1 : 拒绝
 1 : 允许
 0 : 继承

```

所以，我们给不同的组不同的权限，然后 给用户分配 用户组，用户的权限就 继承了用户组的权限啦



### 用户权限

**我们曾经说过，用户权限等级 高于 用户组权限**

比如：

一个 名为：张三 的老师，他属于管理组，但是，他得罪了校长，校长 给他打入冷宫30 天，期间不可以查看学生信息

这个时候，他原先就属于管理组，总不能给他 t 出 管理组吧？

这个时候，用户权限就派上用场了

```


try
{
	// 权限信息
	
	$per = array(
		'xuesheng.chakan'=>-1
	);
	
    // 查出张三的信息
    $user = Sentry::findUserById(1);

    // login 字段是必须的
    $user->email = 'yccphp@163.com';
    $user->permissions = $per;

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
    echo 'login 字段是必须的';
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

假如 在管理员组中 xuesheng.chakan ＝ 1  ，我们就可以定义用户权限 xuesheng.chakan = -1 ,这个时候，用户的权限，就覆盖了管理员组的权限，他就不可以访问了




### 验证用户是否具有指定的权限

比如我们检测 张三 老师 是否具有查看学生的权限

```

try
{
    // 查出张三老师
    $user = Sentry::findUserByID(1);

    // 要检查多个权限需要使用一个数组来进行参数传递
    if ($user->hasAccess('xuesheng.chakan'))
    {
        // 有权限
    }
    else
    {
        // 没权限
    }
}
catch (Cartalyst\Sentry\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

这个检查权限，会先把 用户的权限查出来，用户组的权限查出来，然后合并，检查

### 验证用户是否具有指定权限的一种

也就是说，我给出很多权限，你只要有 具有一个，你就通过了

```

try
{
    $user = Sentry::getUserProvider()->findById(1);

    // admin 或者 foo 随便具有其中一个，你就通过
    if ($user->hasAnyAccess(array('admin', 'foo')))
    {
        // 有权限
    }
    else
    {
        // 没权限
    }
}
catch (Cartalyst\Sentry\UserNotFoundException $e)
{
    echo '用户不存在';
}

```

### 检查用户是否被激活

我们做网站的什么，通常用户没被激活是不可以登陆的，我们这就是检查他是不是被激活了

```
try
{
    
    $user = Sentry::findUserByLogin('yccphp@163.com');

    // 检查是否被激活
    if ($user->isActivated())
    {
        // 是
    }
    else
    {
        //否
    }
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}

```


### 检查用户是否存在一个指定的分组中

我们做权限控制的时候，有时候，一些东西，只能 admin 这个管理员组可以访问，所以我们也没必要做那么细，直接检查是否存在于 admin 这个管理员组就行了

```

try
{
    $user = Sentry::findUserByID(1);

    // 查询组
    $admin = Sentry::findGroupByName('admin');

    // 检查
    if ($user->inGroup($admin))
    {
        // 存在
    }
    else
    {
        // 不存在
    }
}
catch (Cartalyst\Sentry\Users\UserNotFoundException $e)
{
    echo '用户不存在';
}
catch (Cartalyst\Sentry\Groups\GroupNotFoundException $e)
{
    echo '管理员组不存在';
}

```

权限到这讲完了，其实权限这个东西，没那么复杂，也不是需要多么高深的代码来实现，只是一种原理而已

### 扩展篇

我们上面学会了，怎么给用户权限，怎么检查权限，那么，问题来了，

我们需要在每个 action 里面，都检查用户权限吗 ？

答案是，不需要

```
// 他会 获得当前访问的路由名称
Route::currentRouteName()

```

我们可以在 filters.php 里面，

```

	// 获得当前登陆的用户
	$user = Sentry::getUser();
	
	$per = Route::currentRouteName();

	// 要检查多个权限需要使用一个数组来进行参数传递
	if ($user->hasAccess($per))
    {
        // 用户有权限访问当前的 action 
        
    }
    else
    {
        // 没权限
    }




```

哈哈，这个东西手册里面可是没有哦，感谢 @ShineForce 的提醒，为了让大家可以偷下懒，特意去看了下 Route 的代码
