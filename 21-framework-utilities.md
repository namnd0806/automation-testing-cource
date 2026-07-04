# 🔧 PHẦN 21: FRAMEWORK UTILITIES

> **Mục tiêu**: Tạo utility classes để support framework (WaitUtil, ScreenshotUtil, ConfigReader).

## BaseTest Class

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
        driver.quit();
    }
    
    public WebDriver getDriver() {
        return driver;
    }
}
```

## WaitUtil

```java
public class WaitUtil {
    public static void waitForVisible(WebDriver driver, By locator) {
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        wait.until(ExpectedConditions.visibilityOfElementLocated(locator));
    }
}
```

[← Bài trước](20-data-driven-testing.md) | [Bài tiếp →](22-log4j-logging.md)
