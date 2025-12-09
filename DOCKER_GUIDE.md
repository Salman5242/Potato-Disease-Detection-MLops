# Docker Setup Guide for Potato Disease Detection

This guide will help you build and run the Potato Disease Detection application using Docker.

## Prerequisites

1. **Install Docker Desktop** for Windows
   - Download from: https://www.docker.com/products/docker-desktop
   - Make sure Docker Desktop is running before executing commands

2. **Verify Docker Installation**
   ```bash
   docker --version
   docker-compose --version
   ```

## Quick Start

### Option 1: Using Docker Compose (Recommended)

1. **Build and start the container:**
   ```bash
   docker-compose up --build
   ```

2. **Access the application:**
   - Open your browser and go to: `http://localhost:8080`

3. **Stop the container:**
   ```bash
   docker-compose down
   ```

### Option 2: Using Docker Commands

1. **Build the Docker image:**
   ```bash
   docker build -t potato-disease-detection .
   ```

2. **Run the container:**
   ```bash
   docker run -d -p 8080:8080 --name potato-app potato-disease-detection
   ```

3. **Access the application:**
   - Open your browser and go to: `http://localhost:8080`

4. **Stop the container:**
   ```bash
   docker stop potato-app
   docker rm potato-app
   ```

## Advanced Usage

### View Container Logs

```bash
# Using docker-compose
docker-compose logs -f

# Using docker
docker logs -f potato-app
```

### Access Container Shell

```bash
# Using docker-compose
docker-compose exec potato-disease-app bash

# Using docker
docker exec -it potato-app bash
```

### Rebuild After Code Changes

```bash
# Using docker-compose
docker-compose up --build

# Using docker
docker build -t potato-disease-detection .
docker stop potato-app
docker rm potato-app
docker run -d -p 8080:8080 --name potato-app potato-disease-detection
```

### Run in Background (Detached Mode)

```bash
docker-compose up -d
```

### View Running Containers

```bash
docker ps
```

### Stop and Remove Everything

```bash
# Using docker-compose
docker-compose down -v

# Using docker
docker stop potato-app
docker rm potato-app
docker rmi potato-disease-detection
```

## Volume Mounts

The docker-compose.yml file includes volume mounts for:
- `./static:/app/static` - Persists uploaded images
- `./Models:/app/Models` - Allows model updates without rebuilding

## Troubleshooting

### Port Already in Use
If port 8080 is already in use, modify the port mapping in `docker-compose.yml`:
```yaml
ports:
  - "8081:8080"  # Change 8081 to any available port
```

### Model Not Found Error
Ensure the model file exists at: `Models/.keras/Alpha.keras`

### Out of Memory
If you encounter memory issues, increase Docker Desktop memory allocation:
- Open Docker Desktop → Settings → Resources → Memory
- Increase to at least 4GB

### Permission Issues on Windows
Run PowerShell or Command Prompt as Administrator

## Production Deployment

For production deployment, consider:

1. **Use environment variables for configuration**
2. **Set up HTTPS with a reverse proxy (nginx)**
3. **Use a production WSGI server like Gunicorn:**

   Modify the Dockerfile CMD to:
   ```dockerfile
   CMD ["gunicorn", "--bind", "0.0.0.0:8080", "--workers", "4", "app:app"]
   ```

   Add to requirements.txt:
   ```
   gunicorn==21.2.0
   ```

4. **Push to Docker Hub:**
   ```bash
   docker tag potato-disease-detection yourusername/potato-disease-detection:latest
   docker push yourusername/potato-disease-detection:latest
   ```

## Health Check

The container includes a health check that runs every 30 seconds. Check health status:

```bash
docker ps
# Look for the STATUS column showing "healthy"
```

## Additional Resources

- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Flask Deployment Options](https://flask.palletsprojects.com/en/2.3.x/deploying/)
