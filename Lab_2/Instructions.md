Hands-on exercise 2: Managing volumes and data in Docker containers



Step 1: Create a volume


Create a new volume using the following command in terminal:


docker volume create mydata


This command will create a new volume named mydata.


Step 2: Mount the volume to the container


Run the following command to start the container and mount the volume to the /app/data directory in the container:


docker stop myapp
docker rm myapp
docker run -d -p 8080:80 -v mydata:/app/data --name myapp myapp


This command will start the container and map port 8080 on the host to port 80 in the container. The "-v" option is used to mount the "mydata" volume to the "/app/data" directory in
the container.


Step 3: Add data to the volume


Add some data to the volume by running the following command:


docker exec -it myapp bash


This command will start a Bash shell in the running container.
Once you are in the container, create a new file in the /app/data directory:


echo "Hello World" > /app/data/test.txt
exit


This will create a new file named test.txt in the /app/data directory of the container.


Step 4: Verify the data is persisted


To verify that the data is persisted, stop and remove the myapp container:


docker stop myapp
docker rm myapp


Then, start a new container and mount the mydata volume:


docker run -d -p 8080:80 --name myapp -v mydata:/app/data myapp


Finally, verify that the data is still present in the volume:


docker exec -it myapp cat /app/data/test.txt


This should output "Hello, world!".


Step 5: Backup the volume


To backup the volume, run the following command:


docker run --rm -v mydata:/volume -v $(pwd):/backup alpine tar -czvf /backup/mydata.tar.gz /volume


This will create a backup of the mydata volume in a file named mydata.tar.gz in the current directory.


Step 6: Create a new volume


Create a new volume using the following command:


docker volume create mydatabackup


This command will create a new volume named mydatabackup.


Step 7: Restore the volume


To restore the volume, run the following command:


docker run --rm -v mydatabackup:/volume -v $(pwd):/backup alpine tar -xzvf /backup/mydata.tar.gz


This will extract the contents of the mydata.tar.gz backup file to the mydatabackup volume.


Step 8: Verify the data is restored


To verify that the data is restored, start a new container and mount the mydatabackup volume:


docker run -d -p 8082:80 --name myapp2 -v mydatabackup:/app/data myapp


Then, verify that the data is present in the volume:


docker exec -it myapp2 sh
cat /app/data/test.txt
exit


This should output "Hello, world!".

Congratulations!


You've completed Hands-on exercise 2 where you've learned how to create and manage volumes and data on them.