MiniFramework 简介
====================

MiniFramework 是一款超轻量级的 PHP 开发框架，用以满足开发者最基础的 MVC 分层开发需求，在支持数据库和缓存访问等少量功能基础上，做到尽可能精简，以保证框架运行效率。

目录结构
====================

```
MiniFramework/
|--- App/                   应用案例
|    |--- Controllers/      控制器
|    |--- Layouts/          布局
|    |--- Models/           模型
|    |--- Public/           站点根目录
|    |    |--- css/         样式
|    |    |--- index.php    应用入口文件
|    |--- Views/            视图
|
|--- Mini/                  框架核心
```

部署应用
====================

请将 Apache 或 Nginx 的站点根目录指向 App 中的 Public 目录。

如果你可以通过访问类似于

`http://localhost/index.php?c=index&a=index`

这样的 URL 获得一个“Hello World!”页面，这说明你已经部署成功了。

设置伪静态
====================

本框架在设置了 Rewrite 规则后，可实现类似下面这种伪静态访问方式

`http://localhost/Controller/Action/param1/value1/param2/value2`

运行于 Apache 的设置方法：

向 Public 目录中添加一个 .htaccess 文件（附带的应用案例中已提供），内容如下：

```
RewriteEngine on
RewriteRule !.(bmp|gif|ico|jpg|png|js|css)$ index.php
```

运行于 Nginx 的设置方法：

在 nginx.conf 中，找到对应的站点，向 server{} 中添加如下设置：

```
location / {
    index  index.html index.php;
    if (!-e $request_filename) {
        rewrite ^/(.*)$ /index.php last;
    }
}
```

使用布局
====================

MiniFramework 的布局 (Layout) 功能默认是关闭的，如需使用，请在你的应用入口文件 `Public/index.php` 中定义常量 `LAYOUT_ON` 的值为 `true`，例如：

```
define('LAYOUT_ON', true);
```

提示：附带的应用案例中，已经开启了布局功能，并演示了如何使用布局。

连接数据库
====================

MiniFramework 目前只支持 MySQL 数据库，使用方法如下：

```
$db = Db::factory ('Mysql',
    array (
        'host'      => 'localhost', //主机地址
        'port'      => 3306,        //端口
        'dbname'    => 'mydbname',  //库名
        'username'  => 'myuser',    //用户名
        'passwd'    => '123456',    //密码
        'charset'   => 'utf8'       //字符编码
    )
);
```

使用缓存
====================

MiniFramework 支持三种缓存方式，分别是：Memcache、Redis 和 File（磁盘文件存储）。

使用方法如下：

```
//以最常用的 Memcache 为例
$cache = Cache::factory ('Memcache',
    array (
        'host'      => 'localhost', //主机
        'port'      => 11211,       //端口
        'prefix'    => 'MINI_'      //缓存名前缀，默认值为空
    )
);

//写入一个名为 test 的缓存，值为 abc，有效时间为 3600 秒
$cache->set('test', 'abc', 3600);

//读取名为 test 的缓存
$test = $cache->get('test');
```

其他
====================

关于控制器、模型和视图的使用方法，请参考附带的应用案例中提供的相关代码。

关于作者
====================

作者：Jason Wei

信箱：jasonwei06@hotmail.com

博客：http://www.sunbloger.com

MiniFramework 最初为本人学习目的而编写，经过不断的补充完善演变而来，其中有很多不足之处，还望大家多多批评指点。