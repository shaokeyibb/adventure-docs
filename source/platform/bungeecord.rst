==========
BungeeCord 
==========

Adventure 支持最新版本的 BungeeCord 以及兼容 BungeeCord 的下游项目, 例如 Waterfall.

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
            // 对于开发构建
            mavenCentral()
         }

   声明依赖:

.. tabs::
   
   .. group-tab:: Maven

      .. code:: xml

         <dependency>
         <groupId>net.kyori</groupId>
         <artifactId>adventure-platform-bungeecord</artifactId>
         <version>4.0.0</version>
         </dependency>
   
   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         dependencies {
            implementation "net.kyori:adventure-platform-bungeecord:4.0.0"
         }


   .. group-tab:: Gradle (Kotlin)

      .. code:: kotlin

         dependencies {
            implementation("net.kyori:adventure-platform-bungeecord:4.0.0")
         }

用法
-----

你首先应该获得一个 ``BungeeAudiences`` 对象通过使用 ``BungeeAudiences.create(plugin)``.
该对象是线程安全的, 因此如果需要的话可以被在不同的线程中重用.
该对象党插件关闭时也应当被 *关闭*.

请注意不是所有功能都在代理服务端中可用. 发送聊天消息, action bar 消息, 标题, boss 血条, tab 列表页眉和页脚都是支持的, 但是其他所有的请求都将会静默失败.

一个有关如何适当的初始化该平台的简单示例如下:

.. code:: java

   public class MyPlugin extends Plugin {
     private BungeeAudiences adventure;

     public @NonNull BungeeAudiences adventure() {
       if(this.adventure == null) {
         throw new IllegalStateException("Cannot retrieve audience provider while plugin is not enabled");
       }
       return this.adventure;
     }

     @Override
     public void onEnable() {
       this.adventure = BungeeAudiences.create(this);
     }

     @Override
     public void onDisable() {
       if(this.adventure != null) {
         this.adventure.close();
         this.adventure = null;
       }
     }

   }

组件序列化器
---------------------

对于那些尚未被 ``Audience`` 实现的功能, ``BungeeCordComponentSerializer`` 允许你在 Adventure :doc:`Components </text>` 和原生 BungeeCord 聊天组件 API 之间相互转换.

.. caution::

    对于代理服务端的某些领域 (比如, 发送服务器列表相应), 组件序列化器无法在 ``BungeeAudiences`` 实例初始化完成前被适当的注入. 当尚未创建一个 ``BungeeAudiences`` 实例前使用 Adventure ``Component`` 实例 **将不会** 工作.
