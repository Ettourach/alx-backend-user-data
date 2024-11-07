# personal Data #

# What is it :
Personal data is any information that identifies or can be linked to an individual, including names, contact details, IP addresses, and location data. It’s a crucial aspect of data privacy, as it requires protection to respect individuals' privacy and to comply with laws like GDPR. Responsible handling of personal data involves securing it, gaining user consent, and ensuring transparency in its use.

# Learning Objectives

#1.Examples of Personally Identifiable Information (PII)
Here are some common examples of Personally Identifiable Information (PII):

1.Full Name : First and last names that uniquely identify a person.
2.Home Address : Street address, city, postal code.
3.Email Address : Any personal or work email address.
4.Phone Number : Mobile, home, or work numbers.
5.Social Security Number (SSN) : A unique government-issued ID number.
6.Passport Number : A unique identifier for international travel.
7.Driver’s License Number : A unique number linked to a driver's record.
8.Credit Card Information : Card number, CVV, and expiration date.
9.Bank Account Information : Account numbers and routing numbers.
10.Biometric Data : Fingerprints, facial recognition, voice patterns.
11.IP Address : Identifies devices and often their locations online.
12.Medical Records : Health details linked to an individual.

#2.Implement a log filter that will obfuscate PII fields    always on short paragraf
To implement a log filter that obfuscates PII fields, create a custom logging function that identifies sensitive information (e.g., names, emails, SSNs) within log entries and replaces or masks it before saving or displaying the logs. This can be done using regex patterns to detect PII and replacing characters with asterisks or placeholders (e.g., john.doe@example.com becomes ****@example.com). Such filtering can be integrated into the logging configuration, ensuring PII is automatically obfuscated across all logs.

#python code exemple:

import re
import logging

# Custom filter to mask emails
class PIIObfuscationFilter(logging.Filter):
    def filter(self, record):
        record.msg = re.sub(r'[\w\.-]+@[\w\.-]+', '****@****.***', record.msg)
        return True

# Set up logging with the filter
logger = logging.getLogger('PII_Logger')
logger.addFilter(PIIObfuscationFilter())
handler = logging.StreamHandler()
logger.addHandler(handler)

# Example log
logger.info("User email: john.doe@example.com")
Output:

markdown

User email: ****@****.***
This code replaces email addresses with ****@****.*** before logging.
#3. encrypt a password and check the validity of an input password
To securely encrypt a password and check the validity of an input password, you can use the bcrypt library in Python. This library provides a safe way to hash and verify passwords.

Steps:
Install the bcrypt library:

#bash
pip install bcrypt
Hash and check passwords:

#python code exemple:

import bcrypt

# Hash a password
def hash_password(password):
    # Generate a salt and hash the password
    salt = bcrypt.gensalt()
    hashed_password = bcrypt.hashpw(password.encode('utf-8'), salt)
    return hashed_password

# Check if the input password matches the hashed password
def check_password(input_password, stored_hashed_password):
    # Compare the input password with the stored hashed password
    if bcrypt.checkpw(input_password.encode('utf-8'), stored_hashed_password):
        return True
    else:
        return False

# Example usage
password = "my_secret_password"
hashed_password = hash_password(password)

# User input for validation
input_password = "my_secret_password"

if check_password(input_password, hashed_password):
    print("Password is valid!")
else:
    print("Invalid password!")
Explanation:
hash_password: This function generates a salt and hashes the password using bcrypt.hashpw().
check_password: This function compares the input password (after encoding) with the stored hashed password using bcrypt.checkpw().
#Output:
csharp
Password is valid!
This method ensures that passwords are never stored in plaintext and are securely hashed for verification

#4. authenticate to a database using environment variables.
you can securely store your database credentials (like username, password, host, and database name) in environment variables and then retrieve them in your code
Steps:
Set environment variables: You can set environment variables in your terminal or in a .env file.

In the terminal (Linux/macOS):

#bash

export DB_HOST="your-database-host"
export DB_USER="your-username"
export DB_PASSWORD="your-password"
export DB_NAME="your-database-name"
In the .env file (if using Python dotenv library):

#makefile

DB_HOST=your-database-host
DB_USER=your-username
DB_PASSWORD=your-password
DB_NAME=your-database-name
Install the necessary Python libraries:

For environment variable management, you can use the python-dotenv library.
For database connection, you can use psycopg2 (PostgreSQL), mysql-connector (MySQL), or another appropriate library for your database.
Install libraries:

#bash

"pip install python-dotenv psycopg2"

Code to authenticate to the database:

#python exemple:

import os
from dotenv import load_dotenv
import psycopg2  # Use the appropriate library for your database

# Load environment variables from the .env file (if using dotenv)
load_dotenv()

# Retrieve database credentials from environment variables
db_host = os.getenv('DB_HOST')
db_user = os.getenv('DB_USER')
db_password = os.getenv('DB_PASSWORD')
db_name = os.getenv('DB_NAME')

# Connect to the database
try:
    connection = psycopg2.connect(
        host=db_host,
        user=db_user,
        password=db_password,
        dbname=db_name
    )
    print("Connection successful")
except Exception as e:
    print(f"Error connecting to the database: {e}")
finally:
    if connection:
        connection.close()
Explanation:
load_dotenv(): Loads environment variables from a .env file (if you're using it).
os.getenv(): Retrieves the value of the environment variable.
psycopg2.connect(): Connects to the PostgreSQL database using the credentials retrieved from the environment variables.
For MySQL (using mysql-connector):

#python exemple:

import os
from dotenv import load_dotenv
import mysql.connector

# Load environment variables from .env file
load_dotenv()

# Retrieve database credentials from environment variables
db_host = os.getenv('DB_HOST')
db_user = os.getenv('DB_USER')
db_password = os.getenv('DB_PASSWORD')
db_name = os.getenv('DB_NAME')

# Connect to the MySQL database
try:
    connection = mysql.connector.connect(
        host=db_host,
        user=db_user,
        password=db_password,
        database=db_name
    )
    print("Connection successful")
except mysql.connector.Error as err:
    print(f"Error: {err}")
finally:
    if connection:
        connection.close()
Notes:
Make sure you do not hardcode sensitive information like passwords or API keys directly in your code. Using environment variables is a safer approach.
If you're using a .env file, ensure it is included in your .gitignore to avoid accidental commits of sensitive data.
Example .env file:
makefile

DB_HOST=localhost
DB_USER=myuser
DB_PASSWORD=mypassword
DB_NAME=mydatabase
This way, your code can securely authenticate to the database using environment variables for sensitive credentials.




 
