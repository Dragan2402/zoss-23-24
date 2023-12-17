# ZOSS-Tim 5

  - Bane Geric, R2-32/2023
  - Dragan Mirkovic, R2-27/2023


# Project Overview

This project aims to illustrate a complex system and its layers. It will showcase each module, representing a technology, along with its strengths, weaknesses, and security aspects. For each module, the most common security attacks, potential solutions, and security issues they introduce will be presented.

# Project Structure

The main readme file will explain the architecture and technologies used, along with their explanations. The project will be organized with a folder structure where each folder corresponds to a unit in the system. Additionally, there will be two folders: "resources" and "additional things." The "resources" folder represents external sources used in the development of this solution, while the "additional things" folder will contain interesting facts, articles recommended for reading to expand knowledge, and other miscellaneous items.

## Architecture

The architecture consists of four layers:
1. [**Database Layer** - Cassandra NoSQL database](https://github.com/Dragan2402/zoss-23-24/tree/main/1.%20Database%20layer)
2. **Service Layer** - Django application
3. **Load Balancer** - Nginx
4. **Client Layer** - Angular application

## Brief Description of Technologies:

### 1. Cassandra

Cassandra is an open-source NoSQL distributed database that manages large amounts of data across commodity servers. It is a decentralized, scalable storage system designed to handle vast volumes of data across multiple commodity servers, providing high availability without a single point of failure.

### 2. Django
-- Description to be added --

### 3. Nginx
-- Description to be added --

### 4. Angular

Angular is a development platform, built on TypeScript. As a platform, Angular includes:
- A component-based framework for building scalable web applications
- A collection of well-integrated libraries covering various features, including routing, forms management, client-server communication, and more
- A suite of developer tools to aid in code development, building, testing, and updating

Detailed information on security and attacks will be explained for each technology in their respective dedicated chapters.
