Hands-on exercise 4: Creating and running multi-container apps using Docker Compose

Step 1: Download app code


While in the root folder download the following application code by typing the following commands in terminal:

wget https://dockerlabapp.blob.core.windows.net/users/usersapp.zip
unzip usersapp
cd usersapp

Step 2: Create a Dockerfile for the Node.js application


Create new file in usersapp folder named Dockerfile.node by typing the following command in terminal:

nano Dockerfile.node
Add the following content in the Dockerfile.node:

FROM node:16-alpine


WORKDIR /app


ENV DATABASE_HOST=<postgres_container_name>
ENV DATABASE_PORT=5432
ENV DATABASE_USERNAME=postgres
ENV DATABASE_PASSWORD=foo
ENV DATABASE_NAME=postgres


COPY . .


RUN npm install


EXPOSE 3000


ENTRYPOINT ["node", "src/index.js"]


This Dockerfile defines a Node.js application that connects to a PostgreSQL database. The environment variables are set to connect to the database running in the other container.

 
Step 3: Create a Dockerfile for the PostgreSQL database


Create a new file named Dockerfile.postgres in usersapp folder by typing the following command in terminal:

nano Dockerfile.postgres
Add the following content in Dockerfile.postgres:

FROM postgres:latest


ENV POSTGRES_PASSWORD=foo


COPY user.sql /docker-entrypoint-initdb.d/user.sql


COPY . /repo


CMD ["postgres", "-c", "log_statement=all"]


This Dockerfile defines a PostgreSQL database with a custom configuration. The user.sql file is copied to the container's initialization directory to create a new user and database. The entire current directory is copied to a directory in the container.


Step 4: Create a Docker Compose file


Create a new file named docker-compose.yml by typing the following command in terminal:

nano docker-compose.yml
Add the following content in docker-compose.yml:  

version: '3'  
services:  
  postgres:  
    build:  
      context: .  
      dockerfile: Dockerfile.postgres  
    container_name: postgresdb
    ports:  
      - "5432:5432"  
    environment:
      DATABASE_USERNAME: postgres
      DATABASE_PASSWORD: foo
      DATABASE_NAME: postgres
    networks:
      - mynetwork
  app:  
    build:  
      context: .  
      dockerfile: Dockerfile.node  
    container_name: userapi
    ports:  
      - "3001:3000"
    environment:
      DATABASE_HOST: postgresdb
      DATABASE_USERNAME: postgres
      DATABASE_PASSWORD: foo
      DATABASE_NAME: postgres  
    networks:
      - mynetwork
    depends_on:  
      - postgres  

networks:
  mynetwork:
    name: mynetwork
    external: true

This Docker Compose file defines two services, postgres and app. The postgres service uses the Dockerfile.postgres file to build the PostgreSQL image and maps port 5432 to the host. The app service uses the Dockerfile.node file to build the Node.js image and maps port 3000 to the host. The app service depends on the postgres service to start the database before starting the application. Both containers are in the same network mynetwork.


Step 5: Start the containers


Start the containers using Docker Compose by running the following command in terminal:

sudo apt install docker-compose
docker-compose up -d
docker ps

This will start the containers in the background.


Step 6: Test the connection


Test the connection between the two containers by visiting http://localhost:3001/users


curl localhost:3001/users



Congratulations!


In this hands-on exercise, you learned how to create and run multi-container applications using Docker Compose, and how to define services in a Docker Compose file, build Docker images from Dockerfiles, and connect containers to each other.

