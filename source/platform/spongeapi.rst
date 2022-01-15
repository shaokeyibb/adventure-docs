=========
SpongeAPI 
=========

Adventure 为面向 *Minecraft: Java 版* 1.12 的 SpongeAPI 7 提供平台.
对于 SpongeAPI 8 以及更高版本 (支持 *Minecraft: Java 版* 1.16.4), Adventure 是其原生文本依赖库, 无需平台支持.

要想开始使用此平台, 添加这些构件到你的构建文件:

首先, 添加仓库:

.. tabs::
   
   .. group-tab:: Maven

      .. code:: xml

         <repositories>
             <!-- ... -->
             <repository> <!-- 对于开发构建 -->
               <id>sonatype-oss</id>
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
         <artifactId>adventure-platform-spongeapi</artifactId>
         <version>4.0.0</version>
         </dependency>
   
   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         dependencies {
            implementation "net.kyori:adventure-platform-spongeapi:4.0.0"
         }


   .. group-tab:: Gradle (Kotlin)

      .. code:: kotlin

         dependencies {
            implementation("net.kyori:adventure-platform-spongeapi:4.0.0")
         }


用法
~~~~~

SpongeAPI 平台既可以通过 Guice 依赖注入,也可以直接创建. 我们推荐使用注入以减少样板代码.

一个相当直截了当的插件示例:

.. code:: java

   @Plugin(/* [...] */)
   public class MyPlugin {
     private final SpongeAudiences adventure;

     @Inject
     MyPlugin(final SpongeAudiences adventure) {
       this.adventure = adventure;
     }

     public @NonNull SpongeAudiences adventure() {
       return this.adventure;
     }
   }


这将会设置一个可以为玩家或者任何 ``MessageReceiver`` 提供听众实例的 ``SpongeAudiences`` 实例.
