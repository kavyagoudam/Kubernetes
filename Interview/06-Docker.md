

1. Question: How would you free up disk space on a Docker host? 
Answer: Remove unused containers with: docker container prune
Remove unused images (dangling and unreferenced): docker image prune -a
Remove unused volumes: docker volume prune
Clean up unused networks: docker network prune
If you need to reclaim space from stopped containers, old images, and other objects in one go: docker system prune -a

2. Question: How would you troubleshoot and debug a running Docker container?
Answer: Access the container’s shell using: docker exec -it <container_name> /bin/
Check container logs using: docker logs <container_name>
Inspect the container’s state with: docker inspect <container_name>

3.  How would you troubleshoot and resolve network connectivity issues between Docker containers?
Answer: Verify network configuration using: docker network inspect <network_name>
Check connectivity between containers using: docker exec -it <container_name> ping <other_container_ip_or_name>
If DNS resolution is failing, ensure you are using the correct container names, as Docker provides built-in DNS resolution for containers on the same network.

4. Question: How would you investigate and resolve a container that continuously restarts?
Answer: Examine the logs using: docker logs <container_name>
Use the docker inspect command to check the container’s restart policy: docker inspect <container_name> | grep RestartPolicy
You may need to adjust the policy to avoid endless restarts: docker run --restart=on-failure:3 <container_image>

5. Question: What steps would you take to secure a Docker container?
Answer: Avoid running applications as root inside the container. In the Dockerfile, create a non-root user and set it: RUN useradd -ms /bin/ myuser && USER myuser
Enable user namespaces to map container users to different users on the host.
Only expose the necessary ports and limit external access with firewall rules. You can also use Docker’s bridge network mode for isolation.

6. Question: How would you roll back to a previous Docker image version?
Answer: If you tagged the previous image version properly, you can start the container with the old version: docker run -d my_image:previous_version
If the old image is still present on the system, you can list all available images and their tags using: docker images
Then, run the older version using its image ID: docker run -d <image_id>

7. Question: How would you force stop a Docker container that refuses to stop?
Answer: Attempt a graceful stop: docker stop <container_name>
If the container does not stop after the default timeout, you can forcibly kill it using: docker kill <container_name>
If the issue persists and the container is unresponsive, you can investigate deeper using docker inspect to see if there are resource constraints or issues with the container’s process. Additionally, check the system's logs for any low-level kernel issues that may be affecting Docker.
8. Question: How would you limit CPU and memory usage of a Docker container?
Answer: You can limit CPU usage by using the --cpus flag: docker run --cpus="1.5" my_container 
To limit memory usage, use the --memory flag: docker run --memory="512m" my_container
You can also combine both CPU and memory limits for stricter control: docker run --cpus="1" --memory="512m" my_container

9. Question: How would you optimize the Docker image size?
Answer: Use a minimal base image like alpine, which is smaller in size compared to ubuntu or debian. Ensure multi-stage builds are implemented. Clean up apt-get or yum caches after installing packages. Use .dockerignore to exclude unnecessary files such as test data or local configuration files. Combine commands using && to reduce the number of layers in the final image.

10. Question: Why does the container exit immediately after it starts, and how would you troubleshoot this?
Answer: Check the logs using docker logs <container_name> to see if there is any error or termination reason. Make sure the container is running a long-running process, such as a web server or tail -f /dev/null, if testing. If using a custom script, ensure the script has proper error handling and logging.

11. Question: How would you resolve a Docker container failing to start due to a port conflict?
Answer: Identify which process is using the port by running lsof -i :80 or netstat -tuln | grep 80. If you need to bind to another port on the host, you can do this using the -p option in Docker: docker run -d -p 8080:80 my_container

12. Question: What are the steps you would take to troubleshoot connectivity issues to a Docker container from the host?
Answer: Ensure the container is running: docker ps.
Check if the container port is mapped to the host using docker port <container_id>. Verify network access using curl or telnet to check if the port is open and responding.

13. Question: How would you prevent data loss between Docker container restarts?
Answer: Use Docker volumes to store data outside the container’s writable layer. This ensures that data is preserved even if the container is stopped or deleted. docker run -d -v /host/data:/container/data my_container This binds a directory from the host (/host/data) to a directory in the container (/container/data). Alternatively, use Docker-managed volumes: docker run -d -v my_volume:/container/data my_container

14. Question: How would you handle dependency issues during Docker image builds?
Answer: Ensure all required dependencies are listed in the Dockerfile and installed before they are needed. Use a package manager (e.g., apt-get, yum, or pip) to install dependencies in the correct order. For language-specific dependencies (e.g., Node.js, Python), make sure the required package files (package.json, requirements.txt) are copied into the image before running installation commands.
