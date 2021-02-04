# Spring Boot 測試

## 簡介

### 什麼是 Spring Boot

Spring Boot 是現在 Java 語言最流行的 Spring framework 的進階精簡版，也是目前 Java 後端開發的主流選擇，多數開發者都會用其開發 RESTful API，本篇我們不會著墨在 Spring Boot 的知識以及撰寫，僅會針對測試相關的部分進行描述。

## 環境設定

### 建立一個 Spring Boot 專案

要從零開始建立一個 Spring Boot 專案其實非常簡單，Spring 框架至今已發展近 20 年，也有完善的生態系以及資源供我們使用。我們先前往以下網站，就可以建立一個簡單的 Spring Boot 專案：[https://start.spring.io/](https://start.spring.io/) 

![](../.gitbook/assets/jie-tu-20210204-xia-wu-4.50.29.png)

進入網站後我們一樣選擇 Maven Project，按照之前建立 Maven Project 的方式，撰寫 GroupId 和 ArtifactId，Spring Initializr 會幫我們按照上面的配置建立 Maven 專案。因為我們要做的是一個 RESTful API 的 Spring Boot 專案，所以在 Add Dependencies 上記得要加上 Spring Web。

### 專案載入 eclipse

配置完成後，點擊 Generate 把專案下載下來並放到指定的路徑底下，再透過以下順序 import 進 eclipse：

File -&gt; Import

![](../.gitbook/assets/jie-tu-20210204-xia-wu-4.59.36.png)

Maven -&gt; Existing Maven Projects

![](../.gitbook/assets/jie-tu-20210204-xia-wu-5.00.03.png)

選擇我們剛剛建立的專案存放的位子

![](../.gitbook/assets/jie-tu-20210204-xia-wu-5.00.59.png)

打開專案可以看到由 Spring Initialzr 幫我們自動生成的 pom.xml：

```markup
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.4.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.java.unitTest</groupId>
	<artifactId>SpringBootTest</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>SpringBootTest</name>
	<description>Demo project for Spring Boot Testing</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```


