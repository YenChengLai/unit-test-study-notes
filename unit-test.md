# 單元測試

### 什麼是單元測試

單元測試 \( Unit test \)，簡單來說就是在一支程式完成時，對於程式的正確度進行驗證，根據語言的特性，大致可以分成三種型態：

* 測試功能 \( Testing of functions \)
  * 以函式為主進行測試，主要適用於 functional programming 的語言
  * e.g. scala、Haskell
* 測試流程 \( Testing of procedures \)
  * 以流程為主進行測試，主要適用於 procedural programming 的語言
  * e.g. git、visual basic、C
* 測試類別 \( Testing of classes \)
  * 以類別 \(物件\) 為主進行測試，主要適用於 Object-Oriented programming 的語言
  * e.g. Java、C\#、C++

事實上，「單元」就是針對 _**該語言的最小構成物件**_；單元測試就是對這個構成的物件進行測試，接下來的篇幅會以物件導向語言為主，所以著重於「測試類別」。

### 單元測試的要求

以物件導向語言來說，我們可以簡單列出以下要求：

1. 在不該發生錯誤的時候不拋錯 \( 正確性 \)
2. 能夠應對不同的 input 有合理的回應 \( 彈性 \)
3. 不會造成整個 application 異常或 shut down \( 穩定性 \)

## 單元測試的架構

單元測試的邏輯架構上，主流可以區分為兩種：

### AAA

1. Arrange：初始化
   * 設定單元測試所需要的前提
   * e.g. 設定物件狀態、相依套件、建立mock物件等等
2. Act：執行
   * 測試的目標，並取得實際結果
   * 執行時應僅針對一個方法，否則易模糊焦點
3. Assert：驗證結果
   * 測試的結果是否如同需求所預期
   * e.g. 1 + 1 是不是真的 = 2

### Given, When, Then

1. Given：設定執行前狀態 \( pre-condition \)
   * 單元測試前所需的前置準備、前提
2. When：執行行為
   * 執行該行為並取得結果
3. Then：驗證行為執行後狀態 \( post-condition \)
   * 對應結果是否同設定的目標

### 舉例說明

我們使用_**使用者登入**_的情境來作為說明：

**AAA**

* UserDAO 裡設定狀態為已登入 ****\( Arrange \)
* UserService 使用 UserDAO 執行下單 \( Act \)
* 下單是否成功 \( Assert \)

**Given, When, Then**

* User 已登入 \( Given \)
* User 登出系統 \( When \)
* User 無法下單 \( Then \)

仔細比對以上兩種框架，不難發現兩者其實都是在講同一件事，只是針對的面向不同，一個是針對測試本身，一個是針對操作行為。

其實，這就是時常談論到的 _**TDD**_ \( Test-Driven Design / Development 測試驅動設計 / 開發 \) 和 _**BDD**_ \( Behavior-Driven Design / Development 行為驅動設計 / 開發 \) 之間看待事情的切入點；說到底，當行為限縮在最小單位的單元測試時，不論是 TDD 還是 BDD ，都是執行一樣的行為的。

