# Mockito 簡介與操作

![credit to: https://site.mockito.org/](../.gitbook/assets/mockito.png)

### 簡介

**什麼是Mockito ?**

Mockito 是現階段 Java 語言在單元測試上用以做 mock 測試最盛行的工具，主要可搭配 JUnit 對 Java code 進行測試，尤其是 Spring test 主要也是使用 Mockito 對於複雜的 Bean、Service、Component 等進行生命週期的管理。

**mock test**

我們執行單元測試，目的就是為了確認受測模組的功能是否正常，輸出結果是否同我們所預期，以及在日後對功能的修正、更改或擴充時，是否有向下相容而不影響既有功能。

mock test 的核心就是 mock object，說白了就是創造一個假的物件，而這個物件我們可以自行去設定他的內容、狀態，這麼做有幾個好處：

* **降低複雜的依賴關係**

  假設測試的是一個登入模組，模組的核心功能是 **驗證** 和 **授權**，根據`UserObject`中的內容值進行判斷，如果我們真的訪問資料庫查詢`UserObject`的內容，那不僅執行耗效能，一旦資料遭到異動測試的結果也會受影響。

* **模擬難以重現的狀態**

  假設廣告模組設定第 100 位訪問者會得到 5 折折價券，為了測第 100 次的功能而連續執行無效的 99 次測試很明顯是不切實際的，以模擬物件狀態進行單元測試才是合理的做法。

簡言之，mock test 的好處就是大量減低依賴性，以便集中針對重要模組進行測試。

### 環境配置

要使用 Mockito 第一步需要確保專案的 Java Build Path 中有必要的 dependencies，也就是專案要引入 Mockito 需要的 jar。市面上管理 jar 的主流工具有兩個：`gradle` 與 `maven`，行內環境則是以 maven 為主。

**Maven Setting**

在 `pom.xml` 中加入下列 dependency：

```markup
<dependency>
   <groupId>org.mockito</groupId>
   <artifactId>mockito-core</artifactId>
   <version>2.23.0</version>
</dependency>
<dependency>
   <groupId>org.mockito</groupId>
   <artifactId>mockito-junit-jupiter</artifactId>
   <version>2.23.0</version>
</dependency>
```

### 撰寫第一個 Mockito test

我們先從建立一隻基本的 Unit Test 開始

```java
package com.cathaybk.jUnitTest;

public class MockitoTestCase {

}
```

要使用 Mockito 建立 mock object，有兩種方式：

1. 使用靜態的 mock\(\) 方法
2. 使用 @Mock annotation

最後長相：

```java
package com.cathaybk.jUnitTest;

import static org.junit.jupiter.api.Assertions.assertEquals;

import java.util.List;

import org.junit.Rule;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.junit.runner.RunWith;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.junit.MockitoJUnit;
import org.mockito.junit.MockitoJUnitRunner;
import org.mockito.junit.MockitoRule;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
@RunWith(MockitoJUnitRunner.class)
public class MockitoTestCase {

    @Rule
    public MockitoRule initRule = MockitoJUnit.rule();

    @Mock
    List<String> mockList;

    @Test
    void whenNotUseMockAnnotation_thenCorrect() {
        mockList.add("one");
        Mockito.verify(mockList).add("one");
        assertEquals(0, mockList.size());

        Mockito.when(mockList.size()).thenReturn(100);
        assertEquals(100, mockList.size());
    }

}
```

#### 資源

Mockito 官方網站：[https://site.mockito.org/](https://site.mockito.org/) 

Baeldung 教學： [https://www.baeldung.com/mockito-junit-5-extension](https://www.baeldung.com/mockito-junit-5-extension) 

Spring + Mockito：[https://kucw.github.io/blog/2020/2/spring-unit-test-mockito/](https://kucw.github.io/blog/2020/2/spring-unit-test-mockito/)

