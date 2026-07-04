# 👂 PHẦN 18: TESTNG LISTENERS

> **Mục tiêu**: Sử dụng TestNG Listeners để customize test execution behavior và auto capture screenshots.

---

## 📑 MỤC LỤC

1. [ITestListener Interface](#itestlistener-interface)
2. [Screenshot on Failure](#screenshot-on-failure)
3. [Logging on Test Events](#logging-on-test-events)
4. [Enable Listeners](#enable-listeners)

---

## 🎧 ITestListener Interface

> **ITestListener** = Interface để listen các events trong test execution

### Create Custom Listener

```java
import org.testng.ITestListener;
import org.testng.ITestResult;

public class TestListener implements ITestListener {
    
    @Override
    public void onTestStart(ITestResult result) {
        System.out.println("TEST STARTED: " + result.getName());
    }
    
    @Override
    public void onTestSuccess(ITestResult result) {
        System.out.println("TEST PASSED: " + result.getName());
    }
    
    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("TEST FAILED: " + result.getName());
        System.out.println("Error: " + result.getThrowable().getMessage());
    }
    
    @Override
    public void onTestSkipped(ITestResult result) {
        System.out.println("TEST SKIPPED: " + result.getName());
    }
    
    @Override
    public void onStart(ITestContext context) {
        System.out.println("TEST SUITE STARTED: " + context.getName());
    }
    
    @Override
    public void onFinish(ITestContext context) {
        System.out.println("TEST SUITE FINISHED: " + context.getName());
        System.out.println("Total Passed: " + context.getPassedTests().size());
        System.out.println("Total Failed: " + context.getFailedTests().size());
        System.out.println("Total Skipped: " + context.getSkippedTests().size());
    }
}
```

---

## 📸 Screenshot on Failure

### Screenshot Listener

```java
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.testng.ITestListener;
import org.testng.ITestResult;
import java.io.File;
import java.nio.file.Files;
import java.nio.file.StandardCopyOption;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class ScreenshotListener implements ITestListener {
    
    @Override
    public void onTestFailure(ITestResult result) {
        System.out.println("Test Failed: " + result.getName());
        
        // Get driver instance from test class
        Object testClass = result.getInstance();
        WebDriver driver = ((BaseTest) testClass).getDriver();
        
        // Take screenshot
        takeScreenshot(driver, result.getName());
    }
    
    private void takeScreenshot(WebDriver driver, String testName) {
        try {
            // Create timestamp
            String timestamp = LocalDateTime.now()
                .format(DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss"));
            
            // Screenshot filename
            String fileName = testName + "_" + timestamp + ".png";
            
            // Capture screenshot
            TakesScreenshot ts = (TakesScreenshot) driver;
            File source = ts.getScreenshotAs(OutputType.FILE);
            
            // Create directory if not exists
            File screenshotDir = new File("test-output/screenshots");
            if (!screenshotDir.exists()) {
                screenshotDir.mkdirs();
            }
            
            // Save file
            File destination = new File(screenshotDir, fileName);
            Files.copy(source.toPath(), destination.toPath(), 
                      StandardCopyOption.REPLACE_EXISTING);
            
            System.out.println("Screenshot saved: " + fileName);
            
        } catch (Exception e) {
            System.out.println("Failed to take screenshot: " + e.getMessage());
        }
    }
}
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
    
    // Getter for Listener
    public WebDriver getDriver() {
        return driver;
    }
}
```

---

### Test Class extends BaseTest

```java
public class LoginTest extends BaseTest {
    
    @Test
    public void testValidLogin() {
        driver.get("https://example.com/login");
        driver.findElement(By.id("email")).sendKeys("test@example.com");
        driver.findElement(By.id("password")).sendKeys("Test@123");
        driver.findElement(By.id("loginBtn")).click();
        
        // This will pass
        Assert.assertTrue(driver.getTitle().contains("Dashboard"));
    }
    
    @Test
    public void testInvalidLogin() {
        driver.get("https://example.com/login");
        driver.findElement(By.id("email")).sendKeys("wrong@example.com");
        driver.findElement(By.id("password")).sendKeys("wrong");
        driver.findElement(By.id("loginBtn")).click();
        
        // This will fail → Screenshot will be taken
        Assert.assertTrue(driver.getTitle().contains("Dashboard"));
    }
}
```

---

## 📝 Logging on Test Events

### Extended Listener với Logging

```java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class ExtendedListener implements ITestListener {
    private static final Logger logger = LogManager.getLogger(ExtendedListener.class);
    
    @Override
    public void onTestStart(ITestResult result) {
        logger.info("========================================");
        logger.info("TEST STARTED: " + result.getName());
        logger.info("========================================");
    }
    
    @Override
    public void onTestSuccess(ITestResult result) {
        logger.info("✅ TEST PASSED: " + result.getName());
        long duration = result.getEndMillis() - result.getStartMillis();
        logger.info("Duration: " + duration + " ms");
    }
    
    @Override
    public void onTestFailure(ITestResult result) {
        logger.error("❌ TEST FAILED: " + result.getName());
        logger.error("Error Message: " + result.getThrowable().getMessage());
        
        // Take screenshot
        Object testClass = result.getInstance();
        WebDriver driver = ((BaseTest) testClass).getDriver();
        takeScreenshot(driver, result.getName());
        
        // Log screenshot path
        logger.info("Screenshot captured for failed test");
    }
    
    @Override
    public void onTestSkipped(ITestResult result) {
        logger.warn("⚠️ TEST SKIPPED: " + result.getName());
        logger.warn("Reason: " + result.getThrowable().getMessage());
    }
    
    @Override
    public void onFinish(ITestContext context) {
        logger.info("========================================");
        logger.info("TEST SUITE COMPLETED: " + context.getName());
        logger.info("Total Tests Run: " + 
            (context.getPassedTests().size() + 
             context.getFailedTests().size() + 
             context.getSkippedTests().size()));
        logger.info("Passed: " + context.getPassedTests().size());
        logger.info("Failed: " + context.getFailedTests().size());
        logger.info("Skipped: " + context.getSkippedTests().size());
        logger.info("========================================");
    }
}
```

---

## 🔧 Enable Listeners

### Cách 1: Trong testng.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<suite name="Test Suite">
    
    <!-- Enable listeners -->
    <listeners>
        <listener class-name="com.example.listeners.ScreenshotListener"/>
        <listener class-name="com.example.listeners.ExtendedListener"/>
    </listeners>
    
    <test name="Login Tests">
        <classes>
            <class name="com.example.tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

---

### Cách 2: Dùng @Listeners Annotation

```java
import org.testng.annotations.Listeners;

@Listeners({ScreenshotListener.class, ExtendedListener.class})
public class LoginTest extends BaseTest {
    
    @Test
    public void testLogin() {
        // Test code
    }
}
```

---

## ✅ TÓM TẮT BÀI HỌC

📌 **ITestListener** = Interface để listen test events  
📌 **onTestFailure** = Screenshot + logging khi test fail  
📌 **onTestSuccess** = Log duration, status  
📌 **Enable**: Qua testng.xml hoặc @Listeners annotation  
📌 **BaseTest pattern**: Provide getDriver() cho Listener  

---

## 🎯 SAU KHI HỌC BUỔI NÀY

### Checklist

- [ ] Hiểu ITestListener interface
- [ ] Biết tạo custom listener
- [ ] Biết auto screenshot on failure
- [ ] Biết enable listeners

### 📝 Thực hành

**Bài 1: Basic Listener**

```java
// 1. Tạo TestListener implement ITestListener
// 2. Override onTestStart, onTestSuccess, onTestFailure
// 3. Print test name và status
// 4. Enable trong testng.xml
```

**Bài 2: Screenshot Listener**

```java
// 1. Tạo ScreenshotListener
// 2. Screenshot on failure với timestamp
// 3. Save vào test-output/screenshots
// 4. Test với 1 test pass và 1 test fail
```

**Bài 3: Extended Listener**

```java
// 1. Add logging
// 2. Log test duration
// 3. Print summary (total/passed/failed/skipped)
```

---

[← Bài trước: TestNG Advanced](17-testng-advanced.md) | [Bài tiếp: Page Object Model →](19-page-object-model.md)

---

**Happy Listening!** 👂  
*"Listeners: The silent guardians of your tests."*
