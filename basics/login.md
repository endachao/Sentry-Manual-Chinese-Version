# 登陆 & 登出 & 是否登陆


#### 使用 sentry

在控制器里面，我们使用 sentry，需要在控制器顶部 use 以下

```
use Sentry

```


#### 登陆 之 基于凭证登陆用户

首先，登陆非常简单，就是收集表单数据，验证表单数据，我们这里主要讲解怎么验证表单数据

看下面代码

```

		$cred = [
            'email'=>Input::get('email'),
            'password'=>Input::get('password'),
        ];

        try{
            $user = Sentry::authenticate($cred,false);

            if($user){
                return Redirect::route('admin.index.index');
            }
        }catch (\Exception $e){
            return Redirect::route('admin.login')->withErrors(array('login' => $e->getMessage()));
        }
        

```

我们使用 Input 接受到了表单数据，然后我们调用了 Sentry::authenticate 来验证登陆数据，那么这个 authenticate 的参数是什么意思呢？我们看下面参数表

Param        | Required | Default | Type  | Description
------------ | -------- | ------- | ----- | -----------------------------------
$credentials | true     | null    | array | 数组内必须包含用户凭证，例如 `email` 和 `password`。`password`.
$remember    | false    | false   | bool  |  它将设置一个 Cookie，用来标记 Sentry 是否应该记住这个用户。


第一个参数就是 我们用户填入的 email 和密码数组，
第二个参数则为 布尔值，为 true 则表示记住这个用户,你可以拿他实现网站上面的 记住我 的这个功能

简单的几行代码，即可实现我们的 验证数据登陆啦，是不是很简单呢？




#### 登陆 之 根据所提供的 user 模型登陆

看文档看到这个方法，不知道有啥用，先根据 用户id 查询出它的信息，然后在登陆，好像我是没咋用，但还是和大家说说吧

```
	// 通过 user id 查找用户
    $user = Sentry::findUserById(1);

    // 登录用户
    Sentry::login($user, false);

```

老规矩，我们看 Sentry::login 所需的参数

Param        | Required | Default | Type   | Description
------------ | -------- | ------- | ------ | -----------------------------------
$user        | true     | null    | object | 一个 user 的查询结果
$remember    | false    | false   | bool   | 是否记住用户

和我们上面的 基于数据登陆很像对不对，只不过 第一个参数是 user 的查询结果罢了

   



#### 检测用户是否登陆

检测用户是否登陆也是非常方便的，我们来看代码

```
if ( ! Sentry::check())
{
    // 用户“未登录”或“未激活”
}
else
{
    // 用户已经登录
}

```
Sentry::check() 就是检测用户是否登陆的方法，

返回值

true : 用户已经登陆
false : 用户没有登陆


#### 退出登录

上面的方法是不是使用起来很简单呢？我们的退出登录也是非常简单的，只需一行代码

```
Sentry::logout();

```



> 以上就是 登录，检测是否登陆，退出的方法了，是不是很简单呢？哈哈，有问题可以加 我们的交流QQ群：365969825 ，或者新浪微博 [@袁超](http://weibo.com/28ex)