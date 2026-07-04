# 🖱️ PHẦN 12: ACTIONS CLASS - MOUSE & KEYBOARD

> **Mục tiêu**: Sử dụng Actions class để thực hiện các thao tác phức tạp với chuột và bàn phím.

## 📑 MỤC LỤC

1. [Actions Class Overview](#actions-class-overview)
2. [Mouse Actions](#mouse-actions)
3. [Keyboard Actions](#keyboard-actions)

## 🖱️ Mouse Actions

### Hover (moveToElement)

```java
Actions actions = new Actions(driver);
WebElement menu = driver.findElement(By.id("menu"));
actions.moveToElement(menu).perform();
```

### Click, Double Click, Right Click

```java
// Click
actions.click(element).perform();

// Double click
actions.doubleClick(element).perform();

// Right click (context click)
actions.contextClick(element).perform();
```

### Drag and Drop

```java
WebElement source = driver.findElement(By.id("draggable"));
WebElement target = driver.findElement(By.id("droppable"));

// Kéo thả
actions.dragAndDrop(source, target).perform();
```

## ⌨️ Keyboard Actions

```java
// Nhấn phím Enter
actions.sendKeys(Keys.ENTER).perform();

// Tổ hợp phím Ctrl+A
actions.keyDown(Keys.CONTROL)
       .sendKeys("a")
       .keyUp(Keys.CONTROL)
       .perform();
```

## ✅ TÓM TẮT

📌 **Mouse**: moveToElement(), click(), doubleClick(), dragAndDrop()  
📌 **Keyboard**: sendKeys(), keyDown(), keyUp()

[← Bài trước: Alerts/Frames](11-alerts-frames-windows.md) | [Bài tiếp: JavaScript →](13-javascript-executor.md)
