# Flyway #

## 什么是Flyway ##

Flyway 是一款开源的 Flyway 对数据库进行版本管理主要由Metadata表和6种命令完成，Metadata 主要用于记录元数据，每种命令功能和解决的问题范围不一样.数据库版本管理工具，它更倾向于规约优于配置的方式。Flyway 可以独立于应用实现管理并跟踪数据库变更，支持数据库版本自动升级，并且有一套默认的规约，不需要复杂的配置，Migrations 可以写成 SQL 脚本，也可以写在J ava 代码中，不仅支持 Command Line 和 Java API，还支持 Build 构建工具和 Spring Boot 等，同时在分布式环境下能够安全可靠地升级数据库，同时也支持失败恢复等。

## Flyway 背景 ##

通常在项目开始时会针对数据库进行全局设计，但在开发产品新特性过程中，难免会遇到需要更新数据库 Schema 的情况，比如：添加新表，添加新字段和约束等，这种情况在实际项目中也经常发生。那么，当开发人员完成了对数据库更的 SQL 脚本后，如何快速地在其他开发者机器上同步？并且如何在测试服务器上快速同步？以及如何保证集成测试能够顺利执行并通过呢？

其实，以上问题可以通过 Flyway 工具来解决，Flyway 可以实现自动化的数据库版本管理，并且能够记录数据库版本更新记录，Flyway 官网对 Why database migrations 结合示例进行了详细的阐述，有兴趣可以参阅一下。

## Flyway 工作原理 ##

Flyway 对数据库进行版本管理主要由 Metadata 表和6种命令完成，Metadata 主要用于记录元数据，每种命令功能和解决的问题范围不一样.

具体内容可以参考博客[快速掌握和使用 Flyway](https://blog.waterstrong.me/flyway-in-practice/)。

## Flyway 使用 ##
使用命令行、Gradle 操作 Flyway 的方式在博客中有详细讲到，这里补充一下使用 Maven 的 Flyway plugin。

### Maven ###

#### 首先需要在 Maven 中引入 Flyway 插件 

```xml
<plugin>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-maven-plugin</artifactId>
    <version>5.0.2</version>
    <configuration>
        ....
    </configuration>
</plugin>
```
#### 在 Maven 中配置 Flyway 有多种方式 

[多种配置方式详见官网](https://flywaydb.org/documentation/maven/#system-properties)

##### 在 pom.xml 中通过 configuration 标签进行配置 

```xml
<plugin>
    ...
    <configuration>
        <user>myUser</user>
        <password>mySecretPwd</password>
        <schemas>
            <schema>schema1</schema>
            <schema>schema2</schema>
            <schema>schema3</schema>
        </schemas>
        <placeholders>
            <keyABC>valueXYZ</keyABC>
            <otherplaceholder>value123</otherplaceholder>
        </placeholders>
    </configuration>
</plugin>
```
##### 在 Maven Properties 中进行配置 

```xml
<project>
    ...
    <properties>
        <!-- Properties are prefixed with flyway. -->
        <flyway.user>myUser</flyway.user>
        <flyway.password>mySecretPwd</flyway.password>

        <!-- List are defined as comma-separated values -->
        <flyway.schemas>schema1,schema2,schema3</flyway.schemas>

        <!-- Individual placeholders are prefixed by flyway.placeholders. -->
        <flyway.placeholders.keyABC>valueXYZ</flyway.placeholders.keyABC>
        <flyway.placeholders.otherplaceholder>value123</flyway.placeholders.otherplaceholder>
    </properties>
    ...
</project>
```

##### 在 Maven settings.xml 中配置
```xml
<settings>
    <servers>
        <server>
            <!-- By default Flyway will look for the server with the id 'flyway-db' -->
            <!-- This can be customized by configuring the 'serverId' property -->
            <id>flyway-db</id>
            <username>myUser</username>
            <password>mySecretPwd</password>
        </server>
    </servers>
</settings>
```

#### 命令
- mvn flyway:migrate
    + 迁移数据库
- mvn flyway:clean
    + 丢弃掉所有在 schema 上记录的对象，**会删除整个表！**
- mvn flyway:info
    + 打印迁移的信息和状态
- mvn flyway:validate
    + 验证 classpath 路径下的脚本
- mvn flyway:baseline
    + 从现有的数据库中拉一个基线
- mvn flyway:repair
    + 修复 schema 表中的问题

#### SQL 脚本名称
- prefix: 脚本前缀，默认 V 是版本化的，R 是可以重复执行的
- version: 使用点“.”或者单下划线“_”，可以有多个，如：3.1.1.1
- separator: 可配置的分隔符, 默认是双下划线“__” 
- description: 下划线分隔名称描述
- suffix: 文件后缀，默认是 .sql

例如： V2__CreateLoginTable.sql
V2.1__UpdateUserInfo.sql

## Flyway 与原有项目整合 ##

### Spring 项目 ###

#### 引入依赖 

##### Maven 

```xml
<dependency>
    <groupId>org.flywaydb</groupId>
    <artifactId>flyway-core</artifactId>
    <version>5.0.2</version>
</dependency>
```

##### Gradle 
```groove
compile "org.flywaydb:flyway-core:5.0.2"
```

#### 配置 

```xml
   <bean id="flyway" class="org.flywaydb.core.Flyway" init-method="migrate">
        <property name="baselineOnMigrate" value="true" />
        <property name="locations" value="classpath:db/migration" />
        <property name="dataSource" ref="dataSource" />
    </bean>

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean" depends-on="flyway">
        <property name="dataSource" ref="dataSource" />
        <property name="mapperLocations" value="classpath:mybatis-mapper/*.xml"/>
    </bean>
```

其中，名为 Flyway 的 bean 是 Flyway 的实例，主要需要设置的 property 有：
- **baselineOnMigrate** 该选项表示是否从已有的数据库拉基线出来，如果是旧项目**一定要置为true**
- locations 指定脚本文件存放的位置， 默认为 "classpath:db/migration"
- dataSource 需要管理的数据库实例

为了保证先执行数据库脚本，需要让数据库工厂 Bean（或者其它数据库操作的关键 Bean）的初始化依赖于 Flyway，如上面的 
```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean" depends-on="flyway"> 。
```

### Springboot 项目 ###

引入 Flyway 依赖即可，可以在 application.properties 或者 application.yml 文件中对 Flyway 进行配置。

