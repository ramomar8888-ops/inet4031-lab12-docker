INET 4031 Lab 12
Docker: Three Tier Application
Overview

This project containerizes a three tier web application using Docker and Docker Compose. The system consists of an Apache reverse proxy, a Flask API, and a MariaDB database. Each service runs in its own container and communicates over a shared Docker network.

The goal is to demonstrate how multi service applications can be modularized, deployed, and managed using containerization.

Architecture

The application follows a three tier design:

Web Tier (Apache)
Handles incoming HTTP requests on port 80 and acts as a reverse proxy. Routes API requests to the Flask service.
Application Tier (Flask)
Provides REST API endpoints for ticket management. Handles business logic and communicates with the database.
Data Tier (MariaDB)
Stores persistent ticket data. Initialized with a schema and seed data on first run.

Flow of Request:
Client → Apache → Flask → MariaDB

Key Concepts Demonstrated
Containerization

Each service runs in an isolated container with its own dependencies. This ensures consistency across environments.

Reverse Proxy

Apache does not process application logic. It forwards API requests to Flask using proxy rules defined in flask-app.conf.

Service Discovery

Containers communicate using service names defined in Docker Compose. For example, Flask connects to the database using db as the hostname.

Environment Variables

Sensitive data such as database credentials are stored in a .env file and injected into containers at runtime. This prevents secrets from being committed to version control.

Health Checks

Containers are monitored using health checks. A container can be running but not healthy. Dependencies are configured so services only start when required components are fully ready.

Repository Structure
apache/
Contains Apache Dockerfile and reverse proxy configuration
app/
Contains Flask application, dependencies, and database initialization script
docker-compose.yml
Defines services, networking, volumes, and startup dependencies
.env
Stores environment variables for database credentials
.gitignore
Prevents sensitive files such as .env from being committed
Key Implementation Decisions
Layer Caching in Docker

requirements.txt is copied and installed before app.py to take advantage of Docker layer caching. This avoids reinstalling dependencies on every build.

Persistent Storage

MariaDB uses a named volume to persist data across container restarts. This ensures data durability.

Startup Ordering

Services depend on health checks rather than just container startup.

Database must be healthy before Flask starts
Flask must be healthy before Apache starts

This prevents connection failures during initialization.

Verification Results

The application was validated through the following checks:

Docker Compose successfully builds and runs all containers
Database and Flask containers report healthy status
Web dashboard loads correctly through Apache
API health endpoint confirms database connectivity
Ticket creation and retrieval works correctly
Data persists after restarting the database container
All automated checks pass using the provided script
