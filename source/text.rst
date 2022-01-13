======================
文本 (聊天组件)
======================

组件 (Component) 代表了 Minecraft 聊天组件

创建组件
^^^^^^^^^^^^^^^^^^^

.. code:: java

   // 创建一行显示 "You're a Bunny! Press <key> to jump!" 的文本, 包含一些颜色和样式.
   final TextComponent textComponent = Component.text("You're a ")
     .color(TextColor.color(0x443344))
     .append(Component.text("Bunny", NamedTextColor.LIGHT_PURPLE))
     .append(Component.text("! Press "))
     .append(
       Component.keybind("key.jump")
         .color(NamedTextColor.LIGHT_PURPLE)
         .decoration(TextDecoration.BOLD, true)
     )
     .append(Component.text(" to jump!"));
   // 现在你可以将 `textComponent` 发送给一些事物, 例如一个客户端

你也可以使用一个可变的 builder, 用来创建一个包含子组件 (children) 的不可变 (final) 组件.

.. code:: java

   // Creates a line of text saying "You're a Bunny! Press <key> to jump!", with some colouring and styling.
   final TextComponent textComponent2 = Component.text()
     .content("You're a ")
     .color(TextColor.color(0x443344))
     .append(Component.text().content("Bunny").color(NamedTextColor.LIGHT_PURPLE).build())
     .append(Component.text("! Press "))
     .append(
       Component.keybind().keybind("key.jump")
         .color(NamedTextColor.LIGHT_PURPLE)
         .decoration(TextDecoration.BOLD, true)
         .build()
     )
     .append(Component.text(" to jump!"))
     .build();
   // now you can send `textComponent2` to something, such as a client

格式化组件
^^^^^^^^^^^^^^^^^^^^

Styles 是一个 TextColor 和 TextDecoration 的超集, 且可被应用于文本组件中.
TextColor 可提供 RGB 光谱中的任何颜色.
你也可以使用 NamedTextColor 来从默认调色板中选取一些颜色.
以下 TextDecorations 可被使用:

* *斜体(Italic)*
* **加粗(Bold)**
* 删除线(Strikethrough)
* 下划线(Underlined)
* 混淆(Obfuscated)

事件
^^^^^^^

目前文本组件中有两种事件可被使用.
悬浮事件 (Hover events) 允许你在当用户将他们的鼠标悬浮在文本上时显示另一个组件, 物品 (item) 或实体 (entity).
当一个用户点击文本组件时, 一个可以执行以下行为之一的点击事件 (click event) 将被触发:

* 打开一个网址
* 打开一个文件
* 运行一段指令
* 建议一段指令
* 改变书的页码
* 复制字符串到剪贴板

序列化和反序列化组件
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

序列化组件到 JSON, 旧版风格 (legacy), 以及纯文本 (plain) 的形式也是支持的.

组件可被使用 :doc:`/serializer/index` 序列化.

在你的应用程序中使用组件
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

在你的应用程序中使用组件的方式毫无疑问将会非常依赖于你将要实现的目标.

然而，最常见的任务可能就是将一个组件发送到某种 Minecraft 客户端中,
执行此操作的方法将依赖于你的应用程序运行的平台,
然而, 这可能涉及到先将组件序列化为 Minecraft 的 JSON 格式,
然后再将 JSON 通过平台提供的另一个方法发送.

文本依赖库与平台无关，因此也不会提供任何方式将组件发送到客户端.
一些平台实现了 :ref:`Adventure natively <native-support>`,
因此 ``Components`` 可以通过他们的 API 直接被使用.
对于其他平台 (Spigot/Bukkit, BungeeCord, 以及 SpongeAPI 7),
我们提供一个可以随您的插件一起被分发的兼容桥作为 :ref:`platforms`.
