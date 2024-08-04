# Notes: Testing Odoo
# 笔记：测试 Odoo

Testing UI code is an important, but often overlooked, part of a solid development
process. Odoo javascript test suite is quite large, but at the same time, not
particularly intuitive. So, let us take some time to discuss the main ideas.

测试 UI 代码是一个重要的环节，但它在稳固的开发流程中常常被忽视。Odoo JavaScript 测试套件相当庞大，但同时，它也不怎么直观。因此，让我们花一些时间讨论主要的概念。

Roughly speaking, there are two different kind of tests: integration tests (testing
a feature/business flow, by running all the relevant code) and unit tests (testing
some behaviour, usually by running only a component or a small unit of code).

粗略地说，有两种不同的测试类型：集成测试（通过运行所有相关代码来测试功能/业务流程）和单元测试（测试某些行为，通常仅运行组件或一小段代码）。

Both of these kind of tests are different:

这两种测试类型存在差异：

- integration tests are useful to make sure something works as expected. However,
  usually, they take more time to run, take more CPU/memory, and are harder to
  debug when they fail. On the flip side, they are necessary to prove that a system
  work and they are easier to write.
- 集成测试用于确保某些功能按预期工作。但是，通常情况下，它们需要更多时间运行，占用更多 CPU/内存，并且在失败时更难调试。另一方面，它们是证明系统正常工作的必要手段，并且更容易编写。

- unit tests are useful to ensure that a specific piece of code works. They are
  quick to run, are focused on a specific feature. When they fail, they identify
  a problem in a smaller scope, so it is easier to find the issue. However, they
  are usually harder to write, since one needs to find a way to _isolate_ as much
  as possible something.
- 单元测试用于确保特定代码片段正常工作。它们运行速度快，专注于特定功能。当它们失败时，它们会识别出较小范围内的问题，因此更容易找到问题。但是，它们通常更难编写，因为需要找到一种方法来尽可能地 _隔离_ 某些内容。

Odoo (javascript) test suite contains both kind of tests: integration tests are
made with _tours_ and unit tests with _QUnit_

Odoo（JavaScript）测试套件包含这两种类型的测试：集成测试使用 _tours_ 完成，单元测试使用 _QUnit_ 完成。

## Tours

A tour is basically a sequence of steps, with some selectors and parameters to
describe what the step should do (click on an element, fill an input, ...). Then
the code in the addon `web_tour` will execute each step sequentially, waiting
between each step if necessary.

Tour 本质上是一系列步骤，它包含一些选择器和参数，用于描述每一步应该做什么（点击某个元素，填写输入框等）。然后，`web_tour` 插件中的代码将顺序执行每个步骤，如果需要，在每一步之间等待。

## QUnit tests
## QUnit 测试

A QUnit test is basically a short piece of code that exercise a feature, and
make some assertions. The main test suite can be run by simply going to the
`/web/tests` route.

QUnit 测试本质上是一小段代码，用于测试某个功能，并进行一些断言。可以通过简单地访问 `/web/tests` 路由来运行主要测试套件。


![contact](notes/contact.png)