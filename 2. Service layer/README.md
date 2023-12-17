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



### TODO
-- text about CSRF --


### TODO
-- text about SQL injection


## Attack on publicly exposed Werkzeug Debugger


### TODO
-- text about publicly exposed Werkzeug Debugger
