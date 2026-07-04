# 📄 PHẦN 19: PAGE OBJECT MODEL

> **Mục tiêu**: Design pattern để organize code tốt hơn - tách UI elements thành các Page classes.

## POM là gì?

**Page Object Model** = Tách locators và actions của mỗi page thành class riêng

**Lợi ích**:
- Maintainability: Update 1 chỗ
- Reusability: Dùng lại methods
- Readability: Code dễ đọc

## Example: LoginPage

```java
public class LoginPage {
    WebDriver driver;
    
    By emailField = By.id("email");
    By passwordField = By.id("password");
    By loginBtn = By.id("loginBtn");
    
    public LoginPage(WebDriver driver) {
        this.driver = driver;
    }
    
    public void login(String email, String password) {
        driver.findElement(emailField).sendKeys(email);
        driver.findElement(passwordField).sendKeys(password);
        driver.findElement(loginBtn).click();
    }
}
```

## @FindBy & PageFactory

```java
import org.openqa.selenium.support.FindBy;
import org.openqa.selenium.support.PageFactory;

public class LoginPage {
    @FindBy(id = "email")
    WebElement emailField;
    
    @FindBy(id = "password")
    WebElement passwordField;
    
    public LoginPage(WebDriver driver) {
        PageFactory.initElements(driver, this);
    }
    
    public void login(String email, String password) {
        emailField.sendKeys(email);
        passwordField.sendKeys(password);
    }
}
```

[← Bài trước](18-testng-listeners.md) | [Bài tiếp →](20-data-driven-testing.md)
