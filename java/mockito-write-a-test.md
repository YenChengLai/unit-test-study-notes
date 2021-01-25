# Mockito 撰寫測試

## 撰寫測試

### 程式架構

如同 JUnit 時使用的架構，我們在 src/test/main 的路徑底下建立相對應的測試程式：

```text
src/main/java
  |-- com.java.unitTest.controller
      |-- LoginController.java
  |-- com.java.unitTest.dto
      |-- User.java
  |-- com.java.unitTest.repository
      |-- UserRepository.java
  |-- com.java.unitTest.service
      |-- AuthenticationService.java
src/test/java
  |-- com.java.unitTest.controller
      |-- LoginContollerTest.java
```

### 撰寫測試

因為是登入模組，我們以 LoginController.java 作為 SUT \( 主要測試對象 \)，並運用 Mockito 提供的套件來撰寫測試程式 LoginControllerTest.java：

```java
package com.java.unitTest.controller;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.Mockito;

import com.java.unitTest.service.AuthenticationService;

public class LoginControllerTest {

	private LoginController loginCtrl; // SUT : System Under Test

	private AuthenticationService authenticationSvc;

	@BeforeEach
	public void setUp() {
		authenticationSvc = Mockito.mock(AuthenticationService.class);
		loginCtrl = new LoginController(authenticationSvc);
	}

	@Test
	public void expectedToGetHome() {
		// arrange
		Mockito.when(authenticationSvc.authenticate(Mockito.anyString(), Mockito.anyString())).thenReturn(true);

		// act
		String viewPath = loginCtrl.service("visitor", "visit123");
		
		// assert
		Assertions.assertNotNull(viewPath);
		Assertions.assertEquals("/home", viewPath);
	}
}

```

以下說明幾個觀點與語法：

* LoginController 是 SUT，我們希望他盡可能得不要受到其他的外部物件影響狀態，造成測試的結果不穩定，所以 AuthenticationService 設計成 mock object 來固定他的結果。
* Mockito.mock\(AuthenticationService.class\)：
  * 回傳一個 mock 的 AuthenticationService 物件，擁有真實的 AuthenticationService 物件有的一切接口
* Mockito.when\(authenticationSvc.authenticate\(Mockito.anyString\(\), Mockito.anyString\(\)\)\)：
  * 註冊這個 mock object 在使用任何字串傳入 authenticate 方法時，應觸發後續事件
  * Mockito.anyString\(\) 代表傳入任何字串參數都會觸發，當然也可以是明確的傳入參數，e.g. authenticationSvc.authenticate\("tom", "tom123"\)，這樣只有傳入 tom 和 tom123 時才會觸發
  * 也可以設定為整個物件，但多數情境都是針對方法註冊
* Mockito.thenReturn\(true\)：
  * 搭配 when 使用，如果 when 中註冊的是方法，這裡的回傳值只能是方法的回傳型別，e.g. 因為 authenticate 方法的回傳值是 boolean，這裡的 thenReturn 只能註冊 true 或 false
  * 簡單來說就是支援範型效果，根據 when 註冊的型別做對應

以下簡單整理一張表：

<table>
  <thead>
    <tr>
      <th style="text-align:center">&#x65B9;&#x6CD5;&#x540D;&#x7A31;</th>
      <th style="text-align:center">mock()</th>
      <th style="text-align:center">when()</th>
      <th style="text-align:center">thenReturn()</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">&#x7528;&#x9014;</td>
      <td style="text-align:center">&#x5EFA;&#x7ACB; mock object</td>
      <td style="text-align:center">&#x8A3B;&#x518A; mock object &#x7684;&#x60C5;&#x5883;</td>
      <td style="text-align:center">&#x8A2D;&#x5B9A; mock object &#x60C5;&#x5883;&#x7684;&#x56DE;&#x50B3;&#x503C;</td>
    </tr>
    <tr>
      <td style="text-align:center">&#x50B3;&#x5165;&#x53C3;&#x6578;</td>
      <td style="text-align:center">class</td>
      <td style="text-align:center">&#x53EF;&#x4EE5;&#x662F;&#x7269;&#x4EF6;&#x6216;&#x65B9;&#x6CD5;</td>
      <td
      style="text-align:center">&#x8996; when &#x7684;&#x50B3;&#x5165;&#x53C3;&#x6578;&#x6C7A;&#x5B9A;</td>
    </tr>
    <tr>
      <td style="text-align:center">&#x56DE;&#x50B3;&#x503C;</td>
      <td style="text-align:center">mock &#x7269;&#x4EF6;</td>
      <td style="text-align:center">OngoingStubbing &#x7269;&#x4EF6;</td>
      <td style="text-align:center">
        <p>when &#x50B3;&#x5165;&#x7269;&#x4EF6; =&gt; &#x7269;&#x4EF6;</p>
        <p>when &#x50B3;&#x5165;&#x65B9;&#x6CD5; =&gt; &#x65B9;&#x6CD5;&#x56DE;&#x50B3;&#x503C;</p>
      </td>
    </tr>
  </tbody>
</table>

### 進階 - 使用 Annotation

為了更順利的說明接下來的概念，我們先增加一隻 UserLookupService.java，程式路徑至於 com.java.unitTest.service 的 package 下，並且修改和其相關聯的 User.java、UserRepository.java：

User.java

```java
package com.java.unitTest.dto;

public class User {

	public enum UserType {
		REGULAR_USER, ADMIN_USER
	};

	private String username;
	private String password;
	private boolean live = true;
	private final UserType userType;

	User(String username, String password, UserType userType) {
		this.username = username;
		this.password = password;
		this.userType = userType;
	}

	public static User createRegularUser(String username, String password) {
		return new User(username, password, UserType.REGULAR_USER);
	}

	public static User createAdminUser(String username, String password) {
		return new User(username, password, UserType.ADMIN_USER);
	}

	public String getUsername() {
		return username;
	}

	public String getPassword() {
		return password;
	}

	public boolean isLive() {
		return live;
	}

	public UserType getUserType() {
		return userType;
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + (live ? 1231 : 1237);
		result = prime * result + ((password == null) ? 0 : password.hashCode());
		result = prime * result + ((userType == null) ? 0 : userType.hashCode());
		result = prime * result + ((username == null) ? 0 : username.hashCode());
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		User other = (User) obj;
		if (live != other.live)
			return false;
		if (password == null) {
			if (other.password != null)
				return false;
		} else if (!password.equals(other.password))
			return false;
		if (userType != other.userType)
			return false;
		if (username == null) {
			if (other.username != null)
				return false;
		} else if (!username.equals(other.username))
			return false;
		return true;
	}

}
```

UserRepository.java

```java
package com.java.unitTest.repository;

import java.util.HashMap;
import java.util.Map;

import com.java.unitTest.dto.User;

public class UserRepository {

	private Map<String, User> users = new HashMap<>();

	public UserRepository() {
		// regular users
		users.put("john", User.createRegularUser("John", "john123"));
		users.put("jack", User.createRegularUser("Jack", "jack123"));
		users.put("peter", User.createRegularUser("Peter", "peter123"));
		
		// admin users
		users.put("frank", User.createAdminUser("Frank", "frank123"));
	}

	public User findByUserName(String username) {
		return users.get(username);
	}
}
```

UserLookupService.java

```java
package com.java.unitTest.service;

import java.util.Set;
import java.util.stream.Collectors;

import com.java.unitTest.dto.User;
import com.java.unitTest.repository.UserRepository;

public class UserLookupService {

	private UserRepository userRepository;

	public Set<User> getRegularUsers() {
		return getUsersByUserType(User.UserType.REGULAR_USER);
	}

	public Set<User> getAdminUsers() {
		return getUsersByUserType(User.UserType.ADMIN_USER);
	}

	private Set<User> getUsersByUserType(User.UserType userType) {
		return userRepository.findAll().stream().filter(user -> user.isLive() && user.getUserType() == userType)
				.collect(Collectors.toSet());
	}

}
```

接下來我們寫一支 UserLookupServiceTest.java：

```java
package com.java.unitTest.service;

import java.util.LinkedList;
import java.util.List;
import java.util.Set;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.Mockito;
import org.mockito.junit.MockitoJUnitRunner;
import org.mockito.junit.jupiter.MockitoExtension;

import com.java.unitTest.dto.User;
import com.java.unitTest.repository.UserRepository;

@RunWith(MockitoJUnitRunner.class)
@ExtendWith(MockitoExtension.class)
public class UserLookupServiceTest {

	@Mock
	private UserRepository userRepository;

	@InjectMocks
	private UserLookupService userLookupSvc;

	@Test
	public void getRegularUsers() {
		// arrange
		List<User> userList = new LinkedList<>();
		userList.add(User.createRegularUser("John", "john123"));
		userList.add(User.createRegularUser("Jack", "jack123"));
		userList.add(User.createAdminUser("Joseph", "joseph123"));

		Mockito.when(userRepository.findAll()).thenReturn(userList);

		// act
		Set<User> regularUsers = userLookupSvc.getRegularUsers();

		// assert
		Assertions.assertNotNull(regularUsers);
	}

}
```

上面這段程式碼中，有幾點需要說明的事項：

* 使用 @Mock：
  * 要建立 Mockito 的 mock object 有兩種方式：
    * Mockito.mock\( 物件class \) =&gt; 我們最一開始使用的方法
    * @Mock 標記在屬性上，該屬性就會被視為 mock 物件
      * 需要在 Class 上加上 @ExtendWith\(MockitoExtension.class\) 和  @RunWith\(MockitoJUnitRunner.class\)
* 使用 @InjectMocks：
  * 這個 Annotation 的主要目的在於將我們用 @Mock 或 @Spy 的物件自動注入到這個指定的類別中
  * 剛剛的 UserLookupService.java 第 11 行有一個 UserRepository 物件，在 test 中使用 @InjectMocks 就會將我們剛剛宣告為 @Mock 的 userRepository 注入其中，在實際執行時就會使用這個物件
  * 讀者自己在嘗試時不妨將這行註解移除，試試看會得到什麼結果

### 

## 參考資源

Mockito 官方網站：[https://site.mockito.org/](https://site.mockito.org/) 

Baeldung 教學： [https://www.baeldung.com/mockito-junit-5-extension](https://www.baeldung.com/mockito-junit-5-extension) 

Spring + Mockito：[https://kucw.github.io/blog/2020/2/spring-unit-test-mockito/](https://kucw.github.io/blog/2020/2/spring-unit-test-mockito/)

