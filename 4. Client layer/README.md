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

**Title: Understanding and Mitigating XSS Attacks in Angular Projects**

**Introduction:**

Cross-Site Scripting (XSS) attacks pose a significant threat to web applications, including those built with Angular. XSS is not only about stealing data but has evolved to encompass injecting malicious content. The repercussions of XSS attacks include account impersonation, data theft, session hijacking, and cookie theft. This essay explores the causes of XSS attacks, focusing on unsanitized input, dynamic content generation, and template injection, while providing mitigation strategies based on the OWASP Cross Site Scripting Prevention Cheat Sheet.

**Causes of XSS Attacks:**

1. **Unsanitized Input:**

   - **Description:** All user inputs should be treated as potentially malicious. Failure to sanitize user inputs allows attackers to inject scripts that can later execute when viewed by other users.
   - **Mitigation:** Implement strict input validation and sanitize user inputs before rendering them in the application. Angular's built-in mechanisms for input validation can help achieve this.

2. **Dynamic Content Generation:**

   - **Description:** Generating dynamic content without proper encoding or escaping leaves room for attackers to inject malicious scripts.
   - **Mitigation:** To enhance security in Angular, it is crucial to utilize the built-in templating system, which automatically escapes dynamic content, preventing the execution of potentially harmful scripts. Developers should refrain from employing methods like `dangerouslySetInnerHTML` in Angular, as they can introduce vulnerabilities by allowing unescaped content to be rendered in the application, increasing the risk of cross-site scripting attacks.

3. **Template Injection:**
   - **Description:** Improper handling of templates can lead to injection vulnerabilities, enabling attackers to execute arbitrary code.
   - **Mitigation:** Strict input validation is imperative. Ensuring that all user inputs undergo thorough validation on the server-side before rendering in the application. Second, leveraging Angular's built-in contextual escaping mechanisms, such as the {{ variable | pipe }} syntax. Contextual escaping ensures that data is appropriately rendered in the intended context, preventing the inadvertent execution of scripts. Third, adopting a Content Security Policy (CSP) serves as a robust defense mechanism. By implementing and enforcing CSP headers on the server, developers can control and restrict the types of content the application is allowed to load, thereby mitigating the impact of template injection attacks.

**XSS Defense Philosophy:**

To achieve perfect injection resistance, every variable in a web application must go through validation and be properly escaped or sanitized. While frameworks streamline this process, developers should be aware of security gaps and use output encoding and HTML sanitization when needed.

**Output Encoding:**

Output encoding is a fundamental defense against XSS. It involves converting untrusted input into a safe form, preventing it from being executed as code. Different contexts require specific encoding methods:

1. **HTML Contexts:**

   - Use HTML entity encoding (`&lt;`, `&gt;`, etc.) to safely insert variables between HTML tags.

2. **HTML Attribute Contexts:**

   - Apply HTML attribute encoding to variables placed in HTML attribute values. Use quotation marks to prevent context changes.

3. **JavaScript Contexts:**

   - Place variables in JavaScript only within "quoted data values" to ensure safety. Use proper encoding methods, such as `\xHH` format.

4. **CSS Contexts:**

   - Variables should be placed only in CSS property values. Use style.property or CSS encoding methods for safety.

5. **URL Contexts:**
   - Use URL encoding for variables placed in URLs. Be cautious about different contexts where URLs are used.

**HTML Sanitization:**

When users author HTML content, output encoding alone may break intended functionality. HTML sanitization, using tools like DOMPurify, helps strip dangerous HTML, providing a safe HTML string.

**Safe Sinks:**

Security professionals emphasize safe sinks, where variables are placed into webpages without execution. Refactor code to use safe sinks like textContent or value, avoiding unsafe sinks like innerHTML.

**Conclusion:**

XSS attacks remain a persistent threat, but with a combination of secure coding practices, output encoding, HTML sanitization, and awareness of framework security features, developers can significantly reduce the risk of XSS vulnerabilities in Angular projects.

## Cross-Site Request Forgery(CSRF):

![CSRF Attack graph](angularCSRF.png)

## SSRF Attack

![SSRF Attack graph](angularSSRF.png)

References:

- [4.1 OWSAP Top 10 Web Application Security Risks](https://owasp.org/www-project-top-ten/)
- [4.2 Angular Security](https://angular.io/guide/security)
- [4.3 OWSAP Prevention measures](https://community.f5.com/t5/technical-articles/mitigating-owasp-web-application-security-top-10-2021-risks/ta-p/311403)
