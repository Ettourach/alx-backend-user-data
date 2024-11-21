#!/usr/bin/env python3

# readme file for folder 0x03-user_authentication_service

# Learning Objectives

1. what is user authentication service and bcrypt?
*User Authentication Service
A user authentication service is a system that verifies the identity of users trying to access a digital application or service. It ensures that only authorized users can access specific resources by validating their credentials, such as a username and password, or other methods like tokens or biometrics. This service is crucial for securing sensitive data and maintaining user privacy.

*Example: A login system on a website checks if the email and password entered by a user match those stored in the database. If valid, the user is granted access

# python code

from flask import Flask, request, jsonify

app = Flask(__name__)

# Simulated database
users = {"ettourach@gmail.com": "securepassword"}

@app.route('/login', methods=['POST'])
def login():
    data = request.json
    email = data.get('email')
    password = data.get('password')
    
    if users.get(email) == password:
        return jsonify({"message": "Login successful!"}), 200
    return jsonify({"error": "Invalid credentials"}), 401

# Start the app
if __name__ == "__main__":
    app.run()

Note:This Flask app simulates a basic user authentication process where credentials are checked against a dictionary

*bcrypt is a Python library used for securely hashing and verifying passwords. It is based on the bcrypt hashing algorithm, which is designed to be slow and computationally expensive, making it resistant to brute-force attacks. The library helps developers store user passwords securely by hashing them instead of storing them as plain text.

*why use it 
-Password Security: Hashing ensures that even if a database is compromised, passwords are not stored in a readable format.
-Salting: Automatically adds a random salt to the hash, making it unique even for identical passwords.
-Resistance to Attacks: Its built-in computational expense helps mitigate brute-force attacks.

2. Declaretion API routes in a Flask app
*In Flask, API routes are declared using the @app.route() decorator. This decorator binds a URL endpoint to a Python function, which is executed when the route is accessed. You can specify HTTP methods (e.g., GET, POST) and pass parameters via the URL or request body.

Basic Example:

# python code 

from flask import Flask, jsonify, request

app = Flask(__name__)

# Define a GET route
@app.route('/hello', methods=['GET'])
def say_hello():
    return jsonify({"message": "Hello, World!"})

# Define a POST route
@app.route('/echo', methods=['POST'])
def echo_message():
    data = request.json
    return jsonify({"you_sent": data})

# Define a route with a URL parameter
@app.route('/user/<username>', methods=['GET'])
def greet_user(username):
    return jsonify({"message": f"Hello, {username}!"})

# Run the app
if __name__ == "__main__":
    app.run(debug=True)

How it works:

1.GET /hello
Returns a JSON response: {"message": "Hello, World!"}.
2.POST /echo
Takes JSON input (e.g., {"text": "Hi there"}) and echoes it back:
{"you_sent": {"text": "Hi there"}}.
3.GET /user/<username>
ACcessing /user/Ilyas would return:
{"message": "Hello, Ilyas!"}.

*Testing the API:
You can use tools like:
Postman: To send requests and view responses.
cURL: Command-line HTTP client.
Example cURL for POST /echo:

# bash

curl -X POST -H "Content-Type: application/json" \
-d '{"text": "Hi Ilyas"}' http://127.0.0.1:5000/echo

3. geting and seting cookies

*In Flask, you can manage cookies with the request and response objects. Here's how you can get and set cookies in your Flask application:
1.Setting Cookies
You set cookies using the response.set_cookie() method, which allows you to store data in the user's browser.
2.Getting Cookies
You retrieve cookies using the request.cookies.get() method, which gives you the value of the cookie sent by the browser in the request.

Example of Setting and Getting Cookies:
# python code

from flask import Flask, request, make_response

app = Flask(__name__)

# Route to set a cookie
@app.route('/set_cookie')
def set_cookie():
    # Create a response object
    resp = make_response("Cookie has been set!")
    
    # Set a cookie with key 'user_id' and value '12345', expires in 30 days
    resp.set_cookie('user_id', '12345', max_age=30*24*60*60)
    
    return resp

# Route to get the cookie
@app.route('/get_cookie')
def get_cookie():
    # Get the cookie value with key 'user_id'
    user_id = request.cookies.get('user_id')
    
    if user_id:
        return f"User ID from cookie: {user_id}"
    else:
        return "No user_id cookie found."

# Run the app
if __name__ == '__main__':
    app.run(debug=True)

How it works:
1.Setting a Cookie (/set_cookie route):
The set_cookie() method is used on the response object to set a cookie with the name user_id and a value of 12345.
The cookie is given an expiry of 30 days with the max_age parameter.
2.Getting a Cookie (/get_cookie route):
The request.cookies.get('user_id') retrieves the value of the user_id cookie sent in the request.
If the cookie exists, it returns the value (12345), otherwise, it shows a message saying no cookie was found.

Additional Cookie Options:
-max_age: The duration (in seconds) for which the cookie will be valid.
-expires: You can also set a specific expiration time using a datetime object.
-secure: Use this option to ensure the cookie is sent only over HTTPS.
-httponly: This makes the cookie inaccessible to JavaScript, reducing XSS vulnerabilities.
-samesite: Controls whether cookies are sent with cross-site requests. Options are None, Lax, or Strict.

Example with secure and httponly:
#python code 

from datetime import datetime, timedelta

@app.route('/set_secure_cookie')
def set_secure_cookie():
    resp = make_response("Secure cookie has been set!")
    
    # Set secure cookie (only sent over HTTPS and not accessible via JavaScript)
    resp.set_cookie('secure_user', '67890', max_age=timedelta(days=7), secure=True, httponly=True)
    
    return resp

Testing:
-After accessing /set_cookie, the browser will store the cookie.
-When you visit /get_cookie, it will retrieve and display the cookie value.
Important Notes:
-Cookies are stored on the client-side, meaning users can inspect or modify them.
-Security: Ensure sensitive information is not stored directly in cookies. Always hash or encrypt important data like passwords.

4. Retrieve request form data

*In Flask, you can retrieve form data sent in a POST request using the request.form object. This object acts like a dictionary, where you can access form fields by their names.

Steps to Retrieve Form Data:
1.Define the HTML Form:
-Create an HTML form that submits data using the POST method.
-Add form fields with name attributes for identifying the data.
2.Access Form Data in Flask:
-Use the request.form object to retrieve the data by the field's name.

Example Code:
HTML Form (to submit data):
# html

<form action="/submit" method="POST">
    <label for="username">Username:</label>
    <input type="text" id="username" name="username">
    
    <label for="password">Password:</label>
    <input type="password" id="password" name="password">
    
    <button type="submit">Submit</button>
</form>

Flask Route (to handle the form data):
# python code

from flask import Flask, request

app = Flask(__name__)

@app.route('/submit', methods=['POST'])
def submit():
    # Retrieve form data
    username = request.form.get('username')  # Get the value of 'username'
    password = request.form.get('password')  # Get the value of 'password'
    
    return f"Username: {username}, Password: {password}"

if __name__ == '__main__':
    app.run(debug=True)

How It Works:
1.Form Submission:
-The form submits data to the /submit endpoint using the POST method.
2.Access Form Data:
request.form.get('username'): Retrieves the value of the username field.
request.form.get('password'): Retrieves the value of the password field.
3.Response:
The server processes the data and responds with a message containing the submitted values.

Common Methods for Retrieving Data:
-request.form.get('key'): Retrieves a value by key. Returns None if the key is missing.
-request.form['key']: Similar to .get(), but raises a KeyError if the key is missing.
-request.form.to_dict(): Converts all form data into a Python dictionary.

Example with Default Values:
# python code
username = request.form.get('username', 'Guest')  # Default to 'Guest' if not provided

Note:
-Use request.form for form data (submitted via application/x-www-form-urlencoded or multipart/form-data).
-Use request.json for JSON payloads in API requests.

5. Return various HTTP status codes
*In Flask, you can return various HTTP status codes along with your response using the Response object or by specifying the status code directly in the return statement.

1.Returning a Status Code with a Response
You can pass a status code as a second argument in the return statement.
# python code

from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/success')
def success():
    return "Request succeeded!", 200  # HTTP 200 OK

@app.route('/not_found')
def not_found():
    return "Page not found!", 404  # HTTP 404 Not Found

@app.route('/unauthorized')
def unauthorized():
    return "Unauthorized access!", 401  # HTTP 401 Unauthorized

if __name__ == '__main__':
    app.run(debug=True)

2.Using the Response Object
You can create a Response object and explicitly set its status code
# python code

from flask import Flask, Response

app = Flask(__name__)

@app.route('/custom')
def custom_response():
    response = Response("Custom response", status=418)  # HTTP 418 I'm a Teapot
    return response

3.Returning JSON with Status Codes
Use Flask's jsonify to return a JSON response along with a status code.
# python code
from flask import Flask, jsonify

app = Flask(__name__)

@app.route('/json_response')
def json_response():
    return jsonify({"message": "Data created!"}), 201  # HTTP 201 Created

4.Predefined HTTP Status Code Names
Flask provides a dictionary of HTTP status codes with descriptive names for readability.
# python code

from flask import Flask
from werkzeug.http import HTTP_STATUS_CODES

app = Flask(__name__)

@app.route('/predefined')
def predefined_status():
    return "Bad Request!", 400  # Or HTTP_STATUS_CODES[400]

Common HTTP Status Codes:
-200 OK: Request succeeded (default for successful responses).
-201 Created: Resource was successfully created.
-204 No Content: Request succeeded, but no content is returned.
-400 Bad Request: Client sent an invalid request.
-401 Unauthorized: Authentication is required.
-403 Forbidden: Client doesn't have permission.
-404 Not Found: Resource not found.
-500 Internal Server Error: Server encountered an error

Example: Handling Multiple Status Codes
# python code

@app.route('/status/<code>')
def status(code):
    code = int(code)
    message = HTTP_STATUS_CODES.get(code, "Unknown status")
    return f"Status {code}: {message}", code

With these approaches, you can handle various HTTP status codes effectively in your Flask application.


6. Conclusion

To summarize the key learning objectives:

-User Authentication Service: A secure system to verify user identities using techniques like password hashing and token-based authentication. This is crucial for protecting sensitive user data and ensuring only authorized access.
-Installing and Using bcrypt: A Python library for securely hashing passwords. It adds a layer of protection to your authentication process by making passwords harder to crack, demonstrating the importance of strong security practices.
-Working with Cookies: Cookies help in managing user sessions and storing temporary data. By setting and retrieving cookies, applications can maintain a seamless and personalized user experience.
-Retrieving Form Data: Flask's request.form provides a way to handle user-submitted data from HTML forms, enabling dynamic, data-driven interactions.
-HTTP Status Codes: Returning appropriate status codes ensures clear communication between the server and clients, whether for success, errors, or specific conditions.

These concepts together form the foundation for building secure, responsive, and user-friendly web applications.

