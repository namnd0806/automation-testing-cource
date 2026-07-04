# 🪟 PHẦN 11: ALERTS, FRAMES & WINDOWS

> **Mục tiêu**: Xử lý JavaScript Alerts, iFrames và Multiple Windows/Tabs trong Selenium.

## 📑 MỤC LỤC

1. [JavaScript Alerts](#javascript-alerts)
2. [Frames (iFrames)](#frames-iframes)
3. [Multiple Windows](#multiple-windows)

## 🚨 JavaScript Alerts

> **Alert** = Pop-up window từ JavaScript (không phải HTML element)

### Các loại Alerts

1. **Simple Alert** - Chỉ có OK button
2. **Confirmation Alert** - OK và Cancel buttons  
3. **Prompt Alert** - Input field + OK/Cancel

### Handle Alert

```java
// Đợi alert xuất hiện
WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
wait.until(ExpectedConditions.alertIsPresent());

// Switch to alert
Alert alert = driver.switchTo().alert();

// Lấy text
String alertText = alert.getText();
System.out.println("Alert message: " + alertText);

// Accept (click OK)
alert.accept();

// Dismiss (click Cancel) - cho Confirmation alert
alert.dismiss();

// Nhập text (cho Prompt alert)
alert.sendKeys("Test input");
alert.accept();
```

### Example

```java
@Test
public void testAlert() {
    driver.get("https://example.com");
    
    // Click button để show alert
    driver.findElement(By.id("alertBtn")).click();
    
    // Handle alert
    Alert alert = driver.switchTo().alert();
    System.out.println("Alert text: " + alert.getText());
    alert.accept();
}
```

## 🖼️ Frames (iFrames)

> **Frame** = Trang HTML nhúng trong trang HTML khác (như hình trong hình)

### Switch to Frame

```java
// Cách 1: By index (vị trí)
driver.switchTo().frame(0); // Frame đầu tiên

// Cách 2: By name or ID
driver.switchTo().frame("frameName");
driver.switchTo().frame("frameId");

// Cách 3: By WebElement
WebElement frameElement = driver.findElement(By.id("myFrame"));
driver.switchTo().frame(frameElement);
```

### Switch Back

```java
// Quay về parent frame
driver.switchTo().parentFrame();

// Quay về main document (ra khỏi tất cả frames)
driver.switchTo().defaultContent();
```

### Example: Nested Frames

```java
// Frame trong frame
driver.switchTo().frame("outerFrame");
driver.switchTo().frame("innerFrame");

// Thao tác trong innerFrame
driver.findElement(By.id("element")).click();

// Quay về outerFrame
driver.switchTo().parentFrame();

// Quay về main page
driver.switchTo().defaultContent();
```

## 🪟 Multiple Windows

> **Multiple Windows** = Xử lý nhiều tabs/windows cùng lúc

### Get Window Handles

```java
// Lấy handle của window hiện tại
String mainWindow = driver.getWindowHandle();

// Lấy tất cả window handles
Set<String> allWindows = driver.getWindowHandles();
```

### Switch Between Windows

```java
// Click link mở tab mới
driver.findElement(By.linkText("Open New Tab")).click();

// Get all windows
Set<String> windows = driver.getWindowHandles();

// Switch to new window
for (String window : windows) {
    if (!window.equals(mainWindow)) {
        driver.switchTo().window(window);
        break;
    }
}

// Làm việc trong window mới
System.out.println("New window title: " + driver.getTitle());

// Close window hiện tại
driver.close();

// Switch back to main window
driver.switchTo().window(mainWindow);
```

### Complete Example

```java
@Test
public void testMultipleWindows() {
    driver.get("https://example.com");
    
    // Store main window
    String mainWindow = driver.getWindowHandle();
    
    // Click link mở window mới
    driver.findElement(By.id("newWindowLink")).click();
    
    // Wait for new window
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
    wait.until(ExpectedConditions.numberOfWindowsToBe(2));
    
    // Switch to new window
    Set<String> windows = driver.getWindowHandles();
    for (String window : windows) {
        if (!window.equals(mainWindow)) {
            driver.switchTo().window(window);
        }
    }
    
    // Verify new window
    Assert.assertTrue(driver.getTitle().contains("New Window"));
    
    // Close new window
    driver.close();
    
    // Switch back
    driver.switchTo().window(mainWindow);
}
```

## ✅ TÓM TẮT

📌 **Alert**: switchTo().alert(), accept(), dismiss(), sendKeys()  
📌 **Frame**: switchTo().frame(), parentFrame(), defaultContent()  
📌 **Windows**: getWindowHandles(), switchTo().window()

[← Bài trước: Complex UI](10-handling-complex-ui.md) | [Bài tiếp: Actions Class →](12-actions-class.md)
