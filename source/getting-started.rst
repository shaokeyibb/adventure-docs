===============
开始
===============

要想在你的项目中使用 Adventure, 你需要添加如下的依赖和仓库 (如果使用 Gradle):

.. tabs::

   .. group-tab:: Maven

      .. code-block:: xml
        :substitutions:

         <dependency>
            <groupId>net.kyori</groupId>
            <artifactId>adventure-api</artifactId>
            <version>|version|</version>
         </dependency>

   .. group-tab:: Gradle (Groovy)

      .. code-block:: groovy
        :substitutions:

         repositories {
           mavenCentral()
         }

         dependencies {
            implementation "net.kyori:adventure-api:|version|"
         }


   .. group-tab:: Gradle (Kotlin)

      .. code-block:: kotlin
        :substitutions:

         repositories {
           mavenCentral()
         }

         dependencies {
            implementation("net.kyori:adventure-api:|version|")
         }

一些平台已经原生支持 Adventure.
在这种情况下, 你无需添加 Adventure 作为依赖.
要想查看已经包含 Adventure 的平台列表, 请见 :doc:`/platform/native`.

要想在其他平台使用 Adventure, 你可能希望查看一些平台特定 (platform-specific) 的适配器 (adapters).
一个带有受支持适配器的平台列表可见于 :doc:`/platform/index`.

使用快照构建
^^^^^^^^^^^^^^^^^^^^^

要想使用快照构建, 你需要添加如下的仓库:

.. tabs::

   .. group-tab:: Maven

      .. code:: xml

         <repositories>
             <repository>
                <id>sonatype-oss-snapshots</id>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
             </repository>
         </repositories>

   .. group-tab:: Gradle (Groovy)

      .. code:: groovy

         repositories {
            maven {
                name = "sonatype-oss-snapshots"
                url = "https://oss.sonatype.org/content/repositories/snapshots/"
            }
         }

   .. group-tab:: Gradle (Kotlin)

      .. code:: kotlin

         repositories {
            maven(url = "https://oss.sonatype.org/content/repositories/snapshots/") {
                name = "sonatype-oss-snapshots"
            }
         }