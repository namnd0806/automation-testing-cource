# 📸 PHẦN 14: SCREENSHOTS & BROWSER LOGS

> **Mục tiêu**: Capture screenshots khi test fail và lấy browser logs để debugging hiệu quả.

---

## 📑 MỤC LỤC

1. [Take Screenshots](#take-screenshots)
2. [Screenshot on Test Failure](#screenshot-on-test-failure)
3. [Browser Console Logs](#browser-console-logs)
4. [Network Logs](#network-logs)

---

## 📸 Take Screenshots

### Basic Screenshot

```java
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.OutputType;
import java.io.File;
import java.nio.file.Files;
import java.nio.file.StandardCopyOption;

// Cast driver to TakesScreenshot
TakesScreenshot ts = (TakesScreenshot) driver;

// Capture screenshot as File
File sourceFile = ts.getScreenshotAs(OutputType.FILE);

// Save to destination
File destFile = new File("screenshots/test.png");
Files.copy(sourceFile.toPath(), destFile.toPath(), StandardCopyOption.REPLACE_EXISTING);

System.out.println("Screenshot saved: " + destFile.getAbsolutePath());
```

---

### Screenshot với Timestamp

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class ScreenshotUtil {
    
    public static void takeScreenshot(WebDriver driver, String testName) {
        try {
            // Create timestamp
            String timestamp = LocalDateTime.now()
                .format(DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"));
            
            // Create filename
            String fileName = testName + "_" + timestamp + ".png";
            
            // Capture screenshot
            TakesScreenshot ts = (TakesScreenshot) driver;
            File source = ts.getScreenshotAs(OutputType.FILE);
            
            // Create screenshots directory if not exists
            File screenshotDir = new File("screenshots");
            if (!screenshotDir.exists()) {
                screenshotDir.mkdirs();
            }
            
            // Save file
            File destination = new File("screenshots/" + fileName);
            Files.copy(source.toPath(), destination.toPath(), 
                      StandardCopyOption.REPLACE_EXISTING);
            
            System.out.println("Screenshot saved: " + fileName);
            
        } catch (Exception e) {
            System.out.println("Failed to take screenshot: " + e.getMessage());
        }
    }
}

// Usage
ScreenshotUtil.takeScreenshot(driver, "login_test");
// Output: login_test_20241225_153045.png
```

---

## ❌ Screenshot on Test Failure

### Cách 1: Try-Catch trong Test

```java
@Test
public void testLogin() {
    try {
        driver.get("https://example.com/login");
        driver.findElement(By.id("email")).sendKeys("test@example.com");
        driver.findElement(By.id("password")).sendKeys("Test@123");
        driver.findElement(By.id("loginBtn")).click();
        
        // Verify
        Assert.assertTrue(driver.getTitle().contains("Dashboard"));
        
    } catch (Exception e) {
        // Take screenshot on failure
        ScreenshotUtil.takeScreenshot(driver, "testLogin_failed");
        throw e; // Re-throw để test vẫn fail
    }
}
```

---

### Cách 2: TestNG Listener (✅ Recommend)

```java
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener implements ITestListener {
    
    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("Test Failed: " + result.getName());
        
        // Get driver from test class
        Object testClass = result.getInstance();
        WebDriver driver = ((BaseTest) testClass).getDriver();
        
        // Take screenshot
        takeScreenshot(driver, result.getName());
    }
    
    @Override
    public void onTestSuccess(ITestResult result) {
        System.out.println("Test Passed: " + result.getName());
    }
    
    @Override
    public void onTestStart(ITestResult result) {
        System.out.println("Test Started: " + result.getName());
    }
    
    private void takeScreenshot(WebDriver driver, String testName) {
        try {
            TakesScreenshot ts = (TakesScreenshot) driver;
            File source = ts.getScreenshotAs(OutputType.FILE);
            
            String timestamp = LocalDateTime.now()
                .format(DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"));
            
            File dest = new File("screenshots/" + testName + "_" + timestamp + ".png");
            Files.copy(source.toPath(), dest.toPath(), 
                      StandardCopyOption.REPLACE_EXISTING);
            
            System.out.println("Screenshot saved: " + dest.getName());
        } catch (Exception e) {
            System.out.println("Failed to capture screenshot: " + e.getMessage());
        }
    }
}
```

---

### Enable Listener trong testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<suite name="Test Suite">
    <listeners>
        <listener class-name="com.example.listeners.TestListener"/>
    </listeners>
    
    <test name="Login Tests">
        <classes>
            <class name="com.example.tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

---

### BaseTest Class

```java
public class BaseTest {
    protected WebDriver driver;
    
    @BeforeMethod
    public void setup() {
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }
    
    @AfterMethod
    public void teardown() {
        if (driver != null) {
            driver.quit();
        }
    }
    
    // Getter cho Listener
    public WebDriver getDriver() {
        return driver;
    }
}
```

---

## 📋 Browser Console Logs

### Get Console Logs

```java
import org.openqa.selenium.logging.LogEntries;
import org.openqa.selenium.logging.LogEntry;
import org.openqa.selenium.logging.LogType;
import java.util.logging.Level;

// Get console logs
LogEntries logs = driver.manage().logs().get(LogType.BROWSER);

// Print all logs
System.out.println("=== Browser Console Logs ===");
for (LogEntry entry : logs) {
    System.out.println(
        entry.getLevel() + " " + 
        entry.getTimestamp() + " " +
        entry.getMessage()
    );
}
```

---

### Filter ERROR Logs Only

```java
LogEntries logs = driver.manage().logs().get(LogType.BROWSER);

System.out.println("=== Console Errors ===");
for (LogEntry entry : logs) {
    // Chỉ print ERROR và SEVERE
    if (entry.getLevel().equals(Level.SEVERE)) {
        System.out.println("ERROR: " + entry.getMessage());
    }
}
```

---

### Check for Console Errors in Test

```java
@Test
public void testNoConsoleErrors() {
    driver.get("https://example.com");
    
    // Perform actions
    driver.findElement(By.id("button")).click();
    
    // Check for console errors
    LogEntries logs = driver.manage().logs().get(LogType.BROWSER);
    
    List<String> errors = new ArrayList<>();
    for (LogEntry entry : logs) {
        if (entry.getLevel().equals(Level.SEVERE)) {
            errors.add(entry.getMessage());
        }
    }
    
    // Assert no errors
    Assert.assertTrue(errors.isEmpty(), 
        "Console errors found: " + errors);
}
```

---

## 🌐 Network Logs

### Capture Network Activity

```java
import org.openqa.selenium.devtools.DevTools;
import org.openqa.selenium.devtools.v120.network.Network;
import org.openqa.selenium.devtools.v120.network.model.RequestId;

ChromeDriver chromeDriver = new ChromeDriver();
DevTools devTools = chromeDriver.getDevTools();
devTools.createSession();

// Enable network tracking
devTools.send(Network.enable(Optional.empty(), Optional.empty(), Optional.empty()));

// Listen to network requests
devTools.addListener(Network.requestWillBeSent(), request -> {
    System.out.println("Request: " + request.getRequest().getUrl());
});

// Listen to network responses
devTools.addListener(Network.responseReceived(), response -> {
    System.out.println("Response: " + 
        response.getResponse().getUrl() + " - " + 
        response.getResponse().getStatus()
    );
});

// Navigate
chromeDriver.get("https://example.com");
```

---

## 💼 Complete Example

```java
public class ScreenshotDemo {
    
    @Test
    public void testWithScreenshotAndLogs() {
        WebDriver driver = new ChromeDriver();
        
        try {
            // Navigate
            driver.get("https://example.com/login");
            
            // Take screenshot before action
            ScreenshotUtil.takeScreenshot(driver, "before_login");
            
            // Perform login
            driver.findElement(By.id("email")).sendKeys("test@example.com");
            driver.findElement(By.id("password")).sendKeys("Test@123");
            driver.findElement(By.id("loginBtn")).click();
            
            // Take screenshot after action
            ScreenshotUtil.takeScreenshot(driver, "after_login");
            
            // Check console errors
            LogEntries logs = driver.manage().logs().get(LogType.BROWSER);
            for (LogEntry entry : logs) {
                if (entry.getLevel().equals(Level.SEVERE)) {
                    System.out.println("Console Error: " + entry.getMessage());
                }
            }
            
            // Verify
            Assert.assertTrue(driver.getTitle().contains("Dashboard"));
            
        } catch (Exception e) {
            // Screenshot on failure
            ScreenshotUtil.takeScreenshot(driver, "test_failed");
            throw e;
            
        } finally {
            driver.quit();
        }
    }
}
```

---

## ✅ TÓM TẮT BÀI HỌC

📌 **Screenshot**: TakesScreenshot, getScreenshotAs(OutputType.FILE)  
📌 **On Failure**: Dùng TestNG Listener để tự động screenshot  
📌 **Console Logs**: driver.manage().logs().get(LogType.BROWSER)  
📌 **Filter**: Level.SEVERE cho errors  
📌 **Network**: Dùng Chrome DevTools Protocol  

---

## 🎯 SAU KHI HỌC BUỔI NÀY

### Checklist

- [ ] Biết take screenshot cơ bản
- [ ] Biết tự động screenshot khi test fail
- [ ] Biết lấy và filter console logs
- [ ] Biết check console errors trong test

### 📝 Thực hành

**Bài 1: Screenshot Util**

Tạo utility class với methods:
```java
// 1. takeScreenshot(driver, testName)
// 2. takeScreenshotWithTimestamp(driver, testName)
// 3. takeScreenshotAsBase64(driver)
```

**Bài 2: Listener**

```java
// 1. Tạo TestListener implement ITestListener
// 2. Screenshot on failure
// 3. Print console logs on failure
// 4. Enable trong testng.xml
```

**Bài 3: Console Error Check**

```java
// 1. Navigate to a page
// 2. Perform actions
// 3. Get console logs
// 4. Assert no SEVERE errors
```

---

[← Bài trước: JavaScript Executor](13-javascript-executor.md) | [Bài tiếp: TestNG Basics →](15-testng-basics.md)

---

**Happy Testing!** 📸  
*"A picture is worth a thousand debugging hours."*
