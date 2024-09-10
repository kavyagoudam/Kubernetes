Clutch Docker Commands You Need to Know!

Are you a Docker enthusiast looking to boost your skills? Here are some essential Docker commands to get you started:

📍Image Management📍

- `docker images`: List all Docker images
- `docker pull <image_name>`: Download a Docker image
- `docker rmi <image_name>`: Remove a Docker image

📍Container Management📍

- `docker ps`: List running containers
- `docker ps -a`: List all containers (running and stopped)
- `docker run <options> <image_name>`: Create and start a container
- `docker start <container_id or container_name>`: Start a stopped container
- `docker stop <container_id or container_name>`: Stop a running container
- `docker restart <container_id or container_name>`: Restart a container
- `docker rm <container_id or container_name>`: Remove a stopped container

📍Container Inspection📍

- `docker inspect <container_id or container_name>`: Display container details
- `docker logs <container_id or container_name>`: View container logs
- `docker exec -it <container_id or container_name> <command>`: Execute a command in a container

📍Docker Compose📍

- `docker-compose up`: Create and start containers
- `docker-compose down`: Stop and remove containers
- `docker-compose ps`: List containers managed by Docker Compose

📍Registry Management📍

- `docker login`: Log in to a Docker registry
- `docker push <image_name>`: Push a Docker image to a registry
- `docker tag <image_name> <new_image_name>`: Tag a Docker image
- `docker logout`: Log out from a Docker registry

📍Networking📍

- `docker network ls`: List Docker networks
- `docker network inspect <network_id or network_name>`: Display network details

Mastering these commands will take your Docker skills to the next level!
