# Part 1: Owl Framework 🦉
# 第 1 部分：Owl 框架 🦉

Components are the basic UI building blocks in Odoo. Odoo components are made
with the Owl framework, which is a component system custom made for Odoo.
组件是 Odoo 中基本的 UI 构建块。Odoo 组件是用 Owl 框架制作的，Owl 是一个专为 Odoo 定制的组件系统。

Let us take some time to get used to Owl itself. The exercises in this section
may be artificial, but their purpose is to understand and practice the basic
notions of Owl quickly
让我们花一些时间来熟悉 Owl 本身。本节中的练习可能是非真实业务的，但它们的目的是快速理解和练习 Owl 的基本概念。

![1.0](images/1.0.png)

## 1.1 A `Counter` Component
## 1.1 `Counter` 组件

Let us see first how to create a sub component.
让我们首先看看如何创建一个子组件。

- Extract the `counter` code from the `AwesomeDashboard` component into a new
  component `Counter`.
- 将 `AwesomeDashboard` 组件中的 `counter` 代码提取到一个新的组件 `Counter` 中。  
- You can do it in the same file first, but once it's done, update your code to
  move the `Counter` in its own file. (don't forget the `/** @odoo-module **/`)
- 你可以先在同一个文件中进行操作，但完成之后，更新你的代码，将 `Counter` 移动到它自己的文件中。（不要忘记 `/** @odoo-module **/`）  
- also, make sure the template is in its own file, with the same name.
- 另外，确保模板在它自己的文件中，并使用相同的名称。

<details>
  <summary><b>Preview</b></summary>

![1.1](images/1.1.png)

</details>

<details>
  <summary><b>Resources</b></summary>

- [owl: github repository](https://github.com/odoo/owl)
- [owl: documentation](https://github.com/odoo/owl#documentation)
- [owl: using sub components](https://github.com/odoo/owl/blob/master/doc/reference/component.md#sub-components)
- [odoo: documentation on assets](https://www.odoo.com/documentation/master/developer/reference/frontend/assets.html)

</details>

## 1.2 A `Todo` Component
## 1.2 `Todo` 组件

We will modify the `AwesomeDashboard` component to keep track of a list of todos.
This will be done incrementally in multiple exercises, that will introduce
various concepts.
我们将修改 `AwesomeDashboard` 组件以跟踪待办事项列表。这将通过多个练习逐步完成，这些练习将介绍各种概念。

First, let's create a `Todo` component that display a task, which is described by an id (number), a description (string), and a status `done` (boolean). For
example:
首先，让我们创建一个 `Todo` 组件来显示一个任务，该任务由一个 id（数字）、一个描述（字符串）和一个状态 `done`（布尔值）来描述。例如：

```js
{ id: 3, description: "buy milk", done: false }
```

- create a `Todo` component that receive a `todo` in props, and display it: it
  should show something like `3. buy milk`
- 创建一个 `Todo` 组件，它在 props 中接收一个 `todo`，并显示它：它应该显示类似于 `3. buy milk` 的内容。  
- also, add the bootstrap classes `text-muted` and `text-decoration-line-through` on the task if it is done
- 此外，如果任务已完成，则在任务上添加 bootstrap 类 `text-muted` 和 `text-decoration-line-through`。
- modify `AwesomeDashboard` to display a `Todo` component, with some hardcoded
  props to test it first. For example:
- 修改 `AwesomeDashboard` 以显示一个 `Todo` 组件，并使用一些硬编码 props 进行初步测试。例如：  
  ```js
    setup() {
        ...
        this.todo = { id: 3, description: "buy milk", done: false };
    }
  ```

<details>
  <summary><b>Preview</b></summary>

![1.2](images/1.2.png)

</details>

<details>
  <summary><b>Resources</b></summary>

- [owl: props](https://github.com/odoo/owl/blob/master/doc/reference/props.md)
- [owl: Dynamic attributes](https://github.com/odoo/owl/blob/master/doc/reference/templates.md#dynamic-attributes)
- [owl: Dynamic class attributes](https://github.com/odoo/owl/blob/master/doc/reference/templates.md#dynamic-class-attribute)

</details>

## 1.3 Props Validation
## 1.3 Props 验证

The `Todo` component has an implicit API: it expects to receive in its props the
description of a todo in a specified format: `id`, `description` and `done`.
Let us make that API more explicit: we can add a props definition that will let
Owl perform a validation step in dev mode. It is a good practice to do that for
every component.
`Todo` 组件有一个隐式 API：它期望在 props 中接收以指定格式描述的待办事项：`id`、`description` 和 `done`。让我们使该 API 更加明确：我们可以添加一个 props 定义，它将让 Owl 在开发模式下执行验证步骤。这是一个良好的实践，应该对每个组件都这样做。

- Add props validation to Todo
- 在 `Todo` 中添加 props 验证。
- make sure it fails in dev mode
- 确保它在开发模式下失败。

<details>
  <summary><b>Resources</b></summary>

- [owl: props validation](https://github.com/odoo/owl/blob/master/doc/reference/props.md#props-validation)
- [odoo: debug mode](https://www.odoo.com/documentation/master/developer/reference/frontend/framework_overview.html#debug-mode)
- [odoo: activate debug mode](https://www.odoo.com/documentation/master/applications/general/developer_mode.html#developer-mode)

</details>

## 1.4 A List of todos
## 1.4 待办事项列表

Now, let us display a list of todos instead of just one todo. For now, we can
still hardcode the list.
现在，让我们显示待办事项列表，而不仅仅是一个待办事项。目前，我们仍然可以对列表进行硬编码。

- Change the code to display a list of todos, instead of just one, and use
  `t-foreach` in the template
- 更改代码以显示待办事项列表，而不是只有一个待办事项，并在模板中使用 `t-foreach`。  
- think about how it should be keyed
- 考虑一下它应该如何进行键控。

<details>
  <summary><b>Preview</b></summary>

![1.4](images/1.4.png)

</details>

<details>
  <summary><b>Resources</b></summary>

- [owl: t-foreach](https://github.com/odoo/owl/blob/master/doc/reference/templates.md#loops)

</details>

## 1.5 Adding a todo
## 1.5 添加待办事项

So far, the todos in our list are hardcoded. Let us make it more useful by allowing
the user to add a todo to the list.
到目前为止，我们列表中的待办事项都是硬编码的。让我们通过允许用户向列表中添加待办事项来使其更有用。

- add input above the task list with placeholder `Enter a new task`
- 在任务列表上方添加一个输入框，占位符为 `Enter a new task`。
- add an event handler on the `keyup` event named `addTodo`
- 在输入框上添加一个名为 `addTodo` 的 `keyup` 事件处理程序。
- implement `addTodo` to check if enter was pressed (`ev.keyCode === 13`), and
  in that case, create a new todo with the current content of the input as description
- 实现 `addTodo` 以检查是否按下了 Enter 键 (`ev.keyCode === 13`)，如果按下了 Enter 键，则使用输入框的当前内容作为描述创建一个新的待办事项。  
- make sure it has a unique id (can be just a counter)
- 确保它拥有唯一的 id（可以只是一个计数器）。
- then clear the input of all content
- 然后清除输入框中的所有内容。
- bonus point: don't do anything if input is empty
- 加分项：如果输入框为空，则不执行任何操作。

Notice that nothing updates in the UI: this is because Owl does not know that it
should update the UI. This can be fixed by wrapping the todo list in a `useState`:
请注意，UI 中没有任何更新：这是因为 Owl 不知道它应该更新 UI。可以通过将待办事项列表包裹在一个 `useState` 中来解决这个问题：

```js
this.todos = useState([]);
```

<details>
  <summary><b>Preview</b></summary>

![1.5](images/1.5.png)

</details>

<details>
  <summary><b>Resources</b></summary>

- [owl: reactivity](https://github.com/odoo/owl/blob/master/doc/reference/reactivity.md)

</details>

## 1.6 Focusing the input
## 1.6 聚焦输入框

Let's see how we can access the DOM with `t-ref`. For this exercise, we want to
focus the `input` from the previous exercise whenever the dashboard is mounted.
让我们看看如何使用 `t-ref` 访问 DOM。在这个练习中，我们希望在仪表盘挂载时聚焦前面的练习中的 `input`。

Bonus point: extract the code into a specialized hook `useAutofocus`
加分项：将代码提取到一个专门的钩子 `useAutofocus` 中。

<details>
  <summary><b>Resources</b></summary>

- [owl: component lifecycle](https://github.com/odoo/owl/blob/master/doc/reference/component.md#lifecycle)
- [owl: hooks](https://github.com/odoo/owl/blob/master/doc/reference/hooks.md)
- [owl: useRef](https://github.com/odoo/owl/blob/master/doc/reference/hooks.md#useref)

</details>

## 1.7 Toggling todos
## 1.7 切换待办事项

Now, let's add a new feature: mark a todo as completed. This is actually
trickier than one might think: the owner of the state is not the same as the
component that displays it. So, the `Todo` component need to communicate to its
parent that the todo state needs to be toggled. One classic way to do this is
by using a callback prop `toggleState`
现在，让我们添加一个新功能：将待办事项标记为已完成。这实际上比想象的要复杂：状态的所有者与显示它的组件并不相同。因此，`Todo` 组件需要向其父组件传达待办事项状态需要被切换的信息。一种经典的方法是使用回调 prop `toggleState`。

- add an input of type="checkbox" before the id of the task, which is checked if
  the `done` state is true,
- 在任务的 id 之前添加一个类型为 "checkbox" 的输入框，如果 `done` 状态为 true，则选中该输入框。  
- add a callback props `toggleState`
- 添加一个回调 props `toggleState`。
- add a `click` event handler on the input in `Todo`, and make sure it calls
  the `toggleState` function with the todo id.
- 在 `Todo` 中的输入框上添加一个 `click` 事件处理程序，并确保它使用待办事项 id 调用 `toggleState` 函数。  
- make it work!
- 使其正常工作！

<details>
  <summary><b>Preview</b></summary>

![1.7](images/1.7.png)

</details>

<details>
  <summary><b>Resources</b></summary>

- [owl: binding function props](https://github.com/odoo/owl/blob/master/doc/reference/props.md#binding-function-props)

</details>

## 1.8 Deleting todos
## 1.8 删除待办事项

The final touch is to let the user delete a todo.
最后的修饰是让用户能够删除待办事项。

- add a new callback prop `removeTodo`
- 添加一个新的回调 prop `removeTodo`。
- add a `<span class="fa fa-remove">` in the Todo component
- 在 `Todo` 组件中添加一个 `<span class="fa fa-remove">`。
- whenever the user clicks on it, it should call the `removeTodo` method
- 每当用户点击它时，它应该调用 `removeTodo` 方法。
- make it work as expected
- 使其按预期工作。

<details>
  <summary><b>Preview</b></summary>

![1.8](images/1.8.png)

</details>

## 1.9 Generic components with Slots
## 1.9 使用 Slots 的通用组件

Owl has a powerful slot system to allow you to write generic components. This is
useful to factorize common layout between different parts of the interface.
Owl 拥有强大的 Slot 系统，允许你编写通用组件。这对于在界面不同部分之间进行常见布局的分解非常有用。

- write a `Card` component, using the following bootstrap html structure:
- 使用以下 bootstrap HTML 结构编写一个 `Card` 组件：

  ```html
  <div class="card" style="width: 18rem;">
    <img src="..." class="card-img-top" alt="..." />
    <div class="card-body">
      <h5 class="card-title">Card title</h5>
      <p class="card-text">
        Some quick example text to build on the card title and make up the bulk
        of the card's content.
      </p>
      <a href="#" class="btn btn-primary">Go somewhere</a>
    </div>
  </div>
  ```

- this component should have two slots: one slot for the title, and one for
  the content (the default slot). For example, here is how one could use it:
- 该组件应该有两个 Slot：一个用于标题，一个用于内容（默认 Slot）。例如，以下是如何使用它：

  ```xml
  <Card>
    <t t-set-slot="title">Card title</t>
    <p class="card-text">Some quick example text...</p>
    <a href="#" class="btn btn-primary">Go somewhere</a>
  </Card>

  ```

- bonus point: if the `title` slot is not given, the `h5` should not be
  rendered at all
- 加分项：如果未提供 `title` Slot，则 `h5` 应该根本不渲染。

<details>
  <summary><b>Preview</b></summary>

![1.9](images/1.9.png)

</details>

<details>
  <summary><b>Resources</b></summary>

- [owl: slots](https://github.com/odoo/owl/blob/master/doc/reference/slots.md)
- [owl: slot props](https://github.com/odoo/owl/blob/master/doc/reference/slots.md#slots-and-props)
- [bootstrap: documentation on cards](https://getbootstrap.com/docs/5.2/components/card/)

</details>

## 1.10 Miscellaneous small tasks
## 1.10 杂项小任务

- add prop validation on the Card component
- 在 `Card` 组件上添加 props 验证。
- try to express in the prop validation system that it requires a `default`
  slot, and an optional `title` slot
- 尝试在 props 验证系统中表达它需要一个 `default` Slot 和一个可选的 `title` Slot。

<details>
  <summary><b>Resources</b></summary>

- [owl: props validation](https://github.com/odoo/owl/blob/master/doc/reference/props.md#props-validation)

</details>
