# Maven的安装和配置

maven地址
https://mvnrepository.com/

maven下载地址
https://maven.apache.org/download.cgi

maven归档地址
https://archive.apache.org/dist/maven/maven-3/

## 环境变量配置

1. 新增 `MAVEN_HOME` 环境变量，只想maven项目

```text
MAVEN_HOME=C:\software\maven\apache-maven-3.8.6
```

2. 新增 `path` 环境变量

```text
%MAVEN_HOME%\bin
```

3. 验证maven环境变量配置

```text
mvn -version
mvn -v

Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: C:\software\maven\apache-maven-3.8.6
Java version: 11.0.19, vendor: Oracle Corporation, runtime: C:\software\jdk11\Install
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 11", version: "10.0", arch: "amd64", family: "windows"
```

![maven -v](./imgs/maven-version.png)

## 修改 maven 配置

### 修改本地仓库地址

1. 打开 maven 项目下的 `conf/settings.xml` 配置文件;
2. 通过 `<localRepository>` 标签配置自定义本地仓库目录;

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 https://maven.apache.org/xsd/settings-1.2.0.xsd">
    <!-- localRepository
      maven本地仓库目录配置
     | The path to the local repository maven will use to store artifacts.
     |
      默认缓存在 ${user.home}/.m2/repository
     | Default: ${user.home}/.m2/repository
      配置自定义仓库目录设置
    <localRepository>/path/to/local/repo</localRepository>
    -->
    <!-- 配置maven本地仓库地址 -->
    <localRepository>C:\software\maven\repository</localRepository>

    .....
</settings>
```

### 修改镜像地址

1. 打开 maven 项目下的 `conf/settings.xml` 配置文件;
2. 通过 `<mirror>` 标签添加自定义镜像地址;

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 https://maven.apache.org/xsd/settings-1.2.0.xsd">
    ...
    <mirrors>
        <!-- 阿里云镜像 -->
        <mirror>
            <!-- 镜像唯一标识符，内容可以自定义，需要保证全局唯一，避免与其它仓库的id重复 -->
            <id>aliyun-maven</id>
            <!-- 镜像的描述名称，内容可以自定义 -->
            <name>阿里云公共仓库</name>
            <!-- 镜像的访问地址， -->
            <url>https://maven.aliyun.com/repository/public</url>
            <!-- 当前镜像覆盖的原始仓库，maven 默认中央仓库的 id 为 center -->
            <mirrorOf>central</mirrorOf>
        </mirror>
    </mirrors>
    ...
</settings>
```

## 项目结构与pom文件

### maven gavp

maven 工程相对于之前的项目，多出了一组 `gavp` 属性 (`groupdId`, `artifactId`, `version`, `packaging`)， `gav`
属性需要在创建项目的时候指定，`p` 有默认值，这四个属性主要为每一个项目在 maven 仓库中做一个标识。

- `groupId`: com.{公司/BU}.业务线.[子业务线]
- `artifactId`: 产品线-模块名
- `version`: 主版本号.次版本号.修订号 (1.0.1)
    - 主版本号: 当做了不兼容的 API 修改，或者增加了能改变产品方向的新功能;
    - 次版本号: 当作了向下兼容的功能新增 (新增类，接口等);
    - 修订号: 修复bug等;
- `packaging`: 表示需要将项目打包成什么类型的文件
    - `jar` (默认值): 代表普通 java 工程，打包以后是 `jar` 结尾的文件;
    - `war`: 代表 java 的 web 工程，打包以后是 `.war`结尾的文件;
    - `pom`: 代表不会打包，用来做继承的父工程;

### maven 标准项目结构

```text
my-project/
├── src/
│   ├── main/              # 项目主要代码
│   │   ├── java/          # Java源代码目录
│   │   └── resources/     # 资源目录，存放配置文件、静态资源等
│   └── test/              # 项目测试代码
│       ├── java/          # 单元测试目录
│       └── resources/     # 测试资源目录
├── target/                # 构建输出目录
└── pom.xml                # maven 项目管理文件
```

# Maven 工程构建

## 构建的概念和构建过程

项目构建是指将源代码、依赖库和资源文件等转换成可执行文件或可部署的应用程序的过程，在这个过程中包括编译源码、链接依赖库、打包和部署等多个步骤。

项目构建是软件开发过程中至关重要的一部分，他能够有效的提高软件开发效率，使得开发人员能够更加专注于应用程序的开发和维护，而不必关心应用程序的构建细节。

同时项目构建能够将多个开发人员的代码汇合到一起，并能够自动化项目的构建和部署，大大降低项目的出错风险和提高开发效率。

maven 默认生命周期定义了构建时所需执行的所有步骤，是maven生命周期的核心部分

```text
包含命令: compile -> test -> package -> install -> deploy
```

## 命令方式项目构建

| 命令            | 描述                       |
|---------------|--------------------------|
| `mvn compile` | 编译项目，生成 `target` 文件      |
| `mvn package` | 打包项目，生成 `jar` 或 `war` 文件 |
| `mvn clean`   | 清理编译 或 打包 后的项目结构         |
| `mvn install` | 打包后上传到 `maven` 本地仓库      |
| `mvn deploy`  | 只打包，上传到 `maven` 私服仓库     |
| `mvn site`    | 生成站点                     |
| `mvn test`    | 执行测试代码                   |

### `mvn clean`

清理编译 或 打包 后的项目结构

![mvn clean](./imgs/mvn-clean-cmd.png)

清理后得到目录

![mvn clean result](./imgs/mvn-clean-result.png)

### `mvn compile`

编译项目，生成 `target` 文件

![mvn compile](./imgs/mvn-compile-cmd.png)

编译后得到 `target` 产物如下

![mvn compile result](./imgs/mvn-compile-result.png)

### `mvn package`

打包项目，生成 `.jar` 或 `.war` 文件

![mvn package](./imgs/mvn-package-cmd.png)

打包后得到 `target` 产物如下

![mvn package result](./imgs/mvn-package-result.png)


### `mvn install`

打包后上传到 `maven` 本地仓库

![mvn install](./imgs/mvn-install-cmd.png)

打包完成后上传到 `maven` 本地仓库

![mvn install repository](./imgs/mvn-install-repository.png)






