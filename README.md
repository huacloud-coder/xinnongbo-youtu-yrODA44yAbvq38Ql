
## 1\. Java POM模块化是什么


在Java项目中，特别是在使用Maven作为构建工具时，"POM模块化"是一个重要的概念，它指的是将大型项目拆分成多个更小、更易于管理的模块（或称为子项目）。每个模块都有自己的`pom.xml`文件，该文件定义了模块的构建配置，包括依赖关系、插件、目标平台等。


### 1\.1 POM（Project Object Model）


POM是Maven项目管理和构建的核心文件，它通常是一个名为`pom.xml`的XML文件。POM文件包含了项目的所有配置信息，Maven通过这些信息来构建项目、管理依赖以及执行其他构建任务。


### 1\.2 模块化


模块化是一种将软件分解成一组独立但可互操作的模块的技术。在Maven项目中，模块化意味着将大型应用程序或库拆分成更小的组件，每个组件都负责一组特定的功能或业务逻辑。这些组件（即模块）可以通过Maven的依赖管理机制相互依赖，从而形成一个完整的应用程序或库。


### 1\.3 Maven模块化项目的优点


（1）**可重用性**：模块可以被多个项目共享和重用。


（2）**易于管理**：大型项目拆分成多个小模块后，每个模块都可以独立构建和测试，从而简化了整个项目的构建和测试过程。


（3）**清晰的依赖关系**：通过POM文件中的依赖声明，可以清晰地看到模块之间的依赖关系。


（4）**团队协作**：不同的模块可以由不同的团队或开发者并行开发，提高了开发效率。


（5）**灵活性**：模块化使得项目更加灵活，可以更容易地添加、删除或替换模块。


### 1\.4 Maven模块化项目的结构


一个Maven模块化项目通常包含一个父POM文件和多个子模块。父POM文件定义了所有子模块共享的构建配置和依赖管理策略。子模块则继承父POM的配置，并根据需要添加特定的配置或依赖。


### 1\.5 示例


假设有一个名为`MyProject`的Maven模块化项目，它包含三个子模块：`common`、`module-a`和`module-b`。项目的目录结构可能如下所示：



```
MyProject/  
|-- pom.xml (父POM)  
|-- common/  
|   |-- pom.xml  
|   |-- src/  
|       |-- main/  
|           |-- java/  
|               |-- com/example/common/  
|-- module-a/  
|   |-- pom.xml  
|   |-- src/  
|       |-- main/  
|           |-- java/  
|               |-- com/example/modulea/  
|-- module-b/  
    |-- pom.xml  
    |-- src/  
        |-- main/  
            |-- java/  
                |-- com/example/moduleb/

```

在这个例子中，`MyProject/pom.xml`是父POM文件，它定义了所有子模块共有的配置和依赖。`common`、`module-a`和`module-b`是子模块，它们分别包含自己的`pom.xml`文件和源代码。这些子模块可以通过Maven的依赖机制相互依赖，也可以依赖外部库。


通过模块化，`MyProject`项目变得更加清晰、易于管理和维护。开发者可以独立地构建和测试每个模块，而不必担心它们之间的依赖关系。同时，模块化的结构也使得项目更加灵活，可以更容易地根据需求进行扩展或修改。


## 2\. Java Pom两个模块需要互相引用方法示例


在Maven项目中，当两个模块（或称为子项目）需要互相引用时，通常意味着这两个模块之间存在紧密的依赖关系。然而，Maven的常规依赖管理并不直接支持循环依赖（即A依赖B，B又依赖A），因为这会导致构建过程中的死锁。不过，大多数情况下，可以通过重新设计模块结构或利用Maven的特性（如聚合和继承）来避免直接的循环依赖。


但假设我们的场景是合理的，比如两个模块分别负责不同的业务逻辑，但确实需要共享一些公共的类或接口，而这些类或接口又分布在两个模块中。这种情况下，我们可以考虑将共享的部分提取到一个新的模块中，然后让这两个模块都依赖于这个新模块。


我将展示一个简化的例子，其中两个模块`module-a`和`module-b`通过Maven的聚合（Aggregation）和继承（Inheritance）机制来组织，并假设它们通过共享一个公共的父POM来管理依赖，而不是直接互相引用。


### 2\.1 项目结构



```
my-project/  
|-- pom.xml (父POM)  
|-- module-a/  
|   |-- pom.xml  
|   |-- src/  
|       |-- main/  
|           |-- java/  
|               |-- com/example/modulea/ModuleA.java  
|-- module-b/  
|   |-- pom.xml  
|   |-- src/  
|       |-- main/  
|           |-- java/  
|               |-- com/example/moduleb/ModuleB.java  
|-- common/  
    |-- pom.xml  
    |-- src/  
        |-- main/  
            |-- java/  
                |-- com/example/common/SharedClass.java

```

### 2\.2 父POM (`my-project/pom.xml`)



```
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0modelVersion>  
    <groupId>com.examplegroupId>  
    <artifactId>my-projectartifactId>  
    <version>1.0-SNAPSHOTversion>  
    <packaging>pompackaging>  
  
    <modules>  
        <module>module-amodule>  
        <module>module-bmodule>  
        <module>commonmodule>  
    modules>  
project>

```

### 2\.3 `common` 模块 (`common/pom.xml`)



```
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0modelVersion>  
    <parent>  
        <groupId>com.examplegroupId>  
        <artifactId>my-projectartifactId>  
        <version>1.0-SNAPSHOTversion>  
    parent>  
  
    <artifactId>commonartifactId>  
  
    <dependencies>  
          
    dependencies>  
project>

```

### 2\.4 `module-a` 和 `module-b` 的POM文件


这两个模块的POM文件将非常相似，除了它们的`artifactId`和可能的一些特定依赖外。它们都将依赖于`common`模块。



```
  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0modelVersion>  
    <parent>  
        <groupId>com.examplegroupId>  
        <artifactId>my-projectartifactId>  
        <version>1.0-SNAPSHOTversion>  
    parent>  
  
    <artifactId>module-aartifactId>  
  
    <dependencies>  
        <dependency>  
            <groupId>com.examplegroupId>  
            <artifactId>commonartifactId>  
            <version>${project.version}version>  
        dependency>  
          
    dependencies>  
project>

```

### 2\.5 结论


在这个例子中，`module-a`和`module-b`没有直接互相引用，而是通过共享一个`common`模块来避免循环依赖。这是处理Maven项目中模块间依赖关系的推荐方式。如果确实需要两个模块直接互相引用，那么可能需要重新考虑我们的项目结构或设计模式。


## 3\. 如何使用Maven模块化


使用Maven进行模块化是一种将大型项目分解为更小、更易于管理的部分的方法。每个模块都是一个独立的Maven项目，拥有自己的`pom.xml`文件，但可以通过Maven的继承和聚合特性与其他模块相关联。以下是如何使用Maven进行模块化的基本步骤：


### 3\.1 创建父POM


首先，我们需要创建一个父POM（`pom.xml`），它将作为所有子模块的通用配置模板。父POM通常不包含源代码，而是定义了项目共有的配置，如依赖管理、插件配置、目标平台等。



```
  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0modelVersion>  
    <groupId>com.examplegroupId>  
    <artifactId>my-project-parentartifactId>  
    <version>1.0-SNAPSHOTversion>  
    <packaging>pompackaging>  
  
      
    <dependencyManagement>  
        <dependencies>  
              
        dependencies>  
    dependencyManagement>  
  
      
    <build>  
        <pluginManagement>  
              
        pluginManagement>  
    build>  
  
      
    <modules>  
        <module>module-amodule>  
        <module>module-bmodule>  
          
    modules>  
project>

```

注意：`pom`表明这是一个聚合POM，它不会构建任何实际的产品，而是用来聚合和管理其他模块。


### 3\.2 创建子模块


然后，我们需要在父POM的同级目录下（或指定的任何子目录中）创建子模块。每个子模块都应该有自己的`pom.xml`文件，并且通常会继承自父POM。



```
  
<project xmlns="http://maven.apache.org/POM/4.0.0"  
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">  
    <modelVersion>4.0.0modelVersion>  
    <parent>  
        <groupId>com.examplegroupId>  
        <artifactId>my-project-parentartifactId>  
        <version>1.0-SNAPSHOTversion>  
    parent>  
  
    <artifactId>module-aartifactId>  
  
      
    <dependencies>  
          
    dependencies>  
  
      
project>

```

### 3\.3 构建项目


在父POM所在的目录下运行Maven命令，Maven会自动找到并构建所有列在标签下的子模块。



```
bash复制代码

mvn clean install

```

这个命令会首先清理之前构建生成的文件，然后编译、测试并安装所有子模块到本地Maven仓库中。


### 3\.4 依赖管理


在父POM中定义的部分允许我们指定依赖项及其版本号，但不会在父POM中实际引入这些依赖项。子模块可以通过声明相同的依赖项（不包括版本号）来继承这些依赖项及其版本号。


### 3\.5 插件管理


类似地，部分允许我们在父POM中定义插件及其配置，但不会在父POM中实际执行这些插件。子模块可以通过继承这些插件配置来简化插件配置过程。


### 3\.6 结论


通过Maven模块化，我们可以将大型项目分解为更小、更易于管理的部分，同时利用Maven的继承和聚合特性来共享配置和依赖项。这有助于提高项目的可维护性、可重用性和可扩展性。


 本博客参考[楚门加速器p](https://tianchuang88.com)。转载请注明出处！
