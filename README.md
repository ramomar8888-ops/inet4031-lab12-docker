# Docker Three-Tier Ticket System

## Overview

This project is a containerized three-tier web application built using Docker and Docker Compose. It simulates a real-world ticketing system and demonstrates how multiple services communicate in a distributed environment.

The system includes:

- Apache2 (Reverse Proxy / Web Server)
- Flask (Backend API)
- MariaDB (Database)

All services run in isolated containers and communicate over a shared Docker network.

---

## Architecture

User → Apache (Port 80) → Flask API (Port 5000) → MariaDB Database

Apache acts as a reverse proxy and forwards API requests to the Flask backend. Flask handles application logic and communicates with the MariaDB database to store and retrieve ticket data.

Docker Compose manages service orchestration, networking, and service health dependencies.

---

## Services

### Apache (web)
- Acts as reverse proxy
- Exposes port 80 to host
- Forwards `/api/*` requests to Flask

### Flask (app)
- Python REST API
- Handles ticket creation and retrieval
- Connects to MariaDB using environment variables

### MariaDB (db)
- Stores ticket data
- Initialized using `init.sql`
- Persists data using Docker volumes
