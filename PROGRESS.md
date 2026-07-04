# 📚 DANH SÁCH TẤT CẢ CÁC PHẦN - AUTOMATION TESTING COURSE

> **25 phần học** - Từ Foundation đến CI/CD

---

## ✅ ĐÃ TẠO (5 phần)

### Module 1: Foundation

| # | File | Trạng thái | Nội dung |
|---|------|-----------|----------|
| 01 | [01-automation-testing-fundamentals.md](01-automation-testing-fundamentals.md) | ✅ Hoàn thành | Automation là gì? Manual vs Automation. ROI. Test Pyramid. Selenium Ecosystem |
| 02 | [02-java-refresher.md](02-java-refresher.md) | ✅ Hoàn thành | OOP (Class, Inheritance, Polymorphism). Collections (ArrayList, HashMap). Exception Handling. File I/O |
| 03 | [03-maven-build-tool.md](03-maven-build-tool.md) | ✅ Hoàn thành | Maven là gì? pom.xml. Dependencies. Maven Lifecycle. Commands (clean, test) |

### Module 2: Selenium Core

| # | File | Trạng thái | Nội dung |
|---|------|-----------|----------|
| 04 | [04-selenium-introduction.md](04-selenium-introduction.md) | ✅ Hoàn thành | Selenium Suite. WebDriver Architecture. Setup project. First test. WebDriver basics |
| 05 | [05-locators-8-strategies.md](05-locators-8-strategies.md) | ✅ Hoàn thành | findElement vs findElements. 8 locators (ID, Name, ClassName, TagName, LinkText, PartialLinkText, CSS, XPath). Priority |

---

## 📝 CẦN TẠO (20 phần còn lại)

### Module 2: Selenium Core (tiếp)

| # | File | Nội dung chính |
|---|------|----------------|
| 06 | `06-css-selector-deep-dive.md` | **CSS Selector chi tiết**: Basic (tag, #id, .class). Attributes ([name='value'], [*=], [^=], [$=]). Combinators (space, >, +, ~). Pseudo-classes (:first-child, :nth-child). 50+ examples thực tế |
| 07 | `07-xpath-deep-dive.md` | **XPath chi tiết**: Absolute vs Relative. Axes (parent, child, following, preceding, ancestor). Functions (contains, text, starts-with, normalize-space). Dynamic XPath strategies. 50+ examples |
| 08 | `08-webelement-actions.md` | **WebElement Methods**: sendKeys, click, clear, submit. getText, getAttribute, getCssValue. isDisplayed, isEnabled, isSelected. getTagName, getSize, getLocation. Real examples |
| 09 | `09-waits-synchronization.md` | **Waits chi tiết**: Thread.sleep (BAD). Implicit Wait. Explicit Wait. WebDriverWait. FluentWait. ExpectedConditions (30+ conditions). Custom wait conditions. Best practices |

---

### Module 3: Selenium Advanced

| # | File | Nội dung chính |
|---|------|----------------|
| 10 | `10-handling-complex-ui.md` | **Dropdowns**: Select class (selectByVisibleText, selectByValue, selectByIndex, getOptions). Checkboxes & Radio buttons. Multi-select. Custom dropdowns (không dùng select tag) |
| 11 | `11-alerts-frames-windows.md` | **Alerts**: accept, dismiss, getText, sendKeys. **Frames**: switchTo.frame (by index, name, WebElement). switchTo.defaultContent. **Windows**: getWindowHandles, switchTo.window |
| 12 | `12-actions-class.md` | **Actions API**: Mouse actions (moveToElement hover, click, doubleClick, contextClick rightclick, dragAndDrop). Keyboard (sendKeys, keyDown, keyUp với SHIFT/CTRL/ALT). build vs perform |
| 13 | `13-javascript-executor.md` | **JavascriptExecutor**: executeScript, executeAsyncScript. Scroll (scrollIntoView, scrollBy, scrollTo). Click hidden elements. Change attributes. Get/Set values. Handle disabled elements |
| 14 | `14-screenshots-logs.md` | **Screenshots**: TakesScreenshot interface. getScreenshotAs. Save file. Screenshot on failure. **Browser logs**: manage().logs(). Console errors. Network logs |

---

### Module 4: TestNG Framework

| # | File | Nội dung chính |
|---|------|----------------|
| 15 | `15-testng-basics.md` | **TestNG Introduction**: TestNG vs JUnit. Annotations (@Test, @BeforeMethod, @AfterMethod, @BeforeClass, @AfterClass, @BeforeTest, @AfterTest, @BeforeSuite, @AfterSuite). Execution order. testng.xml |
| 16 | `16-testng-annotations.md` | **Annotations Deep Dive**: @Test attributes (priority, enabled, description, dependsOnMethods, groups, alwaysRun, timeOut). @Parameters. @DataProvider (1D, 2D data, return Object[][]) |
| 17 | `17-testng-advanced.md` | **TestNG Advanced**: Test groups (define, run groups). Parallel execution (tests, classes, methods, thread-count). Test dependencies. Soft vs Hard assertions. Assert methods (assertEquals, assertTrue, assertNotNull...) |
| 18 | `18-testng-listeners.md` | **Listeners**: ITestListener interface. Methods (onStart, onFinish, onTestSuccess, onTestFailure, onTestSkipped, onTestFailedButWithinSuccessPercentage). Custom listeners. Screenshot on failure with listeners |

---

### Module 5: Framework Design

| # | File | Nội dung chính |
|---|------|----------------|
| 19 | `19-page-object-model.md` | **POM Design**: Tại sao POM? Structure (pages package, tests package). Page class design (@FindBy, PageFactory.initElements). Best practices (return types, method naming, waits in pages) |
| 20 | `20-data-driven-testing.md` | **Test Data Management**: Read Excel (Apache POI - Workbook, Sheet, Row, Cell). Read CSV (BufferedReader). Read JSON (org.json). Read Properties. Integrate với @DataProvider |
| 21 | `21-framework-utilities.md` | **Utility Classes**: BaseTest (WebDriver init, @BeforeMethod, @AfterMethod). ConfigReader (properties file). ExcelReader (read/write). ScreenshotUtil. WaitUtil (custom waits). StringUtil, DateUtil |

---

### Module 6: Reporting & Logging

| # | File | Nội dung chính |
|---|------|----------------|
| 22 | `22-log4j-logging.md` | **Logging**: Tại sao log? Log4j2 setup. log4j2.xml config (Console, File appenders). Logger levels (TRACE, DEBUG, INFO, WARN, ERROR, FATAL). Best practices (log meaningful info, not sensitive data) |
| 23 | `23-reporting.md` | **ExtentReports**: Setup (ExtentReports, ExtentTest). createTest, log(Status, details). addScreenshot. Flush report. HTML output. **Allure**: Setup, @Step, Allure.step(), attachments, categories, beautiful reports |

---

### Module 7: CI/CD & Best Practices

| # | File | Nội dung chính |
|---|------|----------------|
| 24 | `24-jenkins-cicd.md` | **Jenkins**: Jenkins là gì? Install (Windows/Mac). Create freestyle job. Configure (Git repo, Maven goals). Build triggers (Poll SCM, cron). Post-build actions (Email, reports). Jenkins Pipeline (Jenkinsfile basics) |
| 25 | `25-best-practices-career.md` | **Best Practices**: Code organization (packages). Naming conventions. DRY principle. Config externalization. Exception handling. Screenshot strategy. Code review checklist. **Interview Prep**: Top 50 questions. Resume tips. Career roadmap |

---

## 🎯 LỘ TRÌNH HỌC ĐỀ XUẤT

### Week 1-2: Foundation + Selenium Basics
- ✅ Phần 1-5 (đã tạo)
- 📝 Phần 6-9 (CSS, XPath, WebElement, Waits)

### Week 3-4: Selenium Advanced
- 📝 Phần 10-14 (Dropdowns, Alerts, Actions, JavaScript, Screenshots)

### Week 5-6: TestNG Framework
- 📝 Phần 15-18 (TestNG basics, annotations, advanced, listeners)

### Week 7-8: Framework Design
- 📝 Phần 19-21 (POM, Data-Driven, Utilities)

### Week 9: Reporting & Logging
- 📝 Phần 22-23 (Log4j, ExtentReports, Allure)

### Week 10: CI/CD & Polish
- 📝 Phần 24-25 (Jenkins, Best Practices)

---

## 📊 TIẾN ĐỘ

```
[█████░░░░░░░░░░░░░░░] 20% hoàn thành (5/25 phần)

Module 1: Foundation        [███████████] 100% (3/3)
Module 2: Selenium Core     [████░░░░░░░░]  33% (2/6)
Module 3: Selenium Advanced [░░░░░░░░░░░░]   0% (0/5)
Module 4: TestNG            [░░░░░░░░░░░░]   0% (0/4)
Module 5: Framework Design  [░░░░░░░░░░░░]   0% (0/3)
Module 6: Reporting         [░░░░░░░░░░░░]   0% (0/2)
Module 7: CI/CD             [░░░░░░░░░░░░]   0% (0/2)
```

---

## 🚀 BƯỚC TIẾP THEO

### Option 1: Tiếp tục tạo từng file
Tôi sẽ tạo tiếp:
- Phần 6: CSS Selector Deep Dive
- Phần 7: XPath Deep Dive
- Phần 8: WebElement Actions
- ...

### Option 2: Tạo outline cho tất cả
Tạo skeleton files (header + structure) cho tất cả 20 phần còn lại, sau đó fill content dần.

### Option 3: Focus vào phần quan trọng
Ưu tiên tạo các phần quan trọng nhất:
- Phần 6: CSS Selector (★★★★★)
- Phần 7: XPath (★★★★★)
- Phần 9: Waits (★★★★★)
- Phần 19: POM (★★★★★)
- Phần 23: Reporting (★★★★)

---

## 💡 GHI CHÚ

**Design đã áp dụng**:
- ✅ Tiếng Việt (key terms tiếng Anh)
- ✅ Mermaid diagrams cho flow/architecture
- ✅ Tables cho comparisons
- ✅ Emoji headers cho dễ scan
- ✅ Code examples chi tiết
- ✅ Real-world use cases
- ✅ Best practices + Common mistakes
- ✅ Thực hành exercises

**Format nhất quán**:
- 📑 Mục lục đầu file
- Sections với headers rõ ràng
- Code blocks với syntax highlighting
- Navigation links cuối file
- Quote motivational cuối file

---

Bạn muốn tôi:
1. **Tiếp tục tạo từng file chi tiết** (Phần 6, 7, 8...)?
2. **Tạo skeleton cho tất cả** (structure only)?
3. **Focus vào top 5 phần quan trọng nhất**?

Cho tôi biết hướng nào bạn muốn! 😊
