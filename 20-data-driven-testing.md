# 📊 PHẦN 20: DATA-DRIVEN TESTING

> **Mục tiêu**: Test với nhiều bộ data từ Excel, CSV.

## Data-Driven là gì?

Chạy cùng test với nhiều data sets khác nhau.

## Excel với Apache POI

```xml
<dependency>
    <groupId>org.apache.poi</groupId>
    <artifactId>poi-ooxml</artifactId>
    <version>5.2.3</version>
</dependency>
```

```java
public Object[][] readExcel(String file, String sheet) {
    // Read Excel và return Object[][]
}

@DataProvider(name = "loginData")
public Object[][] getData() {
    return readExcel("data.xlsx", "Sheet1");
}

@Test(dataProvider = "loginData")
public void testLogin(String email, String password) {
    loginPage.login(email, password);
}
```

[← Bài trước](19-page-object-model.md) | [Bài tiếp →](21-framework-utilities.md)
