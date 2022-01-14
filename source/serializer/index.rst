================
文本序列化器
================

在 Adventure 的数据和其他格式之间进行转换的最底层 （lowest-level） 方式就是序列化器 (serializers).
一些序列化器转换为标准格式, 而另一些转换为 Adventure 自有的格式.

.. toctree::
    :maxdepth: 1
    :caption: 序列化器

    gson
    legacy
    plain

可以使用这些序列化器中的任意一个转换组件:

.. code:: java

   // 创建一个文本组件
   final TextComponent textComponent = Component.text("Hello ")
     .color(NamedTextColor.GOLD)
     .append(
       Component.text("world")
         .color(NamedTextColor.AQUA).
         decoration(TextDecoration.BOLD, true)
     )
     .append(Component.text("!").color(NamedTextColor.RED))
     .build();

   // 将 textComponent 转换为 Minecraft 用于序列化的 JSON 格式.
   final String json = GsonComponentSerializer.gson().serialize(textComponent);

   // 将 textComponent 转换为一个旧版风格的字符串 - "&6Hello &b&lworld&c!"
   final String legacy = LegacyComponentSerializer.legacyAmpersand().serialize(textComponent);

   // 将 textComponent 转换为一个纯文本字符串 - "Hello world!"
   final String plain = PlainTextComponentSerializer.plainText().serialize(textComponent);

当然也同样可以通过反序列化反转.

.. code:: java

   // 将 Minecraft 用于序列化的 JSON 格式转换为一个 Component
   final Component component = GsonComponentSerializer.gson().deserialize(json);

   // 将一个旧版风格字符串 (使用格式化代码) 转换为一个 TextComponent
   final Component component = LegacyComponentSerializer.legacyAmpersand().deserialize("&6Hello &b&lworld&c!");

   // 将一个纯文本转换为一个 TextComponent
   final Component component = PlainTextComponentSerializer.plainText().deserialize("Hello world!");
