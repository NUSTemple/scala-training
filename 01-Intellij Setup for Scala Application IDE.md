

## IDEA Intellij Setup for Scala/Spark Application

Software preparation

1. Install IDEA Intellij from https://www.jetbrains.com/idea/download/ 

2. Install Java 1.8 SDK from https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

   - When installing Java JDK, try to avoid space in the folder name. for example, you can use c:\Java\SDK as installation folder.

> ![01-Java](./img/01-Java.png)

3. Install Maven from https://maven.apache.org/download.cgi. 
4. Install Scala 2.11.8 from https://downloads.lightbend.com/scala/2.11.8/scala-2.11.8.msi. 

---
Once all preparation is done, we will start to create project in IDEA Intellij. 

> ![01-Intellij-Setup-01](./img/01-Intellij-Setup-01.png)

1. Open Intellij and create a Maven project.
2. Choose Project SDK as Java 1.8.
3. Choose a archetype as project folder template. For example, scala-archetype-simple

> ![01-Intellij-Setup-02](./img/01-Intellij-Setup-02.png)

4. Name your GroupId and ArtifactId

> ![01-Intellij-Setup-03](./img/01-Intellij-Setup-03.png)

	- GroupId: group name, for example, com.micron.f10ds
	- ArtifactId: unique jar name, for example, my-first-scala-app

If you do this inside Micron environment, please update your maven `settings.xml` (for example, C:\Users\TAN PENG\.m2\settings.xml) to https://github.com/NUSTemple/training/blob/master/templates/settings.xml to replace the maven repo and add proxy.

If project is created successfully, you should see below folder structure. 
> ![01-Intellij-Setup-04](./img/01-Intellij-Setup-04.png)

`.idea` folder is used by IDEA intellij only. it is not related to your project code
`src` folder is used to store your functional code
`test` folder is used to store your test code
`pom.xml` is used for maven configuration. This file will use to download project dependency and also tell maven how to build your application. 

for simplicity, you can replace your `pom.xml` to this file https://github.com/NUSTemple/training/blob/master/templates/pom.xml. 

With this, you can start to code your first scala applications. 

