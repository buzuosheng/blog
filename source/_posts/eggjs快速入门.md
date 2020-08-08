---
title: eggjs快速入门
date: 2020-05-06 21:40:13
tags:
---

## Koa

> Koa是一个新的web框架，由Express幕后的原班人马打造，致力于成为web应用和API开发领域中的一个更小、更富有表现力、更健壮的基石。

Koa相对于Express有几个显著的特点。

- Koa中间件选择了**洋葱模型**。
- Koa除了Express的Request和Response两个对象外，增加了**Context对象**。
- 将捕获异常中间件放在其他中间件之间，就可以捕获到它们的异常了。

### 中间件洋葱模型

![中间件洋葱图](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMC4AxGKPtpapRbUfbgWrJHKia0qGotyBWHUQT5RJkZBXPfh9bERTbSpm0TB8xkykicSnlX9Tq9OP5Nw/0?wx_fmt=png)

中间件执行顺序：

![中间件执行顺序](https://mmbiz.qpic.cn/mmbiz_gif/GY9ZJPx6bMC4AxGKPtpapRbUfbgWrJHKVvJiaG4TgOfXMBRf63TuuqB3RsUOCqrSrlUGVrByLToH93qSwdVnJoA/0?wx_fmt=gif)

每个中间件就像是洋葱的一层，所有的请求经过一个中间件都会执行两次，这样可以非常方便的后置处理逻辑。

## Egg.js

> Egg.js为企业级框架和应用而生，我们希望由Egg孕育出更多上层框架，帮助开发团队和开发人员降低开发和维护成本。

Egg不直接提供功能，只是集成各种功能插件。简单来说，就一个词，那就是**轻量化**。

Egg是基于Koa开发的，选择其作为基础框架，在它的模型基础上，进行了一些增强。

- 扩展

在基于Egg的框架或者应用中，我们可以定义`app/extend/{application, context, request, response}.js`来扩展Koa中对应的四个对象的原型，这样我们就可以快速的增加更多的辅助方法。

- 插件

**一个插件可以包含`extend`，`middleware`，`config`**。`extend`扩展基础对象的上下文，提供各种工具类、属性。`mdidleware`增加一个或多个中间件，提供请求的前置、后置处理逻辑。`config`配置各个环境下插件自身的默认配置项。

## 第一个项目

使用脚手架可以快速生成项目。

``` 
npm init egg --type=simple
```

egg项目文件结构规范。

```
egg-project
├── package.json
├── app.js (可选)
├── agent.js (可选)
├── app
|   ├── router.js
│   ├── controller
│   |   └── home.js
│   ├── service (可选)
│   |   └── user.js
│   ├── middleware (可选)
│   |   └── response_time.js
│   ├── schedule (可选)
│   |   └── my_task.js
│   ├── public (可选)
│   |   └── reset.css
│   ├── view (可选)
│   |   └── home.tpl
│   └── extend (可选)
│       ├── helper.js (可选)
│       ├── request.js (可选)
│       ├── response.js (可选)
│       ├── context.js (可选)
│       ├── application.js (可选)
│       └── agent.js (可选)
├── config
|   ├── plugin.js
|   ├── config.default.js
│   ├── config.prod.js
|   ├── config.test.js (可选)
|   ├── config.local.js (可选)
|   └── config.unittest.js (可选)
└── test
    ├── middleware
    |   └── response_time.test.js
    └── controller
        └── home.test.js
```

- `app/router.js`，用于配置URL路由规则。
- `app/controller/**`，用于解析用户的输入，处理后返回相应的结果。
- `app/service/**`，用于编写业务逻辑层，可选。
- `app/middleware/**`，用于编写中间件。
- `app/public/**`，用于放置静态资源。
- `app/extend/**`，用于扩展框架。
- `config/config.{env}.js`，用于编写配置文件。
- `config/plugin.js`用于配置需要加载的插件。
- `test/**`，用于单元测试。
- `app.js`和`agent.js`，用于自定义启动时的初始化工作，可选。#

初始化项目后，直接使用`npm run dev`启动项目，在`localhost:7001`就可以看到。

![hi, egg](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMC4AxGKPtpapRbUfbgWrJHKSO4q1npAic2sUfJibZTbLY9CLlYtG2QDPObp9yO6q0YAcd5pycLXibjIQ/0?wx_fmt=png)

## 连接数据库

使用sequelize连接数据库，首先安装`egg-sequelize`和`mysql2`。

``` 
npm install --save egg-sequelize mysql2
```

在`config/plugin.js`中启用eggsequelize插件。

``` js
// config/plugin.js
exports.sequelize = {
  enable: true,
  package: 'egg-sequelize',
};
```

在`config/config.default.js`中添加sequelize配置。

``` js
// config/config.default.js
config.sequelize = {
  dialect: 'mysql',
  host: '127.0.0.1',
  port: 3306,
  database: 'egg-sequelize-doc-default',
  username: 'root',
  password: 'w123456',
}
```

在数据库中创建表并插入数据。

![数据库](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMC4AxGKPtpapRbUfbgWrJHKJM2UgkvuqKNa5xvKtnxkra0X22Ae39ISb98OhrdPD12w9DZPAUf5uA/0?wx_fmt=png)

在`app/model/user.js`中编写代码实现业务逻辑。

``` js
// app/model/user.js
'use strict';

module.exports = app => {
  const {
    STRING,
    INTEGER,
  } = app.Sequelize;

  const User = app.model.define('user', {
    id: {
      type: INTEGER,
      primaryKey: true,
      autoIncrement: true,
    },
    name: STRING(50),
    sex: STRING(4),
    pass: STRING(32),
  }, {
    freezeTableName: true,
    timestamps: false,
  });

  return User;
};

```

这个Model就可以在Controller和Service中通过`app.model.User`访问，编写Controller调用这个Model。

```js
// app/controller/user.js
'use strict';

const Controller = require('egg').Controller;

class UserController extends Controller {
  async index() {
    const _ctx = this.ctx;
    const user = await _ctx.model.User.findAll();
    _ctx.body = user;
  }
}

module.exports = UserController;

```

将controller挂载到路由上。

``` js
// app/router.js
module.exports = app => {
  const { router, controller } = app;
  router.resources('users', '/users', controller.users);
};
```

使用`npm run dev`命令启动项目。在`localhost:7001/user`看到数据库中的数据。

![访问成功](https://mmbiz.qpic.cn/mmbiz_png/GY9ZJPx6bMC4AxGKPtpapRbUfbgWrJHKb44mMQ6oIMMiapSt2aib2sZmtkw4GmzDxO97V26sgzmOpGuhpS4RyoPw/0?wx_fmt=png)

访问成功！

![个人微信公众号](https://img-blog.csdnimg.cn/20200407111014270.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxOTA3ODA2,size_16,color_FFFFFF,t_70#pic_center)