![Odoo Logo](https://odoocdn.com/openerp_website/static/src/img/assets/png/odoo_logo_small.png)

# Introduction to JS framework
# JavaScript 框架简介

## Introduction
## 简介

For this training, we will put ourselves in the shoes of the IT staff for the fictional Awesome T-Shirt company, which is in the business of printing customised tshirts for online customers.
在本训练中，我们将扮演虚拟公司 Awesome T-Shirt 的 IT 人员，该公司专门为线上客户打印定制 T 恤。
The Awesome T-Shirt company uses Odoo for managing its orders, and built a dedicated odoo module to manage their workflow. The project is currently a simple kanban view, with a few columns.
Awesome T-Shirt 公司使用 Odoo 来管理订单，并构建了一个专用的 Odoo 模块来管理其工作流程。该项目目前是一个简单的看板视图，包含几个列。

The usual process is the following: a customer looking for a nice t-shirt can simply order it on the Awesome T-Shirt website, and give the url for any image that he wants. He also has to fill some basic informations, such as the desired size, and amount of t-shirts. Once he confirms his order, and once the payment is validated, the system will create a task in our project application.
通常流程如下：一个想要购买漂亮 T 恤的客户可以在 Awesome T-Shirt 网站上直接下单，并提供任何他想要的图片的网址。他还需要填写一些基本信息，例如想要的尺寸和 T 恤数量。一旦他确认订单，并且支付验证完成，系统将在他项目应用程序中创建一个任务。

The Awesome T-shirt big boss, Bafien Ckinpaers, is not happy with our implementation. He believe that by micromanaging more, he will be able to extract more revenue from his employees.
Awesome T-Shirt 的大老板 Bafien Ckinpaers 对我们的实现并不满意。他认为通过更细致的管理，他将能够从员工身上获取更多收入。
As the IT staff for Awesome T-shirt, we are tasked with improving the system. Various independant tasks need to be done.
作为 Awesome T-Shirt 的 IT 人员，我们的任务是改进系统。需要完成各种独立的任务。

Let us now practice our odoo skills!
现在让我们练习一下我们的 Odoo 技能吧！

## Setup
## 设置

Clone this repository, add it to your addons path, make sure you have
a recent version of odoo (master), prepare a new database, install the `awesome_tshirt`
addon, and ... let's get started!
克隆此仓库，将其添加到您的插件路径中，确保您拥有 Odoo 的最新版本（master 分支），准备一个新的数据库，安装 `awesome_tshirt` 插件，然后... 开始吧！

## Notes
## 笔记

Here are some short notes on various topics, in no particular order:
以下是一些关于各种主题的简短笔记，没有特定顺序：

- [The Odoo Javascript Ecosystem](notes_odoo_js_ecosystem.md)
- [Architecture](notes_architecture.md)
- [Views](notes_views.md)
- [Fields](notes_fields.md)
- [Concurrency](notes_concurrency.md)
- [Network requests](notes_network_requests.md)
- [Testing Odoo Code](notes_testing.md)

## Exercises
## 练习

- Part 1: [🦉 Owl framework 🦉](exercises_1_owl.md)
- Part 2: [Odoo web framework](exercises_2_web_framework.md)
- Part 3: [Fields and Views](exercises_3_fields_views.md)
- Part 4: [Miscellaneous](exercises_4_misc.md)
- Part 5: [Custom kanban view](exercises_5_custom_kanban_view.md)
- Part 6: [Creating a view from scratch](exercises_6_creating_views.md)
- Part 7: [Testing](exercises_7_testing.md)
