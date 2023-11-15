<h2>Hands-on exercise 4: Creating and running multi-container apps using Docker Compose</h2>
<h3>&nbsp;</h3>
<h3>Step 1: Download app code</h3>
<p>&nbsp;</p>
<div>While in the root folder download the following application code by typing the following commands in <strong>terminal</strong>:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>wget https://dockerlabapp.blob.core.windows.net/users/usersapp.zip
unzip usersapp
cd usersapp</code></pre>
</div>
<div>&nbsp;</div>
<h3>Step 2: Create a Dockerfile for the Node.js application</h3>
<p>&nbsp;</p>
<div>Create new file in <em>usersapp</em> folder named <em>Dockerfile.node</em> by typing the following command in <strong>terminal</strong>:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>nano Dockerfile.node</code></pre>
</div>
<p>Add the following content in the <em>Dockerfile.node</em>:</p>
<div>
<pre class="language-markup"><code>FROM node:16-alpine


WORKDIR /app


ENV DATABASE_HOST=&lt;postgres_container_name&gt;
ENV DATABASE_PORT=5432
ENV DATABASE_USERNAME=postgres
ENV DATABASE_PASSWORD=foo
ENV DATABASE_NAME=postgres


COPY . .


RUN npm install


EXPOSE 3000


ENTRYPOINT ["node", "src/index.js"]</code></pre>
</div>
<div>&nbsp;</div>
<div>&nbsp;</div>
<div>This Dockerfile defines a Node.js application that connects to a PostgreSQL database. The environment variables are set to connect to the database running in the other container.</div>
<div>&nbsp;</div>
<div>&emsp;</div>
<h3>Step 3: Create a Dockerfile for the PostgreSQL database</h3>
<p>&nbsp;</p>
<div>Create a new file named <em>Dockerfile.postgres</em> in <em>usersapp</em> folder by typing the following command in <strong>terminal</strong>:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>nano Dockerfile.postgres</code></pre>
</div>
<p>Add the following content in <em>Dockerfile.postgres:</em></p>
<div>
<pre class="language-markup"><code>FROM postgres:latest


ENV POSTGRES_PASSWORD=foo


COPY user.sql /docker-entrypoint-initdb.d/user.sql


COPY . /repo


CMD ["postgres", "-c", "log_statement=all"]</code></pre>
</div>
<div>&nbsp;</div>
<div>&nbsp;</div>
<div>This Dockerfile defines a PostgreSQL database with a custom configuration. The user.sql file is copied to the container's initialization directory to create a new user and database. The entire current directory is copied to a directory in the container.</div>
<p>&nbsp;</p>
<h3>Step 4: Create a Docker Compose file</h3>
<p>&nbsp;</p>
<div>Create a new file named <em>docker-compose.yml</em> by typing the following command in <strong>terminal:</strong></div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>nano docker-compose.yml</code></pre>
</div>
<p>Add the following content in <em>docker-compose.yml</em>:&nbsp;&nbsp;</p>
<div>
<pre class="language-javascript"><code>version: '3'  
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
    external: true</code></pre>
</div>
<div>&nbsp;</div>
<div>This Docker Compose file defines two services, <em>postgres</em>&nbsp;and <em>app</em>. The <em>postgres </em>service uses the <em>Dockerfile.postgres</em> file to build the PostgreSQL image and maps port 5432 to the host. The <em>app </em>service uses the <em>Dockerfile.node</em> file to build the Node.js image and maps port 3000 to the host. The <em>app</em>&nbsp;service depends on the <em>postgres</em>&nbsp;service to start the database before starting the application. Both containers are in the same network <em>mynetwork</em>.</div>
<p>&nbsp;</p>
<h3>Step 5: Start the containers</h3>
<p>&nbsp;</p>
<div>Start the containers using Docker Compose by running the following command in <strong>terminal</strong>:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>sudo apt install docker-compose
docker-compose up -d
docker ps</code></pre>
</div>
<div>&nbsp;</div>
<div>This will start the containers in the background.</div>
<p>&nbsp;</p>
<h3>Step 6: Test the connection</h3>
<p>&nbsp;</p>
<div>Test the connection between the two containers by visiting <code>http://localhost:3001/users</code></div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>curl localhost:3001/users</code></pre>
</div>
<p><br><br></p>
<div><strong>Congratulations!</strong></div>
<p>&nbsp;</p>
<div>In this hands-on exercise, you learned how to create and run multi-container applications using Docker Compose, and how to define services in a Docker Compose file, build Docker images from Dockerfiles, and connect containers to each other.</div>
<p><br><br></p>