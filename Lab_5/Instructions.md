<h2>Hands-on exercise 5: Running containers with rootless access</h2>
<p>&nbsp;</p>
<h3>Step 1: Create a new user</h3>
<p>While in terminal enter the following</p>
<div>
<pre class="language-markup"><code>whoami
sudo adduser appuser</code></pre>
</div>
<p>&nbsp;</p>
<div>This command will create a new user called <em>appuser</em>.</div>
<p>&nbsp;</p>
<div>Reset the password for <strong><em>current</em></strong> user and switch to the newly created user with the following command in terminal:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>sudo passwd

sudo su - appuser
</code></pre>
</div>
<p>&nbsp;</p>
<div>This command will switch to the <em>appuser</em> account, which we will use to run our containers with rootless access.</div>
<p>&nbsp;</p>
<div>While in terminal, run the following command:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>id</code></pre>
</div>
<p>&nbsp;</p>
<div>You should see result similar to this:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>uid=1001(appuser) gid=1001(appuser) groups=1001(appuser)</code></pre>
</div>
<p>&nbsp;</p>
<h3>Step 2: Add non-root user to the docker group</h3>
<p>&nbsp;</p>
<div>The following is considered as the recommended approach to allow your user to run Docker commands without needing <em>sudo</em>.</div>
<div>&nbsp;</div>
<div>Here's how you can do it. First, check if your user is already a member of the docker group by running the following command in terminal:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>groups</code></pre>
</div>
<div>&nbsp;</div>
<div>If <em>docker</em>&nbsp;is not in the list, proceed with the following steps.</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>su &ndash; &lt;root_user_name&gt;</code></pre>
</div>
<div>&nbsp;</div>
<div>Add your user to the docker group using <em>sudo</em> user:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>sudo usermod -aG docker appuser</code></pre>
</div>
<p>&nbsp;</p>
<h3>Step 3: Create a Dockerfile in a new directory for your app using non-root user</h3>
<p>&nbsp;</p>
<p>Write the following command in your terminal:</p>
<div>
<pre class="language-markup"><code>sudo su - appuser</code></pre>
</div>
<p>After that create a new directory and navigate to it, using the following command:</p>
<div>
<pre class="language-markup"><code>mkdir myapp5
cd myapp5</code></pre>
</div>
<div>&nbsp;</div>
<div>Create a new file called <em>Dockerfile</em> in terminal using the following command:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>nano Dockerfile</code></pre>
</div>
<p>Add the following content in the newly created <em>Dockerfile:</em></p>
<pre class="language-markup"><code># Use an official Node.js runtime as a parent image
FROM node:10-alpine


# Set the working directory to /app
WORKDIR /app


# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages
RUN npm install


# Add a non-root user to run the app
RUN addgroup -S appgroup &amp;&amp; adduser -S -G appgroup appuser


# Set the user to the non-root user
USER appuser


# Make port 8080 available to the world outside this container
EXPOSE 8080


# Define environment variable
ENV NODE_ENV production


# Run the command to start the app
CMD ["npm", "start"]</code></pre>
<div>This Dockerfile uses the official Node.js 10 runtime as a base image, sets the working directory to /app, copies the current directory into the container, installs any needed packages, adds a non-root user to run the app, sets the user to the non-root user, exposes port 8080, defines an environment variable, and runs the command to start the app.</div>
<p>&nbsp;</p>
<h3>Step 4: Create a new file called package.json</h3>
<blockquote>
<p>&nbsp;</p>
</blockquote>
<div>Add the following content into a package.json 0file</div>
<p>&nbsp;</p>
<div>Using nano create package.json file:</div>
<pre class="language-markup"><code>nano package.json</code></pre>
<div>&nbsp;</div>
<div>Fill the newly created file with the following information:<br><br></div>
<div>
<pre class="language-javascript"><code>{
"name": "myapp",
"version": "1.0.0",
"description": "My App",
"main": "index.js",
"scripts": {
"start": "node index.js"
},
"dependencies": {
"express": "^4.17.1"
}
}</code></pre>
</div>
<p>&nbsp;</p>
<div>This package.json file defines the name, version, description, main file, scripts, and dependencies for your app. In this example, we are using the Express web framework.</div>
<p>&nbsp;</p>
<h3>Step 5: Create a new file called index.js</h3>
<p>&nbsp;</p>
<p>&nbsp;</p>
<div>Using nano create index.js file:</div>
<pre class="language-markup"><code>nano index.js
</code></pre>
<div>&nbsp;</div>
<div>Fill the newly created file with the following information:</div>
<div>&nbsp;</div>
<pre class="language-javascript"><code>const express = require('express')
const app = express()

app.get('/', (req, res) =&gt; {
res.send('Hello World!')
})

app.listen(8080, () =&gt; {
console.log('App listening on port 8080')
})</code></pre>
<div>This index.js file creates a new Express app that listens on port 8080 and returns a "Hello World!" message for the root route.</div>
<p>&nbsp;</p>
<h3>Step 6: Build Docker image</h3>
<p>Build a Docker image with the tag <em>myappimage5</em> using the <em>Dockerfile</em> in the current directory by running a following command</p>
<div>
<pre class="language-markup"><code>docker build -t myappimage5 .</code></pre>
</div>
<p>&nbsp;</p>
<h3>Step 7: Run Docker container</h3>
<p>&nbsp;</p>
<div>Run the Docker container with port 8080 mapped to the host and uses the <em>myappimage5</em> image.</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>docker run -d -p 8085:8080 --name myapp5 myappimage5</code></pre>
</div>
<p>&nbsp;</p>
<div>Now you have a Docker container running your app with a non-root user. You can access the app by running:</div>
<p>&nbsp;</p>
<div>
<pre class="language-markup"><code>curl http://localhost:8085</code></pre>
</div>
<p>&nbsp;</p>
<div>Verify that the container is running with rootless access by running the following command inside the container:</div>
<p>&nbsp;</p>
<div>&nbsp;</div>
<p>&nbsp;</p>
<div>This command will display the user and group IDs of the current user, which should be non-root.</div>
<p>&nbsp;</p>
<div>&emsp;</div>
<p>&nbsp;</p>