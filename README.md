# HCL-DAY-13-Task
# Task01:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

# Initialize Chrome WebDriver
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))

# Maximize window so everything is clearly visible
driver.maximize_window()

# Task 1: Open the online shopping website
print("Opening website...")
driver.get("https://www.saucedemo.com/")
print("Shopping website opens successfully")
time.sleep(2)  # Pause to observe homepage

# Task 2: Extract credentials and perform automated login
print("Scraping credentials from page...")
credentials_text = driver.find_element(By.ID, "login_credentials").text
username = credentials_text.split("\n")[1]  # 'standard_user'

password_text = driver.find_element(By.CLASS_NAME, "login_password").text
password = password_text.split("\n")[1]  # 'secret_sauce'
time.sleep(1)

# Locate input elements
username_input = driver.find_element(By.ID, "user-name")
password_input = driver.find_element(By.ID, "password")
login_button = driver.find_element(By.ID, "login-button")

# Enter username
print(f"Entering username: {username}")
username_input.send_keys(username)
time.sleep(1.5)  # Pause after typing username

# Enter password
print("Entering password...")
password_input.send_keys(password)
time.sleep(1.5)  # Pause after typing password

# Click login
print("Clicking login button...")
login_button.click()
time.sleep(2)  # Pause to observe post-login screen

# Output confirmation message
if "inventory.html" in driver.current_url:
    print(f"Automated Login Successful! Logged in as: {username}")
else:
    print("Login failed.")

# Keep browser open for observation
# driver.quit()
```
# Output:
<img width="1907" height="1026" alt="image" src="https://github.com/user-attachments/assets/37a48044-c205-4812-b912-fe45a360190b" />
<img width="1902" height="1020" alt="image" src="https://github.com/user-attachments/assets/4d9ff465-e03c-4ca7-b905-5d2b34f3a6fe" />

# Task-2:
# code:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import TimeoutException, NoSuchElementException
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

# Initialize Chrome WebDriver
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
driver.maximize_window()
wait = WebDriverWait(driver, 10)

def close_ad_if_present(driver):
    """Detects and closes Google Vignette ads or ad overlays if they pop up."""
    time.sleep(1)
    
    # Handle Google Vignette ad iframe / dismiss button
    try:
        # Check if an ad iframe is present and switch to it
        iframes = driver.find_elements(By.TAG_NAME, "iframe")
        for iframe in iframes:
            if "google_ads" in iframe.get_attribute("id") or "aswift" in iframe.get_attribute("id"):
                driver.switch_to.frame(iframe)
                try:
                    # Click dismiss/close button inside ad iframe if visible
                    dismiss_btn = driver.find_element(By.XPATH, "//div[@id='dismiss-button'] | //span[text()='Close']")
                    dismiss_btn.click()
                    print("Ad closed successfully!")
                except NoSuchElementException:
                    pass
                driver.switch_to.default_content()
    except Exception:
        driver.switch_to.default_content()

    # If full page URL changed to an ad URL (like #google_vignette), navigate back or refresh
    if "google_vignette" in driver.current_url:
        driver.back()
        time.sleep(1)

# 1. Open the Alerts demo page
driver.get("https://demo.automationtesting.in/Alerts.html")
time.sleep(2)

# Check and clear initial ads
close_ad_if_present(driver)

# ------------------------------------------------
# Step 1: Click the "Alert with OK" menu button
# ------------------------------------------------
print("Clicking 'Alert with OK' tab...")
ok_tab = wait.until(EC.element_to_be_clickable((By.XPATH, "//a[contains(text(),'Alert with OK ')]")))
ok_tab.click()

# Handle potential ad after tab click
close_ad_if_present(driver)
time.sleep(1.5)

# ------------------------------------------------
# Step 2: Click the red button to trigger the alert
# ------------------------------------------------
print("Clicking 'click the button to display an alert box:'...")
alert_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[contains(@class,'btn-danger')]")))
alert_button.click()

# ------------------------------------------------
# Step 3: Handle the automated alert popup
# ------------------------------------------------
alert = wait.until(EC.alert_is_present())

print("\nAlert Text:")
print(alert.text)
time.sleep(1.5)  # Pause to observe the open alert box

# Accept the alert (click OK)
alert.accept()
print("Alert accepted successfully!")

time.sleep(4)
# driver.quit()
```
# output:
<img width="1913" height="1015" alt="image" src="https://github.com/user-attachments/assets/1184dec4-cb00-442e-b5fb-e7b828a2fda9" />

# Task03:
# code:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

# Initialize Chrome WebDriver
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
driver.maximize_window()
wait = WebDriverWait(driver, 10)

def close_ad_if_present(driver):
    """Detects and closes Google Vignette ads or ad overlays if they pop up."""
    time.sleep(1)
    try:
        iframes = driver.find_elements(By.TAG_NAME, "iframe")
        for iframe in iframes:
            if "google_ads" in iframe.get_attribute("id") or "aswift" in iframe.get_attribute("id"):
                driver.switch_to.frame(iframe)
                try:
                    dismiss_btn = driver.find_element(By.XPATH, "//div[@id='dismiss-button'] | //span[text()='Close']")
                    dismiss_btn.click()
                    print("Ad closed successfully!")
                except NoSuchElementException:
                    pass
                driver.switch_to.default_content()
    except Exception:
        driver.switch_to.default_content()

    if "google_vignette" in driver.current_url:
        driver.back()
        time.sleep(1)

# 1. Open the Alerts demo page
driver.get("https://demo.automationtesting.in/Alerts.html")
time.sleep(2)
close_ad_if_present(driver)

# ------------------------------------------------
# Step 1: Click "Alert with OK & Cancel" tab
# ------------------------------------------------
print("Clicking 'Alert with OK & Cancel' tab...")
ok_cancel_tab = wait.until(EC.element_to_be_clickable((By.XPATH, "//a[contains(text(),'Alert with OK & Cancel')]")))
ok_cancel_tab.click()

close_ad_if_present(driver)
time.sleep(1.5)

# ------------------------------------------------
# Step 2: Click "click the button to display a confirm box"
# ------------------------------------------------
print("Clicking 'click the button to display a confirm box'...")
confirm_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[contains(@class,'btn-primary')]")))
confirm_button.click()

# ------------------------------------------------
# Step 3: Handle the Confirmation Alert (Accept - OK)
# ------------------------------------------------
alert = wait.until(EC.alert_is_present())

print("\nConfirmation Alert Text:")
print(alert.text)
time.sleep(1.5)  # Pause to observe the alert box

# Accept the alert (clicks OK)
alert.accept()

# Verify page result text after clicking OK
result_text = driver.find_element(By.ID, "demo").text
print(f"Page Result: {result_text}")

time.sleep(4)
# driver.quit()
```
#  output:
<img width="1915" height="1018" alt="image" src="https://github.com/user-attachments/assets/1a1a98d6-5dd0-4dcf-b1fd-81cae09f107a" />
<img width="1901" height="1026" alt="image" src="https://github.com/user-attachments/assets/21c8bbbb-f44d-4f5d-b4fd-9a0b1f47d607" />
