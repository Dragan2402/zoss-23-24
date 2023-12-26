# NGINX Security Considerations

NGINX is very powerful, open-source, web server, load balancer, reverse proxy and more used by more than 60% of the internet web sites. Therefore it can be very interesting target for attackers. There are several reasons why would NGINX be interesting for attackers such as: many high-traffic and critical websites use Nginx for its performance and scalability, it is often used as a reverse proxy and load balancer, handling incoming requests and distributing traffic across multiple servers. Attacking the proxy layer allows attackers to impact multiple servers behind it. As NGINX can be used to handle SSL/TLS certificates, attackers may attempt to exploit vulnerabilities related to encryption protocols or certificate handling.

### HTTP Strict Transport Security with MitM attack
The Man-in-the-Middle (MitM) attack is a fundamental network session hijacking technique. This attack can block, alter, or intercept network traffic. For example, an attacker using MitM might discreetly capture a user's login details, steal credit card information, or launch a denial-of-service attack. In this attack, the attacker positions themselves between the victim and the desired server. If traffic is encrypted, the attacker may devise ways to decrypt it. They might either listen to the communication passively or modify messages actively before passing them on. If messages were decrypted, they're re-encrypted (possibly after alteration) and then forwarded. MitM attacks can lead to data breaches, financial scams, and eroding user confidence.

#### How a MitM Attack Works
A successful MitM attack works in two stages. The initial stage is about intercepting traffic. In the next stage, the attacker might just eavesdrop, or they could decrypt, modify, re-encrypt, and transfer messages between the communicating parties.
Several interception techniques include:
- **Public Wi-Fi Sniffing**: Attacker creates a deceptive Wi-Fi hotspot in public area. Unsuspecting users connect, allowing the attacker to access all their traffic.
- **ARP Spoofing**: Through fake ARP messages, an attacker can link their MAC address with another host's IP, diverting traffic intended for that IP to themselves.
- **SSL Stripping**: In this approach, attackers force victims to shift from a secure HTTPS connection to insecure HTTP, simplifying traffic interception and reading.
- **IP Spoofing**: Here, by tweaking packet headers in an IP address, an attacker masquerades as a genuine application. Thus, when users try accessing a linked URL, they're directed to the attacker's site.
- **DNS Spoofing (or Cache Poisoning)**: This involves compromising a DNS server and modifying a website’s address record. Consequently, users trying to visit the website are misled by the tampered DNS record to the attacker's site.

Once two-way SSL traffic is intercepted, the challenge is decrypting it without raising suspicion from either the user or the application. Several techniques can accomplish this:
- **SSL Stripping**: This technique intercepts the secure authentication (TLS) sent from the application towards the user, converting a HTTPS connection to a less secure HTTP one. The user receives an unencrypted version of the application's website, whereas the attacker keeps the encrypted connection with the actual application. This exposes the user's entire session to the attacker.
- **SSL Hijacking**: In this method, during a TCP handshake, the attacker provides counterfeit authentication keys to both the user and the application. This creates an illusion of a secure connection, while in reality, the attacker oversees the whole session.
- **HTTPS Spoofing**: When a user initially tries to connect to a secure site, this method sends a fake certificate to the user's browser. This certificate contains a digital signature linked to the targeted application. The browser cross-references this with its list of trusted sites. If deemed trustworthy, the attacker gains access to information the user inputs before it reaches the actual application.

#### Mitigations
There is a primary way to defend against MitM attacks during HTTP sessions and it's called **HTTP Strict Transport Security (HSTS)**. HTTPS, denoting HTTP encrypted with SSL or TLS, stands as a main component in establishing website traffic security. By using encryption, it significantly complicates an attacker's ability to intercept, change, or forge communication between a user and the website.
In scenarios where a user manually enters a web domain (omitting the http:// or https:// prefix) or follows an unsecured http:// link, the initial request to the website is transmitted without encryption, utilizing plain HTTP. Although most secure websites promptly respond with a redirect to move the user to an HTTPS connection, a strategically positioned attacker could execute a man-in-the-middle (MITM) attack, intercepting the initial HTTP request and potentially gaining control over the user's session.
To mitigate this potential vulnerability, HTTP Strict Transport Security (HSTS) comes into play by instructing the browser that a specific domain should only be accessed using HTTPS. Even if a user inputs or follows a basic HTTP link, the browser elevates the connection to HTTPS, providing an additional layer of protection against potential security threats.

This is established by sending the following HTTP response header from secure (HTTPS) websites:
```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
```
The optional `includeSubDomain`s parameter tells the browser that the HSTS policy also applies to all subdomains of the current domain. The `always` parameter ensures that the header is set for all responses, including internally generated error responses.

It is important to point out that if you use inherited headers you just need to place the `add_header` directive in the top‑level server block, **but** if a block includes an `add_header` directive itself, it does not inherit headers from enclosing blocks, and you need to redeclare **all** `add_header` directives:
```nginx
server {
    listen 443 ssl;

    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    # This 'location' block inherits the STS header
    location / {
        root /usr/share/nginx/html;
    }

    # Because this 'location' block contains another 'add_header' directive,
    # we must redeclare the STS header
    location /servlet {
        add_header X-Served-By "My Servlet Handler";
        add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
        proxy_pass http://localhost:8080;
    }
}
```

### DDoS attack
Application-layer attacks, referred to as layer 7 DDoS attacks, focus on the uppermost layer of the OSI (Open Systems Interconnection) model—the application layer. This layer encompasses common internet requests like HTTP GET and HTTP POST. What makes these attacks potent is their ability to consume both server and network resources, resulting in notable disruptions with a lower total bandwidth requirement compared to alternative DDoS attack types. Moreover, application layer attacks tend to be more sophisticated and challenging to identify and counteract than their counterparts.

#### Attack scenario
There are two attack scenarios for this kind of attack:
- **HTTP flood**: Involves sending a large number of HTTP requests to the target, for example, using a botnet or a network of compromised devices. The target may become unavailable if it is unable to process a large number of requests.
- **Sloworis**: An application-layer attack tactic characterized by the transmission of incomplete HTTP requests to the target while maintaining open connections for an extended duration. This technique aims to deplete the resources of the target, rendering it unavailable. To illustrate, the command `perl slowloris.pl -dns 192.0.2.1` utilizes the Slowloris.pl tool, scripted in Perl, to launch a Slowloris attack against a web server located at IP address 192.0.2.1. By sending partial HTTP requests and prolonging the connection duration, this approach disrupts the target's functionality, causing unavailability.

#### Mitigations
Speaking about mitigations in NGINX there are several setting that can be configured in order to prevent DDoS attacks:
- **Limiting the Number of Connections**: Limiting the number of connections that can be made from a single IP address can help prevent a single source from flooding your servers with requests. You can implement this method with Nginx using the `limit_conn` directive.
For example, the following configuration limits the number of connections to 100 per IP address:
```nginx
...
server {
    ...
    limit_conn conn_limit_per_ip 100;
}
```
You should be aware of setting this property on very low number can lead to weak performance, so you have to find the perfect balance. Determining your sweet spot involves monitoring your server’s performance and making adjustments based on how it handles incoming traffic.

- **Limiting the Request Size**: You can use the `limit_req` directive to limit request rates in Nginx. This directive limits the request rate to a specified number of requests per second for a particular key, which is typically based on the client’s IP address.
The following is an example configuration that limits the request rate to 10 requests per second per IP address:
```nginx
limit_req_zone $binary_remote_addr zone=req_limit_per_ip:10m rate=5r/s;
server {
    ...
    limit_req zone=req_limit_per_ip burst=10 nodelay;
}
```
In this example, the `limit_req_zone` directive defines the rate limit zone, and the `rate` parameter sets the limit to five requests per second (10r/s). The `burst` parameter specifies the number of requests that are allowed to exceed the limit, and the `nodelay` parameter ensures that requests that exceed the limit are immediately rejected.

- **Nginx Timeout Parameters**: Setting limits on how long Nginx will wait for certain events to occur, such as establishing a connection or receiving a response from a server, can prevent resources from being consumed by connections that take too long to complete.
You can use the `proxy_connect_timeout`, `proxy_send_timeout`, and `proxy_read_timeout` directives to implement this method. For example, the following configuration sets the timeout parameters to 10 seconds:
```nginx
server {
    ...
    proxy_connect_timeout10s;
    proxy_send_timeout 10s;
    proxy_read_timeout 10s;
}
```

- **Blacklisting and Whitelisting IP Ranges**: Utilizing IP range blacklisting and whitelisting is a DDoS protection strategy that revolves around defining which IP addresses are granted or denied access to your servers. In the context of blacklisting, access to a specific IP address range is restricted, whereas whitelisting permits access exclusively to a designated IP address range.
In Nginx, this approach can be implemented through the use of directives such as `deny` and `allow`. For instance, consider the following configuration, which denies access to all IP addresses except those falling within the 192.0.2.0/24 range:
```nginx
server {
    ...
    deny all;
    allow 192.0.2.0/24;
}
```
When defining which IPs to block, monitor server logs for patterns of excessive requests from specific IPs, refer to IP reputation databases for known malicious IPs, and consider using geolocation data to block IP ranges associated with a high rate of DDoS attacks.

### Carriage Return Line Feed (CRLF) Injection
Carriage Return Line Feed (CRLF) Injection is a type of web application security vulnerability that occurs when an attacker is able to inject CRLF characters (Carriage Return "\r" and Line Feed "\n") into input data. This type of injection can lead to various security issues, particularly in the context of HTTP headers.
In the HTTP protocol, CRLF is used to signify the end of a header line and the beginning of a new one. An attacker exploiting CRLF Injection may attempt to insert these characters into user inputs or data transmitted between a client and a server, with the goal of manipulating or injecting additional headers.

According to nginx documentation, `$uri` and `$document_uri` contain the normalized URI whereas the normalization includes URL decoding the URI.
This is example of how you do **not** want to configure your nginx:
```nginx
location /static/ {
  return 302 https://example.com$uri;
}
```
This works fine until we try to inject a CRLF like `/static/%0d%0aX-Foo:%20CLRF`. The response will look like this:
```
> GET /static/%0d%0aX-Foo:%20CLRF HTTP/1.1
> Host: 127.0.0.1:8081
> User-Agent: curl/7.74.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 302 Moved Temporarily
< Server: nginx/1.19.8
< Date: Mon, 29 Mar 2021 09:05:45 GMT
< Content-Type: text/html
< Content-Length: 145
< Connection: keep-alive
< Location: http://172.17.0.1/static/
< X-Foo: CLRF
< 
...
```

#### Mitigations
As we can see there is an **injected header**. However there is a solution for this problem and that is to avoid using `$uri` or `$document_uri` and insead use `$request_uri`. Now our location block looks like this:
```nginx
location /static/ {
    return 302 http://172.17.0.1$request_uri;
}
```

