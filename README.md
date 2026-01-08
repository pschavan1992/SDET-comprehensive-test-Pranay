# SDET-comprehensive-test-Pranay
Written test codes under this repo

Program 1 *

class Account {
    double balance;
    double interestRate;

    public Account(double balance, double interestRate) {
        this.balance = balance;
        this.interestRate = interestRate;
    }

    // Method to be overridden by subclasses
    public void calculateAndAddInterest() {
        double interest = balance * (interestRate / 100);
        balance += interest;
        System.out.println("Generic Interest added. New balance: " + balance);
    }
}

class SavingsAccount extends Account {
    public SavingsAccount(double balance, double interestRate) {
        super(balance, interestRate);
    }

    @Override
    public void calculateAndAddInterest() {
        double interest = balance * (interestRate / 100);
        balance += interest;
        System.out.println("Savings Account Interest (at " + interestRate + "%) added: " + interest);
        System.out.println("Total Savings Balance: " + balance);
    }
}

class CurrentAccount extends Account {
    public CurrentAccount(double balance, double interestRate) {
        super(balance, interestRate);
    }

    @Override
    public void calculateAndAddInterest() {
        // Typically current accounts have very low or no interest
        double interest = balance * (interestRate / 100);
        balance += interest;
        System.out.println("Current Account Interest (at " + interestRate + "%) added: " + interest);
        System.out.println("Total Current Balance: " + balance);
    }
}

// Main class 
public class Main {
    public static void main(String[] args) {
        Account mySavings = new SavingsAccount(1000.0, 5.0); // 5% Interest
        Account myCurrent = new CurrentAccount(2000.0, 1.0); // 1% Interest

        System.out.println("--- Processing Accounts ---");
        mySavings.calculateAndAddInterest();
        myCurrent.calculateAndAddInterest();
    }
}

Program 2 * 

class Student:
    def __init__(self, name, grade, age):
        //Initialize the three variables
        self.name = name
        self.grade = grade
        self.age = age

    def display(self):
        // Method to display information of the Student object
        print("--- Student Display ---")
        print(f"Name: {self.name}")
        print(f"Grade: {self.grade}")
        print(f"Age: {self.age}")

//Create the School Class (Derived Class)

class School(Student):    
def SchoolStudentDisplay(self):        
//Method to display information specifically from the School object        
print("--- School Student Display ---")       
print(f"Name: {self.name}")       
print(f"Grade: {self.grade}")      
print(f"Age: {self.age}")
//Execution/Testing# Create an object using the 
School classschool_obj = School("John Doe", "/10th", 15)
//Call the display method from the base class
school_obj.display()
# Call the specialized method from the School class
school_obj.SchoolStudentDisplay()


Program 3 *

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.firefox.service import Service
from webdriver_manager.firefox import GeckoDriverManager

def verify_logo():
    # Initialize Firefox driver
    driver = webdriver.Firefox(service=Service(GeckoDriverManager().install()))
    
    try:
        # Launch browser
        driver.get("https://www.makemytrip.com/")
        driver.maximize_window()
        
        # Verify if logo is present using its class or alt attribute
        # Note: Selectors may change; common practice is using the alt text
        logo = driver.find_element(By.XPATH, "//img[@alt='Make My Trip']")
        
        if logo.is_displayed():
            print("Success: MakeMyTrip logo is present on the page.")
        else:
            print("Failure: Logo found in DOM but not visible.")
            
    except Exception as e:
        print(f"Error occurred: {e}")
    finally:
        driver.quit()

verify_logo()


Program 4 *

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

//Generic function to click an element
def click_element(driver, xpath):
    element = WebDriverWait(driver, 10).until(EC.element_to_be_clickable((By.XPATH, xpath)))
    element.click()

//Generic function to enter text
def enter_text(driver, xpath, text):
    element = WebDriverWait(driver, 10).until(EC.visibility_of_element_to_be_clickable((By.XPATH, xpath)))
    element.send_keys(text)

def automation_test():
    driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
    driver.maximize_window()
    
    try:
        driver.get("https://www.makemytrip.com/")
        
        // Close potential login popups if they appear
        try:
            click_element(driver, "//span[@class='commonModal__close']")
        except:
            pass

        // 1. Click on Flights
        click_element(driver, "//li[@data-cy='menu_Flights']")

        // 2. Select OneWay
        click_element(driver, "//li[@data-cy='oneWayTrip']")

        // 3. Enter FROM location
        click_element(driver, "//label[@for='fromCity']")
        enter_text(driver, "//input[@placeholder='From']", "Delhi")
        // Select first suggestion
        click_element(driver, "(//li[@role='option'])[1]")

        // 4. Enter TO location
        click_element(driver, "//label[@for='toCity']")
        enter_text(driver, "//input[@placeholder='To']", "Mumbai")
        // Select first suggestion
        click_element(driver, "(//li[@role='option'])[1]")

        print("Automation steps completed successfully.")

    except Exception as e:
        print(f"An error occurred: {e}")
    finally:
        driver.quit()

automation_test()


Program 5 * 

import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;
import io.github.bonigarcia.wdm.WebDriverManager;

public class MakeMyTripTest {
    WebDriver driver;

    @BeforeMethod
    public void setup() {
        // Repeated code to launch the browser
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.get("https://www.makemytrip.com/");
        
        // Handle potential popups
        try {
            Thread.sleep(3000); // Wait for modal
            driver.findElement(By.xpath("//span[@class='commonModal__close']")).click();
        } catch (Exception e) {
            System.out.println("No modal found.");
        }
    }

    @Test(priority = 1)
    public void verifyLogoTest() {
        // Question 3 Implementation
        WebElement logo = driver.findElement(By.xpath("//img[@alt='Make My Trip']"));
        Assert.assertTrue(logo.isDisplayed(), "MakeMyTrip logo is not present!");
    }

    @Test(priority = 2)
    public void flightSelectionTest() {
        // Question 4 Implementation using XPaths
        clickElement("//li[@data-cy='menu_Flights']");
        clickElement("//li[@data-cy='oneWayTrip']");
        
        // Enter FROM city
        clickElement("//label[@for='fromCity']");
        enterText("//input[@placeholder='From']", "Delhi");
        clickElement("(//li[@role='option'])[1]");

        // Enter TO city
        clickElement("//label[@for='toCity']");
        enterText("//input[@placeholder='To']", "Mumbai");
        clickElement("(//li[@role='option'])[1]");
    }

    // Generic function to interact with browser
    public void clickElement(String xpath) {
        driver.findElement(By.xpath(xpath)).click();
    }

    public void enterText(String xpath, String value) {
        driver.findElement(By.xpath(xpath)).sendKeys(value);
    }

    @AfterMethod
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}


Program 6 * 

Create a Maven Project
To run everything via Maven commands, you must define your dependencies in the pom.xml file.

1. Configure pom.xml
Add these dependencies to your Maven project to include Selenium, TestNG, and WebDriverManager:

XML

<dependencies>
    <dependency>
        <groupId>org.seleniumhq.selenium</groupId>
        <artifactId>selenium-java</artifactId>
        <version>4.10.0</version>
    </dependency>
    <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>7.7.1</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>io.github.bonigarcia</groupId>
        <artifactId>webdrivermanager</artifactId>
        <version>5.4.1</version>
    </dependency>
</dependencies>
2. Run via Maven Commands
Open your terminal or command prompt in the project root folder and use the following commands:
To compile the code: mvn compile
To run all test programs: mvn test
To clean previous builds and run: mvn clean test


Answer 7 * 

To complete this, follow these Git commands in terminal.
Create and Switch to a New Branch: Replace your-branch-name with something descriptive (e.g., api-testing-assignment).
Bash
git checkout -b your-branch-name
Add Your Files: After writing your code/scripts in the folder, stage them:
Bash
git add .
Commit Your Changes:
Bash
git commit -m "Add API testing programs and collection"
Push to GitHub:
Bash
git push origin your-branch-name


Answer 8 *

We need to create a Collection in Postman and add the following requests.
GET: Retrieve ObjectURL: https://api.restful-api.dev/objects/5
Positive Case: Send the request. 
Expect 200 OK.
Negative Case: Change 5 to a non-existent ID (e.g., 999999). 
Expect 404 Not Found.POST: Add New ObjectURL: https://api.restful-api.dev/objectsBody (JSON): (Note: ID is auto-generated, do not include it).JSON{ "name": "Apple MacBook Pro 16", "data": { "year": 2023, "price": 1849.99, "CPU model": "Intel Core i9", "Hard disk size": "1 TB" }}
Positive Case: Send valid JSON. 
Expect 200 OK or 201 Created.
Negative Case: Send an empty Body or invalid JSON format. 
Expect 400 Bad Request.PUT: Update ObjectURL: https://api.restful-api.dev/objects/5Body (JSON):JSON{ "name": "Updated Device Name", "data": { "color": "Silver" }}
Positive Case: Use a valid ID. 
Expect 200 OK.
Negative Case: Try to update an ID that doesn't exist. 
Expect 404 Not Found.DELETE: Remove ObjectURL: https://api.restful-api.dev/objects/7
Positive Case: Delete the record. Expect 200 OK (with a success message).
Negative Case: Delete the same ID again. Expect 404 Not Found.


Answer 9 *

JMeter: Thread Group and Response Validation
This task requires you to simulate user traffic to a specific URL and verify the server's response.
Step-by-Step Implementation:
Create a Thread Group: Right-click on Test Plan -> Add -> Threads (Users) -> Thread Group.
Add HTTP Request: Right-click on Thread Group -> Add -> Sampler -> HTTP Request.
Server Name or IP: www.makemytrip.com.
Protocol: https.
Add Response Assertion: Right-click on HTTP Request -> Add -> Assertions -> Response Assertion.
In "Field to Test," select Response Code.
In "Patterns to Test," click Add and enter 200.
Add Listeners: Right-click on Thread Group -> Add -> Listener -> Assertion Results (and also View Results Tree to see details).
Run the Test: Click the green Start button to execute and verify the results in the Assertion Results listener.


Program 10 *

Import pytest
from selenium import webdriver
from selenium.webdriver.common.by import By

//Pytest Fixture to initialize and close the browser
@pytest.fixture
def driver():
    driver = webdriver.Chrome()
    driver.maximize_window()
    yield driver
    driver.quit()

def test_verify_w3schools_logo(driver):
    # Launch the browser and open the URL
    driver.get("https://www.w3schools.com/") #
    
    # Locate the logo using an appropriate selector
    # Typically, the logo has an ID or a specific class
    try:
        logo = driver.find_element(By.ID, "w3-logo") # Common ID for W3Schools logo
        assert logo.is_displayed() #
        print("W3Schools logo is present on the page.")
    except Exception as e:
        pytest.fail(f"Logo not found: {e}")
To Run:
Open your terminal and run the command: pytest test_w3schools.py






