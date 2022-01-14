=========
Boss 血条
=========

构造一个 Boss 血条
^^^^^^^^^^^^^^^^^^^^^^^^

Boss 血条由以下内容组成:
  * 一个组件, 被用作 boss 血条标题
  * 一个从 0 到 1 的数字, 被用作决定 boss 血条有多满
  * 一种颜色, 将会在 <1.9 的客户端上被降采样
  * 一个叠加层 (overlay), 被用作决定 boss 血条视觉上分段的数量


**示例:**

.. code:: java

  private @Nullable BossBar activeBar;

  public void showMyBossBar(final @NonNull Audience target) {
    final Component name = Component.text("Awesome BossBar");
    // 创建一个无进度 (progress), 无凹槽 (notches), 红色的 boss 血条
    final BossBar emptyBar = BossBar.bossBar(name, 0, BossBar.Color.RED, BossBar.Overlay.PROGRESS);
    // 创建一个拥有 50% 进度, 10 个凹槽, 绿色的 boss 血条
    final BossBar halfBar = BossBar.bossBar(name, 0.5f, BossBar.Color.GREEN, BossBar.Overlay.NOTCHED_10);
    // 等等..
    final BossBar fullBar = BossBar.bossBar(name, 1, BossBar.Color.BLUE, BossBar.Overlay.NOTCHED_20);

    // 发送一个 bossbar 给你的听众
    target.showBossBar(fullBar);

    // 将其本地存储以在之后手动隐藏
    this.activeBar = fullBar;
  }

  public void hideActiveBossBar(final @NonNull Audience target) {
    target.hideBossBar(this.activeBar);
    this.activeBar = null;
  }


更改一个已被激活的 Boss 血条
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Boss 血条是可变的, 并且在它们的对象中监听更改,
游戏内试图将会自动改变而不需要手动刷新它们!

因此, 如果一个 boss 血条当前是被激活的

.. code:: java

   final BossBar bossBar = BossBar.bossBar(Component.text("Cat counter"), 0, BossBar.Color.RED, BossBar.Overlay.PROGRESS);

并且传入了一个组件的 ``#name()`` 被调用

.. code:: java

   final Component newText = Component.text("Duck counter");

   bossBar.name(newText);

该 boss 血条将会被自动更新.  ``progress``, ``color`` 和 ``overlay`` 同理.
