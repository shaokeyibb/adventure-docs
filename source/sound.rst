=============
音效 (Sound)
=============

Adventure 包含一个 API 以播放任何内建或资源包提供的音效. 注意不是所有的平台都实现了播放音效.

构造一个音效
^^^^^^^^^^^^^^^^^^^^

音效由以下内容组成:
  * 一个 Key (更广为人知的说法是 ``Identifier`` 或者 ``ResourceLocation``), 被用作决定要播放的音效. 可以使用任何从资源包自定义的音效. 如果客户端不理解这个音效, 则将会被忽略 (客户端日志将会打印一个警告提示).
  * 一个音效来源, 被用作告诉客户端听到的音效的类型. 客户端的音效设定也归因于来源.
  * 一个数字, 被用作决定音效可被听到的半径.
  * 一个从 0 到 2 的数字, 被用作决定要播放音效的音高。

**示例:**

.. code:: java

  // 使用标准的音量和音高创建一个内建的音效
  Sound musicDisc = Sound.sound(Key.key("music_disc.13"), Sound.Source.MUSIC, 1f, 1f);

  // 从我们的资源包创建一个拥有更高音高的音效
  Sound myCustomSound = Sound.sound(Key.key("adventure", "rawr"), Sound.Source.AMBIENT, 1f, 1.1f);

播放一个音效
^^^^^^^^^^^^^^^

.. warning::

  客户端可以同时播放多个音效, 但是从 1.16 开始被限制为只能同时播放 8 个音效.

  因为 `MC-138832 <https://bugs.mojang.com/browse/MC-138832>`_, 使用发射器 (emitter) 播放的音效的音量和升高可能被忽略.

  如 `MC-146721 <https://bugs.mojang.com/browse/MC-146721>`_ 所述, 任何立体声音效都将不会在一个指定位置或跟随一个实体播放 , 因此, 位置和发射器参数将会忽略.

一旦你创建了一个音效, 便可以使用多种方法向一个听众播放:

.. code:: java

  // 在听众所在的位置播放一个音效
  audience.playSound(sound);

  // 在指定位置播放一个音效
  audience.playSound(sound, 100, 0, 150);

  // 播放一个跟随听众成员的音效
  audience.playSound(sound, Sound.Emitter.self());

  // 播放一个跟随其他发射器 (通常是一个实体) 的音效
  audience.playSound(sound, someEntity);

停止一个音效
^^^^^^^^^^^^^^^

一个音效停止 (sound stop) 将可以停止选定的音效 -- 从客户端正在播放的所有音效, 到特定名称的音效.

.. code:: java

   public void stopMySound(final @NonNull Audience target) {
    // 为目标停止一个音效
    target.stopSound(SoundStop.named(Key.key("music_disc.13"));
    // 为目标停止所有天气音效
    target.stopSound(SoundStop.source(Sound.Source.WEATHER));
    // 为目标停止所有音效
    target.stopSound(SoundStop.all());
  }

音效停止也可以通过使用下面示例块中的方法被构造.
或者，他们也可以被直接从一个音效中构造.

.. code:: java

  // 获得一个能够停止特性音效的音效停止对象
  mySound.asStop();

  // 音效也可以通过使用 stopSound 方法被直接停止
  audience.stopSound(mySound);

创建一个自定义音效
^^^^^^^^^^^^^^^^^^^^^^^^

使用 ``sounds.json`` 来在一个资源包中定义音效. 进一步了解有关内容可阅读 `Minecraft Wiki <https://minecraft.gamepedia.com/Sounds.json>`_

