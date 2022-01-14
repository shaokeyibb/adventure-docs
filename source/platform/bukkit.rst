======
Bukkit
======

适用于 Bukkit 的 Adventure 平台实现支持 Minecraft 1.7.10 至 1.17.1 的 Paper, Spigot, 以及 Bukkit.

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

   声明依赖:

.. tabs::
   
   .. group-tab:: Maven

      .. code:: xml

         <dependency>
         <groupId>net.kyori</groupId>
         <artifactId>adventure-platform-bukkit</artifactId>
         <version>4.0.0</version>
         </dependency>
   
   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         dependencies {
            implementation "net.kyori:adventure-platform-bukkit:4.0.0"
         }


   .. group-tab:: Gradle (Kotlin)

      .. code:: kotlin

         dependencies {
            implementation("net.kyori:adventure-platform-bukkit:4.0.0")
         }

用法
-----

你首先应该获得一个 ``BukkitAudiences`` 对象通过使用 ``BukkitAudiences.create(plugin)``. 该对象是线程安全的, 因此如果需要的话可以被在不同的线程中重用.
从这里开始, Bukkit 的 ``CommandSender`` 和 ``Player`` 将可以被转换为
``Audience`` 通过在 ``BukkitAudiences`` 上使用适当的方法.

为了清理资源以及提高 ``/reload`` 的成功的可能性, 当一个插件被关闭时, 听众对象也会被被关闭.

.. code:: java

   public class MyPlugin extends JavaPlugin {

     private BukkitAudiences adventure;

     public @NonNull BukkitAudiences adventure() {
       if(this.adventure == null) {
         throw new IllegalStateException("Tried to access Adventure when the plugin was disabled!");
       }
       return this.adventure;
     }

     @Override
     public void onEnable() {
       // 为插件实例化听众对象
       this.adventure = BukkitAudiences.create(this);
       // 然后进行其他初始化操作
     }

     @Override
     public void onDisable() {
       if(this.adventure != null) {
         this.adventure.close();
         this.adventure = null;
       }
     }
   }

该听众提供器应该被直接用做序列化器上, 它将处理跨版本发送消息的兼容性措施.


组件序列化器
---------------------

对于未覆盖 ``Audience`` 接口的领域, Bukkit 平台提供了 ``MinecraftComponentSerializer`` (可用于基于 Craftbukkit 的服务器), 和 ``BungeeComponentSerializer`` (可用于基于 Spigot 和 Paper 的服务器) 来直接在 Adventure :doc:`Components </text>` 和其他组件类型中作转换. 对于使用那些不直接与原生类型继承的用途,JSON 和旧版风格格式的序列化器在这些运行的服务器版本上也被暴露于 ``BukkitComponentSerializer``.
