=========
听众 (Audiences)
=========

听众 (audience), 论其核心, 是一些内容的 0 个或更多观众 (viewers) 的组合.
听众的概念是 Adventure 与其他 Minecraft 平台相比最明显的区别.

作为一个 API, ``Audience`` 被设计为一个面向任何玩家,命令发送者 (command sender), 控制台 (console),
或者其他可以接收文本，标题，boss 血条，以及其他 Minecraft 媒体的对象的通用接口.
这将允许拓展听众以覆盖不止一个单独接收者 (receive) - 可能 "听众" 可以包含一个队伍, 服务器, 世界,
或者所有满足某些条件的玩家 (例如拥有指定权限). 这个通用的接口也允许通过优雅的降低功能性来减少模板代码.
例如，向一个命令发送者发送一个 boss 血条显然没有什么用，而且你不能向 Minecraft 1.7 的客户端发送标题.

你通常会从 :doc:`/platform/index` 的其中之一获取听众实例.
Adventure API 自身包含两个听众实现: 一个不支持任何行为 (因此什么也不会做).
``Audience.empty()``, 一个将行为转发给听众对象中的每一个成员,
``Audience.audience()`` 及相关方法, 以及为你实现转发逻辑的 ``ForwardingAudience``.

大多数用户将会主要使用这个 API 来显示由 API 其他部分创建的内容.

指针
^^^^^^^^

听众对象也可以提供任意的信息，例如玩家的显示名称 (display name) 或者 UUID. 这都籍由指针系统完成.

例如:

.. code:: java

  // 从一个听众成员获取 uuid, 返回一个 Optional<UUID>
  audience.get(Identity.UUID);

  // 获取玩家的玩家的显示名称, (亦或者) 返回一个默认值
  audience.getOrDefault(Identity.DISPLAY_NAME, Component.text("no display name!"));
