# Java集成Mybatis
> pom.xml引入mybatis
```
        <!--mybatis起步依赖-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>1.2.0</version>
        </dependency>
        <!--MySQL连接驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```
> 在配置文件application.properties中添加数据库坐标   

```  
# MySQL数据库配置文件
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://192.168.1.218:3306/test?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=MyPass@123
```  

> 创建表  
```mysql
/*
 Navicat Premium Data Transfer

 Source Server         : 192.168.1.218
 Source Server Type    : MySQL
 Source Server Version : 80023
 Source Host           : 192.168.1.218:3306
 Source Schema         : test

 Target Server Type    : MySQL
 Target Server Version : 80023
 File Encoding         : 65001

 Date: 01/05/2021 22:21:52
*/

SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for user
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user`  (
  `id` int(0) NOT NULL,
  `username` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL,
  `password` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL,
  `name` varchar(255) CHARACTER SET utf8 COLLATE utf8_general_ci DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci ROW_FORMAT = Dynamic;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES (1, 'zhangsan', '123', '张三');
INSERT INTO `user` VALUES (2, 'lisi', '123', '李四');

SET FOREIGN_KEY_CHECKS = 1;

```
> 创建实体Bean  

```java
package com.doubao.learn.helloworld.domain;

public class User {
    private Integer id;
    private String username;
    private String password;
    private String name;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", name='" + name + '\'' +
                '}';
    }
}

```  

> 创建mapper映射关系  

```  
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.doubao.learn.helloworld.mapper.UserMapper">
    <select id="queryUserList" resultType="user">
        select * from user
    </select>
</mapper>
```  

> 创建接口  

```java
package com.doubao.learn.helloworld.mapper;

import com.doubao.learn.helloworld.domain.User;

import java.util.List;

public interface UserMapper {
    public List<User> queryUserList();
}

```  

> 配置Mybatis信息(让Mybaits和Spring boot产生关联)  

```shell
# 配置mybatis信息
# spring集成Mybatis环境
# pojo别名扫描包
mybatis.type-aliases-package=com.doubao.learn.helloworld.domain
mybatis.mapper-locations=classpath:mapper/*Mapper.xml
```  

