# ⏱️ PHẦN 9: WAITS & SYNCHRONIZATION

> **Mục tiêu**: Nắm vững các loại waits trong Selenium để handle timing issues - kỹ năng quan trọng để tránh flaky tests.

---

## 📑 MỤC LỤC

1. [Tại sao cần Waits?](#tại-sao-cần-waits)
2. [Thread.sleep() - BAD Practice](#threadsleep---bad-practice)
3. [Implicit Wait](#implicit-wait)
4. [Explicit Wait](#explicit-wait)
5. [Fluent Wait](#fluent-wait)
6. [ExpectedConditions](#expectedconditions)
7. [Custom Wait Conditions](#custom-wait-conditions)
8. [Best Practices](#best-practices)

---

## 🎯 Tại sao cần Waits?

### Problem: Timing Issues

```java
// ❌ Code này sẽ FAIL
driver.get("https://example.com");
driver.findElement(By.id("email")).sendKeys("test@example.com"); // NoSuchElementException!
// Vì page chưa load xong, element chưa có
```

### Web page loading

```
Browser: GET request → Server
Server: Processing... (2-3 seconds)
Server: Return HTML
Browser: Parse HTML
Browser: Download CSS, JS, images...
JavaScript: Manipulate DOM (AJAX calls...)
Page: Fully loaded ✅

→ Element xuất hiện sau vài giây!
```

---

## ❌ Thread.sleep() - BAD Practice

```java
// ❌ KHÔNG BAO GIỜ dùng Thread.sleep()!
driver.get("https://example.com");
Thread.sleep(5000); // Wait 5 seconds
driver.findElement(By.id("email")).sendKeys("test@example.com");
```

**Vấn đề**:
- ❌ **Fixed time**: Luôn đợi 5s, dù element xuất hiện sau 1s
- ❌ **Waste time**: Nếu page load nhanh, vẫn đợi đủ 5s
- ❌ **Still fail**: Nếu page load chậm hơn 5s, vẫn fail
- ❌ **Not dynamic**: Không adapt với network speed

---

## ⏲️ Implicit Wait

> **Implicit Wait** = Đợi toàn cục cho TẤT CẢ findElement() calls

### Setup

```java
// Set implicit wait 10 seconds
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
```

### How it works

```java
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

// Mỗi findElement() sẽ tự động retry trong 10 seconds
driver.findElement(By.id("email")); // Đợi max 10s
driver.findElement(By.id("password")); // Đợi max 10s
```

**Ưu điểm**:
- ✅ Set một lần, apply cho tất cả
- ✅ Đơn giản, dễ dùng

**Nhược điểm**:
- ❌ Global, không flexible
- ❌ Có thể conflict với Explicit Wait
- ❌ Không custom được conditions

---

## ⏳ Explicit Wait

> **Explicit Wait** = Đợi cho một condition cụ thể

### WebDriverWait

```java
// Tạo WebDriverWait với timeout 10s
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// Đợi element visible
WebElement email = wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("email")));
email.sendKeys("test@example.com");
```

### Example: Wait for clickable

```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// Đợi button clickable rồi mới click
WebElement loginBtn = wait.until(ExpectedConditions.elementToBeClickable(By.id("loginBtn")));
loginBtn.click();
```

**Ưu điểm**:
- ✅ Flexible, custom cho từng case
- ✅ Nhiều conditions (visible, clickable, present...)
- ✅ Chỉ đợi khi cần

---

## 🔄 Fluent Wait

> **Fluent Wait** = Explicit Wait + Polling interval + Ignore exceptions

```java
Wait<WebDriver> wait = new FluentWait<>(driver)
    .withTimeout(Duration.ofSeconds(30))        // Max timeout: 30s
    .pollingEvery(Duration.ofMillis(500))       // Check every 500ms
    .ignoring(NoSuchElementException.class);    // Ignore exception

WebElement element = wait.until(driver -> {
    return driver.findElement(By.id("dynamic-element"));
});
```

**Use case**:
- ✅ AJAX elements
- ✅ Dynamic content
- ✅ Custom polling

---

## 🎯 ExpectedConditions

### Common Conditions

```java
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

// 1. visibilityOfElementLocated - Element visible
wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("email")));

// 2. elementToBeClickable - Element clickable
wait.until(ExpectedConditions.elementToBeClickable(By.id("loginBtn")));

// 3. presenceOfElementLocated - Element present in DOM
wait.until(ExpectedConditions.presenceOfElementLocated(By.id("hidden")));

// 4. invisibilityOfElementLocated - Element not visible
wait.until(ExpectedConditions.invisibilityOfElementLocated(By.id("loader")));

// 5. titleContains - Page title contains
wait.until(ExpectedConditions.titleContains("Dashboard"));

// 6. textToBePresentInElementLocated - Text present
wait.until(ExpectedConditions.textToBePresentInElementLocated(
    By.id("message"), "Success"
));
```

---

## ✅ Best Practices

### 1. Prefer Explicit over Implicit

```java
// ❌ BAD - Implicit
driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));

// ✅ GOOD - Explicit
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("email")));
```

### 2. Wait for right condition

```java
// ❌ BAD - Chỉ đợi present (có thể chưa visible)
wait.until(ExpectedConditions.presenceOfElementLocated(By.id("btn")));

// ✅ GOOD - Đợi clickable
wait.until(ExpectedConditions.elementToBeClickable(By.id("btn")));
```

### 3. Create reusable wait methods

```java
public class WaitUtil {
    
    public static void waitForVisible(WebDriver driver, By locator, int seconds) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(seconds));
        wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
    }
    
    public static void waitForClickable(WebDriver driver, By locator, int seconds) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(seconds));
        wait.until(ExpectedConditions.elementToBeClickable(locator));
    }
}

// Usage
WaitUtil.waitForClickable(driver, By.id("loginBtn"), 10);
```

---

## ✅ TÓM TẮT BÀI HỌC

📌 **Thread.sleep()** = ❌ Never use!  
📌 **Implicit Wait** = Global, đơn giản nhưng ít flexible  
📌 **Explicit Wait** = ✅ Recommend, flexible, custom conditions  
📌 **Fluent Wait** = Explicit + polling + ignore exceptions  
📌 **ExpectedConditions** = visibilityOf, clickable, textToBe...  

---

[← Bài trước: WebElement Actions](08-webelement-actions.md) | [Bài tiếp: Handling Complex UI →](10-handling-complex-ui.md)
