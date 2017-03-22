# MyBatis_Generator_Test

# Mybatis生成器生成代码 

通过 Mybatis 提供的 generator 项目自动生成；model，dao，mapp-xml代码；
解决重复编写问题，提高开发效率
## 手动编译过程jar包过程
1.编译 mybatis-generator-core.jar 包
2.下载 mysql-connector-java-5.1.41-bin.jar包

### 下载generator编译

克隆，编译执行成功后，
在target目录生成了mybatis-generator-core-1.3.6-SNAPSHOT.jar

```bash
git clone https://github.com/mybatis/gener
cd generator/core/mybatis-generator-core/
##此过程可能需要一些时间；耐心等待
mvn clean package
```
注释: **懒人方案，可以直接google编译好的mybatis-generator-core.jar 包**

### 下载mysql的java驱动包

msyql java 驱动包 mysql-connector-java-5.1.41-bin.jar
```bash
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.41.tar.gz
tar -zxvf mysql-connector-java-5.1.41.tar.gz
ll mysql-connector-java-5.1.41/mysql-connector-java-5.1.41-bin.jar
```

## 使用方法

环境说明及准备：
1. 数据库类型:mysql;数据库名: t_llw,数据表:ci_project;用户名root，密码123456
2. 自动生成代码保存目录 CreateResult；自行创建
注释
3. 自行准备mybatis-generator-core.jar包和mysql-connector-java-x.x.xx-bin.jar包
目录结构如下：
```bash
test_generator
├── CreateResult
├── generator_config.xml
├── mybatis-generator-core-1.3.6.jar
└── mysql-connector-java-5.1.41-bin.jar
```

###  创建用于保存生成结果的目录

```bash
mkdir CreateResult
```

### 创建自动生成代码xml文件
generator_config.xml 内容如下
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <!-- classPathEntry:数据库的JDBC驱动,换成你自己的驱动位置 -->
    <!-- <classPathEntry location="/Users/weichen/src/mybatisGenerator/test_generator/mysql-connector-java-5.1.41-bin.jar" /> -->
    <classPathEntry location="mysql-connector-java-5.1.41-bin.jar" />

    <context id="DB2Tables" targetRuntime="MyBatis3">
        <commentGenerator>
          <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
          <property name="suppressAllComments" value="true" />
          <!-- 此属性用于指定在生成的注释是否将包括MBG代时间戳 -->
          <property name="suppressDate" value="true" />
        </commentGenerator>

        <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://192.168.178.121:3306/t_llw" userId="root" password="123456">
        </jdbcConnection>

        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

		<!-- targetProject:自动生成代码的位置 -->

		<!-- 生成Model -->
        <javaModelGenerator targetPackage="com.model" targetProject="CreateResult">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="true" />
			<!-- 是否针对string类型的字段在set的时候进行trim调用 -->
            <property name="trimStrings" value="false" />
        </javaModelGenerator>

		<!-- 生成Mapper.xml -->
        <sqlMapGenerator targetPackage="com.mapper" targetProject="CreateResult">
            <property name="enableSubPackages" value="true" />

            <property name="enableDeleteByExample" value="false" />
            <property name="enableCountByExample" value="false" />
            <property name="enableUpdateByExample" value="false" />
            <property name="enableSelectByExample" value="false" />
            <property name="selectByExampleQueryId" value="false" />
        </sqlMapGenerator>

		<!-- 生成Mapper.java -->
        <javaClientGenerator targetPackage="com.mapper" targetProject="CreateResult" type="XMLMAPPER">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

		<!--<ignoreColumn column="被忽略的字段的名字" />-->
    <table tableName="ci_project" domainObjectName="MyProject" enableDeleteByExample = "false" enableCountByExample = "false" enableUpdateByExample = "false"			enableSelectByExample = "false" selectByExampleQueryId = "false" />

    </context>
</generatorConfiguration>
```

### 执行自动生成

```bash
#输出succes 为成功
java -jar mybatis-generator-core-1.3.6.jar -configfile generator_config.xml -overwrite
```
```bash
#生成成功后目录结构

```bash
test_generator
├── CreateResult
│   └── com
│       ├── mapper
│       │   ├── MyProjectMapper.java
│       │   └── MyProjectMapper.xml
│       └── model
│           └── MyProject.java
├── generator_config.xml
├── mybatis-generator-core-1.3.6.jar
└── mysql-connector-java-5.1.41-bin.jar
```

## 命令参数简单说明

|  参数     | 说明      |
| ---       | ---       |
| -configfile XXX (必须的)  |  指定配置文件的名称   |
| -overwrite (可选的)  |  新生成的文件会覆盖原有的文件 |
| -tables table1, table2,... (可选的)  |  运行制定表   |

## 参考
http://evoupsight.com/blog/2016/08/15/idea-mybatis-codes-generator/
http://mbg.cndocs.tk/running/runningFromCmdLine.html
http://blog.csdn.net/isea533/article/details/42102297
