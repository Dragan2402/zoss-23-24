# ZOSS-Tim 5

  - Bane Geric, R2-32/2023
  - Dragan Mirkovic, R2-27/2023


# Shipping Solutions Overview

This project aims to showcase the software system Shipping Solutions, including its complexity, the technologies used, and the cyber attacks and threats that each part of the system faces. It also highlights the mitigation and preventive measures taken to achieve a secure environment.

Shipping Solutions is a logistics application designed for shippers and carriers to facilitate the international shipping process. The main functionality for shippers includes receiving quotes and making contracts with carriers, while carriers define quotas/prices for services and digitize their business. The application handles a large amount of data and information crucial to be preserved, such as contractual documents, transactions, and reports on goods. These resources are significant and could be exposed to potential misuse.

The selection of appropriate technologies was crucial. Therefore, the technology stack of Cassandra, Flask, Nginx, and Angular was chosen, providing good performance and the necessary security for the system. An overview of these technologies is provided in the "Brief Description of Technologies" section.

The structure of this work and how it is organized is presented in the following section, "Project Structure."

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

Cassandra is an open-source NoSQL distributed and scalable storage system designed to handle large amounts of data across commodity servers that provides high availability without a single point of failure[[1]](https://cassandra.apache.org/doc/latest/index.html). 
Data is distributed across nodes, supporting replication and multi-data center deployment for redundancy and disaster recovery. The system scales linearly with the addition of nodes, providing uninterrupted read and write throughput. Cassandra is fault-tolerant, automatically replicating data to multiple nodes, with seamless node replacement in case of failures. It offers tunable consistency levels, prioritizing availability and partition tolerance[[1]](https://cassandra.apache.org/doc/latest/index.html). 

The data model is a wide column store, allowing dynamic table modifications without blocking updates. However, Cassandra lacks support for joins and subqueries, emphasizing denormalization through features like collections. The system is Java-based, managed and monitored using Java Management Extensions (JMX), and provides metrics through tools like nodetool and Dropwizard[[1]](https://cassandra.apache.org/doc/latest/index.html). Security analysis for Cassandra database is described on the following [page](https://github.com/Dragan2402/zoss-23-24/tree/main/1.%20Database%20layer).

### 2. Flask

Flask is a micro web framework written in Python and used for creating web applications, that provides a solid core with the basic services, while extensions provide the rest. It has no database abstraction layer, form validation, or any other components where pre-existing third-party libraries provide common functions. However, all of them can be used in Flask as extensions. It contains two main dependencies. The routing, debugging, and Web Server Gateway Interface (WSGI) subsystems come from Werkzeug, while template support is provided by Jinja2 [[4]](https://coddyschool.com/upload/Flask_Web_Development_Developing.pdf).

Werkzeug is a utility library that is used by Flask. It provides several featues:
- Provides request and response objects that encapsulate incoming HTTP requests and outgoing HTTP responses and supports URL routing with patterns
- Provides features like interactive debugging, a built-in debugger, and a reloader to automatically restart the server when changes are detected
- The Web Server Gateway Interface (WSGI) is a simple calling convention for web servers to forward requests to web applications or frameworks written in the Python programming language. Flask itself is built on top of Werkzeug's WSGI toolkit, allowing it to seamlessly interface with different web servers[[5]](https://werkzeug.palletsprojects.com/en/3.0.x/)

Jinja2 is a template engine for the Python programming language. It is similar to the Django web framework but provides Python-like expressions while ensuring that the templates are evaluated in a sandbox. It is a text-based template language and thus can be used to generate any markup as well as source code. Jinja2 implements automatic HTML escaping to prevent cross-site scripting (XSS) attacks[[6]](https://jinja.palletsprojects.com/en/3.1.x/).

Further security considerations are described on the following [page](https://github.com/Dragan2402/zoss-23-24/tree/main/2.%20Service%20layer).

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
- [5. Werkzeug documentation](https://werkzeug.palletsprojects.com/en/3.0.x/)
- [6. Jinja documentation](https://jinja.palletsprojects.com/en/3.1.x/)
