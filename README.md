# PRODIGY_ST_Task-05
This project automates the checkout process of an e-commerce demo website using Java, Selenium WebDriver, and TestNG.

## 🌐 Tested On: SauceDemo (https://www.saucedemo.com/)

🎯 Objective

Automate the checkout process including:

1.Login
2.Add product to cart
3.Verify cart page
4.Fill checkout form(Negative + Positive test)
5.Complete purchase
6.Verify success message
7.Validate page transitions and form validations

🔧 Project Setup (Maven)
1️⃣ Add Dependencies in pom.xml
<dependencies>

    <!-- Selenium -->
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.18.1</version>
    </dependency>

    <!-- TestNG -->
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.9.0</version>
        <scope>test</scope>
    </dependency>

    <!-- WebDriverManager -->
    <dependency>
        <groupId>io.github.bonigarcia</groupId>
        <artifactId>webdrivermanager</artifactId>
        <version>5.6.3</version>
    </dependency>

</dependencies>

📂 Test Script: CheckoutTest.java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

import io.github.bonigarcia.wdm.WebDriverManager;

import java.time.Duration;

public class CheckoutTest {

    WebDriver driver;
    WebDriverWait wait;

    @BeforeMethod
    public void setup() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        wait = new WebDriverWait(driver, Duration.ofSeconds(10));
        driver.get("https://www.saucedemo.com/");
    }

    @Test
    public void testCheckoutFlow() {

        // 1️⃣ Verify Login Page
        Assert.assertTrue(driver.getTitle().contains("Swag Labs"));

        // 2️⃣ Login
        driver.findElement(By.id("user-name")).sendKeys("standard_user");
        driver.findElement(By.id("password")).sendKeys("secret_sauce");
        driver.findElement(By.id("login-button")).click();

        // Verify Inventory Page Transition
        wait.until(ExpectedConditions.urlContains("inventory"));
        Assert.assertTrue(driver.getCurrentUrl().contains("inventory"));

        // 3️⃣ Add Item to Cart
        driver.findElement(By.id("add-to-cart-sauce-labs-backpack")).click();

        WebElement cartBadge = driver.findElement(By.className("shopping_cart_badge"));
        Assert.assertEquals(cartBadge.getText(), "1");

        // 4️⃣ Go to Cart Page
        driver.findElement(By.className("shopping_cart_link")).click();
        wait.until(ExpectedConditions.urlContains("cart"));
        Assert.assertTrue(driver.getCurrentUrl().contains("cart"));

        // Verify Item Name
        String itemName = driver.findElement(By.className("inventory_item_name")).getText();
        Assert.assertEquals(itemName, "Sauce Labs Backpack");

        // 5️⃣ Proceed to Checkout
        driver.findElement(By.id("checkout")).click();
        wait.until(ExpectedConditions.urlContains("checkout-step-one"));

        // 6️⃣ Negative Test – Form Validation
        driver.findElement(By.id("continue")).click();
        WebElement errorMessage = driver.findElement(By.className("error-message-container"));
        Assert.assertTrue(errorMessage.getText().contains("Error"));

        // 7️⃣ Fill Checkout Form
        driver.findElement(By.id("first-name")).sendKeys("John");
        driver.findElement(By.id("last-name")).sendKeys("Doe");
        driver.findElement(By.id("postal-code")).sendKeys("12345");
        driver.findElement(By.id("continue")).click();

        wait.until(ExpectedConditions.urlContains("checkout-step-two"));
        Assert.assertTrue(driver.getCurrentUrl().contains("checkout-step-two"));

        // 8️⃣ Finish Order
        driver.findElement(By.id("finish")).click();

        // 9️⃣ Verify Success Message
        wait.until(ExpectedConditions.urlContains("checkout-complete"));
        String successMessage = driver.findElement(By.className("complete-header")).getText();
        Assert.assertEquals(successMessage, "Thank you for your order!");
    }

    @AfterMethod
    public void tearDown() {
        driver.quit();
    }
}

✅ What This Automation Covers
✔ Page Transitions Verified

| From      | To              |
| --------- | --------------- |
| Login     | Inventory       |
| Inventory | Cart            |
| Cart      | Checkout Step 1 |
| Step 1    | Step 2          |
| Step 2    | Order Complete  |

✔ Form Validation Tested
  Attempt checkout without filling fields
  Verified error message displayed

✔ Cart Verification
  Item added successfully
  Cart badge updated
  Correct item appears in cart

✔ Order Completion Verification
  Success message validated:
  "Thank you for your order!"

  📊 Sample TestNG Output

  ===============================================
Default Suite
Total tests run: 1, Passes: 1, Failures: 0
===============================================

⚠ Issues Encountered During Automation

| Issue           | Description                       | Resolution            |
| --------------- | --------------------------------- | --------------------- |
| Dynamic Loading | Page transition delay             | Used `WebDriverWait`  |
| Form Validation | Error container loads dynamically | Explicit wait         |
| Driver Version  | Chrome mismatch                   | Used WebDriverManager |

✔ Task Requirements Check

| Requirement                    | Status |
| ------------------------------ | ------ |
| Automate checkout process      | ✅ Done |
| Add items to cart              | ✅ Done |
| Fill checkout form             | ✅ Done |
| Negative + Positive validation | ✅ Done |
| Verify page transitions        | ✅ Done |
| Verify success message         | ✅ Done |
| Document script                | ✅ Done |
| Mention issues encountered     | ✅ Done |

## ▶ How to Run

1. Clone the repository
2. Open project in IntelliJ / Eclipse
3. Run `CheckoutTest.java` as TestNG Test
4. Make sure Chrome browser is installed

## 👤 Author

**Sanjana Khandare**  
GitHub: https://github.com/sanjanakhandare
QA Automation Intern – ProDigy Infotech  

🔹 Tech Stack: Java, Selenium WebDriver, TestNG  
🔹 Task: Automated E-Commerce Checkout Flow  

