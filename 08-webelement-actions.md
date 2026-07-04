# 🎬 PHẦN 8: WEBELEMENT ACTIONS

> **Mục tiêu**: Thành thạo tất cả WebElement methods để thao tác với elements (click, type, get text...).

---

## 📑 MỤC LỤC

1. [WebElement Interface](#webelement-interface)
2. [Input Actions](#input-actions)
3. [Retrieve Information](#retrieve-information)
4. [Element State](#element-state)
5. [Element Properties](#element-properties)
6. [Common Scenarios](#common-scenarios)

---

## 🎯 WebElement Interface

> **WebElement** = Object đại diện cho 1 HTML element trên page

### Get WebElement

```java
// findElement - Return 1 WebElement
WebElement emailField = driver.findElement(By.id("email"));

// findElements - Return List<WebElement>
List<WebElement> links = driver.findElements(By.tagName("a"));
```

---

## ⌨️ Input Actions

### 1. sendKeys() - Nhập text

```java
WebElement emailField = driver.findElement(By.id("email"));
emailField.sendKeys("test@example.com");

WebElement passwordField = driver.findElement(By.id("password"));
passwordField.sendKeys("Test@123");
```

**Special keys**:
```java
// Enter key
emailField.sendKeys(Keys.ENTER);

// Tab
emailField.sendKeys(Keys.TAB);

// Combine
emailField.sendKeys("test@example.com" + Keys.ENTER);
```

---

### 2. click() - Click element

```java
WebElement loginButton = driver.findElement(By.id("loginBtn"));
loginButton.click();

WebElement checkbox = driver.findElement(By.id("agree"));
checkbox.click(); // Check/Uncheck
```

---

### 3. clear() - Xóa text

```java
WebElement searchBox = driver.findElement(By.name("q"));
searchBox.clear(); // Xóa text hiện tại
searchBox.sendKeys("Selenium"); // Nhập text mới
```

---

### 4. submit() - Submit form

```java
WebElement form = driver.findElement(By.tagName("form"));
form.submit(); // Submit form

// Or
WebElement submitButton = driver.findElement(By.id("submit"));
submitButton.submit(); // Submit via button
```

---

## 📊 Retrieve Information

### 1. getText() - Lấy text hiển thị

```java
WebElement heading = driver.findElement(By.tagName("h1"));
String headingText = heading.getText();
System.out.println("Heading: " + headingText);

WebElement message = driver.findElement(By.id("success-msg"));
String msg = message.getText();
System.out.println("Message: " + msg);
```

---

### 2. getAttribute() - Lấy attribute value

```java
WebElement link = driver.findElement(By.linkText("Login"));
String href = link.getAttribute("href");
String target = link.getAttribute("target");

WebElement input = driver.findElement(By.id("email"));
String type = input.getAttribute("type"); // "text"
String name = input.getAttribute("name"); // "user_email"
String value = input.getAttribute("value"); // Current value
```

---

### 3. getCssValue() - Lấy CSS property

```java
WebElement element = driver.findElement(By.id("error-msg"));
String color = element.getCssValue("color");
String fontSize = element.getCssValue("font-size");
String display = element.getCssValue("display");
```

---

### 4. getTagName() - Lấy tag name

```java
WebElement element = driver.findElement(By.id("submit"));
String tag = element.getTagName(); // "button" or "input"
```

---

## ✅ Element State

### 1. isDisplayed() - Element có hiển thị không?

```java
WebElement element = driver.findElement(By.id("error-msg"));

if (element.isDisplayed()) {
    System.out.println("Element is visible");
} else {
    System.out.println("Element is hidden");
}
```

---

### 2. isEnabled() - Element có enabled không?

```java
WebElement submitBtn = driver.findElement(By.id("submit"));

if (submitBtn.isEnabled()) {
    submitBtn.click();
} else {
    System.out.println("Button is disabled");
}
```

---

### 3. isSelected() - Element có selected không? (checkbox/radio)

```java
WebElement checkbox = driver.findElement(By.id("agree"));

if (checkbox.isSelected()) {
    System.out.println("Checkbox is checked");
} else {
    checkbox.click(); // Check it
}
```

---

## 📐 Element Properties

### 1. getSize() - Lấy kích thước

```java
WebElement element = driver.findElement(By.id("banner"));
Dimension size = element.getSize();

System.out.println("Width: " + size.getWidth());
System.out.println("Height: " + size.getHeight());
```

---

### 2. getLocation() - Lấy vị trí

```java
WebElement element = driver.findElement(By.id("logo"));
Point location = element.getLocation();

System.out.println("X: " + location.getX());
System.out.println("Y: " + location.getY());
```

---

### 3. getRect() - Lấy size + location

```java
WebElement element = driver.findElement(By.id("banner"));
Rectangle rect = element.getRect();

System.out.println("X: " + rect.getX());
System.out.println("Y: " + rect.getY());
System.out.println("Width: " + rect.getWidth());
System.out.println("Height: " + rect.getHeight());
```

---

## 💼 Common Scenarios

### Scenario 1: Login

```java
// Find elements
WebElement emailField = driver.findElement(By.id("email"));
WebElement passwordField = driver.findElement(By.id("password"));
WebElement loginButton = driver.findElement(By.id("loginBtn"));

// Perform actions
emailField.sendKeys("test@example.com");
passwordField.sendKeys("Test@123");
loginButton.click();

// Verify
WebElement welcomeMsg = driver.findElement(By.id("welcome"));
System.out.println("Welcome message: " + welcomeMsg.getText());
```

---

### Scenario 2: Verify Element State

```java
// Check if element is displayed
WebElement errorMsg = driver.findElement(By.id("error"));
if (errorMsg.isDisplayed()) {
    System.out.println("Error: " + errorMsg.getText());
}

// Check if button is enabled
WebElement submitBtn = driver.findElement(By.id("submit"));
if (submitBtn.isEnabled()) {
    submitBtn.click();
}

// Check if checkbox is selected
WebElement checkbox = driver.findElement(By.id("agree"));
if (!checkbox.isSelected()) {
    checkbox.click();
}
```

---

## ✅ TÓM TẮT BÀI HỌC

📌 **Input**: sendKeys(), click(), clear(), submit()  
📌 **Get Info**: getText(), getAttribute(), getCssValue()  
📌 **State**: isDisplayed(), isEnabled(), isSelected()  
📌 **Properties**: getSize(), getLocation(), getTagName()  

---

[← Bài trước: XPath](07-xpath-deep-dive.md) | [Bài tiếp: Waits & Synchronization →](09-waits-synchronization.md)
