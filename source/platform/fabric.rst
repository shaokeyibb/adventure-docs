======
Fabric
======

Adventure支持面向 *Minecraft: Java 版* 1.16 以及更高版本的 Fabric, 无论是服务端还是客户端都可以使用. 每一个 Minecraft 的主要版本通常都需要一个新版本的平台.

该平台支持所有特性, 包含本地化 (localization) 和自定义渲染器 (renderers).

----------
依赖
----------

fabric 平台被打包为一个模组, 被设计为通过 jar-in-jar 的打包方式包含在模组中. 于其他 adventure 项目一样, 发行版分布在 Maven Central, 快照版则在 Sonatype OSS 上.

添加这些构件到你的构建文件:

首先, 添加仓库:

.. tabs::
   
   .. group-tab:: Maven

      .. code:: xml

         <repositories>
             <!-- ... -->
             <repository> <!-- 对于开发构建 -->
               <id>sonatype-oss-snapshots</id>
               <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
             </repository>
             <!-- ... -->
         </repositories>
   
   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         repositories {
            // 对于开发构建
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
            // 对于开发构建
            maven(url = "https://oss.sonatype.org/content/repositories/snapshots/") {
                name = "sonatype-oss-snapshots"
            }
            // 对于发行版
            mavenCentral()
         }

.. tabs::
   
   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         dependencies {
            modImplementation include("net.kyori:adventure-platform-fabric:4.1.0") // 对于 Minecraft 1.17
         }


   .. group-tab:: Gradle (Kotlin)

      .. code:: kotlin

         dependencies {
            modImplementation(include("net.kyori:adventure-platform-fabric:4.1.0")!!) // 对于 Minecraft 1.17
         }

fabric 平台需要 *fabric-api-base* 提供的一些语言更改事件, 并且可以选用 Colonel_ 以允许在未安装模组的客户端上使用 ``Component`` 和 ``Key`` 参数类型. 无需其他依赖.

.. attention::

   每一个主要的 Minecraft 发行版都需要一个不同的平台版本. 下表展示的平台版本是适用于每一个 Minecraft 发行版的最新发行版本. 旧的发行版本可能不会再受到支持.

   =================  ======================================
   Minecraft 版本       ``adventure-platform-fabric`` 版本
   =================  ======================================
   1.16.2-1.16.4      4.0.0
   1.17.x             4.1.0
   1.18               5.0.0-SNAPSHOT
   =================  ======================================


------
客户端
------

当一个服务器可用时, 逻辑服务端上的 Fabric 平台可通过一个 ``FabricServerAudiences`` 实例在任何时候被访问.
默认情况下, 可翻译组件将被使用总翻译器 (the global translator) 渲染, 但是可以在平台初始化期间传递自定义渲染器.

除了 ``permission`` 方法, 所有 ``AudienceProvider`` 接口的方法都是受支持的. 直到 Fabric 发布一个合适的权限 API, 其才将得到支持.

要开始使用 Adventure, 以如下方式设置一个听众提供器 (audience provider):

.. code:: java

   public class MyMod implements ModInitializer {
     private FabricServerAudiences adventure;

     public FabricServerAudiences adventure() {
       if(this.adventure == null) {
         throw new IllegalStateException("Tried to access Adventure without a running server!");
       }
     }

     @Override
     public void onInitialize() {
       // 使用服务器生命周期回调 (the server lifecycle callbacks) 注册
       // 这将确保任何平台数据都将在不同的游戏实例间被清除
       // 这在能在一个模组初始化过程中存在多个服务端实例的内置服务端中十分重要
       ServerLifecycleEvents.SERVER_STARTING.register(server -> this.platform = FabricServerAudiences.of(server));
       ServerLifecycleEvents.SERVER_STOPPED.register(server -> this.platform = null);
     }
   }

在这里, 可以为玩家以及任何其他获得听众实例 ``CommandSource``. 特制的序列化器实例也同样可用, 以允许在组件序列化中使用游戏信息.

~~~~~~~~~~~~~~~~~~~~
本地化 (Localization)
~~~~~~~~~~~~~~~~~~~~

作为平台翻译支持的一部分A, ``PlayerLocales.CHANGED_EVENT`` 将会在一个服务器内的玩家从他们的客户端接受一个更新的语言时被调用, 并且允许访问该玩家当前的语言.

~~~~~~~~
指令
~~~~~~~~

Fabric 平台提供自定义的参数类型以在 Brigadier 命令中指定 ``Key`` 和 ``Component`` 参数, 以及有 helpers 可以很容易的从一个 ``CommandSourceStack`` (yarn: ``ServerCommandSource``) 实例获取一个 ``Audience`` 实例.

.. warning::

    如果使用了这些自定义的参数类型, 原版客户端将无法加入服务器除非 Colonel_ 模组被安装在服务器上. 就像这个平台一样, 它很小, 并且可以很容易的包含在你的模组 jar 中.

作为一个示例, 这里有一个简单的指令, 可以回显输入的任何内容:

.. code:: java


   // A potential method to be in the mod initializer class above
   private static final String ARG_MESSAGE = "message";

   void registerCommands(final CommandDispatcher dispatcher, final boolean isDedicated) {
     dispatcher.register(literal("echo").then(argument(ARG_MESSAGE, component()).executes(ctx -> {
       final AdventureCommandSourceStack source = this.adventure().audience(ctx.getSource());
       final Component message = component(ctx, ARG_MESSAGE);

       source.sendMessage(Component.text("You said: ").append(message));
     }));
   }

------
客户端
------

在 Fabric 平台上, 纯客户端的操作也是受支持的. 因为客户端是一个单例, 并且在客户端上只有一个主体可以采取行动: 客户端玩家, 因此设置比其在服务端上更少,

这意味着对于大多数用户, ``FabricClientAudiences`` 对象可以被视为一个单例, 唯一的例外是那些使用自定义渲染器的用户.
浙江使 Adventure 听众变得相当简单, 如同如下代码所示:

.. code:: java

   void doThing() {
     // 获取听众对象
     final Audience client = FabricClientAudiences.of().audience();

     // 做一些事情. 这仅在玩家在游戏内时工作.
     client.sendMessage(Component.text("meow", NamedTextColor.DARK_PURPLE));
   }

``Audience`` 接口的全部功能都是可用的, 包括本地化!

-------------------------
使用原生类型
-------------------------

遗憾的是, Adventure 不能为游戏中每一个需要使用聊天组件的地方提供 API. 然而, 对于那些未被 ``Audience`` 中的 API 覆盖的领域, 有可能在原生和 Adventure 的组件类型中进行转换. 查看在 ``FabricAudiences`` 中的方法以了解可用的内容.


.. _Colonel: https://gitlab.com/stellardrift/colonel
