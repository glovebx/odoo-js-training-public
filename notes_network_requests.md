# Notes: Network Requests
# 笔记：网络请求

A web app such as the Odoo web client would not be very useful if it was unable
to talk to the server. Loading data and calling model methods from the browser
is a very common need.
如果一个 Web 应用程序（例如 Odoo Web 客户端）无法与服务器通信，它将毫无用处。从浏览器加载数据和调用模型方法是一个非常常见的需求。

Roughly speaking, there are two different kind of requests:
粗略地说，有两种不同的请求类型：

- calling a controller (an arbitrary route)
- 调用控制器（任意路由）
- calling a method on a model (`/web/dataset/call_kw/some_model/some_method`). This
  will call the python code from the corresponding method, and return the result.
- 调用模型上的方法（`/web/dataset/call_kw/some_model/some_method`）。这将调用对应方法的 Python 代码，并返回结果。

In odoo these two kind of requests are done with `XmlHTTPRequest`s, in `jsonrpc`.
在 Odoo 中，这两种请求都是通过 `XmlHTTPRequest` 的 `jsonrpc` 来完成的。

## Calling a method on a model (orm service)
## 调用模型上的方法（ORM 服务）

Let us first see the most common request: calling a method on a model. This is
usually what we need to do.
让我们首先看看最常见的请求：调用模型上的方法。这通常是我们需要做的。

There is a service dedicated to do just that: `orm_service`, located in `core/orm_service.js`
It provides a way to call common model methods, as well as a generic `call` method:
有一个专门用于执行此操作的服务：`orm_service`，它位于 `core/orm_service.js` 中。它提供了一种调用常见模型方法的方法，以及一个通用的 `call` 方法：

```js
setup() {
    this.orm = useService("orm");
    onWillStart(async () => {
        // will read the fields 'id' and 'descr' from the record with id=3 of my.model
        // 将从 my.model 中 id=3 的记录读取字段 'id' 和 'descr'
        const data = await this.orm.read("my.model", [3], ["id", "descr"]);
        // ...
    });
}
```

Here is a list of its various methods:
以下是它的各种方法列表：

- `create(model, records, kwargs)`
- `nameGet(model, ids, kwargs)`
- `read(model, ids, fields, kwargs)`
- `readGroup(model, domain, fields, groupby, kwargs)`
- `search(model, domain, kwargs)`
- `searchRead(model, domain, fields, kwargs)`
- `searchCount(model, domain, kwargs)`
- `unlink(model, ids, kwargs)`
- `webReadGroup(model, domain, fields, groupby, kwargs)`
- `webSearchRead(model, domain, fields, kwargs)`
- `write(model, ids, data, kwargs)`

Also, in case one needs to call an arbitrary method on a model, there is:
此外，如果需要在模型上调用任意方法，可以使用：

- `call(model, method, args, kwargs)`

Note that the specific methods should be preferred, since they can perform some
light validation on the shape of their arguments.
请注意，应该优先使用特定的方法，因为它们可以对参数的形态进行一些轻微验证。

## Calling a controller (rpc service)
## 调用控制器（RPC 服务）

Whenever we need to call a specific controller, we need to use the (low level)
`rpc` service. It only exports a single function that perform the request:
无论何时需要调用特定控制器，都需要使用（底层）`rpc` 服务。它只导出一个执行请求的函数：

```
rpc(route, params, settings)
```

Here is a short explanation on the various arguments:
以下是各个参数的简要说明：

- `route` is the target route, as a string. For example `/myroute/`
- `route` 是目标路由，以字符串形式表示。例如 `/myroute/`
- `params`, optional, is an object that contains all data that will be given to the controller
- `params`（可选），是一个包含将传递给控制器的所有数据的对象。
- `settings`, optional, for some advance control on the request (make it silent, or
  using a specific xhr instance)
- `settings`（可选），用于对请求进行一些高级控制（使其静默，或使用特定的 xhr 实例）。

For example, a basic request could look like this:
例如，一个基本请求可能看起来像这样：

```js
setup() {
    this.rpc = useService("rpc");
    onWillStart(async () => {
        const result = await this.rpc("/my/controller", {a: 1, b: 2});
        // ...
    });
}
```
