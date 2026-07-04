# 📊 PHẦN 23: EXTENT REPORTS

> **Mục tiêu**: Generate reports đẹp với ExtentReports.

## Add Dependency

```xml
<dependency>
    <groupId>com.aventstack</groupId>
    <artifactId>extentreports</artifactId>
    <version>5.0.9</version>
</dependency>
```

## Setup

```java
ExtentReports extent = new ExtentReports();
ExtentSparkReporter spark = new ExtentSparkReporter("reports/report.html");
extent.attachReporter(spark);

ExtentTest test = extent.createTest("Login Test");
test.pass("Test passed");
test.fail("Test failed");

extent.flush();
```

[← Bài trước](22-log4j-logging.md) | [Bài tiếp →](24-jenkins-cicd.md)
