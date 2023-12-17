# Angular Framework and Security

Angular 2, more commonly known simply as Angular, is a single-page TypeScript framework developed by Google. It follows a component-based architecture, making it a powerful and feature-rich tool. Only recently has the security of client-side frontend layers started to gain importance.

OWASP (Open Web Application Security Project) has presented a list of the top 10 threats for web applications in 2021[[4.1]](https://owasp.org/www-project-top-ten/).

![OWSAP Top 10 2021](OWSAP-top-10-2021.png)

A comprehensive set of preventive measures was published by OWASP that should prevent these attacks from happening[[4.3]](https://community.f5.com/t5/technical-articles/mitigating-owasp-web-application-security-top-10-2021-risks/ta-p/311403):

1. Define security requirements.
2. Leverage security frameworks and libraries.
3. Secure database access.
4. Encode and escape data.
5. Validate all inputs.
6. Implement digital identity.
7. Enforce access controls.
8. Protect data everywhere.
9. Implement security logging and monitoring.
10. Handle all errors and exceptions.

## Angular Security

Angular is a relatively secure layer in systems, as long as the recommendations provided by the Angular security team are followed. These include using the latest version, avoiding modifications to the core Angular, and refraining from using libraries marked as insecure[[4.2]](https://angular.io/guide/security).


## XSS Attack

![XSS Attack graph](angularXSS.png)

## Cross-Site Request Forgery(CSRF):

![CSRF Attack graph](angularCSRF.png)

## SSRF Attack

![SSRF Attack graph](angularSSRF.png)

References:
- [4.1 OWSAP Top 10 Web Application Security Risks](https://owasp.org/www-project-top-ten/)
- [4.2 Angular Security](https://angular.io/guide/security)
- [4.3 OWSAP Prevention measures](https://community.f5.com/t5/technical-articles/mitigating-owasp-web-application-security-top-10-2021-risks/ta-p/311403)