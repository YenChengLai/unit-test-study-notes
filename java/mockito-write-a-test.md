# Mockito 撰寫測試

## 撰寫測試

### 程式架構

如同 JUnit 時使用的架構，我們在

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

