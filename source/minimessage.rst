===========
MiniMessage
===========

MiniMessage 是一种基于字符串的格式,
以一种易于编辑且人类可读 (human-readable) 的形式表示 Minecraft 聊天组件.

用法
^^^^^^^^^^^^^^^^^^^

添加仓库

.. tabs::

   .. group-tab:: Maven

      .. code:: xml

         <repositories>
             <!-- ... -->
             <repository> <!-- 对于快照构建 -->
               <id>sonatype-oss-snapshots</id>
               <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
             </repository>
             <!-- ... -->
         </repositories>

   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         repositories {
            // 对于快照构建
            maven {
                name = "sonatype-oss-snapshots"
                url = "https://oss.sonatype.org/content/repositories/snapshots/"
            }
            // 对于发行版
            mavenCentral()
         }

   .. group-tab:: Gradle (Kotlin)

      .. code:: kotlin

         repositories {
            // 对于快照构建
            maven(url = "https://oss.sonatype.org/content/repositories/snapshots/") {
                name = "sonatype-oss-snapshots"
            }
            // 对于发行版
            mavenCentral()
         }

   声明依赖:

.. tabs::

   .. group-tab:: Maven

      .. code:: xml

         <dependency>
            <groupId>net.kyori</groupId>
            <artifactId>adventure-text-minimessage</artifactId>
            <version>4.1.0-SNAPSHOT</version>
         </dependency>

   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         dependencies {
            implementation "net.kyori:adventure-text-minimessage:4.1.0-SNAPSHOT"
         }


   .. group-tab:: Gradle (Kotlin)

      .. code:: kotlin

         dependencies {
            implementation("net.kyori:adventure-text-minimessage:4.1.0-SNAPSHOT")
         }

API
^^^

MiniMessage 通过 ``MiniMessage`` 类暴露了一个简单的 API.

对于该接口存在两种不同的实例, ``get()`` 和 ``markdown()``, 后者提供基本的 Markdown 支持, 并额外提供 MiniMessage 支持. 更多信息可查看 Markdown_ 一节.

可以通过 Builder_ 为 MiniMessage 添加额外的自定义.

MiniMessage 允许既允许你序列化组件为 MiniMessage 字符串, 也允许你粘贴/反序列化 MiniMessage 字符串为组件.

格式
^^^^^^^^^^^^^^^^^^^

该依赖库使用标签 (tags). 你做的一切都被使用标签定义. 标签包含一个起始标签和一个结束标签 (``<reset>`` 标签在这里是一个例外).
开始标签是强制性的 (显然如此), 而结束标签并不是.
``<yellow>Hello <blue>World<yellow>!`` 和 ``<yellow>Hello <blue>World</blue>!`` 甚至 ``<yellow>Hello </yellow><blue>World</blue><yellow>!</yellow>`` 所做的都是相同的.

一些标签拥有内部标签 (inner tags). 他们看起来像这样: ``<tag:inner>stuff</tag>``. 例如: ``<hover:show_text:"<red>test:TEST">TEST`` 或者 ``<click:run_command:test>TEST``
正如你所看到的, 有时候这些标签包含组件，有时则只是包含字符串. 具体请参阅下方的详细文档.

单引号 (``'``) 和双引号 (``"``) 是通用的, 但是为了保护智商, 请保持一致, 为你的消息选择其中一个使用. MiniMessage *应该* 很好的处理这些不匹配的引号.

组件尝试尽可能的与原版表示相接近.
使用 `minecraft wiki <https://minecraft.gamepedia.com/Raw_JSON_text_format>`_ 作为参考可能会很有用, 特别是在点击和悬浮事件的行为和值上.

组件
----------------

颜色
******

为后文上色

标签
   ``<_colorname_>``
参数
   * ``_colorname_``, 所有的 minecraft 颜色常量 (参见 `这里 <https://github.com/KyoriPowered/adventure/blob/master/api/src/main/java/net/kyori/adventure/text/format/NamedTextColor.java>`_), 或是 hex 颜色
示例
   * ``<yellow>Hello <blue>World</blue>!``
   * ``<red>This is a <green>test!``
   * ``<#00ff00>R G B!``

.. image:: https://i.imgur.com/wB32YpZ.png
.. image:: https://i.imgur.com/vsN3OHa.png

颜色 (详细)
******************

使用更详细的方式定义颜色

标签
   ``<color:_colorNameOrHex_>``
别名
   ``colour``, ``c``
参数
   * ``_colorNameOrHex_``, 可以是上面的所有值 (包括已命名的颜色和 hex 颜色)
Examples
   * ``<color:yellow>Hello <color:blue>World</color:blue>!``
   * ``<color:#FF5555>This is a <color:#55FF55>test!``

.. image:: https://i.imgur.com/wB32YpZ.png
.. image:: https://i.imgur.com/vsN3OHa.png

装饰 (Decoration)
******************

装饰后文

标签
   ``<_decorationname_>``
参数:
   * | ``_decorationname_`` , 所有 minecraft 装饰 (`参见这里 <https://github.com/KyoriPowered/adventure/blob/master/api/src/main/java/net/kyori/adventure/text/format/TextDecoration.java>`_)
     | 别名有 ``strikethrough`` -> ``st``, ``obfuscated`` -> ``obf``, ``italic`` -> ``em`` 或是 ``i`` 以及 ``bold`` -> ``b`` 可以使用
示例:
   * ``<underlined>This is <bold>important</bold>!``

.. image:: https://i.imgur.com/hREGXQy.png

重置
************

重置所有颜色, 装饰, 悬浮等. 无结束标签

标签
   ``<reset>``
别名
   ``r``
参数
   无
示例
   * ``<yellow><bold>Hello <reset>world!``

.. image:: https://i.imgur.com/bjInUhj.png

点击
************

允许当点击组件时做很多事情.

标签
   ``<click:_action_:_value_>``
参数
   * ``_action_``, 点击事件类型, `该列表 <https://github.com/KyoriPowered/adventure/blob/master/api/src/main/java/net/kyori/adventure/text/event/ClickEvent.java>`_ 之一
   * ``_value_``, 特定事件的参数, 参考 `minecraft wiki <https://minecraft.gamepedia.com/Raw_JSON_text_format>`_
示例
   * ``<click:run_command:/say hello>Click</click> to say hello``
   * ``Click <click:copy_to_clipboard:Haha you suck> this </click>to copy your score!``

.. image:: https://i.imgur.com/J82qOHn.png

悬浮
************

允许当悬浮在组件上时做很多事情.

标签
   ``<hover:_action_:_value_>``
参数
   * ``_action_``, 悬浮事件类型, `该列表 <https://github.com/KyoriPowered/adventure/blob/master/api/src/main/java/net/kyori/adventure/text/event/HoverEvent.java>`_ 之一
   * ``_value_``, 特定事件的参数, 参考 `minecraft wiki <https://minecraft.gamepedia.com/Raw_JSON_text_format>`_
示例
   * ``<hover:show_text:'<red>test'>TEST``

.. image:: https://i.imgur.com/VsHDPTI.png

按键绑定 (Keybind)
************

允许为行为显示配置过的按键

标签
   ``<key:_key_>``
参数
   * ``_key_``, 该行为的按键绑定标识符
示例
   * ``Press <red><key:key.jump> to jump!``

.. image:: https://i.imgur.com/iQmNDF6.png

可翻译键 (Translatable)
************

允许使用玩家的语言环境显示 miencraft 消息

标签
   ``<lang:_key_:_value1_:_value2_>``
参数
   * ``_key_``, 翻译键
   * ``_valueX_``, 被用作该键中占位符的可选参数 (他们最终将会出现在 json 中的 ``with`` 标签内)
示例
   * ``You should get a <lang:block.minecraft.diamond_block>!``
   * ``<lang:commands.drop.success.single:'<red>1':'<blue>Stone'>!``

.. image:: https://i.imgur.com/mpdDMF6.png
.. image:: https://i.imgur.com/esWpnxm.png

插入 (Insertion)
************

允许通过 shift 单机将文本插入聊天

标签
   ``<insertion:_text_>``
参数
   * ``_text_``, 要插入的文本
示例
   * ``Click <insert:test>this</insert> to insert!``

.. image:: https://i.imgur.com/Imhom84.png

Pre
************

在此标签中的标签不会被解析, 对于例如玩家输入很有用

标签
   ``<pre>``
参数
   无
示例
   * ``<gray><<yellow><player><gray>> <reset><pre><message></pre>``

.. image:: https://i.imgur.com/pQqaJnD.png

彩虹色
************

彩虹色的文本?!

标签
   ``<rainbow:[!][phase]>``
参数
   * 相位, 可选的
   * ``!``, 用于反转彩虹色的字面量, 可选的
示例
   * ``<yellow>Woo: <rainbow>||||||||||||||||||||||||</rainbow>!``
   * ``<yellow>Woo: <rainbow:!>||||||||||||||||||||||||</rainbow>!``
   * ``<yellow>Woo: <rainbow:2>||||||||||||||||||||||||</rainbow>!``
   * ``<yellow>Woo: <rainbow:!2>||||||||||||||||||||||||</rainbow>!``

.. image:: https://i.imgur.com/Ertlk2G.png

渐变色 (Gradient)
************

渐变色的文本

标签
   ``<gradient:[color1]:[color...]:[phase]>``
参数
   含有 1 到 n 个颜色的列表, hex 或 已命名的顔色以及一个可选择相位参数 (范围从 -1 到 1) 允许你移动渐变, 创建动画。
示例
   * ``<yellow>Woo: <gradient>||||||||||||||||||||||||</gradient>!``
   * ``<yellow>Woo: <gradient:#5e4fa2:#f79459>||||||||||||||||||||||||</gradient>!``
   * ``<yellow>Woo: <gradient:#5e4fa2:#f79459:red>||||||||||||||||||||||||</gradient>!``
   * ``<yellow>Woo: <gradient:green:blue>||||||||||||||||||||||||</gradient>!``

.. image:: https://i.imgur.com/8qYHCWk.png

字体
***********

允许改变文本的字体

Tag
   ``<font:key>``
参数
   字体的命名空间键 (namespaced key), 默认为 ``minecraft``
示例
   * ``Nothing <font:uniform>Uniform <font:alt>Alt  </font> Uniform``
   * ``<font:myfont:custom_font>Uses a custom font from a resource pack</font>``

.. image:: https://i.imgur.com/0SjeMQm.png

Markdown
^^^^^^^^^^^^^^^^^^^

MiniMessage 还包含一个非常简单的 markdown 拓展. 你可以通过调用 ``MiniMessage.markdown()`` 开启它或者通过使用 Builder_.

注意: 当你调用 ``escapeTokens``时, Markdown 不会被转义, 然而 ``stripTokens`` 可以正常工作.

在默认情况下, markdown 解析器支持以下标记:

* 加粗:
   ``**bold**`` 将被转换为 ``<bold>bold</bold>``

   ``__bold__`` 也将被转换为 ``<bold>bold</bold>``
* 斜体:
   ``*italic*`` 将被转换为 ``<italic>italic</italic>``

   ``_italic_`` 也将被转换为 ``<italic>italic</italic>``
* 下划线:
   ``~~underline~~`` 将被转换为 ``<underlined>underline</underlined>``
* 随机字符:
   ``||obfuscated||`` 将被转换为 ``<obfuscated>obfuscated</obfuscated>``

然而, 这些标记有一次奇怪, 但现在改变它们已经太晚了, 这也就是为什么有:

Markdown 风格
----------------

你上面看到的是默认/遗留版本的风格. 它很有可能最终被移除.

要想使用不同的 markdown 风格, 你可以使用 ``MiniMessage.withMarkdownFlavor(DiscordFlavor.get())`` 或者 Builder_.

discord 风格工作起来像是这样: ``**bold**, *italic*, __underline__, ~~strikethrough~~, ||obfusctated||``

github 工作起来像是这样: ``**bold**, *italic*, ~~strikethrough~~``

额外的, 你可以实现你自己的 markdown 风格. 你可以查看内置的这些风格作为参考!

占位符
^^^^^^^^^^^^^^^^^^^

MiniMessage 为占位符提供两个系统. 这依赖于你怎么算的. 也可以说是 4 个.

最简单的一个系统是简单的字符串替换:
``MiniMessage.get().parse("<gray>Hello <name>", "name", "MiniDigger")``

正如你所看到的, 占位符在消息中被定义为类似于普通标签的样子, 并且被一个键值对列表所解析 (你也可以传入 ``Map<String, String>`` 到这里).

这些消息中的占位符会在任何其他标签解析前被解析. 这意味着替换的内容也可以包含 MiniMessage 标签:
 .. code:: java

    String name = "MiniDigger";
    String rank = "<red>[ADMIN]</red>"
    Map<String, String> placeholders = new HashMap<>();
    placeholders.put("name", rank + name);
    MiniMessage.get().parse("<gray>Hello <name>", "name", placeholders)

模板
----------

第二个系统, 模板系统, 允许你选择是使用字符串作为替换的内容使用还是完整的组件.
由于它们是在主解析循环中被执行的, 因此以字符串作为替换内容时字符串不能包含任何 MiniMessage 标签!

.. code:: java

    MiniMessage.get().parse("<gray>Hello <name>", Template.of("name", Component.text("TEST").color(NamedTextColor.RED)));
    MiniMessage.get().parse("<gray>Hello <name>", Template.of("name", "TEST"));
    List<Template> templates = List.of(Template.of("name", "TEST"), Template.of("name2", "TEST"));
    MiniMessage.get().parse("<gray>Hello <name> and <name2>", Template.of("name", "TEST"));

这些非常强大的功能允许你很容易的从其他地方 (例如一个 itemstack 或是一个占位符 API) 获得组件, 然后将他们包含在你的消息中.

占位符解析器
--------------------

为了使处理 (来自外部的或来自内部的) 占位符 API 更加容易, MiniMessage 允许你提供一个占位符解析器.

一个占位符解析器只是一个 ``Function<String, ComponentLike>``, 允许你无需事先定义标签而处理它们.
当你解析占位符时, 只需要返回一个 Component, 否则返回一个 null.

一个使用 builder api 定义这样一个解析器 (更多信息请见下方的 Builder_ 一节):

.. code :: java

    Function<String, ComponentLike> resolver = (name) -> {
        if (name.equalsIgnoreCase("test")) {
            return Component.text("TEST").color(NamedTextColor.RED);
        }
        return null;
    };

    Component result = MiniMessage.builder().placeholderResolver(resolver).build().parse("<green><bold><test>");

自定义
^^^^^^^^^^^^^

MiniMessage 被设计为可拓展, 可配置和可调整的, 以满足你的需求.

转换
---------------

At the core, its build around the concept of transformations. A transformation is a object, that transforms a component, by changing its style or adding events, some even delete the original component and replace it with new ones.
Explaining all possibilities would be out of scope for this documentation, if you are interested in implementing your own transformations, look at the inbuild ones as a guide.

When the parser encounters a start tag, it will look it up in the transformation registry, and if it finds something, the transformation will be loaded (as in, initialized with the tag name and its parameters) and then added to a list.
When the parser then encounters a string, it will apply all transformations onto that tag.
When the parser encounters a close tag, the transformation for that tag will be removed from the list again, so that further strings will not be transformed anymore.

Transformations are registered into the transformation registry using transformation types.
A transformation type defines a predicate, to check if the given tag can be parsed by the transformation, and a transformation parser, which handles initialization of transformations.

MiniMessage allows you to pass your own transformation registry, which allows you to both disable inbuild transformation types, only allowing a few transformation types or even passing your own transformation types.
MiniMessage also provides convenience methods to do that:
``MiniMessage.withTransformations(TransformationType.COLOR).parse("<green><bold>Hai") == Component.text("<bold>Hai", NamedTextColor.GREEN)``
Bold transformation isn't enabled -> bold tag is not parsed.

Builder
-------

为了使自定义以 MiniMessage 更加方便, 我们提供了一个 Builder. 用途不言自明:

.. code :: java

    MiniMessage minimessage = MiniMessage.builder()
        .removeDefaultTransformations()
        .transformation(TransformationType.COLOR)
        .transformation(TransformationType.DECORATION)
        .markdown()
        .markdownFlavor(DiscordFlavor.get())
        .placeholderResolver(this::resolvePlaceholder)
        .build();

小贴士: 只实例化一次 Minimessage 示例, 并将其放在一个中心位置, 使用它处理你所有的消息是一个好习惯.
如果你想根据用户的权限自定义 MiniMessage 的情况则是一个例外 (例如, 管理员应该被允许在消息中使用颜色和装饰, 普通用户则不能)
