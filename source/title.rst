======
标题
======

构造一个标题
^^^^^^^^^^^^^^^^^^^^

标题由以下内容组成:
  * 一个组件，被用作表示主标题
  * 一个组件，被用作表示副标题
  * 一个可选的 ``Title.Times`` 对象, 可被用作决定淡入 (fade-in), 停留 (stay on screen) 和淡出 (fade-out) 时间


**示例:**

.. code:: java

  public void showMyTitle(final @NonNull Audience target) {
    final Component mainTitle = Component.text("This is the main title", NamedTextColor.WHITE);
    final Component subtitle = Component.text("This is the subtitle", NamedTextColor.GRAY);

    // 创建一个带有默认淡入, 停留和淡出时间的简单标题
    final Title title = Title.title(mainTitle, subtitle);

    // 向你的听众发送该标题
    target.showTitle(title);
  }

  public void showMyTitleWithDurations(final @NonNull Audience target) {
    final Title.Times times = Title.Times.of(Duration.ofMillis(500), Duration.ofMillis(3000), Duration.ofMillis(1000));
    // 该标题使用的 times 对象将会使用 500ms 时间淡入, 3000ms 时间停留, 1000ms 时间淡出
    final Title title = Title.title(Component.text("Hello!"), Component.empty(), times);

    // 发送这个标题, 你也可以使用 Audience#clearTitle() 来在任何时候移除这个标题
    target.showTitle(title);
  }
