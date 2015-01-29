# 安装

安装 sentry 是一件非常简单的事情哦，下面跟着我们一起来安装吧！


#### 加入 composer.json

composer 是一个项目依赖管理工具，我们把我们需要安装的 sentry 版本号加入进入，执行安装命令后，会自动帮我们安装 sentry 并且安装其依赖的软件包

往里面写入

```
"require": {
        "cartalyst/sentry": "2.1.4"
	},
```

现在，在 命令行中，切换到你的项目根目录，输入安装命令来安装吧

```
composer update
```



### app 配置


等你的 composer update 执行完毕后，恭喜你已经完成了安装的 50% ,接下来，我们把 sentry 与我们的 laravel 框架关联起来，这个步骤是每个扩展包安装时，都会走的一步，别紧张，慢慢来



打开 app/config/app.php 文件，往 *providers* 这个数组里面加入一行

```
'Cartalyst\Sentry\SentryServiceProvider',

```

完成后，我们看下面一个数组 *aliases* ,我们往里面加入一行

```
'Sentry'            => 'Cartalyst\Sentry\Facades\Laravel\Sentry',

```

好了，配置完成后，又要恭喜你了，你已经完成了安装的 80% ，哈哈，是不是很简单呢？继续跟着我们来安装吧


### 数据库配置

在进行这一步之前，请先保证你的 app/config/database.php 里面配置了数据库连接信息

在命令行中，输入以下代码运行

```
php artisan migrate --package=cartalyst/sentry

```

这个时候，看下你的数据库里面，是不是多了 5 张数据表呢？哈哈，就是这么智能，但是这些表暂时是没有数据的，我们要做一些初始化工作


### 初始化数据

这个时候，虽然我们数据库里面有了 5 张表，但是我们表里面是没有数据的，我们给他添加一些初始化数据测试一下

在 app/database/seeds/ 下新建 SentrySeeder.php 写入以下内容

```
<?php
/**
 * User: 袁超<yccphp@163.com>
 * Time: 2014.11.19 下午6:12
 */

class SentrySeeder extends Seeder {

    public function run()
    {
        // 清空数据
        DB::table('users')->delete();
        DB::table('groups')->delete();
        DB::table('users_groups')->delete();

        // 创建用户
        Sentry::getUserProvider()->create(array(
            'email'      => '653069653@qq.com',
            'password'   => "101058",
            'first_name' => '超',
            'last_name'  => '袁',
            'activated'  => 1,
        ));

        // 创建用户组
        Sentry::getGroupProvider()->create(array(
            'name'        => 'Admin',
            'permissions' => ['admin' => 1],
        ));

        // 将用户加入用户组
        $adminUser  = Sentry::getUserProvider()->findByLogin('653069653@qq.com');
        $adminGroup = Sentry::getGroupProvider()->findByName('Admin');
        $adminUser->addGroup($adminGroup);
    }
}

```


以上代码都是 laravel 基础知识，看不懂就需要去看看官方手册了哦！

我们打开 app/database/seeds/ DatabaseSeeder.php  在他的 run 方法中，新增一行

```
$this->call('SentrySeeder');

```

接着，我们在命令行运行 
```
php artisan db:seed
```


打开你的数据库，查看 

users 是否有了 用户数据 ？
groups 是否有了 用户组数据 ？

users_groups 是否有了用户id 与 用户组id  ？

如果都有了的话，恭喜你，安装顺利完成！

如果在安装中遇到问题，可加入我们的 qq 交流群：365969825 ，或者在新浪微博 [@袁超](http://weibo.com/28ex)








