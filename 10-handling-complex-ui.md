# 🖱️ PHẦN 10: HANDLING COMPLEX UI

> **Mục tiêu**: Xử lý các UI elements phức tạp như Dropdowns, Checkboxes, Radio buttons trong Selenium.

---

## 📑 MỤC LỤC

1. [Dropdowns - Select Class](#dropdowns---select-class)
2. [Checkboxes](#checkboxes)
3. [Radio Buttons](#radio-buttons)
4. [Custom Dropdowns](#custom-dropdowns)
5. [Multi-select Dropdowns](#multi-select-dropdowns)

---

## 📋 Dropdowns - Select Class

> **Dropdown** = Danh sách các options để user chọn (thường dùng `<select>` tag)

### HTML Dropdown

```html
<select id="country" name="country">
  <option value="">-- Select Country --</option>
  <option value="vn">Vietnam</option>
  <option value="us">United States</option>
  <option value="uk">United Kingdom</option>
</select>
```

---

### Select Class trong Selenium

```java
import org.openqa.selenium.support.ui.Select;

// Tìm dropdown element
WebElement countryDropdown = driver.findElement(By.id("country"));

// Tạo Select object
Select select = new Select(countryDropdown);
```

---

### 1. selectByVisibleText() - Chọn theo text hiển thị

```java
Select select = new Select(driver.findElement(By.id("country")));

// Chọn "Vietnam" (text người dùng nhìn thấy)
select.selectByVisibleText("Vietnam");
```

**Use case**: ✅ Dễ đọc, dễ hiểu nhất

---

### 2. selectByValue() - Chọn theo value attribute

```java
Select select = new Select(driver.findElement(By.id("country")));

// Chọn option có value="vn"
select.selectByValue("vn");
```

**Use case**: ✅ Khi value stable hơn text

---

### 3. selectByIndex() - Chọn theo index (vị trí)

```java
Select select = new Select(driver.findElement(By.id("country")));

// Chọn option thứ 1 (index bắt đầu từ 0)
select.selectByIndex(1); // Vietnam

// Chọn option thứ 2
select.selectByIndex(2); // United States
```

**Use case**: ⚠️ Ít dùng (dễ break khi thứ tự đổi)

---

### 4. getOptions() - Lấy tất cả options

```java
Select select = new Select(driver.findElement(By.id("country")));

// Lấy tất cả options
List<WebElement> allOptions = select.getOptions();

System.out.println("Total options: " + allOptions.size());

// In ra tất cả options
for (WebElement option : allOptions) {
    System.out.println(option.getText());
}
```

**Output**:
```
Total options: 4
-- Select Country --
Vietnam
United States
United Kingdom
```

---

### Complete Dropdown Example

```java
public class DropdownTest {
    
    @Test
    public void testCountryDropdown() {
        // Navigate
        driver.get("https://example.com/register");
        
        // Find dropdown
        WebElement countryDropdown = driver.findElement(By.id("country"));
        Select select = new Select(countryDropdown);
        
        // Chọn Vietnam
        select.selectByVisibleText("Vietnam");
        
        // Verify selection
        WebElement selected = select.getFirstSelectedOption();
        Assert.assertEquals(selected.getText(), "Vietnam");
        
        // Print all options
        List<WebElement> options = select.getOptions();
        System.out.println("Available countries:");
        for (WebElement option : options) {
            System.out.println("- " + option.getText());
        }
    }
}
```

---

## ☑️ Checkboxes

> **Checkbox** = Ô vuông cho phép user chọn/bỏ chọn (có thể chọn nhiều)

### HTML Checkbox

```html
<input type="checkbox" id="agree" name="agree"> I agree to terms
<input type="checkbox" id="newsletter" name="newsletter"> Subscribe to newsletter
```

---

### Check/Uncheck Checkbox

```java
WebElement agreeCheckbox = driver.findElement(By.id("agree"));

// Kiểm tra checkbox đã checked chưa
if (!agreeCheckbox.isSelected()) {
    agreeCheckbox.click(); // Check nếu chưa checked
}

// Uncheck
if (agreeCheckbox.isSelected()) {
    agreeCheckbox.click(); // Uncheck nếu đã checked
}
```

---

## 🔘 Radio Buttons

> **Radio Button** = Nút tròn cho phép user chọn **chỉ 1** option trong nhóm

### HTML Radio Buttons

```html
<h3>Select Gender:</h3>
<input type="radio" id="male" name="gender" value="male"> Male
<input type="radio" id="female" name="gender" value="female"> Female
<input type="radio" id="other" name="gender" value="other"> Other
```

---

### Select Radio Button

```java
// Chọn Male
WebElement maleRadio = driver.findElement(By.id("male"));
maleRadio.click();

// Verify đã chọn
Assert.assertTrue(maleRadio.isSelected());

// Chọn Female → Male tự động bỏ chọn
WebElement femaleRadio = driver.findElement(By.id("female"));
femaleRadio.click();

// Verify
Assert.assertTrue(femaleRadio.isSelected());
Assert.assertFalse(maleRadio.isSelected()); // Male đã bỏ chọn
```

---

## ✅ TÓM TẮT BÀI HỌC

📌 **Dropdown (Select)**: selectByVisibleText(), selectByValue(), selectByIndex()  
📌 **Checkbox**: click(), isSelected() - Chọn nhiều được  
📌 **Radio Button**: click(), isSelected() - Chỉ chọn 1  
📌 **Custom Dropdown**: Click dropdown → Click option (không dùng Select class)  
📌 **Multi-select**: isMultiple(), getAllSelectedOptions(), deselectAll()  

---

[← Bài trước: Waits](09-waits-synchronization.md) | [Bài tiếp: Alerts, Frames & Windows →](11-alerts-frames-windows.md)
