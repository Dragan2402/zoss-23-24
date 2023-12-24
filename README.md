# ZOSS-Tim 5

  - Bane Geric, R2-32/2023
  - Dragan Mirkovic, R2-27/2023


# Project Overview

This project aims to illustrate a complex system and its layers. It will showcase each module, representing a technology, along with its strengths, weaknesses, and security aspects. For each module, the most common security attacks, potential solutions, and security issues they introduce will be presented.

# Project Structure

The main readme file will explain the system architecture and technologies used in project. The project will be organized with a folder structure where each folder corresponds to a unit in the system. Additionally, there will be two folders: [**Resources**](https://github.com/Dragan2402/zoss-23-24/tree/main/Resources) and [**Other**](https://github.com/Dragan2402/zoss-23-24/tree/main/Other). The [**Resources**](https://github.com/Dragan2402/zoss-23-24/tree/main/Resources) folder represents external sources used in the development of this solution, while the [**Other**](https://github.com/Dragan2402/zoss-23-24/tree/main/Other) folder will contain interesting facts, articles recommended for reading to expand knowledge, and other miscellaneous items.

## Architecture

The architecture consists of four layers:
1. [**Database Layer** - Cassandra NoSQL database](https://github.com/Dragan2402/zoss-23-24/tree/main/1.%20Database%20layer)
2. [**Service Layer** - Django application](https://github.com/Dragan2402/zoss-23-24/tree/main/2.%20Service%20layer)
3. [**Load Balancer** - Nginx](https://github.com/Dragan2402/zoss-23-24/tree/main/3.%20Load%20balancer%20layer)
4. [**Client Layer** - Angular application](https://github.com/Dragan2402/zoss-23-24/tree/main/4.%20Client%20layer)

![Architecture Image](ZossArchitecture.png)

## Brief Description of Technologies:

### 1. Cassandra

Cassandra is an open-source NoSQL distributed and scalable storage system designed to handle large amounts of data across commodity servers that provides high availability without a single point of failure[[1]](https://cassandra.apache.org/doc/latest/index.html). Security analysis for Cassandra database is described on the following [page](https://github.com/Dragan2402/zoss-23-24/tree/main/1.%20Database%20layer).

### 2. Flask

Flask is micro-framework used for creating web applications, that provides a solid core with the basic services, while extensions provide the rest. It contains two main dependencies. The routing, debugging, and Web Server Gateway Interface (WSGI) subsystems come from Werkzeug, while template support is provided by Jinja2 [[4]](https://coddyschool.com/upload/Flask_Web_Development_Developing.pdf). Further security considerations are described on the following [page](https://github.com/Dragan2402/zoss-23-24/tree/main/2.%20Service%20layer).

### 3. Nginx

NGINX is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers [[3]](https://www.nginx.com/resources/glossary/nginx/). Common attacks, vulnerabilities and mitigations related to NGINX can be found on the following [page](https://github.com/Dragan2402/zoss-23-24/tree/main/3.%20Load%20balancer%20layer).

### 4. Angular

Angular is a development platform, built on TypeScript[[2]](https://angular.io/docs). As a platform, Angular includes:
- A component-based framework for building scalable web applications
- A collection of well-integrated libraries covering various features, including routing, forms management, client-server communication, and more
- A suite of developer tools to aid in code development, building, testing, and updating
Detailed information about security considerations and attacks are explained on the following [page](https://github.com/Dragan2402/zoss-23-24/tree/main/4.%20Client%20layer).

The first and last layer, **Database Layer** and **Client Layer**, will be covered by **Dragan Mirkovic**, while the internal layers, **Service Layer** and **Load Balancer**, will be handled by Bane Geric.

References: 
- [1. Cassandra documentation](https://cassandra.apache.org/doc/latest/index.html)
- [2. Angular documentation](https://angular.io/docs)
- [3. Nginx documentation](https://www.nginx.com/resources/glossary/nginx/)
- [4. Flask Web Development](https://coddyschool.com/upload/Flask_Web_Development_Developing.pdf)
