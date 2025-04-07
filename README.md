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

### maven 标准项目结构

```text
my-project/
├── src/
│   ├── main/
│   │   ├── java/          # 主代码
│   │   └── resources/     # 主资源（配置文件等）
│   └── test/
│       ├── java/          # 测试代码
│       └── resources/     # 测试资源
├── target/                # 构建输出目录
└── pom.xml                # 项目配置文件
```

### pom文件解析

