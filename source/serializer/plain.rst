===============
纯文本 (Plain)
===============

纯文本文本序列化器将聊天组件与其纯文本表示形式相互转换. 因此它是 Adventure 中最简单的文本序列化器.
该序列化器对于支持旧版客户端, 记录日志, 以及从外部来源的组件中清除样式十分有用,
且提供了一个小型的, 独立的 (self-contained) 文本序列化器示例.

纯文本文本序列化器, 就其性质而言, 不支持任何进阶特性, 包括颜色, 悬浮和点击事件, 网址链接, 或者插入.
如果需要进阶特性, 请考虑使用 :doc:`/minimessage`.

用法
-----

纯文本序列化器器可使用 ``PlainTextComponentSerializer`` 以访问.
你可以使用 ``PlainTextComponentSerializer.plainText()`` 以获取一个默认的实例以静默忽略按键绑定 (keybind) 和可翻译 (translatable) 组件,
或者, 你也可以构造你自己的, 能将组件映射为一些纯文本表示的 ``PlainTextComponentSerializer``

纯文本的反序列化等同于 ``Component.text(string)``. 不会对输入进行预处理.
实现序列化是为了提供 API 一致性.
