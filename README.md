# Assignment Submission: Nginx Reverse Proxy with Docker Compose

Mentor: Upendra Kumar

Name: Sakshee Zala

## 1. Project Overview

This project deploys a full-stack CRUD application using Docker Compose. It features a central Nginx reverse proxy that handles all incoming traffic. The internal services (frontend, backend, database) are completely isolated from the host machine and communicate over custom Docker networks.

- **Proxy (Nginx)**: The only container exposed to the outside (Port 80). It routes website traffic to the frontend and data requests to the backend.
- **Frontend (React/Vite)**: Served internally by its own built-in Nginx server on port 80.
- **Backend (Node.js)**: Runs internally on port 5000 and connects to the database.
- **Database (PostgreSQL)**: Stores application data internally on port 5432.

## 2. Network Architecture

To follow security best practices, the docker-compose.yml file uses two separate custom networks:

- **backend-net**: Connects the backend to the database, and the proxy to the backend.
- **frontend-net**: Connects the proxy to the frontend.

## 3. Configuration Files

- [nginx.conf](frontend/nginx/default.conf)
- [docker-compose.yml](docker-compose.yml)

## 4. Deployment Command

To build the Docker images and start all four containers at the same time, run the following command in the terminal:

```bash
docker compose up --build
```

## 5. Request Flow

1. A user visits http://localhost in their browser. The Nginx proxy catches this request and forwards it to the frontend container to serve the React files.
2. When the user tries to create or load an item, the React app sends an HTTP request to http://localhost/api/items.
3. The Nginx proxy sees the /api/ path and forwards the request to the backend container on port 5000 over the internal network.
4. The backend processes the request, saves or fetches data from the PostgreSQL database, and sends the success response back through the proxy to the user's screen.

## 6. Screenshots

- **Screenshot 1: Starting the Setup**
- **Screenshot 2: Running Containers & Ports**
- **Screenshot 3: Frontend Application**
- **Screenshot 4: Successful item creation**