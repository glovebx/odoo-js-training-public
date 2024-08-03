# Notes: Concurrency
# 笔记：并发

Concurrency is a huge topic in web application: a network request is asynchronous,
so there are a lot of issues/situations that can arises. One need to be careful
when writing asynchronous code.
并发在 Web 应用程序中是一个巨大的主题：网络请求是异步的，因此会产生许多问题和情况。在编写异步代码时需要谨慎。

Roughly speaking, there are two things that we should consider:
粗略地说，我们需要考虑两件事：

- writing efficient code, meaning that we want to parallelize as much as possible.
  Doing 3 requests sequentially takes longer than doing them in parallel
- 编写高效的代码，这意味着我们希望尽可能地并行化。顺序执行 3 个请求比并行执行它们需要更长的时间。  
- writing robust code: at each moment, the internal state of the application
  should be consistent, and the resulting UI should match the user expectation,
  regardless of the order in which requests returned (remember that a request
  can take an arbitrary amount of time to return)
- 编写健壮的代码：在任何时刻，应用程序的内部状态都应该是一致的，并且最终的 UI 应该符合用户的预期，而不管请求返回的顺序（记住，一个请求可能需要任意长的时间才能返回）。 

## Parallel versus sequential
## 并行与串行

Let's talk about efficiency. Assume that we need to load from the server two
(independant) pieces of information. We can do it in two different ways:
让我们谈谈效率。假设我们需要从服务器加载两部分（独立的）信息。我们可以通过两种不同的方式来实现：

```js
async sequentialLoadData() {
    const data1 = await loadData1();
    const data2 = await loadData2();
    // ...
}

async parallelLoadData() {
    const [data1, data2] = await Promise.all([loadData1(), loadData2()]);
    // ...
}

```

The difference will be visible in the network tab: either the requests are fired
sequentially, or in parallel. Obviously, if the two requests are independant,
it is better to make them in parallel. If they are dependant, then they have to
be done sequentially:
在网络选项卡中可以看到区别：请求要么是顺序发出的，要么是并行发出的。显然，如果两个请求是独立的，最好并行执行它们。如果它们是相关的，则必须顺序执行它们：

```js
async sequentialDependantLoadData() {
    const data1 = await loadData1();
    const data2 = await loadData2(data1);
    // ...
}
```

Note that this has implications for the way we design (asynchronous) components:
each component can load its data with an asynchronous `onWillStart` method. But
since a child component is only rendered once its parent component is ready, this
means that all `onWillStart` will run sequentially. As such, there should ideally
only ever be one or two levels of components that load data in such a way. Otherwise,
you end up with a loading cascade, which can be slow.
请注意，这对我们设计（异步）组件的方式有影响：每个组件都可以使用异步 `onWillStart` 方法加载其数据。但是，由于子组件只有在其父组件就绪后才会渲染，这意味着所有 `onWillStart` 都会顺序运行。因此，理想情况下，应该只有一到两层组件以这种方式加载数据。否则，最终会导致加载级联，这会很慢。

A way to solve these issues may be to write a controller or a python model method to
gather all the data directly, so it can be loaded in a single round-trip to the
server.
解决这些问题的另一种方法可能是编写一个控制器或一个 Python 模型方法来直接收集所有数据，以便可以在一次往返服务器的过程中加载所有数据。

## Avoiding corrupted state
## 避免状态损坏

A common concurrency issue is to update the internal state in a non atomic way.
This results in a period of time during which the component is inconsistent, and
may misbehave or crash if rendered. For example:
一个常见的并发问题是在非原子方式下更新内部状态。这会导致组件在一段时间内不一致，并且在渲染时可能会出现错误或崩溃。例如：

```js
async incorrectUpdate(id) {
    this.id = id;
    this.data = await loadRecord(id);
}
```

In the above example, the internal state of the component is inconsistent while
the load request is ongoing: it has the new `id`, but the `data` is from the
previous record. It should be fixed by updating the state atomically:
在上面的示例中，组件的内部状态在加载请求进行时是不一致的：它具有新的 `id`，但 `data` 来自以前的记录。应该通过原子方式更新状态来修复它：

```js
async correctUpdate(id) {
    this.data = await loadRecord(id);
    this.id = id;
}
```

## Mutex
## 互斥锁

As we have seen, some operations have to be sequential. But in practice, actual
code is often hard to coordinate properly. An UI is active all the time, and various
updates can be done (almost) simultaneously, or at any time. In that case, it can become difficult to maintain integrity.
正如我们所看到的，一些操作必须顺序进行。但在实践中，实际代码通常很难正确协调。UI 始终处于活动状态，并且各种更新可以（几乎）同时进行，或者在任何时候进行。在这种情况下，保持完整性可能会变得困难。

Let us discuss a simple example: imagine a `Model` that maintains the state of
a record. The user can perform various actions:
让我们讨论一个简单的示例：想象一个 `Model`，它维护着记录的状态。用户可以执行各种操作：

- update a field, which triggers a call to the server to apply computed fields (`onchange`),
- 更新一个字段，这将触发对服务器的调用以应用计算字段（`onchange`）。
- save the record, which is a call to the server `save` method,
- 保存记录，这是一个对服务器 `save` 方法的调用。
- go to the next record
- 转到下一条记录。

So, what happens if the user update a field, then clicks on save while the onchange is ongoing?
We obviously want to save the record with the updated value, so the code that
perform the save operation need to wait for the return of the onchange.
那么，如果用户更新了一个字段，然后在 `onchange` 进行时点击保存会发生什么？我们显然希望使用更新后的值保存记录，因此执行保存操作的代码需要等待 `onchange` 返回。

Another similar scenario: the user save the record, then go to the next record.
In that case, we also need to wait for the save operation to be completed before
loading the next record.
另一个类似的场景：用户保存记录，然后转到下一条记录。在这种情况下，我们还需要等待保存操作完成，然后再加载下一条记录。

If you think about it, it becomes quickly difficult to coordinate all these
operations. Even more so when you add additional transformations (such as updating
relational fields, loading other data, grouping data, drilling down in some groups,
folding columns in kanban view, ...)
如果您考虑一下，协调所有这些操作会很快变得困难。当您添加更多转换（例如，更新关系字段、加载其他数据、对数据进行分组、在某些组中向下钻取、在看板视图中折叠列等）时，情况会更加复杂。

Many of these interactions can be coordinated with the help of a `Mutex`: it is
basically a queue which wait for the previous _job_ to be complete before executing
the new one. So, here is how the above example could be modelled (in pseudo-code):
许多这种交互可以通过 `Mutex` 来协调：它基本上是一个队列，它等待上一个 _job_ 完成，然后再执行新的 _job_。因此，以下是上面示例的建模方式（伪代码）：

```js
import { Mutex } from "@web/core/utils/concurrency";

class Model {
    constructor() {
        this.mutex = new Mutex();
    }
    update(newValue) {
        this.mutex.exec(async () => {
            this.state = await this.applyOnchange(newValue);
        });
    }
    save() {
        this.mutex.exec(() => {
            this._save();
        });
    }
    _save() {
        // actual save code
    }
    openRecord(id) {
        this.mutex.exec(() => {
            this.state = await this.loadRecord(id)
        });
    }
}
```

## KeepLast

As seen above, many user interactions need to be properly coordinated. Let us
imagine the following scenario: the user selects a menu in the Odoo web client.
Just after, the user changes her/his mind and select another menu. What should
happen?
如上所述，许多用户交互需要适当地协调。让我们想象以下场景：用户在 Odoo Web 客户端中选择一个菜单。就在之后，用户改变了主意并选择了另一个菜单。应该发生什么？

If we don't do anything, there is a risk that the web client displays either of
the action, then switch immediately to the other, depending on the order in which
the requests ends.
如果我们什么也不做，Web 客户端可能会显示任一操作，然后立即切换到另一个操作，这取决于请求结束的顺序。

We can solve this by using a mutex:
我们可以使用互斥锁来解决这个问题：

```js
// in web client

selectMenu(id) {
    this.mutex.exec(() => this.loadMenu(id));
}
```

This will make it determinist: each action from the user will be executed, then
the next action will take place. However, this is not optimal: we
probably want to stop (as much as possible) the first action, and start immediately
the new action, so the web client will only display the second action, and will
do it as fast as possible.
这将使其确定性：用户的每个操作都将被执行，然后下一个操作继续。但是，这不是最佳的：我们可能希望尽可能地停止第一个操作，并立即开始新的操作，这样 Web 客户端只会显示第二个操作，并且会尽可能快地执行。

This can be done by using the `KeepLast` primitive from Odoo: it is basically
like a Mutex, except that it cancels the current action, if any (not really
cancelling, but keeping the promise pending, without resolving it). So, the
code above could be written like this:
这可以通过使用 Odoo 中的 `KeepLast` 原语来实现：它基本上就像一个互斥锁，不同之处在于，它会取消当前操作（如果有）（实际上不会取消，而是保持承诺挂起，不会解决它）。因此，上面的代码可以这样写：

```js
import { KeepLast } from "@web/core/utils/concurrency";
// in web client

class WebClient {
  constructor() {
    this.keepLast = new KeepLast();
  }

  selectMenu(id) {
    this.keepLast.add(() => this.loadMenu(id));
  }
}
```
