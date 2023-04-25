import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import java.io.File;
import java.io.IOException;
import java.util.List;
public class FacebookTestCase2 {
    private static final Logger LOGGER = LoggerFactory.getLogger(FacebookTestCase1.class);

    public static void main(String[] args) throws InterruptedException {
        LOGGER.info("Starting program");
        WebDriver driver = null;
        try {
            // Set the path to the chromedriver executable
            LOGGER.debug("Setting system property for chromedriver");
            System.setProperty("webdriver.chrome.driver", "D:\\Downloads\\chromedriver_win32 (1)\\chromedriver.exe");

            // Create ChromeOptions instance and disable notifications
            LOGGER.debug("Creating ChromeOptions instance");
            ChromeOptions options = new ChromeOptions();
            options.addArguments("--disable-notifications");

            // Create a new instance of the ChromeDriver with options
            LOGGER.debug("Creating ChromeDriver instance");
            driver = new ChromeDriver(options);

            // Navigate to the Facebook website
            LOGGER.info("Navigating to Facebook website");
            driver.get("https://www.facebook.com/");

            // Find the cookie notification and click the "Accept" or "Okay" button
            List<WebElement> cookieNotifications = driver.findElements(By.xpath("//button[@title='Tillåt endast nödvändiga cookies']"));
            if (cookieNotifications.size() > 0) {
                cookieNotifications.get(0).click();
                LOGGER.info("Clicked on the cookie notification button");
            } else {
                LOGGER.info("No cookie notification found on the page");
            }

            // Json file with username and password to login
            LOGGER.debug("Loading JSON file with credentials");
            File jsonFile = new File("C:\\temp\\facebook.json");

            String email = null;
            String password = null;

            try {
                ObjectMapper objectMapper = new ObjectMapper();
                JsonNode jsonNode = objectMapper.readTree(jsonFile);

                email = jsonNode.get("facebookCredentials").get("email").asText();
                password = jsonNode.get("facebookCredentials").get("password").asText();

            } catch (IOException e) {
                e.printStackTrace();
            }


            // Enter login credentials and click login button
            LOGGER.info("Entering login credentials and clicking login button");
            WebElement emailInput = driver.findElement(By.id("email"));
            WebElement passwordInput = driver.findElement(By.id("pass"));
            emailInput.sendKeys(email);
            passwordInput.sendKeys(password);
            LOGGER.info("Credentials loaded");

            Thread.sleep(1000);

            WebElement loginButton = driver.findElement(By.xpath("//button[@name='login']"));
            loginButton.click();
            LOGGER.debug("Pressed login button");
            Thread.sleep(1000);

            // Verify that user is logged in and redirected to their profile page (either new user account page, or regular user account page)
            String expectedUrl1 = "https://www.facebook.com/";
            String expectedUrl2 = "https://www.facebook.com/?sk=welcome";
            String actualUrl = driver.getCurrentUrl();
            if (actualUrl.equals(expectedUrl1) || actualUrl.equals(expectedUrl2)) {
                LOGGER.debug("Login successful");
            } else {
                LOGGER.error("Login failed");
            }
            Thread.sleep(1000);

            // Here starts other tests
            // Here starts the posting test


            // Redirect to the profile page
            driver.get("https://www.facebook.com/me");
            LOGGER.info("Redirected to profile page");


            //Post from the homepage not from profile page
            WebElement createPostButton = driver.findElement(By.xpath("//span[contains(text(), \"What's on your mind\")]"));
            createPostButton.click();
            LOGGER.debug("Clicked on the first posting field ");
            Thread.sleep(2000);

            JavascriptExecutor executor = (JavascriptExecutor)driver;

            // Click on the text field
            WebElement createPostButton2 = driver.findElement(By.xpath("//div[@class='xzsf02u x1a2a7pz x1n2onr6 x14wi4xw x9f619 x1lliihq x5yr21d xh8yej3 notranslate']"));
            executor.executeScript("arguments[0].click();", createPostButton2);
            LOGGER.debug("Clicked on the second posting field ");
            Thread.sleep(2000);

            //Insert the text
            createPostButton2.sendKeys("This is a test post");
            LOGGER.debug("Text inserted ");
            Thread.sleep(2000);


            //Click the Post Button v6
            WebElement postButton = driver.findElement(By.cssSelector("[aria-label='Post']"));
            postButton.click();
            LOGGER.debug("Clicked on the Post button");


            Thread.sleep(3000);
            driver.get("https://www.facebook.com/me");
            LOGGER.info("Redirected to profile page");


            // Wait for the post to be visible on the page
            WebDriverWait wait1 = new WebDriverWait(driver, 3);
            WebElement postContent = wait1.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//div[contains(text(),'This is a test post')]")));

            String postText = postContent.getText();
            if (postText.equals("This is a test post")) {
                LOGGER.debug("Post successful");
            } else {
                LOGGER.error("Post failed");
            }


            // Here ends the posting test
            // Here ends other tests

            //Logging out
            // Click on the profile button and wait for 2 seconds
            WebElement profileButton = driver.findElement(By.cssSelector("[aria-label='Your profile']"));
            profileButton.click();
            LOGGER.debug("Clicked on the profile button");
            Thread.sleep(1000);

            // Click logout button and verify that user is logged out and redirected to login page
            WebElement logoutButton = driver.findElement(By.xpath("//span[text()='Log Out']"));
            logoutButton.click();
            LOGGER.debug("Clicked on the logout button");
            Thread.sleep(1000);

            // Verify if user is logged out and redirected to login page
            String loginPageUrl = "https://www.facebook.com/";
            if (driver.getCurrentUrl().equals(loginPageUrl)) {
                LOGGER.debug("Logged out successfully");
            } else {
                LOGGER.error("Logout failed");
            }
            Thread.sleep(1000);

        } catch (InterruptedException e) {
            LOGGER.error("InterruptedException occurred: " + e.getMessage());
        } catch (Exception e) {
            LOGGER.error("Exception occurred: " + e.getMessage());
        } finally {
            // Close the browser
            driver.quit();
            LOGGER.debug("Browser is closed");
            LOGGER.info("Program is finished");
            LOGGER.info(" ");
        }
    }
}
