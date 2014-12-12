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
$credentials | true     | null    | array | Array that should contain the user credentials like `email` and `password`.
             |          |         |       | 数组内必须包含用户凭证，例如 `email` 和 `password`。
$remember    | false    | false   | bool  | Flag to wether Sentry should remember the user. It sets a Cookie.
             |          |         |       | 它将设置一个 Cookie，用来标记 Sentry 是否应该记住这个用户。


第一个参数就是 我们用户填入的 email 和密码数组，
第二个参数则为 布尔值，为 true 则表示记住这个用户,你可以拿他实现网站上面的 记住我 的这个功能

简单的几行代码，即可实现我们的 验证数据登陆啦，是不是很简单呢？






