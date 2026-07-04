# 🚀 PHẦN 17: TESTNG ADVANCED

> **Mục tiêu**: Groups, Parallel Execution, Dependencies - các tính năng nâng cao của TestNG.

## 📑 MỤC LỤC

1. [Test Groups](#test-groups)
2. [Parallel Execution](#parallel-execution)
3. [Test Dependencies](#test-dependencies)
4. [Retry Failed Tests](#retry-failed-tests)

## 🏷️ Test Groups

### Define Groups

```java
@Test(groups = {"smoke"})
public void testLogin() {
    System.out.println("Smoke test: Login");
}

@Test(groups = {"regression"})
public void testCheckout() {
    System.out.println("Regression test: Checkout");
}

@Test(groups = {"smoke", "regression"})
public void testSearch() {
    System.out.println("Test này thuộc CẢ 2 groups");
}
```

### Run Groups trong testng.xml

```xml
<suite name="Test Suite">
    <test name="Smoke Tests">
        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>
        <classes>
            <class name="com.example.tests.AllTests"/>
        </classes>
    </test>
</suite>
```

## ⚡ Parallel Execution

### Suite Level

```xml
<suite name="Parallel Suite" parallel="tests" thread-count="3">
    <test name="Test 1">
        <classes>
            <class name="com.example.tests.LoginTest"/>
        </classes>
    </test>
    
    <test name="Test 2">
        <classes>
            <class name="com.example.tests.ProductTest"/>
        </classes>
    </test>
</suite>
```

### Parallel Options

- `parallel="tests"` - Chạy các `<test>` tag parallel
- `parallel="classes"` - Chạy các class parallel
- `parallel="methods"` - Chạy các @Test methods parallel

### Example: Method Level

```xml
<suite name="Suite" parallel="methods" thread-count="5">
    <test name="Test">
        <classes>
            <class name="com.example.tests.LoginTest"/>
        </classes>
    </test>
</suite>
```

## 🔄 Retry Failed Tests

### IRetryAnalyzer

```java
import org.testng.IRetryAnalyzer;
import org.testng.ITestResult;

public class RetryAnalyzer implements IRetryAnalyzer {
    private int retryCount = 0;
    private static final int maxRetryCount = 3;
    
    @Override
    public boolean retry(ITestResult result) {
        if (retryCount < maxRetryCount) {
            retryCount++;
            return true; // Retry
        }
        return false; // Don't retry
    }
}
```

### Use Retry

```java
@Test(retryAnalyzer = RetryAnalyzer.class)
public void testFlakyTest() {
    // Test code có thể fail intermittently
}
```

## ✅ TÓM TẮT

📌 **Groups**: Organize tests (smoke, regression)  
📌 **Parallel**: Chạy tests song song để tiết kiệm thời gian  
📌 **Dependencies**: Control test execution order  
📌 **Retry**: Tự động retry failed tests

[← Bài trước: TestNG Annotations](16-testng-annotations.md) | [Bài tiếp: TestNG Listeners →](18-testng-listeners.md)
