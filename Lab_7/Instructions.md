<h2>Hands-on exercise 7: Setting up secure communication between containers and managing secrets</h2>
<br>
<div>For the purposes of this exercise, you will use the containers from task 4 from the previous</div>
<div>day. It is necessary to change the version of the docker-compose.yml file to 3.3.</div>
<br>
<h3>Step 1: Create a new secret</h3>
<div>The first step in this workshop is to create a new secret using the Docker command line</div>
<div>interface. To do this, run the following command (while being in your username root folder):</div>
<br>
<div>
<pre class="language-markup"><code>cd usersapp
echo foo &gt; password.txt</code></pre>
</div>
<div>&nbsp;</div>
<div>This creates a new secret called <em>db_password</em> with the value <em>foo </em>and stores it in the password.txt</div>
<br>
<h3>Step 2: Use newly created secret</h3>
<div>The next step is to use the newly created secret in a<em> docker-compose.yml</em> file. To do this, create a <em>docker-compose.yml</em> file:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>nano docker-compose.yml</code></pre>
</div>
<div>&nbsp;</div>
<div>Add the following lines to your <em>docker-compose.yml</em> file to both, app and db (change <em>DATABASE_PASSWORD</em> with <em>DATABASE_PASSWORD_FILE</em>):</div>
<br>
<div>
<pre class="language-javascript"><code>   environment:
      DATABASE_HOST: postgresdb
      DATABASE_USERNAME: postgres
      DATABASE_NAME: postgres
      DATABASE_PASSWORD_FILE: /run/secrets/db_password
   secrets:
      - db_password
</code></pre>
</div>
add this at the following at the end of <em>docker-compose.yml</em>:</div>
<div>
<pre class="language-markup"><code>secrets:
  db_password:
    file: password.txt</code></pre>
<br><br>
<div>This tells Docker to use the db_password secret in the app service, and to mount it as a file at the path /run/secrets/db_password. T</div>
<br>
<h3>Step 3: Run containers</h3>
<div>The final step is to run the containers using Docker Compose. To do this, run the following command:</div>
<br>
<div>
<pre class="language-markup"><code>docker compose up -d</code></pre>
</div>
<br>
<div>This starts the containers defined in your Compose file. Once the containers are running, you can test the application by running the following commands:</div>
<br>
<div>
<pre class="language-markup"><code>curl localhost:3001/users</code></pre>
</div>
<br>
<div>This downloads the list of users from the running application and prints them to the console. If everything is working correctly, you should see a list of users displayed in the terminal.</div>
<br>
<h3>Step 4: Use Docker Swarm</h3>
<div>Another way to configure configuration si to use Docker Swaarm. To do so, run the following command:<br><br></div>
<div>
<pre class="language-markup"><code>docker swarm init</code></pre>
</div>
</div>
<div>&nbsp;</div>
<div>Follow the instructions if running extra command is needed.</div>
<div>&nbsp;</div>
<div>Create a password for database, using the Docker secret:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>echo "foo" | docker secret create db_password &ndash;
docker secret create db_password password.txt</code></pre>
</div>
<div>&nbsp;</div>
<div>You can comment the <code>file:password.txt&nbsp;</code> in docker-compose.yml and replace it with <code>external: true</code></div>
<div><br>
<div>
<pre class="language-markup"><code>secrets:
  db_password:
    external: true</code></pre>
</div>
</div>
<div>&nbsp;</div>
<div>After you've changed that value apply Docker Compose configuration:<br><br>
<div>
<pre class="language-markup"><code>docker compose up -d</code></pre>
</div>
</div>
<div>&nbsp;</div>
<div>If everything is configured properly, you should be able to see a list of users:</div>
<div><br>
<div>&nbsp;</div>
<br><br></div>