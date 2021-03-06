# lin-cms-tp5-reflex-core
LinCms TP 的反射类核心模块封装，含多种路由注册模式，路由及请求类型验证,基于`wangyu/reflex-core`扩展

* 反射路由模式
* 优化路由注册
* 反射参数验证
* 简洁
* 优秀

# `composer`安装说明

```bash
composer require lin-cms-tp/reflex-core
```

> 如果安装失败，报错 ` but these conflict with your requirements or minimum-stability`

请在更改`composer.json`文件

```
"require": {
    "php": ">=7.1.0",
    /* 省略其他 */
    "wangyu/reflex-core": "dev-master",
    "lin-cms-tp/reflex-core": "dev-master"
},
```

# 使用说明


## 反射路由模式

- 需要更改route.php文件注册路由方式

### 注册模块路由`LinRoute::init()`
```php
use LinCmsTp\Route as LinRoute;
// 注册模块路由
LinRoute::init(); // 等于使用 LinRoute::init('api');
```
### 注册类路由`LinRoute::cls()`
```php
use LinCmsTp\Route as LinRoute;
// 注册类路由
LinRoute::cls(
    'app\api\controller\cms\User', // 类命名空间
    ['Auth','linRouteParam'] // 中间件
);
```

### 注册方法路由`LinRoute::fuc()`
```php
use LinCmsTp\Route as LinRoute;
// 注册类路由
LinRoute::fuc(
    'app\api\controller\cms\User', // 类命名空间
    'login', // 方法
    ['Auth','linRouteParam'] // 中间件
);
```

### 设置API方法注释`@route('路由','请求类型')`

| 类型 | 模式 | 参数 | 说明 |
| --- | --- | --- | --- |
|类|route|rule| 路由前缀设置 |
|Class|route|'cms/user'|  |
|action|route|{'','get'}| 实际等于：{'cms/user/','get'} |
|action|route|{'/user/login','post'}| 实际等于：{'/user/login','post'} |


```php
/**
 * 账户登陆
 * @route('cms/user/login','post')
 * @param Request $request
 * @param('\app\api\validate\user\LoginForm')
 * @return array
 * @throws \think\Exception
 */
public function login(Request $request)
{
    $params = $request->post();
    $user = UserModel::verify($params['nickname'], $params['password']);
    $result = Token::getToken($user);
    logger('登陆领取了令牌', $user['id'], $user['nickname']);
    return $result;
}
```

## 路由中间件


| 类型 | 模式 | 参数 | 说明 |
| --- | --- | --- | --- |
|类|middleware|array| 路由中间件设置，请先在middleware.php设置好 |
|Class|middleware|{'Auth','linRouteParam'}| 相当于设置了'Auth','linRouteParam'这两个中间件 |

- 需要在系统`config`配置`middleware.php`

```php
<?php
return [
    // 默认中间件命名空间
    'default_namespace' => 'app\\http\\middleware\\',
    'linRouteParam' => LinCmsTp\Param::Class
];
```

- 需要在接口类`注释`设置`@middleware`
```php
/**
 * Class Book
 * @route('v1/book')
 * @middleware('Auth')
 * @package app\api\controller\v1
 */
class Book
{}
```

# 联系我们

- QQ: `354007048` 
- Email: `china_wangyu@aliyun.com`