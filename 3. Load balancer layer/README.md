# NGINX Security Considerations

NGINX is very powerful, open-source, web server, load balancer, reverse proxy and more used by more than 60% of the internet web sites. Therefore it can be very interesting target for attackers. There are several reasons why would NGINX be interesting for attackers such as: many high-traffic and critical websites use Nginx for its performance and scalability, it is often used as a reverse proxy and load balancer, handling incoming requests and distributing traffic across multiple servers. Attacking the proxy layer allows attackers to impact multiple servers behind it. As NGINX can be used to handle SSL/TLS certificates, attackers may attempt to exploit vulnerabilities related to encryption protocols or certificate handling.

### HTTP Strict Transport Security with MitM attack

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

### TODO:  SSL/TLS ciphers problems


