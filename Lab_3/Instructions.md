Hands-on exercise 3: Configuring advanced Docker networking scenarios


Step 1: Creating a custom bridge network

In this task, you will learn how to create a custom network and attach containers to it.
To create a custom network, run the following command:

docker network create mynetwork

This will create a new network named mynetwork.

Step 2: Attach container to the network

To attach containers to the network, run the following command:

docker network connect mynetwork myapp

This will connect existing container to the existing network.

Step 3: Create new container using existing image and network

To create new container, run the following command:

docker run -d -p 8083:80 --name myapp3 --network mynetwork myapp


This will start a container named myapp3 and attach it to the mynetwork network.

Step 4: Verify the network configuration

To verify the network configuration, run the following command:

docker inspect myapp
docker inspect network mynetwork

This will display the container configuration, including the network configuration.

Step 5: Test the connection

There are various ways to test the connection between the two containers. Here are a few methods:

 
a) Use the container IP addresses

You can test the connection using the container IP addresses. Run the following commands to get the IP addresses of the two containers:


docker inspect -f '{{ .NetworkSettings.IPAddress }}' myapp
docker inspect -f '{{ .NetworkSettings.IPAddress }}' myapp3

If you don’t see IP address for myapp3 run the following command:

docker inspect myapp3

Use the IP address of myapp3 container in the following command to test the connection from myapp:

docker exec myapp ping <myapp3_ip_address>

If ping still does not work, exec into myapp container and install the ping utility by running the following commands:

docker exec -it myapp /bin/bash


apt-get update -y
apt-get install -y iputils-ping

This will update the package list and install the ping utility. Exit the container and try to ping again.

b) Use the container names

You can also test the connection between the two containers using their names. Run the following command to test the connection from myapp to myapp3:

docker exec -it myapp /bin/bash
/app# ping myapp3
/app# exit


Step 6: Isolating Containers with Networks

You are going to use Docker networks to isolate containers, and to do so, first you will create a new network.

To create a new network, run the following command:

docker network create myisolatednetwork

This will create a new network named myisolatednetwork with the bridge driver.

Step 7: Start a container on the isolated network

To start a container on the isolated network, run the following command:

docker run -d --name isolatedcontainer --network myisolatednetwork nginx

This will start a new container named isolatedcontainer on the myisolatednetwork network.

docker run -d --name isolatedcontainer2 --network myisolatednetwork myapp

Step 8: Verify network isolation

To verify network isolation, attempt to ping the isolatedcontainer from myapp container:

docker exec myapp ping isolatedcontainer

This should fail to ping the isolatedcontainer since it is on a different network.

 


Congratulations!


You've completed Hands-on exercise 3 where you have learned how to create, manage network and use them to isolate the containerized workload in Docker.