<h2>Hands on exercise 1: Creating Dockerfiles for building multi-stage Docker images</h2>
<br>
<h3>Prerequisites</h3>
<br>
<div>Before starting this lab, make sure you have the following installed on your system:</div>
<br><br>
<ul>
<li>Docker</li>
<li>Dotnet SDK v6.0</li>
<li>Code editor<br><br>or</li>
<li>access to Google Cloud Shell Code editor that has all of the above</li>
</ul>
<br>
<h2><strong><em>Java application example</em></strong></h2>
<br>
<h3>Step 1: Create HelloWorld.java file in my-java-app folder</h3>
</div>
<div>
<div>Run the following command in <strong>terminal</strong>:</div>
<div>&nbsp;</div>
</div>
<div>
<pre class="language-javascript"><code>mkdir my-java-app

cd my-java-app

nano HelloWorld.java</code></pre>
<br>
<div>Once HelloWorld.java has been created, populate the file using the following code-sample:</div>
<div>&nbsp;</div>
<pre class="language-markup"><code>public class HelloWorld
{ public static void main(String[] args)
{ System.out.println("Hello, World!"); }
}</code></pre>
<br>
<div>&nbsp;</div>
<div>In this step, we are creating a Java file named HelloWorld.java in a folder named</div>
<div>my-java-app.</div>
<div>&nbsp;</div>
<div>The file contains a simple Java program that prints "Hello, World!" to the</div>
<div>console.</div>
<div>&nbsp;</div>
<h3>Step 2: Create multi-stage Dockerfile</h3>
<div>In root folder (so, one step <strong>above </strong>the my-java-app folder)create a Dockerfile</div>
<div>&nbsp;</div>
<div>
<div>Run the following command in <strong>terminal</strong>:</div>
</div>
<div>
<pre class="language-javascript"><code>cd ../
nano Dockerfile
</code></pre>
<p>Populate the Dockerfile with the following content:</p>
</div>
<div>
<pre class="language-javascript"><code># Stage 1: Build
FROM openjdk:11 AS build

# Set the working directory
WORKDIR /app

# Copy the Java source code to the container
COPY my-java-app/ .

# Compile the Java application
RUN javac HelloWorld.java

# Stage 2: Final
FROM openjdk:11

# Set the working directory
WORKDIR /app

# Copy the compiled application from the build stage
COPY --from=build /app/HelloWorld.class .

# Run the application
CMD ["java", "HelloWorld"]</code></pre>
</div>
<div>&nbsp;</div>
<div>&nbsp;</div>
<div>In this step, you are creating a multi-stage Dockerfile.</div>
<div>The first stage builds the Java</div>
<div>application by copying the Java source code to the container, compiling it, and creating a</div>
<div>compiled Java class file.</div>
<div>The second stage takes the compiled Java class file and runs it in a new container.</div>
<div>&nbsp;</div>
<h3>Step 3: Build Docker image</h3>
<div>&nbsp;</div>
<div>&nbsp;</div>
<div>Next step is to build a Docker image named myappjava using the Dockerfile created</div>
<div>in the previous step.</div>
</div>
<div><br>Run the following command in <strong>terminal</strong>:</div>
<div>
<pre class="language-javascript"><code>docker build -t myappjava .</code></pre>
</div>
<div><br>
<h3>Step 4: Run Docker container</h3>
<div>After the Docker image is successfully built, it is time to run your first container, and you are going to do that by entering the following command in <strong>terminal</strong>:</div>
<div>&nbsp;</div>
</div>
<div>&nbsp;</div>
<div>
<pre class="language-javascript"><code>docker run -d -p 8081:80 --name myappjava myappjava</code></pre>
<br>
<div>&nbsp;</div>
<div>In this step, you are running a Docker container from the myappjava image you built in&nbsp;step 3. The -d flag runs the container in detached mode, -p flag maps port 8081 on the host to port 80 in the container, and --name flag assigns a name to the container.</div>
</div>
<div>&nbsp;</div>
<div>You might test out if the container is running by running the following command in the <strong>terminal</strong>:</div>
<div>&nbsp;</div>
<div>
<pre class="language-markup"><code>docker ps -a</code></pre>
</div>
<div>&nbsp;</div>
<div>You might have noticed that your <em>myappjava</em> is not running. Since the container is running in detached mode, that means that after everything is executed, the container will stop. However you are able to see what was going on with your container, and see the outputs as well by running the following commands in the <strong>terminal</strong><br><br><br></div>
<div>
<pre class="language-javascript"><code>docker logs myappjava</code></pre>
</div>
<div>
<div>&nbsp;</div>
<div>&nbsp;</div>
<br>
<h2><em>.NET application example</em></h2>
<br>
<div>&nbsp;</div>
<h3>Step 1: Create a Dotnet application</h3>
<br>
<div>Create a new .NET application using the command below in the root folder in <strong>terminal</strong>:</div>
<br>
<div>
<pre class="language-javascript"><code>dotnet new webapi -n myapp</code></pre>
</div>
<div>&nbsp;</div>
<div>This command creates a new web API project named <em>myapp</em></div>
<br><br><br>
<h3>Step 2: Create a Dockerfile for the build stage</h3>
<br>
<div>Create a new file named <em>Dockerfile.build </em>in <em>myapp</em> folder using the following command in the<span style="background-color: rgb(170, 170, 170);"> </span><strong>terminal:</strong></div>
<div>&nbsp;</div>
<pre class="language-javascript"><code>cd myapp
nano Dockerfile.build</code></pre>
<br>
<div><span style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;">and add the following content in the </span><em style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;">Dockerfile.build</em><span style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;">:</span></div>
<div>&nbsp;</div>
<div>
<pre class="language-javascript"><code>FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
WORKDIR /app


# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore


# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out


# Build runtime image
FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS runtime
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "myapp.dll"]</code></pre>
</div>
<div>&nbsp;</div>
<div>&nbsp;</div>
<div>This Dockerfile defines a build environment that uses the Dotnet SDK image to build the</div>
<div>application. The application is first restored, and then published in Release mode.</div>
<div>The published output is placed in the "out" directory.</div>
<br>
<div>&nbsp;</div>
<h3>Step 3: Create a Dockerfile for the runtime stage</h3>
<br>
<div>Create a new file named Dockerfile in myapp folder using the <strong>terminal&nbsp;</strong> (or you can use Google Cloud Shell Code Editor)</div>
<br>
<div>
<pre class="language-javascript"><code>nano Dockerfile</code></pre>
</div>
<br>
<div>
<div>and add the following content into it:</div>
</div>
<br>
<div>&nbsp;</div>
<br>
<div>&nbsp;</div>
<div>This Dockerfile defines a runtime environment that uses the Dotnet ASP.NET image to run</div>
<div>the application. The published output from the build stage is copied into the runtime image.</div>
<br>
<div>
<pre class="language-markup"><code>FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 80


FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build
WORKDIR /src
COPY ["myapp.csproj", "."]
RUN dotnet restore "./myapp.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "myapp.csproj" -c Release -o /app/build


FROM build AS publish
RUN dotnet publish "myapp.csproj" -c Release -o /app/publish


FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "myapp.dll"]</code></pre>
</div>
<br>
<h3>Step 4: Build the Docker image</h3>
<br>
<div>To build the Docker image, run the following command in the terminal:</div>
<br>
<div>&nbsp;</div>
<div>
<pre class="language-javascript"><code>docker build -t myapp .</code></pre>
</div>
<br>
<div>&nbsp;</div>
<div>This will build the Docker image using the Dockerfile.</div>
<br>
<h3>Step 5: Run the Docker container</h3>
<br>
<div>To run the Docker container, run the following command:</div>
<br>
<div>&nbsp;</div>
<div>
<pre class="language-javascript"><code>docker run -d -p 8080:80 --name myapp myapp</code></pre>
</div>
<br>
<div>&nbsp;</div>
<div>This will run the Docker container in detached mode and map port 8080 on the host to port 80 in the container.</div>
<br>
<div>&nbsp;</div>
<br>
<div>To test out the .NET app, use the following command in <strong>terminal</strong>:</div>
<div>
<pre class="language-javascript"><code>curl http://localhost:8080/WeatherForecast/</code></pre>
<p>&nbsp;</p>
<p>Congratulations!</p>
<p>&nbsp;</p>
<p>You have reached the end of the Hands-on exercise 1 where you have created multistage builds fot both - Java and .NET application</p>
</div>
</div>