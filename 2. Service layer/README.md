# Flask Security Considerations

As we said in the introduction, Flask is very lightweight Python web framework. It is designed to be flexible and to let developers choose which services they need. Flask itself does not provide any service for accessing databases, validating web forms, authenticating users, or other high-level tasks. For that reason, it is easy to make your application vulnerable to some common web attacks, such as:
- **Cross-Site Request Forgery (CSRF)**
- **SQL Injection**
- **Attack on publicly exposed Werkzeug Debugger**

The first two attacks are very common for all web services, but the last one is very specific to the Flask applications. With these mentioned attacks, attacker is able to steel data, trick user to make unintended actions (e.g. changing account settings, making a transaction, etc.), or even cause damage to the database. 
All of these can be prevented by properly configuring Flask application, using latest versions of modules and using specified modules for these kinds of possible vulnerabilities.

## Injections
Injections are very common attacks for web applications. They have its own category on OWASP Top Ten list and, when talking about Flask, they can be diveded into two classes:
- *Cross-Site Request Forgery (CSRF)*
- *SQL Injection*

![Injections](https://github.com/Dragan2402/zoss-23-24/blob/main/2.%20Service%20layer/Injections.png)

### Cross-Site Request Forgery (CSRF)
Cross-Site Request Forgery (CSRF) is an attack that forces an end user to execute unwanted actions on a web application in which they’re currently authenticated. With a little help of social engineering (such as sending a link via email or chat), an attacker may trick the users of a web application into executing actions of the attacker’s choosing. It inherits the identity and privileges of the victim to perform an undesired function on the victim’s behalf. For most sites, browser requests automatically include any credentials associated with the site, such as the user’s session cookie, IP address, Windows domain credentials, and so forth. Therefore, if the user is currently authenticated to the site, the site will have no way to distinguish between the forged request sent by the victim and a legitimate request sent by the victim.

CSRF attacks target functionality that causes a state change on the server, such as changing the victim’s email address or password, or purchasing something. Forcing the victim to retrieve data doesn’t benefit an attacker because the attacker doesn’t receive the response, the victim does. As such, CSRF attacks target state-changing requests.

By exploiting CSRF, an attacker can acquire the victim's private information through a specific variant of the attack called login CSRF. In this scenario, the attacker forces a non-authenticated user to log in to an account controlled by the attacker. If the victim remains unaware of this manipulation, they might input personal details—such as credit card information—into the account. Subsequently, the attacker can access the account, retrieve this data, and examine the victim's web application activity history.

#### Attack scenario
CSRF attacks are often achieved by tricking a user into clicking a button or submitting a form. For example, say your banking web app is vulnerable to CSRF attacks. An attacker could create a clone of your banking web site that contains the following form:
```html
<form action="https://centralbank.com/api/account" method="POST">
  <input type="hidden" name="transaction" value="transfer">
  <input type="hidden" name="amount" value="100">
  <input type="hidden" name="account" value="999">
  <input type="submit" value="Check your statement now">
</form>
```
The attacker then sends you an email that appears to come from your bank -- cemtralbenk.com instead of centralbank.com -- indicating that your bank statement is ready to view. After clicking the link in the email, you're taken to the malicious web site with the form. You click the button to check your statement. The browser will then automatically send the authentication cookie along with the POST request. Since you're authenticated, the attacker will be able to perform any action that you're allowed to do. In this case, $100 is transferred from you account to account number 999. When the user is authenticated, the bank server essentially trusts the request from the browser. Since this route handler isn't protected from a CSRF attack, an attacker can exploit this trust by tricking someone into performing an operation on the bank server without their knowledge.

#### Mitigations
To prevent this kind of attack there are some setting in Flask application as well as in forms that should be set. First step is to install Flask-WTF library. After that we should wrap our app global object as shown in code below:
```python
...

from flask_wtf.csrf import CSRFProtect

app = Flask(__name__)
app.config.update(
    DEBUG=True,
    SECRET_KEY="secret_sauce",
)

csrf = CSRFProtect()
csrf.init_app(app)

...
```
Now, by default, all POST, PUT, PATCH, and DELETE methods are protected against CSRF, but you should **never** perform a side effect, like changing data in the database, via a GET request.

After that we should use our HTML form like this:
```html
<form hidden id="hack" target="csrf-frame" action="http://localhost:5000/accounts" method="POST" autocomplete="off">
    <input type="number" name="amount" value="2000">
    <input type="number" name="account" value="2">
    <input type="hidden" name="csrf_token" value="123">
</form>
```
At the end, let's see how the CSRF token workflow looks like:
-- slika

### SQL Injection
A SQL injection attack consists of insertion or "injection" of a SQL query via the input data from the client to the application. A successful SQL injection exploit can read sensitive data from the database, modify database data (Insert/Update/Delete), execute administration operations on the database (such as shutdown the DBMS), recover the content of a given file present on the DBMS file system and in some cases issue commands to the operating system. SQL injection attacks are a type of injection attack, in which SQL commands are injected into data-plane input in order to affect the execution of predefined SQL commands.

SQL injection attack occurs when:
- An unintended data enters a program from an untrusted source.
- The data is used to dynamically construct a SQL query

This can lead to very dangerous consequences such as: **reading sensitive data**, **authentication and authorization** with stolen username and password or even **deleting data in database**.

#### Attack scenario
Lets use as an example an application that lets users log in with a username and password. If a user submits the username `wiener` and the password `bluecheese`, the application checks the credentials by performing the following SQL query:
```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
```
If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

In this case, an attacker can log in as any user without the need for a password. They can do this using the SQL comment sequence `--` to remove the password check from the _WHERE_ clause of the query. For example, submitting the username `administrator'--` and a blank password results in the following query:
```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```
This query returns the user whose username is administrator and successfully logs the attacker in as that user.

#### Mitigations
There are several mitigations in order to prevent SQL injection in FLask. First we should validate input on client and on server side. Note that validating input only on client side isn't enough because attacker can send request through command line, bash, postman... This can be done with simple if statemants and regular expressions.

Another way to prevent SQL injection is to sanitize the input values, which means removing or escaping any characters that could be interpreted as SQL commands or keywords. You can use functions or libraries that encode or escape special characters, such as quotes, semicolons, dashes, or comments.

Depending on your choice, you can use _plain_ SQL statement with **psycopg2** for PostgreSQL database or some ORM (most popular is SQLAlchemy). If you choose to use plain SQL then it is mandatory to use prepared SQL statements. 
- **psycopg2** is a PostgreSQL adapter for the Python programming language. It provides support for various advanced features of PostgreSQL, such as server-side cursors, asynchronous communication, and more. Prepared SQL statements prevent attacker to input SQL statement in your statement and then when you execute it they get unexpected result. Below is the example of how to use prepared SQL statement in Flask. The `sql.Literal(user_id)` is used to safely insert the user_id into the prepared statement, preventing SQL injection.
```python
query = sql.SQL("SELECT * FROM users WHERE id = {}").format(sql.Literal(user_id))
cur.execute(query)
```

- On the other hand if you choose ORM then it is little easier to prevent SQL injections. SQLAlchemy is an open-source SQL toolkit and Object-Relational Mapping (ORM) library for Python. It provides a set of high-level API for communicating with relational databases and abstracts the SQL operations, allowing developers to interact with databases using Python objects and methods. SQLAlchemy supports a wide range of relational databases, making it a versatile choice for database operations in Python applications.
In a Flask application, SQLAlchemy is commonly used to interact with databases, and it provides a way to prevent SQL injection by using parameterized queries through its ORM capabilities. This is an example of how to use prepared SQL statement in SQLAlchemy:
```python
user = User.query.filter_by(username='example_user').first()
```


## Attack on publicly exposed Werkzeug Debugger
![Werkzeug](https://github.com/Dragan2402/zoss-23-24/blob/main/2.%20Service%20layer/Werkzeug.png)

### Attack on publicly exposed Werkzeug Debugger
The Werkzeug Debugger is a powerful interactive debugger provided by the Werkzeug utility library, which is often used with Flask. When an unhandled exception occurs during the execution of a Flask application, the Werkzeug Debugger steps in to provide a detailed interactive debugging environment in the web browser. It offers valuable insights into the state of the application, the call stack, and the variables at the point of the error. To enable the Werkzeug Debugger in a Flask application during development, you just need to set the debug mode to True when running the application:
```python
if __name__ == '__main__':
    app.run(debug=True)
```
Exposing the Werkzeug Debugger in a production environment is strongly discouraged due to security risks.

#### Attack scenario
Werkzeug Debugger works in the way that, as soon as something in the code results in an exception or error, a console is opened. This is known as a Remote Code Execution (RCE). You can inject code directly to the application, exposing all data on the server which the application has access to. Now, Werkzeug requires an actual error to trigger the console, as it uses a secret key generated when the application starts, which is only exposed in the Werkzeug Debugger page. Without this secret key you cannot run any commands, that’s why you need an exception to reveal the secret. Also worth noting is that the debugger only accepts commands sent in by the GET-parameter, which will then show up in access logs on the vulnerable host, which is great for forensic analysis and investigation.

#### Mitigations
The simplest way to prevent exposing Werkzeug debugger in production enviroment is to just set it to False. Then you are 100% sure that it is not exposed and cannot be accessed from anybody outside. This code snippet shows how to do it:
```python
if __name__ == '__main__':
    app.run(debug=False)
```
_False_ is also a default value.

There was an actual hacker attack on Patreon which you can read about on the following page:
[Article about hacking Patreon](https://labs.detectify.com/writeups/how-patreon-got-hacked-publicly-exposed-werkzeug-debugger/)
