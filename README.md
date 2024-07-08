# Full-Stack FastAPI and React Template

Welcome to the Full-Stack FastAPI and React template repository. This repository serves as a demo application for interns, showcasing how to set up and run a full-stack application with a FastAPI backend and a ReactJS frontend using ChakraUI.

## Project Structure

The repository is organized into two main directories:

- **frontend**: Contains the ReactJS application.
- **backend**: Contains the FastAPI application and PostgreSQL database integration.

Each directory has its own README file with detailed instructions specific to that part of the application.

## Docker Compose Setup for Your Application

This README will guide you through the setup and understanding of the Docker Compose file used to run your application, including a frontend, backend, Traefik proxy, PostgreSQL database, and Adminer.

### Overview

The Docker Compose file (`docker-compose.yml`) defines how the different parts of your application work together using Docker containers. Here's a brief overview of each service:

- **Traefik**: A reverse proxy that helps route traffic to the appropriate service.
- **Frontend**: The user-facing part of your application.
- **Backend**: The server-side part of your application, including API endpoints.
- **PostgreSQL**: A relational database to store your application's data.
- **Adminer**: A database management tool to interact with your PostgreSQL database.

### Services

#### Traefik

- **Image**: Uses the `traefik:v2.5` Docker image.
- **Ports**: Exposes ports `80` for HTTP and `8090` for Traefik's web UI.
- **Volumes**: Shares the Docker socket to communicate with Docker.
- **Environment Files**: Reads environment variables from `backend/.env` and `frontend/.env`.

**Configuration**:
```
traefik:
  image: traefik:v2.5
  command:
    - "--api.insecure=true"
    - "--providers.docker=true"
    - "--entrypoints.web.address=:80"
  ports:
    - "80:80"
    - "8090:8080"
  volumes:
    - "/var/run/docker.sock:/var/run/docker.sock:ro"
  env_file:
    - backend/.env
    - frontend/.env
  networks:
    - web
```

### Frontend

- **Build**: Uses the Dockerfile located in the `frontend` directory.
- **Labels**: Configures routing rules for Traefik.
- **Environment Files**: Reads environment variables from `frontend/.env`.
- **Depends On**: Depends on the backend service to be running.

**Configuration**:
```
frontend:
  build: ./frontend
  labels:
    - "traefik.http.routers.frontend.rule=Host(`braindeadsprite.mooo.com`)"
    - "traefik.http.routers.frontend.entrypoints=web"
  env_file:
    - frontend/.env
  networks:
    - web
  depends_on:
    - backend
```

### Backend

**Build:** Uses the Dockerfile located in the backend directory.

**Labels:** Configures routing rules and middleware for Traefik.

**Environment Files:** Reads environment variables from `backend/.env`.

**Depends On:** Depends on the PostgreSQL service to be running.

**Configuration:**

```
backend:
  build: ./backend
  labels:
    - "traefik.http.routers.backend.rule=Host(`braindeadsprite.mooo.com`) && (PathPrefix(`/api`) || PathPrefix(`/docs`) || PathPrefix(`/redoc`))"
    - "traefik.http.routers.backend.entrypoints=web"
    - "traefik.http.middlewares.backend-stripprefix.stripprefix.prefixes=/api,/docs,/redoc"
    - "traefik.http.routers.backend.middlewares=backend-stripprefix"
  env_file:
    - backend/.env
  depends_on:
    - postgres
```

### PostgreSQL

**Image:** Uses the `postgres:13` Docker image.

**Environment:**
- `POSTGRES_USER`: test
- `POSTGRES_PASSWORD`: WtlFgKgEfHs48nTs6GxzkmBKX74rr7Tk
- `POSTGRES_DB`: test_bgrl
- `POSTGRES_SERVER`: dpg-cq5adm88fa8c73844plg-a.oregon-postgres.render.com

**Volumes:** Persists data using Docker volumes.

**Configuration:**

```
postgres:
  image: postgres:13
  environment:
    POSTGRES_USER: test
    POSTGRES_PASSWORD: WtlFgKgEfHs48nTs6GxzkmBKX74rr7Tk
    POSTGRES_DB: test_bgrl
    POSTGRES_SERVER: dpg-cq5adm88fa8c73844plg-a.oregon-postgres.render.com
  volumes:
    - pgdata:/var/lib/postgresql/data
  networks:
    - web
```

### Adminer

**Image:** Uses the `adminer` Docker image.

**Ports:** Exposes port `8080` for accessing the Adminer web interface.

**Restart Policy:** Always restarts the container if it stops.

**Configuration:**

```
adminer:
  image: adminer
  ports:
    - "8080:8080"
  networks:
    - web
  restart: always
```

### Volumes

**pgdata:** A named volume to persist PostgreSQL data.

**Configuration:**

```
volumes:
  pgdata:
```

### Networks

**web:** A Docker network to allow communication between services.

**Configuration:**

```
networks:
  web:
```

### How to Use

1. **Install Docker and Docker Compose**: Ensure Docker and Docker Compose are installed on your machine.

2. **Run Docker Compose**:
   Navigate to the directory containing your `docker-compose.yml` file and run:
   ```
    docker-compose up -d
   ```

3. **Access the Application**:
    - Frontend: Open your browser and go to http://braindeadsprite.mooo.com.
    - Backend API: Access the API endpoints at http://braindeadsprite.mooo.com/api.
    - Adminer: Manage your database at http://<your_server_ip_or_domain>:8080.

4. **Stop Docker Compose**: To stop all running services, run:
```sh
docker-compose down
```

### Troubleshooting
1. Traefik Dashboard: Access the Traefik dashboard at http://<your_server_ip_or_domain>:8090 to monitor and troubleshoot routing.
2. Logs: Check the logs for any errors using:
```sh
docker-compose logs
```

## Getting Started with the Application

## Getting Started

To get started with this template, please follow the instructions in the respective directories:

- [Frontend README](./frontend/README.md)
- [Backend README](./backend/README.md)

This README should help both coders and non-coders understand how your Docker Compose setup works and how to interact with it. Feel free to contact me for Paid tutorials.


