# 🏷️ PHẦN 16: TESTNG ANNOTATIONS DEEP DIVE

> **Mục tiêu**: Thành thạo TestNG annotations nâng cao như @DataProvider, @Parameters, Test attributes.

## 📑 MỤC LỤC

1. [@DataProvider](#dataprovider)
2. [@Parameters](#parameters)
3. [Test Attributes](#test-attributes)
4. [Dependency Tests](#dependency-tests)

## 📊 @DataProvider

> **DataProvider** = Cung cấp data cho test method (Data-Driven Testing)

### Basic DataProvider

```java
@DataProvider(name = "loginData")
public Object[][] getLoginData() {
    return new Object[][] {
        {"user1@example.com", "Pass@123"},
        {"user2@example.com", "Pass@456"},
        {"user3@example.com", "Pass@789"}
    };
}

@Test(dataProvider = "loginData")
public void testLogin(String email, String password) {
    driver.get("https://example.com/login");
    driver.findElement(By.id("email")).sendKeys(email);
    driver.findElement(By.id("password")).sendKeys(password);
    driver.findElement(By.id("loginBtn")).click();
    
    Assert.assertTrue(driver.getTitle().contains("Dashboard"));
}
```

**Test sẽ chạy 3 lần** với 3 sets of data!

### DataProvider với Different Class

```java
// Data provider class
public class TestData {
    @DataProvider(name = "userData")
    public static Object[][] getUserData() {
        return new Object[][] {
            {"John", "john@example.com"},
            {"Jane", "jane@example.com"}
        };
    }
}

// Test class
@Test(dataProvider = "userData", dataProviderClass = TestData.class)
public void testUser(String name, String email) {
    System.out.println("Testing: " + name + " - " + email);
}
```

## 🔧 @Parameters

> **Parameters** = Pass parameters từ testng.xml vào test methods

### testng.xml with Parameters

```xml
<suite name="Test Suite">
    <parameter name="browser" value="chrome"/>
    <parameter name="url" value="https://example.com"/>
    
    <test name="Login Test">
        <parameter name="username" value="test@example.com"/>
        <parameter name="password" value="Test@123"/>
        
        <classes>
            <class name="com.example.tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

### Test with @Parameters

```java
@Parameters({"browser", "url"})
@BeforeClass
public void setup(String browser, String url) {
    if (browser.equals("chrome")) {
        driver = new ChromeDriver();
    } else if (browser.equals("firefox")) {
        driver = new FirefoxDriver();
    }
    driver.get(url);
}

@Parameters({"username", "password"})
@Test
public void testLogin(String username, String password) {
    driver.findElement(By.id("email")).sendKeys(username);
    driver.findElement(By.id("password")).sendKeys(password);
    driver.findElement(By.id("loginBtn")).click();
}
```

## ⚙️ Test Attributes

### priority

```java
@Test(priority = 1)
public void testLogin() {
    System.out.println("Login test - Chạy đầu tiên");
}

@Test(priority = 2)
public void testDashboard() {
    System.out.println("Dashboard test - Chạy thứ 2");
}

@Test(priority = 3)
public void testLogout() {
    System.out.println("Logout test - Chạy cuối");
}
```

### enabled

```java
@Test(enabled = true)
public void testActive() {
    System.out.println("Test này sẽ chạy");
}

@Test(enabled = false)
public void testDisabled() {
    System.out.println("Test này KHÔNG chạy");
}
```

### description

```java
@Test(description = "Verify user can login with valid credentials")
public void testValidLogin() {
    // Test code
}
```

### timeOut

```java
@Test(timeOut = 5000) // 5 seconds
public void testWithTimeout() {
    // Test phải hoàn thành trong 5 giây
    // Nếu không sẽ fail
}
```

### expectedExceptions

```java
@Test(expectedExceptions = NoSuchElementException.class)
public void testElementNotFound() {
    // Test sẽ PASS nếu throw NoSuchElementException
    driver.findElement(By.id("nonexistent"));
}
```

## 🔗 Dependency Tests

### dependsOnMethods

```java
@Test
public void testLogin() {
    System.out.println("Login test");
    // Login logic
}

@Test(dependsOnMethods = {"testLogin"})
public void testDashboard() {
    System.out.println("Dashboard test - Chạy SAU testLogin");
    // Dashboard logic
}

@Test(dependsOnMethods = {"testDashboard"})
public void testLogout() {
    System.out.println("Logout test - Chạy SAU testDashboard");
}
```

**Nếu testLogin fail → testDashboard và testLogout bị SKIP!**

## ✅ TÓM TẮT

📌 **@DataProvider**: Provide data cho tests (Data-Driven)  
📌 **@Parameters**: Pass parameters từ testng.xml  
📌 **Attributes**: priority, enabled, description, timeOut  
📌 **Dependencies**: dependsOnMethods để control thứ tự

[← Bài trước: TestNG Basics](15-testng-basics.md) | [Bài tiếp: TestNG Advanced →](17-testng-advanced.md)
