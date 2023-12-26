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
-- text about SQL injection


## Attack on publicly exposed Werkzeug Debugger
![Werkzeug](https://github.com/Dragan2402/zoss-23-24/blob/main/2.%20Service%20layer/Werkzeug.png)

### TODO
-- text about publicly exposed Werkzeug Debugger

[Article about hacking Patreon](https://labs.detectify.com/writeups/how-patreon-got-hacked-publicly-exposed-werkzeug-debugger/)
