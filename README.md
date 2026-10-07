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
