# JUnit 撰寫測試

## 路徑架構

我們在上一篇 JUnit 簡介與環境設定中提到，src/main 和 src/test 兩個路徑的分類，不管是 main 還是 test，我們都會在 java folder 中撰寫程式碼，在 resource 中置放我們要引用的 jar，測試程式和主程式兩者是區隔開來的，但我們不需要去處理 resource 的部分，畢竟交給 maven處理就可以了。

沿用上一篇的專案，我們在 src/main/java 和 src/test/java 底下都建立相同的 package，並且分別建立 Calculator 和 CalculatorTest 兩支 class，前者是專案中真正運行的程式，後者則是我們要撰寫的單元測試。

![](../.gitbook/assets/jie-tu-20210115-shang-wu-10.36.50.png)

## **撰寫 JUnit**

**Calculator**：

```java
package com.java.unitTest;

public class Calculator {

	public int add(int a, int b) {
		return a + b;
	}

}
```

**CalculatorTest**：

```java
package com.java.unitTest;

public class CalculatorTest {

	public void testAdd() {}
	
}
```

假設我們的目的是要測試 Calculartor 中的 add 方法，那我們就會在 CalculatorTest 之中撰寫 testAdd 方法，這是我們在撰寫單元測試上的命名慣例。

> 在 JUnit 4 版本前，由於是採用名稱對應的方式映射測試方法，所以每一個方法的名稱都要以 test 開頭，但 JUnit 4 版本後我們可以另外去註冊測試方式，這部分之後的篇幅會在提及。

設定到這一步，有基礎 Java 概念的各位都知道，不論是 Calculator 或 CalculatorTest 兩個類別都無法執行，Calculator 是專案中的實際程式，有可能只是作為模組本來就不該獨立執行，但測試類別必須能夠執行。

> 一個 Java 程式要能夠獨立執行，一定需要一個程式入口，也就是我們熟知的 main 方法

### 在 Eclipse 中執行 JUnit

為了要在 Eclipse 中執行 JUnit 5，我們需要先引入一些環境需要的 jar 檔，所以又要再一次調整我們的`pom.xml`：

```markup
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.java.unitTest</groupId>
	<artifactId>JUnit</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>${maven.compiler.source}</maven.compiler.target>
		<junit.jupiter.version>5.7.0</junit.jupiter.version>
		<junit.platform.version>1.7.0</junit.platform.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.junit.jupiter</groupId>
			<artifactId>junit-jupiter-engine</artifactId>
			<version>${junit.jupiter.version}</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.junit.platform</groupId>
			<artifactId>junit-platform-runner</artifactId>
			<version>${junit.platform.version}</version>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.1</version>
			</plugin>
			<plugin>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>2.22.2</version>
			</plugin>
		</plugins>
	</build>
</project>
```

以下是我們做的改動：

* 加入 junit-platform-runner、junit-jupiter-engine
* 配置編譯插件 maven-compiler-plugin、maven-surefire-plugin

要進行這些改動的原因，主要是因為 JUnit 5 的關係，JUnit 5 可以分為三部分：

* JUnit Platform
  * 包含 JUnit 要執行所需要的執行環境、測試框架
  * 要 maven、gradle 和 cmd 必須要有此套件
* JUnit Jupiter
  * JUnit 的核心邏輯語法
* JUnit Vintage
  * 向下兼容 JUnit 5 以前版本 \( 非必要 \)

**JUnit 應注意：**

1. Unit test class 命名應對照待側 class，e.g.  login =&gt; loginTest
2. test class 應是 public class，不要是abstract或final \(不要做任何奇怪的舉動\)
3. package src/test/java =&gt; test  ,  production =&gt; src/test/main

