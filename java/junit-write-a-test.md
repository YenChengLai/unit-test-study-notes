# JUnit 撰寫測試

### 路徑架構

我們在上一篇 JUnit 簡介與環境設定中提到，src/main 和 src/test 兩個路徑的分類，不管是 main 還是 test，我們都會在 java folder 中撰寫程式碼，在 resource 中置放我們要引用的 jar，測試程式和主程式兩者是區隔開來的，但我們不需要去處理 resource 的部分，畢竟交給 maven處理就可以了。

沿用上一篇的專案，我們在 src/main/java 和 src/test/java 底下都建立相同的 package，並且分別建立 Calculator 和 CalculatorTest 兩支 class，前者是專案中真正運行的程式，後者則是我們要撰寫的單元測試。

![](../.gitbook/assets/jie-tu-20210115-shang-wu-10.04.47.png)

### JUnit 應注意：

1. Unit test class 命名應對照待側 class，e.g.  login =&gt; loginTest
2. test class 應是 public class，不要是abstract或final \(不要做任何奇怪的舉動\)
3. package src/test/java =&gt; test  ,   production =&gt; src/test/main

