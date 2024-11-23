# Boring-Stuff

![image](https://github.com/user-attachments/assets/79131045-cf12-4900-a5fa-1f6d3c2e8225)

![image](https://github.com/user-attachments/assets/00f4773c-08b3-4385-bdd6-e0175100bdae)

















## **Lecture: Advanced Exception Handling in Python**

### **Duration:** 3 Hours

### **Outline:**
1. **Recap of Basic Exception Handling (10 minutes)**
2. **Intermediate Exception Handling (40 minutes)**
   - Specific Exception Types
   - Multiple Except Blocks
   - Using `else` Block
   - Using `finally` Block
   - Raising Exceptions
3. **Advanced Exception Handling (60 minutes)**
   - Custom Exceptions
   - Nested Try/Except Blocks
   - Context Managers (`with` Statement)
   - Exception Chaining
   - Best Practices
4. **Hands-On Programs (30 minutes)**
   - Intermediate and Advanced Programs
5. **Scenario-Based Programs (40 minutes)**
   - 10 Real-World Scenario Programs
6. **Q&A and Recap (20 minutes)**

---

### **1. Recap of Basic Exception Handling (10 minutes)**

Before diving into advanced topics, let's briefly recap the basics.

- **Try Block:** Encapsulates code that may raise an exception.
- **Except Block:** Handles the exception if it occurs.

**Basic Syntax:**
```python
try:
    # Code that might raise an exception
except:
    # Code to execute if an exception occurs
```

**Example:**
```python
try:
    a = 10
    b = 0
    print(a / b)
except:
    print("An error occurred!")
# Output: An error occurred!
```

---

### **2. Intermediate Exception Handling (40 minutes)**

#### **2.1. Specific Exception Types**

Instead of catching all exceptions, handle specific ones to provide better error management.

**Common Exception Types:**
- `ZeroDivisionError`
- `ValueError`
- `TypeError`
- `FileNotFoundError`
- `IndexError`
- `KeyError`

**Example: Handling `ZeroDivisionError` and `ValueError`**
```python
try:
    numerator = int(input("Enter numerator: "))
    denominator = int(input("Enter denominator: "))
    result = numerator / denominator
    print(f"Result: {result}")
except ZeroDivisionError:
    print("Error: Cannot divide by zero.")
except ValueError:
    print("Error: Please enter valid integers.")
```

#### **2.2. Multiple Except Blocks**

Handle multiple exceptions using separate `except` blocks.

**Example:**
```python
try:
    value = int(input("Enter a number: "))
    inverse = 1 / value
except ZeroDivisionError:
    print("Error: Division by zero is undefined.")
except ValueError:
    print("Error: Invalid input. Please enter an integer.")
else:
    print(f"The inverse of {value} is {inverse}")
```

**Explanation:**
- **`ZeroDivisionError`:** Triggered when dividing by zero.
- **`ValueError`:** Triggered when input cannot be converted to an integer.
- **`else`:** Executes if no exceptions occur.

#### **2.3. Using `else` Block**

The `else` block runs if no exceptions are raised in the `try` block.

**Example:**
```python
try:
    number = int(input("Enter a number: "))
except ValueError:
    print("That's not a valid number!")
else:
    print(f"You entered {number}")
```

#### **2.4. Using `finally` Block**

The `finally` block executes regardless of whether an exception occurred or not. It's typically used for cleanup actions.

**Example:**
```python
try:
    file = open("sample.txt", "r")
    data = file.read()
except FileNotFoundError:
    print("File not found!")
finally:
    file.close()
    print("File closed.")
```

**Note:** If the `try` block fails before `file` is assigned, `file.close()` will raise an error. To prevent this, initialize `file` to `None` before the `try` block.

**Improved Example:**
```python
file = None
try:
    file = open("sample.txt", "r")
    data = file.read()
except FileNotFoundError:
    print("File not found!")
finally:
    if file:
        file.close()
    print("File closed.")
```

#### **2.5. Raising Exceptions**

Use the `raise` statement to trigger exceptions manually.

**Example:**
```python
def validate_age(age):
    if age < 0:
        raise ValueError("Age cannot be negative.")
    return age

try:
    user_age = int(input("Enter your age: "))
    validated_age = validate_age(user_age)
    print(f"Your age is {validated_age}")
except ValueError as ve:
    print(f"Error: {ve}")
```

---

### **3. Advanced Exception Handling (60 minutes)**

#### **3.1. Custom Exceptions**

Create user-defined exceptions by extending the base `Exception` class.

**Example:**
```python
class NegativeNumberError(Exception):
    def __init__(self, message="Number cannot be negative."):
        self.message = message
        super().__init__(self.message)

def sqrt(number):
    if number < 0:
        raise NegativeNumberError
    return number ** 0.5

try:
    num = float(input("Enter a number to find its square root: "))
    result = sqrt(num)
except NegativeNumberError as ne:
    print(f"Error: {ne}")
else:
    print(f"The square root of {num} is {result}")
```

#### **3.2. Nested Try/Except Blocks**

Handle exceptions within exceptions for more granular control.

**Example:**
```python
try:
    data = {"name": "Alice", "age": 25}
    print(data["name"])
    print(data["address"])  # This will raise KeyError
except KeyError as ke:
    print(f"KeyError: {ke}")
    try:
        print(data["age"])
    except KeyError:
        print("Age not found.")
```

#### **3.3. Context Managers (`with` Statement)**

Automatically handle setup and teardown actions, such as opening and closing files.

**Basic Example:**
```python
try:
    with open("data.txt", "r") as file:
        content = file.read()
        print(content)
except FileNotFoundError:
    print("File not found!")
```

**Creating Custom Context Managers:**
Use the `contextlib` module to create custom context managers.

**Example:**
```python
from contextlib import contextmanager

@contextmanager
def managed_resource():
    print("Resource acquired")
    try:
        yield
    finally:
        print("Resource released")

# Usage
try:
    with managed_resource():
        print("Using resource")
        # Simulate an error
        raise ValueError("Something went wrong!")
except ValueError as ve:
    print(f"Error: {ve}")
```

**Output:**
```
Resource acquired
Using resource
Resource released
Error: Something went wrong!
```

#### **3.4. Exception Chaining**

Link exceptions using `from` keyword to indicate the cause of an exception.

**Example:**
```python
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError as zde:
        raise ValueError("Cannot divide by zero.") from zde

try:
    divide(10, 0)
except ValueError as ve:
    print(f"ValueError: {ve}")
    print(f"Original Exception: {ve.__cause__}")
```

**Output:**
```
ValueError: Cannot divide by zero.
Original Exception: division by zero
```

#### **3.5. Best Practices**

- **Be Specific:** Catch specific exceptions instead of using a bare `except`.
  
  ```python
  # Bad Practice
  try:
      # Code
  except:
      print("An error occurred.")
  
  # Good Practice
  try:
      # Code
  except ValueError:
      print("Invalid value entered.")
  ```

- **Avoid Silent Failures:** Always handle exceptions appropriately; avoid empty `except` blocks.
  
  ```python
  # Bad Practice
  try:
      # Code
  except:
      pass  # Silent failure
  
  # Good Practice
  try:
      # Code
  except Exception as e:
      print(f"An error occurred: {e}")
  ```

- **Use `finally` for Cleanup:** Ensure resources are released properly.
  
- **Log Exceptions:** Use the `logging` module to log exceptions for debugging.

  ```python
  import logging

  logging.basicConfig(filename='app.log', level=logging.ERROR)

  try:
      # Code
  except Exception as e:
      logging.error("An error occurred", exc_info=True)
  ```

- **Don't Use Exceptions for Control Flow:** Exceptions should represent unexpected events, not regular operations.

---

### **4. Hands-On Programs (30 minutes)**

Let's work through some **intermediate and advanced** programs to solidify our understanding of exception handling.

#### **4.1. Program: Safe File Reader**

**Description:** Read a file and handle exceptions related to file operations.

```python
def read_file(filename):
    try:
        with open(filename, "r") as file:
            data = file.read()
            print(data)
    except FileNotFoundError:
        print(f"Error: The file '{filename}' was not found.")
    except PermissionError:
        print(f"Error: Permission denied for file '{filename}'.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("File read successfully.")
    finally:
        print("Operation completed.")

# Usage
read_file("example.txt")
```

#### **4.2. Program: User Input Validation**

**Description:** Continuously prompt the user until valid input is received.

```python
def get_positive_integer():
    while True:
        try:
            num = int(input("Enter a positive integer: "))
            if num <= 0:
                raise ValueError("Number must be positive.")
            return num
        except ValueError as ve:
            print(f"Invalid input: {ve}")

# Usage
number = get_positive_integer()
print(f"You entered: {number}")
```

#### **4.3. Program: Calculator with Custom Exceptions**

**Description:** Perform basic arithmetic operations with custom exception handling.

```python
class CalculatorError(Exception):
    pass

def divide(a, b):
    if b == 0:
        raise CalculatorError("Cannot divide by zero.")
    return a / b

def calculate():
    try:
        a = float(input("Enter first number: "))
        b = float(input("Enter second number: "))
        operation = input("Enter operation (+, -, *, /): ")
        
        if operation == '+':
            print(f"Result: {a + b}")
        elif operation == '-':
            print(f"Result: {a - b}")
        elif operation == '*':
            print(f"Result: {a * b}")
        elif operation == '/':
            result = divide(a, b)
            print(f"Result: {result}")
        else:
            print("Invalid operation.")
    except CalculatorError as ce:
        print(f"Calculator Error: {ce}")
    except ValueError:
        print("Invalid input. Please enter numeric values.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Calculation successful.")
    finally:
        print("Calculator session ended.")

# Usage
calculate()
```

#### **4.4. Program: JSON Data Loader**

**Description:** Load and parse JSON data with exception handling.

```python
import json

def load_json(file_path):
    try:
        with open(file_path, "r") as file:
            data = json.load(file)
            print("JSON data loaded successfully.")
            return data
    except FileNotFoundError:
        print(f"Error: File '{file_path}' not found.")
    except json.JSONDecodeError as jde:
        print(f"Error decoding JSON: {jde}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Load operation completed.")

# Usage
data = load_json("data.json")
if data:
    print(data)
```

#### **4.5. Program: Database Connection Simulator**

**Description:** Simulate a database connection with exception handling.

```python
class DatabaseConnectionError(Exception):
    pass

def connect_to_database(db_name):
    if db_name != "my_database":
        raise DatabaseConnectionError(f"Cannot connect to database '{db_name}'.")
    print(f"Connected to database '{db_name}' successfully.")

def perform_query(db_name, query):
    try:
        connect_to_database(db_name)
        # Simulate query execution
        if not query:
            raise ValueError("Query cannot be empty.")
        print(f"Executing query: {query}")
    except DatabaseConnectionError as dce:
        print(f"Database Error: {dce}")
    except ValueError as ve:
        print(f"Value Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Query executed successfully.")
    finally:
        print("Database operation completed.")

# Usage
perform_query("test_db", "SELECT * FROM users")
```

---

### **5. Scenario-Based Programs (40 minutes)**

Here are **10 real-world scenario-based programs** that utilize `try`, `except`, `else`, and `finally` blocks to handle exceptions effectively.

#### **5.1. Scenario 1: Reading Configuration Files**

**Description:** Read configuration settings from a file, handling missing files and incorrect formats.

```python
import json

def read_config(config_file):
    try:
        with open(config_file, "r") as file:
            config = json.load(file)
            return config
    except FileNotFoundError:
        print(f"Configuration file '{config_file}' not found.")
    except json.JSONDecodeError:
        print(f"Configuration file '{config_file}' contains invalid JSON.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Configuration loaded successfully.")
    finally:
        print("Read config operation completed.")

# Usage
config = read_config("config.json")
```

#### **5.2. Scenario 2: User Authentication System**

**Description:** Authenticate users by checking credentials, handling invalid inputs and authentication failures.

```python
class AuthenticationError(Exception):
    pass

def authenticate(username, password):
    # Dummy authentication logic
    if username != "admin" or password != "password123":
        raise AuthenticationError("Invalid username or password.")
    return True

def login():
    try:
        username = input("Username: ")
        password = input("Password: ")
        if authenticate(username, password):
            print("Login successful.")
    except AuthenticationError as ae:
        print(f"Authentication Error: {ae}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("User authenticated.")
    finally:
        print("Login attempt finished.")

# Usage
login()
```

#### **5.3. Scenario 3: Payment Processing**

**Description:** Process payments, handling insufficient funds and invalid payment details.

```python
class PaymentError(Exception):
    pass

def process_payment(balance, amount):
    if amount > balance:
        raise PaymentError("Insufficient funds.")
    return balance - amount

def make_payment():
    try:
        balance = float(input("Enter your current balance: "))
        amount = float(input("Enter payment amount: "))
        new_balance = process_payment(balance, amount)
    except PaymentError as pe:
        print(f"Payment Error: {pe}")
    except ValueError:
        print("Invalid input. Please enter numeric values.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print(f"Payment successful. New balance: {new_balance}")
    finally:
        print("Payment processing completed.")

# Usage
make_payment()
```

#### **5.4. Scenario 4: File Upload System**

**Description:** Upload files, handling file size limits and unsupported formats.

```python
class FileUploadError(Exception):
    pass

def upload_file(file_name, file_size):
    allowed_extensions = ['.jpg', '.png', '.gif']
    max_size = 5 * 1024 * 1024  # 5 MB
    if not any(file_name.endswith(ext) for ext in allowed_extensions):
        raise FileUploadError("Unsupported file format.")
    if file_size > max_size:
        raise FileUploadError("File size exceeds the maximum limit.")
    print(f"File '{file_name}' uploaded successfully.")

def upload():
    try:
        file_name = input("Enter file name: ")
        file_size = int(input("Enter file size in bytes: "))
        upload_file(file_name, file_size)
    except FileUploadError as fue:
        print(f"Upload Error: {fue}")
    except ValueError:
        print("Invalid file size. Please enter an integer value.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Upload completed.")
    finally:
        print("Upload operation finished.")

# Usage
upload()
```

#### **5.5. Scenario 5: Data Export to CSV**

**Description:** Export data to a CSV file, handling I/O errors and data formatting issues.

```python
import csv

def export_to_csv(data, filename):
    try:
        with open(filename, "w", newline='') as csvfile:
            writer = csv.writer(csvfile)
            writer.writerow(["Name", "Age", "Email"])
            for entry in data:
                writer.writerow(entry)
    except IOError:
        print(f"Error: Unable to write to file '{filename}'.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print(f"Data exported to '{filename}' successfully.")
    finally:
        print("Export operation completed.")

# Usage
data = [
    ["Alice", 30, "alice@example.com"],
    ["Bob", 25, "bob@example.com"],
    ["Charlie", 35, "charlie@example.com"]
]
export_to_csv(data, "users.csv")
```

#### **5.6. Scenario 6: Web Scraping with Requests**

**Description:** Scrape data from a website, handling network errors and invalid URLs.

```python
import requests

def fetch_page(url):
    try:
        response = requests.get(url, timeout=5)
        response.raise_for_status()  # Raises HTTPError for bad responses
        return response.text
    except requests.exceptions.HTTPError as he:
        print(f"HTTP Error: {he}")
    except requests.exceptions.ConnectionError:
        print("Error: Failed to connect to the server.")
    except requests.exceptions.Timeout:
        print("Error: The request timed out.")
    except requests.exceptions.RequestException as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Page fetched successfully.")
    finally:
        print("Fetch operation completed.")

# Usage
url = input("Enter the URL to fetch: ")
content = fetch_page(url)
if content:
    print(content[:1000])  # Print first 1000 characters
```

#### **5.7. Scenario 7: Image Processing**

**Description:** Process images, handling unsupported formats and corrupted files.

```python
from PIL import Image

class ImageProcessingError(Exception):
    pass

def process_image(image_path):
    try:
        with Image.open(image_path) as img:
            img = img.convert("L")  # Convert to grayscale
            img.save("processed_" + image_path)
    except FileNotFoundError:
        print(f"Error: Image '{image_path}' not found.")
    except OSError:
        print(f"Error: Cannot process image '{image_path}'. It may be corrupted or in an unsupported format.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print(f"Image '{image_path}' processed successfully.")
    finally:
        print("Image processing completed.")

# Usage
image_path = input("Enter the image file name: ")
process_image(image_path)
```

#### **5.8. Scenario 8: Email Sending System**

**Description:** Send emails, handling SMTP server errors and invalid email addresses.

```python
import smtplib
from email.mime.text import MIMEText

def send_email(sender, recipient, subject, body, smtp_server='smtp.example.com', port=587):
    try:
        msg = MIMEText(body)
        msg['Subject'] = subject
        msg['From'] = sender
        msg['To'] = recipient
        
        with smtplib.SMTP(smtp_server, port) as server:
            server.starttls()
            # server.login('username', 'password')  # Uncomment and set credentials
            server.send_message(msg)
    except smtplib.SMTPAuthenticationError:
        print("Error: Authentication failed. Check your username and password.")
    except smtplib.SMTPConnectError:
        print("Error: Unable to connect to the SMTP server.")
    except smtplib.SMTPRecipientsRefused:
        print("Error: The recipient address was refused by the server.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Email sent successfully.")
    finally:
        print("Email sending operation completed.")

# Usage
sender = input("Enter your email address: ")
recipient = input("Enter recipient's email address: ")
subject = input("Enter email subject: ")
body = input("Enter email body: ")
send_email(sender, recipient, subject, body)
```

**Note:** Replace `'smtp.example.com'` and the port with your SMTP server details. Uncomment and set the `server.login` credentials as needed.

#### **5.9. Scenario 9: Stock Market Data Analysis**

**Description:** Analyze stock data, handling missing data and calculation errors.

```python
import pandas as pd

def analyze_stock(file_path):
    try:
        data = pd.read_csv(file_path)
        if data.empty:
            raise ValueError("CSV file is empty.")
        average_price = data['Close'].mean()
        print(f"Average Closing Price: {average_price}")
    except FileNotFoundError:
        print(f"Error: File '{file_path}' not found.")
    except pd.errors.EmptyDataError:
        print("Error: No data found in the CSV file.")
    except KeyError:
        print("Error: 'Close' column not found in the data.")
    except ValueError as ve:
        print(f"Value Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Stock data analysis completed successfully.")
    finally:
        print("Analysis operation finished.")

# Usage
file_path = input("Enter the path to the stock data CSV file: ")
analyze_stock(file_path)
```

#### **5.10. Scenario 10: Automated Backup System**

**Description:** Backup important files, handling file access errors and storage issues.

```python
import shutil
import os

def backup_files(source_dir, backup_dir):
    try:
        if not os.path.exists(backup_dir):
            os.makedirs(backup_dir)
        shutil.copytree(source_dir, os.path.join(backup_dir, os.path.basename(source_dir)))
    except FileNotFoundError:
        print(f"Error: Source directory '{source_dir}' does not exist.")
    except PermissionError:
        print("Error: Permission denied while accessing directories.")
    except shutil.Error as se:
        print(f"Shutil Error: {se}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print(f"Backup of '{source_dir}' completed successfully.")
    finally:
        print("Backup operation finished.")

# Usage
source_dir = input("Enter the source directory to backup: ")
backup_dir = input("Enter the backup directory: ")
backup_files(source_dir, backup_dir)
```

## **1. User Registration Validation**

**Description:** Validate user inputs during registration, ensuring that the username is alphanumeric and the password meets certain criteria.

```python
def register_user():
    try:
        username = input("Enter username (alphanumeric only): ")
        if not username.isalnum():
            raise ValueError("Username must be alphanumeric.")
        
        password = input("Enter password (minimum 6 characters): ")
        if len(password) < 6:
            raise ValueError("Password must be at least 6 characters long.")
        
        print("User registered successfully!")
    except ValueError as ve:
        print(f"Registration Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Registration attempt completed.")

# Usage
register_user()
```

**Explanation:**
- **Validation Checks:** Ensures the username is alphanumeric and the password is at least 6 characters.
- **Exception Handling:** Raises `ValueError` for invalid inputs and catches unexpected exceptions.
- **Finally Block:** Executes regardless of success or failure, indicating the end of the registration attempt.

---

## **2. Safe Integer Conversion**

**Description:** Convert user input to an integer, handling invalid inputs gracefully.

```python
def convert_to_int():
    try:
        user_input = input("Enter an integer: ")
        number = int(user_input)
        print(f"You entered the integer: {number}")
    except ValueError:
        print("Error: That was not a valid integer.")
    finally:
        print("Conversion attempt finished.")

# Usage
convert_to_int()
```

**Explanation:**
- **Conversion Attempt:** Tries to convert user input to an integer.
- **Exception Handling:** Catches `ValueError` if the input is not a valid integer.
- **Finally Block:** Indicates the completion of the conversion attempt.

---

## **3. File Content Reader**

**Description:** Read and display the contents of a specified file, handling scenarios where the file may not exist.

```python
def read_file_contents():
    try:
        filename = input("Enter the filename to read: ")
        with open(filename, 'r') as file:
            content = file.read()
            print("File Content:")
            print(content)
    except FileNotFoundError:
        print(f"Error: The file '{filename}' does not exist.")
    except IOError:
        print(f"Error: Cannot read the file '{filename}'.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("File read operation completed.")

# Usage
read_file_contents()
```

**Explanation:**
- **File Operations:** Attempts to open and read the specified file.
- **Exception Handling:** Handles `FileNotFoundError` and general `IOError`.
- **Finally Block:** Indicates the end of the file reading operation.

---

## **4. Simple Calculator**

**Description:** Perform basic arithmetic operations, handling division by zero and invalid operator inputs.

```python
def simple_calculator():
    try:
        num1 = float(input("Enter first number: "))
        operator = input("Enter operator (+, -, *, /): ")
        num2 = float(input("Enter second number: "))
        
        if operator == '+':
            result = num1 + num2
        elif operator == '-':
            result = num1 - num2
        elif operator == '*':
            result = num1 * num2
        elif operator == '/':
            if num2 == 0:
                raise ZeroDivisionError("Cannot divide by zero.")
            result = num1 / num2
        else:
            raise ValueError("Invalid operator.")
        
        print(f"Result: {result}")
    except ZeroDivisionError as zde:
        print(f"Calculation Error: {zde}")
    except ValueError as ve:
        print(f"Input Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Calculator operation completed.")

# Usage
simple_calculator()
```

**Explanation:**
- **Arithmetic Operations:** Performs addition, subtraction, multiplication, and division.
- **Exception Handling:** Catches `ZeroDivisionError` for division by zero and `ValueError` for invalid operators.
- **Finally Block:** Indicates the completion of the calculator operation.

---

## **5. Age Verification System**

**Description:** Verify if a user is eligible to vote based on their age, handling invalid age inputs.

```python
def verify_age():
    try:
        age = int(input("Enter your age: "))
        if age < 0:
            raise ValueError("Age cannot be negative.")
        elif age < 18:
            print("You are not eligible to vote.")
        else:
            print("You are eligible to vote.")
    except ValueError as ve:
        print(f"Input Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Age verification completed.")

# Usage
verify_age()
```

**Explanation:**
- **Age Checks:** Validates that age is non-negative and determines voting eligibility.
- **Exception Handling:** Raises and catches `ValueError` for negative ages.
- **Finally Block:** Indicates the end of the age verification process.

---

## **6. Shopping Cart Total Calculator**

**Description:** Calculate the total cost of items in a shopping cart, handling invalid price inputs.

```python
def calculate_total():
    try:
        items = int(input("Enter the number of items: "))
        if items < 0:
            raise ValueError("Number of items cannot be negative.")
        
        total = 0.0
        for i in range(1, items + 1):
            price = float(input(f"Enter price of item {i}: "))
            if price < 0:
                raise ValueError("Price cannot be negative.")
            total += price
        
        print(f"Total cost: ${total:.2f}")
    except ValueError as ve:
        print(f"Input Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Total calculation completed.")

# Usage
calculate_total()
```

**Explanation:**
- **Total Calculation:** Sums up the prices of the specified number of items.
- **Exception Handling:** Handles negative number of items and negative prices with `ValueError`.
- **Finally Block:** Indicates the completion of the total cost calculation.

---

## **7. Temperature Converter**

**Description:** Convert temperatures between Celsius and Fahrenheit, handling invalid numeric inputs.

```python
def temperature_converter():
    try:
        temp = float(input("Enter temperature: "))
        scale = input("Convert to (C)elsius or (F)ahrenheit? ").upper()
        
        if scale == 'C':
            converted = (temp - 32) * 5/9
            print(f"{temp}°F is {converted:.2f}°C")
        elif scale == 'F':
            converted = (temp * 9/5) + 32
            print(f"{temp}°C is {converted:.2f}°F")
        else:
            raise ValueError("Invalid scale selected. Choose 'C' or 'F'.")
    except ValueError as ve:
        print(f"Input Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Temperature conversion completed.")

# Usage
temperature_converter()
```

**Explanation:**
- **Conversion Logic:** Converts temperature based on user-selected scale.
- **Exception Handling:** Catches invalid scale selections and non-numeric temperature inputs.
- **Finally Block:** Indicates the end of the conversion process.

---

## **8. Password Strength Checker**

**Description:** Check the strength of a user-provided password, ensuring it meets certain criteria.

```python
def check_password_strength():
    try:
        password = input("Enter your password: ")
        if len(password) < 8:
            raise ValueError("Password must be at least 8 characters long.")
        if not any(char.isupper() for char in password):
            raise ValueError("Password must contain at least one uppercase letter.")
        if not any(char.islower() for char in password):
            raise ValueError("Password must contain at least one lowercase letter.")
        if not any(char.isdigit() for char in password):
            raise ValueError("Password must contain at least one digit.")
        
        print("Password is strong.")
    except ValueError as ve:
        print(f"Password Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Password strength check completed.")

# Usage
check_password_strength()
```

**Explanation:**
- **Strength Criteria:** Ensures password length, presence of uppercase and lowercase letters, and digits.
- **Exception Handling:** Raises `ValueError` if any criteria are not met.
- **Finally Block:** Indicates the completion of the password strength check.

---

## **9. Loan Eligibility Checker**

**Description:** Determine loan eligibility based on user's income and credit score, handling invalid inputs.

```python
def loan_eligibility():
    try:
        income = float(input("Enter your annual income: "))
        credit_score = int(input("Enter your credit score: "))
        
        if income < 0:
            raise ValueError("Income cannot be negative.")
        if credit_score < 0 or credit_score > 850:
            raise ValueError("Credit score must be between 0 and 850.")
        
        if income > 50000 and credit_score > 700:
            print("You are eligible for the loan.")
        else:
            print("You are not eligible for the loan.")
    except ValueError as ve:
        print(f"Input Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Loan eligibility check completed.")

# Usage
loan_eligibility()
```

**Explanation:**
- **Eligibility Criteria:** Based on income and credit score thresholds.
- **Exception Handling:** Handles negative income and invalid credit score ranges.
- **Finally Block:** Indicates the end of the eligibility check.

---

## **10. Shopping Discount Calculator**

**Description:** Calculate discounts on purchases based on the total amount, handling invalid purchase amounts.

```python
def discount_calculator():
    try:
        purchase_amount = float(input("Enter your total purchase amount: $"))
        if purchase_amount < 0:
            raise ValueError("Purchase amount cannot be negative.")
        
        if purchase_amount >= 500:
            discount = 0.20
        elif purchase_amount >= 200:
            discount = 0.10
        elif purchase_amount >= 100:
            discount = 0.05
        else:
            discount = 0.0
        
        discounted_price = purchase_amount * (1 - discount)
        print(f"Original Price: ${purchase_amount:.2f}")
        print(f"Discount: {discount * 100:.0f}%")
        print(f"Discounted Price: ${discounted_price:.2f}")
    except ValueError as ve:
        print(f"Input Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Discount calculation completed.")

# Usage
discount_calculator()
```

**Explanation:**
- **Discount Logic:** Applies discounts based on purchase amount tiers.
- **Exception Handling:** Catches negative purchase amounts.
- **Finally Block:** Indicates the completion of the discount calculation.

---

## **11. Email Address Validator**

**Description:** Validate the format of an email address, ensuring it contains an '@' symbol and a domain.

```python
def validate_email():
    try:
        email = input("Enter your email address: ")
        if '@' not in email or email.startswith('@') or email.endswith('@'):
            raise ValueError("Invalid email format.")
        local_part, domain = email.split('@', 1)
        if '.' not in domain:
            raise ValueError("Invalid email domain.")
        
        print("Email address is valid.")
    except ValueError as ve:
        print(f"Validation Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Email validation completed.")

# Usage
validate_email()
```

**Explanation:**
- **Validation Rules:** Checks for presence and proper placement of '@' and '.' in the domain.
- **Exception Handling:** Raises `ValueError` for invalid formats.
- **Finally Block:** Indicates the end of the email validation process.

---

## **12. Inventory Management System**

**Description:** Update inventory counts for products, ensuring that stock levels do not go negative.

```python
def update_inventory():
    try:
        products = {
            'apple': 50,
            'banana': 100,
            'orange': 75
        }
        
        print("Current Inventory:")
        for item, count in products.items():
            print(f"{item}: {count}")
        
        product = input("Enter the product to update: ").lower()
        if product not in products:
            raise KeyError(f"Product '{product}' does not exist in inventory.")
        
        change = int(input("Enter the change in stock (positive to add, negative to remove): "))
        new_count = products[product] + change
        if new_count < 0:
            raise ValueError("Stock level cannot be negative.")
        
        products[product] = new_count
        print(f"Updated '{product}' stock to {products[product]}.")
    except KeyError as ke:
        print(f"Inventory Error: {ke}")
    except ValueError as ve:
        print(f"Stock Level Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Inventory update process completed.")

# Usage
update_inventory()
```

**Explanation:**
- **Inventory Updates:** Allows adding or removing stock for existing products.
- **Exception Handling:** Handles non-existent products and attempts to set negative stock levels.
- **Finally Block:** Indicates the completion of the inventory update process.

---

## **13. Simple Voting System**

**Description:** Allow users to vote for candidates, ensuring that each voter can vote only once.

```python
def voting_system():
    try:
        voters = {}
        candidates = ['Alice', 'Bob', 'Charlie']
        print("Candidates:")
        for candidate in candidates:
            print(f"- {candidate}")
        
        voter_id = input("Enter your voter ID: ")
        if voter_id in voters:
            raise PermissionError("You have already voted.")
        
        vote = input("Enter your chosen candidate: ")
        if vote not in candidates:
            raise ValueError("Candidate does not exist.")
        
        voters[voter_id] = vote
        print(f"Thank you for voting for {vote}!")
    except PermissionError as pe:
        print(f"Voting Error: {pe}")
    except ValueError as ve:
        print(f"Vote Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Voting process completed.")

# Usage
voting_system()
```

**Explanation:**
- **Voting Logic:** Allows a voter to vote for a candidate if they haven't voted before.
- **Exception Handling:** Prevents double voting and handles invalid candidate selections.
- **Finally Block:** Indicates the end of the voting process.

---

## **14. Book Rental System**

**Description:** Rent books to users, ensuring that books are available and handling invalid user inputs.

```python
def book_rental_system():
    try:
        inventory = {
            'The Great Gatsby': 3,
            '1984': 5,
            'To Kill a Mockingbird': 2
        }
        
        print("Available Books:")
        for book, stock in inventory.items():
            print(f"'{book}': {stock} copies available")
        
        book_choice = input("Enter the name of the book you want to rent: ")
        if book_choice not in inventory:
            raise KeyError(f"Book '{book_choice}' is not available in inventory.")
        if inventory[book_choice] == 0:
            raise ValueError(f"Sorry, '{book_choice}' is currently out of stock.")
        
        inventory[book_choice] -= 1
        print(f"You have successfully rented '{book_choice}'.")
    except KeyError as ke:
        print(f"Rental Error: {ke}")
    except ValueError as ve:
        print(f"Stock Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Book rental process completed.")

# Usage
book_rental_system()
```

**Explanation:**
- **Rental Logic:** Allows renting a book if it's available in the inventory.
- **Exception Handling:** Handles non-existent books and out-of-stock scenarios.
- **Finally Block:** Indicates the completion of the rental process.

---

## **15. Password Change System**

**Description:** Allow users to change their password, ensuring the new password meets security criteria and matches the confirmation.

```python
def change_password():
    try:
        stored_password = "SecurePass123"
        
        current_password = input("Enter your current password: ")
        if current_password != stored_password:
            raise PermissionError("Current password is incorrect.")
        
        new_password = input("Enter your new password: ")
        confirm_password = input("Confirm your new password: ")
        
        if new_password != confirm_password:
            raise ValueError("New password and confirmation do not match.")
        if len(new_password) < 8:
            raise ValueError("New password must be at least 8 characters long.")
        if not any(char.isdigit() for char in new_password):
            raise ValueError("New password must contain at least one digit.")
        if not any(char.isupper() for char in new_password):
            raise ValueError("New password must contain at least one uppercase letter.")
        
        stored_password = new_password
        print("Password changed successfully!")
    except PermissionError as pe:
        print(f"Authentication Error: {pe}")
    except ValueError as ve:
        print(f"Password Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    finally:
        print("Password change process completed.")

# Usage
change_password()
```

**Explanation:**
- **Password Change Logic:** Validates current password, ensures new password matches confirmation and meets security criteria.
- **Exception Handling:** Handles incorrect current passwords, mismatched confirmations, and weak new passwords.
- **Finally Block:** Indicates the end of the password change process.


Certainly! Below are **5 scenario-based Python programs** that integrate **regular expressions (regex)** with **exception handling**. Each program addresses a unique real-world scenario, utilizing a wide range of regex characters and methods to demonstrate robust pattern matching and validation. These programs are designed to be **intermediate-level**, showcasing the power of regex in conjunction with Python's exception handling mechanisms.

---

## **1. Comprehensive Email Address Validator**

**Description:**
This program validates user-entered email addresses using a comprehensive regex pattern. It ensures that the email adheres to standard formatting rules, including proper placement of characters, domain validation, and exclusion of invalid characters. The program handles invalid inputs gracefully by raising and catching exceptions.

**Code:**
```python
import re

def validate_email():
    try:
        email = input("Enter your email address: ").strip()
        
        # Comprehensive regex pattern for email validation
        email_pattern = re.compile(
            r"^(?!\.)[A-Za-z0-9._%+-]{1,64}(?<!\.)@"  # Local part
            r"(?!-)[A-Za-z0-9-]{1,63}(?<!-)\."        # Domain name
            r"[A-Za-z]{2,6}$"                         # Top-level domain
        )
        
        if not email_pattern.match(email):
            raise ValueError("Invalid email format.")
        
        print("Email address is valid.")
    except ValueError as ve:
        print(f"Validation Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Email validation successful.")
    finally:
        print("Email validation process completed.")

# Usage
validate_email()
```

**Explanation:**
- **Regex Breakdown:**
  - `^(?!\.)`: Asserts that the email does not start with a dot.
  - `[A-Za-z0-9._%+-]{1,64}`: Allows 1 to 64 characters comprising letters, numbers, and specific special characters.
  - `(?<!\.)@`: Ensures that the `@` symbol is not preceded by a dot.
  - `(?!-)[A-Za-z0-9-]{1,63}(?<!-)\.`: Validates the domain name, ensuring it doesn't start or end with a hyphen and is followed by a dot.
  - `[A-Za-z]{2,6}$`: Validates the top-level domain (e.g., `.com`, `.org`) to be 2 to 6 letters long.
  
- **Exception Handling:**
  - Raises a `ValueError` if the email does not match the regex pattern.
  - Catches and reports any unexpected exceptions.
  
- **Finally Block:**
  - Indicates the completion of the email validation process regardless of the outcome.

---

## **2. Password Strength Checker**

**Description:**
This program assesses the strength of a user-provided password by enforcing multiple security criteria using regex. It ensures the password contains uppercase and lowercase letters, digits, special characters, and meets a minimum length requirement. Invalid passwords trigger exceptions, which are handled gracefully.

**Code:**
```python
import re

def check_password_strength():
    try:
        password = input("Enter your password: ")
        
        # Regex patterns for different criteria
        length_pattern = re.compile(r".{8,}")
        uppercase_pattern = re.compile(r"[A-Z]")
        lowercase_pattern = re.compile(r"[a-z]")
        digit_pattern = re.compile(r"\d")
        special_char_pattern = re.compile(r"[!@#$%^&*(),.?\":{}|<>]")
        whitespace_pattern = re.compile(r"\s")
        
        # Validations
        if not length_pattern.search(password):
            raise ValueError("Password must be at least 8 characters long.")
        if not uppercase_pattern.search(password):
            raise ValueError("Password must contain at least one uppercase letter.")
        if not lowercase_pattern.search(password):
            raise ValueError("Password must contain at least one lowercase letter.")
        if not digit_pattern.search(password):
            raise ValueError("Password must contain at least one digit.")
        if not special_char_pattern.search(password):
            raise ValueError("Password must contain at least one special character (!@#$%^&*(),.?\":{}|<>).")
        if whitespace_pattern.search(password):
            raise ValueError("Password must not contain any whitespace characters.")
        
        print("Password is strong.")
    except ValueError as ve:
        print(f"Password Strength Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Password strength validation successful.")
    finally:
        print("Password strength check completed.")

# Usage
check_password_strength()
```

**Explanation:**
- **Regex Patterns:**
  - `.{8,}`: Ensures the password is at least 8 characters long.
  - `[A-Z]`: Checks for at least one uppercase letter.
  - `[a-z]`: Checks for at least one lowercase letter.
  - `\d`: Checks for at least one digit.
  - `[!@#$%^&*(),.?":{}|<>]`: Checks for at least one special character.
  - `\s`: Ensures no whitespace characters are present.
  
- **Exception Handling:**
  - Raises `ValueError` with specific messages for each unmet criterion.
  - Catches and reports any unexpected exceptions.
  
- **Finally Block:**
  - Indicates the completion of the password strength check regardless of the outcome.

---

## **3. Phone Number Formatter and Validator**

**Description:**
This program takes a user-entered phone number, validates its format using regex, and formats it into a standardized form (e.g., `(123) 456-7890`). It handles various invalid input scenarios by raising and catching exceptions.

**Code:**
```python
import re

def format_phone_number():
    try:
        phone_input = input("Enter your phone number (e.g., 123-456-7890): ").strip()
        
        # Regex pattern to match different phone number formats
        phone_pattern = re.compile(
            r"^(?:\+1\s?)?(?:\(\d{3}\)|\d{3})[-.\s]?\d{3}[-.\s]?\d{4}$"
        )
        
        if not phone_pattern.match(phone_input):
            raise ValueError("Invalid phone number format.")
        
        # Extract digits
        digits = re.sub(r"\D", "", phone_input)
        
        if len(digits) == 10:
            formatted_phone = f"({digits[:3]}) {digits[3:6]}-{digits[6:]}"
        elif len(digits) == 11 and digits.startswith('1'):
            formatted_phone = f"+1 ({digits[1:4]}) {digits[4:7]}-{digits[7:]}"
        else:
            raise ValueError("Phone number must have 10 digits or 11 digits starting with '1'.")
        
        print(f"Formatted Phone Number: {formatted_phone}")
    except ValueError as ve:
        print(f"Phone Number Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Phone number validation and formatting successful.")
    finally:
        print("Phone number processing completed.")

# Usage
format_phone_number()
```

**Explanation:**
- **Regex Pattern:**
  - `^(?:\+1\s?)?`: Optional country code (`+1`) with optional space.
  - `(?:\(\d{3}\)|\d{3})`: Area code with or without parentheses.
  - `[-.\s]?`: Optional separator (`-`, `.`, or space).
  - `\d{3}`: Central office code.
  - `[-.\s]?`: Optional separator.
  - `\d{4}$`: Line number.
  
- **Formatting Logic:**
  - Removes all non-digit characters using `re.sub(r"\D", "", phone_input)`.
  - Formats based on the number of digits:
    - **10 digits:** `(123) 456-7890`
    - **11 digits starting with '1':** `+1 (234) 567-8901`
  
- **Exception Handling:**
  - Raises `ValueError` for invalid formats or incorrect number of digits.
  - Catches and reports any unexpected exceptions.
  
- **Finally Block:**
  - Indicates the completion of the phone number processing.

---

## **4. Date Extractor and Validator**

**Description:**
This program extracts dates from a user-provided text input using regex and validates their format. It supports multiple date formats (e.g., `MM/DD/YYYY`, `DD-MM-YYYY`, `YYYY.MM.DD`) and handles invalid date entries by raising exceptions.

**Code:**
```python
import re

def extract_and_validate_dates():
    try:
        text = input("Enter text containing dates: ")
        
        # Regex pattern to match different date formats
        date_pattern = re.compile(
            r"\b("
            r"(0?[1-9]|1[0-2])[\/\-\.](0?[1-9]|[12]\d|3[01])[\/\-\.](\d{4})"  # MM/DD/YYYY, MM-DD-YYYY, MM.DD.YYYY
            r"|"
            r"(0?[1-9]|[12]\d|3[01])[\/\-\.](0?[1-9]|1[0-2])[\/\-\.](\d{4})"  # DD/MM/YYYY, DD-MM-YYYY, DD.MM.YYYY
            r"|"
            r"(\d{4})[\/\-\.](0?[1-9]|1[0-2])[\/\-\.](0?[1-9]|[12]\d|3[01])"    # YYYY/MM/DD, YYYY-MM-DD, YYYY.MM.DD
            r")\b"
        )
        
        matches = date_pattern.findall(text)
        
        if not matches:
            raise ValueError("No valid dates found in the input.")
        
        # Extract matched dates
        dates = [match[0] or match[4] for match in matches]
        
        print("Extracted Dates:")
        for date in dates:
            print(f"- {date}")
    except ValueError as ve:
        print(f"Date Extraction Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("Date extraction and validation successful.")
    finally:
        print("Date processing completed.")

# Usage
extract_and_validate_dates()
```

**Explanation:**
- **Regex Pattern:**
  - Matches three date formats:
    1. `MM/DD/YYYY`, `MM-DD-YYYY`, `MM.DD.YYYY`
    2. `DD/MM/YYYY`, `DD-MM-YYYY`, `DD.MM.YYYY`
    3. `YYYY/MM/DD`, `YYYY-MM-DD`, `YYYY.MM.DD`
  - Utilizes word boundaries (`\b`) to ensure accurate matching.
  - Uses non-capturing groups `(?:...)` to handle different separators.
  
- **Extraction Logic:**
  - Uses `findall` to retrieve all matching date patterns.
  - Extracts dates from the matched groups.
  
- **Exception Handling:**
  - Raises `ValueError` if no valid dates are found.
  - Catches and reports any unexpected exceptions.
  
- **Finally Block:**
  - Indicates the completion of the date extraction and validation process.

---

## **5. URL Validator and Component Extractor**

**Description:**
This program validates user-entered URLs using regex and extracts their components (protocol, domain, path, query parameters). It handles invalid URLs by raising exceptions and provides meaningful error messages.

**Code:**
```python
import re

def validate_and_extract_url():
    try:
        url = input("Enter a URL: ").strip()
        
        # Comprehensive regex pattern for URL validation and extraction
        url_pattern = re.compile(
            r'^(https?:\/\/)'                      # Protocol
            r'(([A-Za-z0-9-]+\.)+[A-Za-z]{2,6})'   # Domain
            r'(:\d+)?'                             # Optional port
            r'(\/[A-Za-z0-9\-._~:/?#\[\]@!$&\'()*+,;=%]*)?$'  # Path and query
        )
        
        match = url_pattern.match(url)
        if not match:
            raise ValueError("Invalid URL format.")
        
        protocol = match.group(1)
        domain = match.group(2)
        port = match.group(3) if match.group(3) else "Default port"
        path_query = match.group(4) if match.group(4) else "/"
        
        print("URL Components:")
        print(f"- Protocol: {protocol}")
        print(f"- Domain: {domain}")
        print(f"- Port: {port}")
        print(f"- Path & Query: {path_query}")
    except ValueError as ve:
        print(f"URL Validation Error: {ve}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")
    else:
        print("URL validation and extraction successful.")
    finally:
        print("URL processing completed.")

# Usage
validate_and_extract_url()
```

**Explanation:**
- **Regex Pattern:**
  - `^(https?:\/\/)`: Captures the protocol (`http://` or `https://`).
  - `(([A-Za-z0-9-]+\.)+[A-Za-z]{2,6})`: Captures the domain name with subdomains and top-level domain (TLD).
  - `(:\d+)?`: Optionally captures the port number (e.g., `:8080`).
  - `(\/[A-Za-z0-9\-._~:/?#\[\]@!$&\'()*+,;=%]*)?$`: Captures the path and query parameters.
  
- **Extraction Logic:**
  - Extracts the protocol, domain, port, and path/query components from the URL.
  - Assigns default values if certain components are missing (e.g., default port, root path).
  
- **Exception Handling:**
  - Raises `ValueError` if the URL does not match the regex pattern.
  - Catches and reports any unexpected exceptions.
  
- **Finally Block:**
  - Indicates the completion of the URL validation and extraction process.

---

Certainly! Below are **3 scenario-based Python programs** that demonstrate the use of **decorators**. Each program includes a **description**, the **code with decorators**, and an **explanation** of how decorators are utilized within the scenario. These examples are designed to help you understand how decorators can enhance and modify the behavior of functions in real-world applications.

---

## **1. Logging Decorator for Function Calls**

### **Scenario: Monitoring Function Usage in an Application**

**Description:**
In a large application, it's essential to monitor when certain functions are called, along with their input arguments and return values. This helps in debugging, auditing, and analyzing the application's behavior. A **logging decorator** can automate this monitoring by wrapping target functions and logging pertinent information every time they are invoked.

### **Code:**
```python
import functools
import logging

# Configure logging
logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')

def log_function_call(func):
    """Decorator to log function calls with arguments and return values."""
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        # Log the function call with arguments
        args_repr = [repr(a) for a in args]                      # Represent positional arguments
        kwargs_repr = [f"{k}={v!r}" for k, v in kwargs.items()]  # Represent keyword arguments
        signature = ", ".join(args_repr + kwargs_repr)
        logging.info(f"Calling {func.__name__}({signature})")
        
        try:
            result = func(*args, **kwargs)
            # Log the return value
            logging.info(f"{func.__name__} returned {result!r}")
            return result
        except Exception as e:
            # Log the exception if one occurs
            logging.error(f"{func.__name__} raised an exception: {e}", exc_info=True)
            raise
    return wrapper

@log_function_call
def add(a, b):
    """Adds two numbers."""
    return a + b

@log_function_call
def divide(a, b):
    """Divides a by b."""
    return a / b

# Usage
if __name__ == "__main__":
    add(5, 3)
    try:
        divide(10, 0)
    except ZeroDivisionError:
        pass
    divide(10, 2)
```

### **Explanation:**

1. **Logging Configuration:**
   - The `logging` module is configured to display messages at the `INFO` level with a specific format that includes the timestamp, log level, and message.

2. **Decorator Definition (`log_function_call`):**
   - **Purpose:** To log every call to the decorated function, including its arguments and return value.
   - **`@functools.wraps(func)`:** Preserves the metadata of the original function (`__name__`, `__doc__`, etc.).
   - **Logging the Call:**
     - Positional (`*args`) and keyword (`**kwargs`) arguments are represented as strings and combined into a signature.
     - Logs an `INFO` message indicating the function being called and its arguments.
   - **Executing the Function:**
     - Attempts to execute the decorated function and captures its return value.
     - Logs an `INFO` message with the function's return value.
   - **Exception Handling:**
     - If the function raises an exception, it logs an `ERROR` message with exception details and re-raises the exception to ensure normal error propagation.
   
3. **Decorated Functions (`add` and `divide`):**
   - Both functions are decorated with `@log_function_call`, enabling automatic logging whenever they are invoked.

4. **Usage Example:**
   - Calls to `add(5, 3)` and `divide(10, 0)` are made.
   - The `divide(10, 0)` call intentionally raises a `ZeroDivisionError`, which is caught in the `try-except` block to prevent the program from crashing.
   - Another call to `divide(10, 2)` is made to demonstrate successful execution.

5. **Sample Output:**
   ```
   2024-04-27 10:00:00,000 - INFO - Calling add(5, 3)
   2024-04-27 10:00:00,001 - INFO - add returned 8
   2024-04-27 10:00:00,002 - INFO - Calling divide(10, 0)
   2024-04-27 10:00:00,003 - ERROR - divide raised an exception: division by zero
   Traceback (most recent call last):
     File "decorator_example.py", line 20, in wrapper
       result = func(*args, **kwargs)
     File "decorator_example.py", line 33, in divide
       return a / b
   ZeroDivisionError: division by zero
   2024-04-27 10:00:00,004 - INFO - Calling divide(10, 2)
   2024-04-27 10:00:00,005 - INFO - divide returned 5.0
   ```

**Key Takeaways:**
- **Decorators** can be used to inject additional behavior (like logging) around existing functions without modifying their code.
- **Exception Handling** within decorators ensures that errors within decorated functions are appropriately logged and handled.
- **Preserving Function Metadata** using `functools.wraps` is crucial for maintaining the identity of the original functions.

---

## **2. Authorization Decorator for Securing Functions**

### **Scenario: Restricting Access to Sensitive Operations**

**Description:**
In applications where certain functions should only be accessible to authorized users, an **authorization decorator** can enforce access control. This decorator checks if the user has the necessary permissions before allowing the function to execute. If the user is not authorized, the decorator raises an exception or returns an error message.

### **Code:**
```python
import functools

def authorize(required_role):
    """Decorator to restrict access to users with a specific role."""
    def decorator_authorize(func):
        @functools.wraps(func)
        def wrapper_authorize(*args, **kwargs):
            # Simulated user data (in a real application, fetch from user session or database)
            user = {
                'username': 'john_doe',
                'role': 'user'  # Possible roles: 'user', 'admin'
            }
            print(f"Authorizing user '{user['username']}' with role '{user['role']}' for '{func.__name__}'")
            
            if user['role'] != required_role:
                raise PermissionError(f"User '{user['username']}' does not have the required '{required_role}' role.")
            
            return func(*args, **kwargs)
        return wrapper_authorize
    return decorator_authorize

@authorize('admin')
def delete_user(user_id):
    """Deletes a user from the system."""
    print(f"User with ID {user_id} has been deleted.")

@authorize('admin')
def add_user(username):
    """Adds a new user to the system."""
    print(f"User '{username}' has been added to the system.")

# Usage
if __name__ == "__main__":
    try:
        add_user('alice')
    except PermissionError as pe:
        print(f"Authorization Error: {pe}")
    
    try:
        delete_user(42)
    except PermissionError as pe:
        print(f"Authorization Error: {pe}")
```

### **Explanation:**

1. **Decorator Definition (`authorize`):**
   - **Purpose:** To restrict access to functions based on the user's role.
   - **Parameter (`required_role`):** Specifies the role required to execute the decorated function (e.g., `'admin'`).
   - **Nested Decorator (`decorator_authorize`):** Wraps the target function.
   - **User Simulation:**
     - In this example, user data is hardcoded. In a real application, user information would typically be retrieved from session data, authentication tokens, or a database.
   - **Authorization Check:**
     - Compares the user's role with the `required_role`.
     - If the roles do not match, raises a `PermissionError`.
     - If authorized, proceeds to execute the decorated function.
   
2. **Decorated Functions (`delete_user` and `add_user`):**
   - Both functions are decorated with `@authorize('admin')`, meaning only users with the `'admin'` role can execute them.

3. **Usage Example:**
   - Attempts to call `add_user('alice')` and `delete_user(42)`.
   - Since the simulated user has the `'user'` role and not `'admin'`, both calls raise a `PermissionError`, which is caught and printed.

4. **Sample Output:**
   ```
   Authorizing user 'john_doe' with role 'user' for 'add_user'
   Authorization Error: User 'john_doe' does not have the required 'admin' role.
   Authorizing user 'john_doe' with role 'user' for 'delete_user'
   Authorization Error: User 'john_doe' does not have the required 'admin' role.
   ```

**Key Takeaways:**
- **Authorization Decorators** can enforce access control by checking user roles or permissions before allowing function execution.
- **Flexible Decorator Parameters:** By passing parameters to decorators (like `required_role`), decorators can be made reusable and adaptable to different scenarios.
- **Exception Handling:** Raising and handling specific exceptions (`PermissionError`) provides clear feedback on authorization failures.

---

## **3. Performance Timing Decorator for Function Optimization**

### **Scenario: Monitoring and Optimizing Function Performance**

**Description:**
To optimize an application's performance, it's vital to monitor how long certain functions take to execute. A **performance timing decorator** can measure and log the execution time of functions, helping developers identify bottlenecks and optimize code effectively.

### **Code:**
```python
import functools
import time

def timer(func):
    """Decorator to measure the execution time of functions."""
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start_time = time.perf_counter()  # High-resolution timer
        try:
            result = func(*args, **kwargs)
            return result
        finally:
            end_time = time.perf_counter()
            elapsed_time = end_time - start_time
            print(f"Function '{func.__name__}' executed in {elapsed_time:.6f} seconds.")
    return wrapper_timer

@timer
def compute_factorial(n):
    """Computes the factorial of a number recursively."""
    if n == 0 or n == 1:
        return 1
    else:
        return n * compute_factorial(n - 1)

@timer
def simulate_processing(delay):
    """Simulates a processing delay."""
    time.sleep(delay)
    print(f"Simulated processing for {delay} seconds.")

# Usage
if __name__ == "__main__":
    print(f"Factorial of 5 is {compute_factorial(5)}\n")
    simulate_processing(2)
```

### **Explanation:**

1. **Decorator Definition (`timer`):**
   - **Purpose:** To measure and report the execution time of the decorated function.
   - **Implementation:**
     - **`time.perf_counter()`:** Provides a high-resolution timer suitable for measuring short durations.
     - **Timing Logic:**
       - Records the start time before function execution.
       - Executes the target function and captures its result.
       - In the `finally` block, records the end time and calculates the elapsed time.
       - Prints the function's execution time with microsecond precision.
   
2. **Decorated Functions (`compute_factorial` and `simulate_processing`):**
   - **`compute_factorial(n)`:**
     - Recursively calculates the factorial of a given number `n`.
     - Decorated with `@timer` to measure its execution time.
   - **`simulate_processing(delay)`:**
     - Simulates a processing delay using `time.sleep(delay)`.
     - Decorated with `@timer` to measure how long the sleep takes.

3. **Usage Example:**
   - **Factorial Calculation:**
     - Calls `compute_factorial(5)`, which calculates `5! = 120`.
     - The decorator measures and prints the execution time.
   - **Simulated Processing:**
     - Calls `simulate_processing(2)`, which sleeps for 2 seconds.
     - The decorator measures and prints the execution time.

4. **Sample Output:**
   ```
   Function 'compute_factorial' executed in 0.000012 seconds.
   Function 'compute_factorial' executed in 0.000009 seconds.
   Function 'compute_factorial' executed in 0.000008 seconds.
   Function 'compute_factorial' executed in 0.000007 seconds.
   Function 'compute_factorial' executed in 0.000007 seconds.
   Factorial of 5 is 120

   Function 'simulate_processing' executed in 2.002345 seconds.
   Simulated processing for 2 seconds.
   ```

   *Note:* The exact execution times may vary based on the system and current load.

**Key Takeaways:**
- **Performance Monitoring:** Decorators can seamlessly integrate performance monitoring into functions without cluttering their core logic.
- **Using `finally` Block:** Ensures that the execution time is recorded and reported even if the function raises an exception.
- **High-Resolution Timing:** Utilizing `time.perf_counter()` provides accurate measurements suitable for profiling functions.

---

## **Conclusion**

These **3 scenario-based Python programs** illustrate how decorators can be effectively used to:

1. **Enhance Functionality Without Modifying Core Logic:**
   - **Logging Decorator:** Automatically logs function calls, arguments, and return values, aiding in debugging and monitoring.
   - **Authorization Decorator:** Enforces access control, ensuring that only authorized users can execute certain functions.
   - **Performance Timing Decorator:** Measures and reports the execution time of functions, facilitating performance optimization.

2. **Promote Code Reusability and Maintainability:**
   - By abstracting common behaviors (logging, authorization, timing) into decorators, you avoid repetitive code and make your functions cleaner and more focused on their primary tasks.

3. **Integrate Seamlessly with Exception Handling:**
   - Decorators can include exception handling mechanisms, ensuring that additional behaviors (like logging errors) are consistently applied across functions.

**Best Practices When Using Decorators:**
- **Use `functools.wraps`:** Always use `functools.wraps` to preserve the original function's metadata, such as its name and docstring.
- **Keep Decorators Focused:** Each decorator should have a single responsibility to maintain clarity and reusability.
- **Handle Exceptions Appropriately:** When decorators involve exception handling, ensure that exceptions are either handled or re-raised to avoid unexpected behavior.





