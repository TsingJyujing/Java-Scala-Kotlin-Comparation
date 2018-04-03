# 如何使用三种语言开始你的SpringBoot项目

## 简介
得益于Kotlin和Scala的动态、函数式的特性，开发网络接口可以迅速完成，但是要用Kotlin或者Scala开发SpringBoot还有一些小坑需要踩过去，下面简述一下。

## Java
官方支持的语言还需要我说什么吗？

自己看：[Quick Start](https://projects.spring.io/spring-boot/#quick-start)

## Scala

首先设置好你的Maven（这里以 Spring Boot 1.59 和 Scala 2.10.7为例）

```xml
<properties>
    <scala.version.main>2.10</scala.version.main>
   <scala.version.sub>7</scala.version.sub>
</properties>

<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>

<dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.scala-lang</groupId>
            <artifactId>scala-library</artifactId>
            <version>${scala.version.main}.${scala.version.sub}</version>
        </dependency>

        <dependency>
            <groupId>org.scalatest</groupId>
            <artifactId>scalatest_${scala.version.main}</artifactId>
            <version>3.0.4</version>
            <scope>test</scope>
        </dependency>

   </dependencies>

   <!-- Maven如何编译Scala的略，这个自己去搞，谁知道你要编译成什么样子啊-->
```

随后设置一个入口方法：

```scala
package 你要啥包就啥包;

import java.util.TimeZone
import org.springframework.boot.SpringApplication
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.builder.SpringApplicationBuilder
import org.springframework.boot.web.support.SpringBootServletInitializer

/**
  * @author tsingjyujing@163.com
  * Run this class to start spring boot application manually
  */
@SpringBootApplication
object Entry extends SpringBootServletInitializer {

    TimeZone.setDefault(TimeZone.getTimeZone("Asia/Shanghai"))

    /**
      * Configure service class
      *
      * @param builder spring application builder
      * @return
      *
      * @see org.springframework.boot.web.support.SpringBootServletInitializer#configure
      */
    override def configure(builder: SpringApplicationBuilder): SpringApplicationBuilder = {
        super.configure(builder)
        builder.sources(classOf[Service])
    }

    /**
      * Run
      * @param args
      */
    def main(args: Array[String]): Unit = {
        SpringApplication.run(classOf[Service], args: _*)
    }
}
```

直接执行就好，也可以作为Spring Boot Application启动。

随后需要编写你的Service类：
```scala
package 你要啥包就啥包;

import java.text.SimpleDateFormat
import org.springframework.boot.autoconfigure.EnableAutoConfiguration
import org.springframework.http.MediaType
import org.springframework.stereotype.Controller
import org.springframework.web.bind.annotation.{RequestMapping, RequestMethod, RequestParam, ResponseBody}

import scala.collection.JavaConverters._

/**
  * Spring Boot 服务类，对外提供接口服务
  *
  * @author tsingjyujing@163.com
  */
@Controller
@EnableAutoConfiguration
class Service {

    val dateParser = new SimpleDateFormat("yyy-MM-dd hh:mm:ss")

    @RequestMapping(
        value = Array("/query/staypoint"),
        method = Array(
            RequestMethod.POST,
            RequestMethod.GET
        ),
        produces = Array(MediaType.APPLICATION_JSON_VALUE)
    )
    @ResponseBody
    private def queryStayPointStatically(
                                            @RequestParam(name = "vehicles") vehicles: String,
                                            @RequestParam(name = "startTime") startTime: String,
                                            @RequestParam(name = "endTime") endTime: String,
                                            @RequestParam(name = "n", required = false, defaultValue = "10") clusterCount: String,
                                            @RequestParam(name = "withPolygon", required = false, defaultValue = "no") withPolygon: String,
                                            @RequestParam(name = "withPoints", required = false, defaultValue = "no") withPoints: String
                                        ): String = {
        //你自己的代码
    }
}
```

需要注意的是：`@RequestMapping`的value必需使用Array生成数组，不能像Java一样只写一个字符串，method之类同理。

## Kotlin
说实话，写Spring Boot的话Kotlin更加实用一点，但是有一点小坑需要注意。

而且IDEA编辑Kotlin代码的时候自动补全经常不会自动跳出来，而且有一点迷之卡顿……

首先洗干净你的小手，准备好POM文件，这里采用Spring Boot 2.0 + Kotlin 1.2.10为例

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.0.RELEASE</version>
</parent>

<properties>
    <kotlin.version>1.2.10</kotlin.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.jetbrains.kotlin</groupId>
        <artifactId>kotlin-stdlib-jre8</artifactId>
        <version>${kotlin.version}</version>
    </dependency>

    <dependency>
        <groupId>org.jetbrains.kotlin</groupId>
        <artifactId>kotlin-reflect</artifactId>
        <version>${kotlin.version}</version>
    </dependency>

    <dependency>
        <groupId>com.fasterxml.jackson.module</groupId>
        <artifactId>jackson-module-kotlin</artifactId>
        <version>2.9.4.1</version>
    </dependency>

</dependencies>

 <!-- Maven如何编译Kotlin的略，这个自己去搞，谁知道你要编译成什么样子啊-->
```

虽然略去了编译的部分，但是如果拟采用JAR包的方式的话，入口函数要注意加上Kt（一会儿会说明）

编写你的入口文件：
```kotlin
package 你要什么包？

import org.springframework.boot.SpringApplication
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.builder.SpringApplicationBuilder
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer
import java.util.*


fun main(args: Array<String>) {
    Entry.setTimeZoneAsChina()
    SpringApplication.run(
            Service::class.java, *args
    )
}

@SpringBootApplication
open class Entry : SpringBootServletInitializer(){

    override fun configure(application: SpringApplicationBuilder): SpringApplicationBuilder {
        setTimeZoneAsChina()
        return application.sources(Service::class.java)
    }

    companion object {

        fun setTimeZoneAsChina() {
            TimeZone.setDefault(
                    TimeZone.getTimeZone(
                            "Asia/Shanghai"
                    )
            )
        }
    }
}
```

需要注意的是，main必需写外面。其次，启动的也不是Entry类，而是EntryKt。

随后编写你的服务吧！

```kotlin
package 你要啥包？

import org.springframework.boot.autoconfigure.EnableAutoConfiguration
import org.springframework.http.MediaType
import org.springframework.stereotype.Controller
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RequestMethod
import org.springframework.web.bind.annotation.RequestParam
import org.springframework.web.bind.annotation.ResponseBody
import java.text.SimpleDateFormat

@Controller
@EnableAutoConfiguration
class Service {

    val dateFormat = SimpleDateFormat("yyyy-MM-dd hh:mm:ss")

    @RequestMapping(
            value = "/query/single_vehicle",
            method = arrayOf(
                    RequestMethod.POST,
                    RequestMethod.GET
            ),
            produces = arrayOf(MediaType.APPLICATION_JSON_VALUE)
    )
    @ResponseBody
    fun queryWholeData(
            @RequestParam(name = "vehicleId") vehicleId: String,
            @RequestParam(name = "startTime") startTime: String,
            @RequestParam(name = "endTime") endTime: String
    ): String {
        //自己写吧
    }

}
```
