# 📝 PHẦN 22: LOG4J LOGGING

> **Mục tiêu**: Add logging vào framework để debug dễ hơn.

## Add Dependency

```xml
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.20.0</version>
</dependency>
```

## Usage

```java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class LoginTest {
    private static final Logger logger = LogManager.getLogger(LoginTest.class);
    
    @Test
    public void testLogin() {
        logger.info("Starting login test");
        logger.debug("Entering credentials");
        logger.error("Login failed");
    }
}
```

[← Bài trước](21-framework-utilities.md) | [Bài tiếp →](23-reporting.md)
