# headintheclouds Flask Web App

This is a simple Flask web application that demonstrates Docker containerization and data persistence using volumes.

## Features

- View a list of users
- Add new users
- Demonstrate data persistence using Docker volumes

## Prerequisites

- [Docker](https://www.docker.com/)
- [Git](https://git-scm.com/)

## Getting Started

1. Clone the repository:

   ```bash
   git clone git@github.com:mili-sucevic/hitc-docker-volume-demo.git
   cd hitc-docker-volume-demo

2. Build the Docker image:

    ```bash
    docker build -t webapp-without-volume .

3. Run the Docker container:

    ```bash
    docker run -d --name webapp-without-volume -p 8080:8080 -v hitc-data:/app/data webapp-without-volume

4. Access the web app at http://localhost:8080

5. Add/List Users

6. Stop and Remove container to demonstrate data loss when removing containers without volumes:

    ```bash 
    docker stop webapp-without-volume
    docker rm webapp-without-volume

7. Re-run the container:
    
    ```bash
    docker run -d --name webapp-without-volume -p 8080:8080 -v hitc-data:/app/data webapp-without-volume

8. Access the web app at http://localhost:8080 again and observe that the previously added users are no longer present.

9. Modify the Dockerfile to include a VOLUME instruction

    ```Dockerfile
    FROM python:3.9

    WORKDIR /app

    COPY requirements.txt .

    RUN pip install --no-cache-dir -r requirements.txt

    COPY . .

    EXPOSE 8080

    # Declare the volume
    VOLUME /app/data

    CMD ["python", "app.py"]

10. Make sure to build the new Docker image with volume defined after making these changes:

    ```bash
    docker build -t webapp-with-volume .

11. When running the container, you can use the -v option to create and mount the volume:

    ```bash
    docker run -d --name webapp-with-volume -p 8080:8080 -v hitc-data:/app/data webapp-with-volume

12. Access the web app at http://localhost:8080 and repeat the same restart steps to ensure data persistence, as the data will now be stored in the 'hitc-data' volume allowing it to survive container restarts.
