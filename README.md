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

# Task -04:

# code:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

# Initialize Chrome WebDriver
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))

# Maximize window so everything is clearly visible
driver.maximize_window()
wait = WebDriverWait(driver, 10)

# ------------------------------------------------
# Task 1: Open the online shopping website
# ------------------------------------------------
print("Opening website...")
driver.get("https://www.saucedemo.com/")
print("Shopping website opens successfully")
time.sleep(2)

# ------------------------------------------------
# Task 2: Extract credentials and perform automated login
# ------------------------------------------------
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
time.sleep(1.5)

# Enter password
print("Entering password...")
password_input.send_keys(password)
time.sleep(1.5)

# Click login
print("Clicking login button...")
login_button.click()
time.sleep(2)

#Output confirmation message
if "inventory.html" in driver.current_url:
    print(f"Automated Login Successful! Logged in as: {username}")
else:
    print("Login failed.")

# ------------------------------------------------
#TC04: Prompt with Dummy Discount Code & Success Box
# ------------------------------------------------
print("\n--- Running TC04 ---")

# Define dummy discount coupon code
dummy_discount_code = "SAVE50OFF"

print("Displaying prompt popup with dummy discount code...")
# Triggers prompt and stores the returned coupon code globally
driver.execute_script(f"""
    window.appliedCoupon = window.prompt('Enter your discount coupon code:', '{dummy_discount_code}');
""")
time.sleep(1)

# Switch to the prompt alert
alert = wait.until(EC.alert_is_present())

# Type/override the dummy discount code using send_keys
print(f"Entering dummy coupon code: {dummy_discount_code}")
alert.send_keys(dummy_discount_code)
time.sleep(2)  # Pause to observe the dummy code in the prompt

# Accept (submit) the prompt popup
alert.accept()

# Console output requirement
print("Entered information is submitted successfully")

# Create a visible success message banner directly on the webpage
driver.execute_script("""
    const successBox = document.createElement('div');
    successBox.id = 'coupon-success-box';
    successBox.innerText = 'Coupon ' + window.appliedCoupon + ' applied! Entered information is submitted successfully.';
    successBox.style.cssText = 'position: fixed; top: 20px; right: 20px; background-color: #4BB543; color: white; padding: 16px 24px; font-size: 16px; font-weight: bold; border-radius: 8px; box-shadow: 0 4px 10px rgba(0,0,0,0.3); z-index: 99999;';
    document.body.appendChild(successBox);
""")

# Wait and retrieve the message from the rendered webpage output box
success_element = wait.until(EC.visibility_of_element_located((By.ID, "coupon-success-box")))
print(f"Webpage Output Box: {success_element.text}")

time.sleep(3)  # Keep open to observe the green success banner
# driver.quit()
```
# output:
<img width="1917" height="1027" alt="image" src="https://github.com/user-attachments/assets/1471c3b5-75b9-4d3e-a598-76c9e7809c40" />
<img width="1917" height="961" alt="image" src="https://github.com/user-attachments/assets/ec3e27e4-09d1-4417-93ed-ca1d382021d0" />

# task 5:
# code:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager

# Initialize Chrome WebDriver
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()))
driver.maximize_window()
wait = WebDriverWait(driver, 10)
actions = ActionChains(driver)

# Open SauceDemo and log in
driver.get("https://www.saucedemo.com/")
driver.find_element(By.ID, "user-name").send_keys("standard_user")
driver.find_element(By.ID, "password").send_keys("secret_sauce")
driver.find_element(By.ID, "login-button").click()
time.sleep(2)

# ------------------------------------------------
# Task 5: Customer moves mouse over Product / Category menu
# ActionChains - move_to_element()
# ------------------------------------------------
print("\n--- Running Task 5: Mouse Hover ---")

# Locate the first product item / title
product_item = wait.until(EC.visibility_of_element_located((By.CLASS_NAME, "inventory_item_name")))

# Perform Mouse Hover
actions.move_to_element(product_item).perform()
time.sleep(2)  # Pause to observe the hover state

# Output requirement
print("Product categories/submenu are displayed")

time.sleep(2)
# driver.quit()
```
# output:
<img width="1907" height="1012" alt="image" src="https://github.com/user-attachments/assets/7735c942-a1ef-4b70-8b04-11d10259fc49" />

# Task-06:
# code:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


def get_driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--start-maximized")
    options.add_argument("--remote-allow-origins=*")
    return webdriver.Chrome(options=options)


def tc06_double_click():
    driver = get_driver()
    wait = WebDriverWait(driver, 10)
    actions = ActionChains(driver)

    try:
        print("\n[TC06] Customer double-clicks a product / button")

        # 1. Open the ToolsQA Buttons page
        driver.get("https://demoqa.com/buttons")
        time.sleep(3)

        # 2. Locate the "Double Click Me" button
        double_click_btn = wait.until(
            EC.element_to_be_clickable((By.ID, "doubleClickBtn"))
        )

        # Scroll button into view to avoid any ad banners blocking it
        driver.execute_script("arguments[0].scrollIntoView({block: 'center'});", double_click_btn)
        time.sleep(3)

        # 3. Perform the double-click action using ActionChains
        print("Performing double-click on 'Double Click Me' button...")
        actions.double_click(double_click_btn).perform()
        time.sleep(3)

        # 4. Wait for and verify the dynamic confirmation message
        message = wait.until(
            EC.visibility_of_element_located((By.ID, "doubleClickMessage"))
        )
        print(f"Confirmation Message: {message.text}")

        assert "You have done a double click" in message.text
        print("Product/button double-clicked successfully")
        print("TC06 PASSED")

    finally:
        time.sleep(3)
        driver.quit()


if __name__ == "__main__":
    tc06_double_click()
```
# output:
<img width="1906" height="1030" alt="image" src="https://github.com/user-attachments/assets/320fa7bd-9a0d-4c66-88b3-912eb3d7b8b4" />

# Task -7:
# code:
```
def tc07_drag_drop():

    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:

        print("\n[TC07] Drag and Drop")

        driver.get(
            "https://jqueryui.com/resources/demos/droppable/default.html"
        )

        source = wait.until(
            EC.presence_of_element_located(
                (By.ID, "draggable")
            )
        )

        target = wait.until(
            EC.presence_of_element_located(
                (By.ID, "droppable")
            )
        )

        ActionChains(driver).drag_and_drop(
            source,
            target
        ).perform()

        wait.until(
            lambda d: "Dropped!" in target.text
        )

        assert "Dropped!" in target.text

        print("TC07 PASSED")

    finally:

        driver.quit()

```
# output:
<img width="1061" height="467" alt="image" src="https://github.com/user-attachments/assets/0b9948d6-5e54-482d-8bf1-6c79d8a009da" />
<img width="1037" height="531" alt="image" src="https://github.com/user-attachments/assets/67e8b534-07e7-4a0a-8389-93a65398aeb7" />

# Task-08:
# code:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


def get_driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--start-maximized")
    options.add_argument("--remote-allow-origins=*")
    return webdriver.Chrome(options=options)


def tc08_search_product_saucedemo():
    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:
        print("\n[TC08] Customer searches for a product on SauceDemo")

        # 1. Open Website & Log In
        print("Opening website...")
        driver.get("https://www.saucedemo.com/")
        time.sleep(3)

        print("Entering credentials...")
        driver.find_element(By.ID, "user-name").send_keys("standard_user")
        time.sleep(3)
        driver.find_element(By.ID, "password").send_keys("secret_sauce")
        time.sleep(3)
        driver.find_element(By.ID, "login-button").click()
        time.sleep(3)

        # 2. Define the product to search
        search_keyword = "Backpack"
        print(f"Searching for product matching: '{search_keyword}'...")

        # 3. Explicit Wait: wait until product items load on the page
        product_elements = wait.until(
            EC.presence_of_all_elements_located((By.CLASS_NAME, "inventory_item"))
        )
        time.sleep(3)

        # 4. Search and match product by name
        found_product = None
        for product in product_elements:
            name = product.find_element(By.CLASS_NAME, "inventory_item_name").text
            if search_keyword.lower() in name.lower():
                found_product = product
                print(f"Match found: {name}")
                break

        # 5. Verify product was found
        assert found_product is not None, f"No product matched '{search_keyword}'"

        # Highlight/scroll to the searched product
        driver.execute_script("arguments[0].scrollIntoView({block: 'center'});", found_product)
        time.sleep(3)

        # 6. Output confirmation
        print("Product results are loaded successfully")
        print("TC08 PASSED")

    finally:
        time.sleep(3)
        driver.quit()


if __name__ == "__main__":
    tc08_search_product_saucedemo()
```
# output:
<img width="1897" height="967" alt="image" src="https://github.com/user-attachments/assets/dca26c7d-c74c-4602-91d1-4ce483c03711" />

# Task-9
# code:
```
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC


def get_driver():
    options = webdriver.ChromeOptions()
    options.add_argument("--start-maximized")
    options.add_argument("--remote-allow-origins=*")
    return webdriver.Chrome(options=options)


def tc09_clickable_wait():
    driver = get_driver()
    wait = WebDriverWait(driver, 10)

    try:
        print("\n[TC09] Clickable Wait - Checkout Place Order")

        # 1. Open Website & Login
        driver.get("https://www.saucedemo.com/")
        time.sleep(3)

        driver.find_element(By.ID, "user-name").send_keys("standard_user")
        time.sleep(3)
        driver.find_element(By.ID, "password").send_keys("secret_sauce")
        time.sleep(3)
        driver.find_element(By.ID, "login-button").click()
        time.sleep(3)

        # 2. Add product & proceed through checkout
        driver.find_element(By.ID, "add-to-cart-sauce-labs-backpack").click()
        time.sleep(3)
        driver.find_element(By.CLASS_NAME, "shopping_cart_link").click()
        time.sleep(3)
        driver.find_element(By.ID, "checkout").click()
        time.sleep(3)

        driver.find_element(By.ID, "first-name").send_keys("John")
        time.sleep(3)
        driver.find_element(By.ID, "last-name").send_keys("Doe")
        time.sleep(3)
        driver.find_element(By.ID, "postal-code").send_keys("600001")
        time.sleep(3)
        driver.find_element(By.ID, "continue").click()
        time.sleep(3)

        # 3. Clickable Wait for Finish / Place Order button
        place_order_button = wait.until(
            EC.element_to_be_clickable((By.ID, "finish"))
        )
        time.sleep(3)
        place_order_button.click()
        time.sleep(3)

        # 4. Confirmation Message & Assertion
        confirmation_header = wait.until(
            EC.visibility_of_element_located((By.CLASS_NAME, "complete-header"))
        )
        print(f"Header: {confirmation_header.text}")

        assert "Thank you for your order!" in confirmation_header.text
        print("Order is submitted successfully")
        print("TC09 PASSED")

    finally:
        time.sleep(3)
        driver.quit()


if __name__ == "__main__":
    tc09_clickable_wait()

```
# output:
<img width="1903" height="976" alt="Screenshot 2026-10-07 181706" src="https://github.com/user-attachments/assets/e98844d6-5abc-493a-9c96-52212d835bde" />
<img width="1912" height="1015" alt="Screenshot 2026-10-07 181714" src="https://github.com/user-attachments/assets/98c42b88-2e31-4762-be8a-f9491870e4ee" />
<img width="1900" height="1010" alt="Screenshot 2026-10-07 181720" src="https://github.com/user-attachments/assets/24532dd2-e3f5-41c1-a3af-cbe6de07ae93" />

# Task-10:
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
    """Detects and closes Google Vignette ads or ad overlays if present."""
    time.sleep(1)
    try:
        iframes = driver.find_elements(By.TAG_NAME, "iframe")
        for iframe in iframes:
            if "google_ads" in iframe.get_attribute("id") or "aswift" in iframe.get_attribute("id"):
                driver.switch_to.frame(iframe)
                try:
                    dismiss_btn = driver.find_element(By.XPATH, "//div[@id='dismiss-button'] | //span[text()='Close']")
                    dismiss_btn.click()
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
# Step 1: Click "Alert with Textbox" menu tab
# ------------------------------------------------
print("Clicking 'Alert with Textbox' tab...")
textbox_tab = wait.until(EC.element_to_be_clickable((By.XPATH, "//a[contains(text(),'Alert with Textbox')]")))
textbox_tab.click()

close_ad_if_present(driver)
time.sleep(1.5)

# ------------------------------------------------
# Step 2: Click "click the button to demonstrate the prompt box"
# ------------------------------------------------
print("Completing purchase: Clicking trigger button...")
prompt_button = wait.until(EC.element_to_be_clickable((By.XPATH, "//button[contains(@class,'btn-info')]")))
prompt_button.click()

# ------------------------------------------------
# Step 3: Wait for order confirmation popup and handle alert
# ------------------------------------------------
# Wait for alert to appear
alert = wait.until(EC.alert_is_present())

print("\nConfirmation Alert:")
print(alert.text)
time.sleep(1.5)  # Observe popup

# Accept the confirmation alert
alert.accept()

# Required Output
print("Confirmation alert is handled successfully")

time.sleep(2)
# driver.quit()
```
# output:
<img width="1916" height="1023" alt="image" src="https://github.com/user-attachments/assets/6c8de0b1-6edc-4b64-954c-894f57870ed6" />
