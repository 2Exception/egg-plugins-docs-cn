# egg-mongoose
[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][codecov-image]][codecov-url]
[![David deps][david-image]][david-url]
[![Known Vulnerabilities][snyk-image]][snyk-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/egg-mongoose.svg?style=flat-square
[npm-url]: https://npmjs.org/package/egg-mongoose
[travis-image]: https://img.shields.io/travis/eggjs/egg-mongoose.svg?style=flat-square
[travis-url]: https://travis-ci.org/eggjs/egg-mongoose
[codecov-image]: https://img.shields.io/codecov/c/github/eggjs/egg-mongoose.svg?style=flat-square
[codecov-url]: https://codecov.io/github/eggjs/egg-mongoose?branch=master
[david-image]: https://img.shields.io/david/eggjs/egg-mongoose.svg?style=flat-square
[david-url]: https://david-dm.org/eggjs/egg-mongoose
[snyk-image]: https://snyk.io/test/npm/egg-mongoose/badge.svg?style=flat-square
[snyk-url]: https://snyk.io/test/npm/egg-mongoose
[download-image]: https://img.shields.io/npm/dm/egg-mongoose.svg?style=flat-square
[download-url]: https://npmjs.org/package/egg-mongoose

Egg.js的mongoose插件.

## 安装

```bash
$ npm i egg-mongoose --save
```

## 配置

改变 `{app_root}/config/plugin.js` 来启用 `egg-mongoose` 插件:

```js
exports.mongoose = {
  enable: true,
  package: 'egg-mongoose',
};
```

## 简单连接

### 配置

```js
// {app_root}/config/config.default.js
exports.mongoose = {
  url: 'mongodb://127.0.0.1/example',
  options: {},
};
// recommended
exports.mongoose = {
  client: {
    url: 'mongodb://127.0.0.1/example',
    options: {},
  },
};
```

### 示例

```js
// {app_root}/app/model/user.js
module.exports = app => {
  const mongoose = app.mongoose;
  const Schema = mongoose.Schema;

  const UserSchema = new Schema({
    userName: { type: String  },
    password: { type: String  },
  });

  return mongoose.model('User', UserSchema);
}

// {app_root}/app/controller/user.js
exports.index = function* (ctx) {
  ctx.body = yield ctx.model.User.find({});
}
```

## 多连接

### 配置

```js
// {app_root}/config/config.default.js
exports.mongoose = {
  clients: {
    // clientId, access the client instance by app.mongooseDB.get('clientId')
    db1: {
      url: 'mongodb://127.0.0.1/example1',
      options: {},
    },
    db2: {
      url: 'mongodb://127.0.0.1/example2',
      options: {},
    },
  },
};
```

### 示例

```js
// {app_root}/app/model/user.js
module.exports = app => {
  const mongoose = app.mongoose;
  const Schema = mongoose.Schema;
  const conn = app.mongooseDB.get('db1'); 

  const UserSchema = new Schema({
    userName: { type: String  },
    password: { type: String  },
  });

  return conn.model('User', UserSchema);
}

// {app_root}/app/model/book.js
module.exports = app => {
  const mongoose = app.mongoose;
  const Schema = mongoose.Schema;
  const conn = app.mongooseDB.get('db2');

  const BookSchema = new Schema({
    name: { type: String },
  });

  return conn.model('Book', BookSchema);
}

// app/controller/user.js
exports.index = function* (ctx) {
  ctx.body = yield ctx.model.User.find({}); // get data from db1
}

// app/controller/book.js
exports.index = function* (ctx) {
  ctx.body = yield ctx.model.Book.find({}); // get data from db2
}
```

### 默认配置

访问 [config/config.default.js](config/config.default.js)查看更多细节.

## Mongo集群支持

```js
// {app_root}/config/config.default.js
exports.mongoose = {
  client: {
    url: 'mongodb://mongosA:27501,mongosB:27501',
    options: {
      mongos: true,
    },
  },
};
```

## 问题 & 建议

请在[这里](https://github.com/eggjs/egg/issues)创建一个issue。

## 贡献

如果你是一个贡献者，请查看这里[CONTRIBUTING](https://eggjs.org/zh-cn/contributing.html)。

## 许可证

[MIT](LICENSE)