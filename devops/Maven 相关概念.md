# Maven 相关概念

## Maven 简介 & 基本概念

> **POM (Project Object Model): 项目对象模型**

Maven 本质是项目管理工具, 将项目开发和管理过程抽象成一个项目对象模型 (POM)

> **Maven 的作用**

1. 项目构建

2. 依赖管理

3. 统一开发结构

### 仓库 & 坐标

> 仓库：存储资源, Jar 包

分类：

1. 本地仓库

2. 私服：小范围内存储资源的仓库, 从中央仓库获取 或 具有版权的资源, 不对外共享

3. Maven 中央仓库：Maven 团队维护, 存储所有资源的仓库

> 坐标：使用唯一标识，描述仓库中资源的位置

组成：

1. groupId：定义当前Maven 项目隶属组织 (org.mybatis)

2. artifactId：定义当前Maven 项目名称

3. version：当前版本号

仓库配置：

user setting: ${user.home}/.m2/settings.xml

user repository: ${user.home}/.m2/repository

配置阿里云仓库：

```xml
<mirror>
      <id>aliyun-public</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun public</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>

    <mirror>
      <id>aliyun-central</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun central</name>
      <url>https://maven.aliyun.com/repository/central</url>
    </mirror>

    <mirror>
      <id>aliyun-spring</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun spring</name>
      <url>https://maven.aliyun.com/repository/spring</url>
    </mirror>

    <mirror>
      <id>aliyun-spring-plugin</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun spring-plugin</name>
      <url>https://maven.aliyun.com/repository/spring-plugin</url>
    </mirror>

    <mirror>
      <id>aliyun-apache-snapshots</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun apache-snapshots</name>
      <url>https://maven.aliyun.com/repository/apache-snapshots</url>
    </mirror>

    <mirror>
      <id>aliyun-google</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun google</name>
      <url>https://maven.aliyun.com/repository/google</url>
    </mirror>

    <mirror>
      <id>aliyun-gradle-plugin</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun gradle-plugin</name>
      <url>https://maven.aliyun.com/repository/gradle-plugin</url>
    </mirror>

    <mirror>
      <id>aliyun-jcenter</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun jcenter</name>
      <url>https://maven.aliyun.com/repository/jcenter</url>
    </mirror>

    <mirror>
      <id>aliyun-releases</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun releases</name>
      <url>https://maven.aliyun.com/repository/releases</url>
    </mirror>

    <mirror>
      <id>aliyun-snapshots</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun snapshots</name>
      <url>https://maven.aliyun.com/repository/snapshots</url>
    </mirror>

    <mirror>
      <id>aliyun-grails-core</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun grails-core</name>
      <url>https://maven.aliyun.com/repository/grails-core</url>
    </mirror>

    <mirror>
      <id>aliyun-mapr-public</id>
      <mirrorOf>*</mirrorOf>
      <name>aliyun mapr-public</name>
      <url>https://maven.aliyun.com/repository/mapr-public</url>
    </mirror>
```

## 依赖传递的冲突问题

- **路径优先**：当依赖出现相同资源时，层级越深，优先级越低

- **声明优先**：当资源在相同层级被依赖时，配置顺序靠前覆盖顺序靠后的

- **传递优先**：当同级配置了相同资源的不同版本，后配置的覆盖先配置的

### 可选依赖

对外隐藏当前所依赖的资源

> \<optional>true</optional>

```xml
<!-- 当别的项目引入该依赖时，该依赖中所依赖Junit 对外不可见 -->
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
      <optional>true</optional>
</dependency>
```

### 排除依赖

主动断开依赖中的资源，被排除的资源无需指定版本

```xml
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <exclusions>
          <exclusion>
              <groupId>org.hamcrest</groupId>
              <artifactId>hamcrest-core</artifactId>
          </exclusion>
      </exclusions>
</dependency>
```

### 依赖范围

作用范围：

- 主程序范围有效 （main 文件夹范围内）

- 测试程序范围有效 （test 文件夹范围内）

- 是否参与打包 （package 指令范围内）

![](https://raw.githubusercontent.com/0x2552/images-repo/main/2023/04/01-14-52-09-2023-04-01-14-52-06-image.png)

## Maven 私服

### 仓库分类

- 宿主仓库 hosted
  
  - 保存无法从中央仓库获取的资源 (自主研发, 第三方非开源项目)

- 代理仓库 proxy
  
  - 代理远程仓库，通过nexus 访问其他公共仓库，例如中央仓库

- 仓库组 group
  
  - 将若干个仓库组成一个群组，简化配置
  
  - 仓库组不能保存资源，属于设计型仓库

### 本地仓库访问私服

> settings.xml

```xml
<!-- 配置访问服务器的权限，用户密码 -->
   <!-- servers
   | This is a list of authentication profiles, keyed by the server-id used within the system.
   | Authentication profiles can be used whenever maven must make a connection to a remote server.
   |-->
  <servers>
    <!-- server
     | Specifies the authentication information to use when connecting to a particular server, identified by
     | a unique name within the system (referred to by the 'id' attribute below).
     |
     | NOTE: You should either specify username/password OR privateKey/passphrase, since these pairings are
     |       used together.
     |
    <server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
    -->

    <!-- Another sample, using keys to authenticate.
    <server>
      <id>siteServer</id>
      <privateKey>/path/to/private/key</privateKey>
      <passphrase>optional; leave empty if not used.</passphrase>
    </server>
    -->
  </servers>
```

```xml
<!-- 自定义私服，资源来源 -->
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
```



### Maven 冷知识之 <java.version> 的作用

![](https://raw.githubusercontent.com/0x2552/images-repo/main/2023/04/05-01-10-47-2023-04-05-01-10-43-image.png)
