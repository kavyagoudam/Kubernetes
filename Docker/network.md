"𝑹𝒖𝒏𝒏𝒊𝒏𝒈 𝑫𝒐𝒄𝒌𝒆𝒓 𝒊𝒔 𝒍𝒊𝒌𝒆 𝒉𝒂𝒗𝒊𝒏𝒈 𝒂 𝒎𝒂𝒈𝒊𝒄  𝒂𝒏𝒅 𝒇𝒐𝒓 𝒚𝒐𝒖𝒓 𝒄𝒐𝒅𝒆—𝒋𝒖𝒔𝒕 𝒅𝒐𝒏’𝒕 𝒇𝒐𝒓𝒈𝒆𝒕 𝒕𝒉𝒂𝒕 𝒘𝒊𝒕𝒉 𝒈𝒓𝒆𝒂𝒕 𝒑𝒐𝒘𝒆𝒓 𝒄𝒐𝒎𝒆𝒔 𝒈𝒓𝒆𝒂𝒕 𝒓𝒆𝒔𝒑𝒐𝒏𝒔𝒊𝒃𝒊𝒍𝒊𝒕𝒚... 𝒂𝒏𝒅 𝒍𝒐𝒕𝒔 𝒐𝒇 𝒄𝒐𝒏𝒕𝒂𝒊𝒏𝒆𝒓𝒔!" 😆

Docker🐬 is a platform that simplifies the process of developing, shipping, and running applications by using containerization technology. Containers allow you to package an application and its dependencies together into a single, portable unit.



✭ Docker Images — Read-only templates used to create containers. Images can be based on a variety of operating systems and software stacks. They are built from Dockerfile instructions. For a Dockerfile, the D has to be uppercase all the time.

✭ Docker Containers — The running instances of Docker images. Containers encapsulate the application and its environment, ensuring consistency across different environments (development, testing, production)

✭ Docker Networking — Primarily used to establish the communication between docker containers and also the outside world. As long as the containers are in the same docker network, they can communicate using the inter container communication

By default, docker has 3 networks they are:

Bridge network
Host network
Null network

✭ Bridge network: While we create containers, if we do not specify the container name, it selects the default network called Default Bridge network. The containers communicate with each other using the container ip and the respective port that are in the same network. To comminicate with containers that are out of our docker host network, we do port binding.

Container IPs are Dynamic. If a container is deleted and recreated again, there is no guarantee that we get the same IP again. So if the other container tries to communicate with in the same network to the old IP of that container, it won’t work. Hence, we create a custom bridge network where one container communicates with the other using the hostname of the containers and in the backend it resolves to the IP address of that container.

✭ Host Network: When a container is directly created in a Host Network, it won’t get any IP address. There’s no network isolation. We cannot run multiple containers in the Host network, because there would be a port conflict.

✭ Null Network: These contianers will not have any IP addresses. You can’t communicate without or out of the container as there is no network. But, the process in the container will be running. Just that it has no communication to it.

![networking](img/network.png)

exercise:-

Deploy a web application named myapp using the Saran/simple-webapp-mysql image. Expose the port to 10400 on the host. The application makes use of two environment variables:

1: DB_Host with the value mysql-db.

2: DB_Password with the value db_pass123.

Make sure to attach it to the newly created network called sk-mysql-network. Also make sure to link the MySQL and the webapp container.

✭ SOLUTION

docker run -p 10400:8080 -e DB_Host=mysql-db -e DB_Password=db_pass123 — — name myapp — — link mysql-db:mysql-db — — network=sk-mysql-network -d saran/simple-webapp-MySQL

-p: is used to define the ports

10400:8080: Here we did port binding for these two ports. 10400 is host port on local machine and 8080 is the port of the web application.

-e: is a command line option used to define environment variables inside the container

— — name: used to defining the name of the web application — myapp

— — link: used to link a container with another container named mysql-db. It also sets alias mysql-bd for the linked container.

— — network: defining the network to be mapped to the container

-d: runs the container in deteched mode so while pulling the image/ setting up the container, it won’t show the details of it in the foreground allowing us to do our other tasks

saran/simple-webapp-MySQL: Specifying the docker image to use for our container.

✭✭✭✭✭✭✭ Let’s get Dockered 🐬✭✭✭✭✭✭✭