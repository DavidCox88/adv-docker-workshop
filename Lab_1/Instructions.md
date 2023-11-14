Hands on exercise 1: Creating Dockerfiles for building multi-stage Docker images

Prerequisites

Before starting this lab, make sure you have the following installed on your system:


Docker
Dotnet SDK v6.0
Code editor

or
access to Google Cloud Shell Code editor that has all of the above

Java application example

Step 1: Create HelloWorld.java file in my-java-app folder
Run the following command in terminal:

mkdir my-java-app

cd my-java-app

nano HelloWorld.java

Once HelloWorld.java has been created, populate the file using the following code-sample:

public class HelloWorld
{ public static void main(String[] args)
{ System.out.println("Hello, World!"); }
}


In this step, we are creating a Java file named HelloWorld.java in a folder named
my-java-app.

The file contains a simple Java program that prints "Hello, World!" to the
console.

Step 2: Create multi-stage Dockerfile
In root folder (so, one step above the my-java-app folder)create a Dockerfile

Run the following command in terminal:
cd ../
nano Dockerfile
Populate the Dockerfile with the following content:

# Stage 1: Build
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
CMD ["java", "HelloWorld"]


In this step, you are creating a multi-stage Dockerfile.
The first stage builds the Java
application by copying the Java source code to the container, compiling it, and creating a
compiled Java class file.
The second stage takes the compiled Java class file and runs it in a new container.

Step 3: Build Docker image


Next step is to build a Docker image named myappjava using the Dockerfile created
in the previous step.

Run the following command in terminal:
docker build -t myappjava .

Step 4: Run Docker container
After the Docker image is successfully built, it is time to run your first container, and you are going to do that by entering the following command in terminal:


docker run -d -p 8081:80 --name myappjava myappjava


In this step, you are running a Docker container from the myappjava image you built in step 3. The -d flag runs the container in detached mode, -p flag maps port 8081 on the host to port 80 in the container, and --name flag assigns a name to the container.

You might test out if the container is running by running the following command in the terminal:

docker ps -a

You might have noticed that your myappjava is not running. Since the container is running in detached mode, that means that after everything is executed, the container will stop. However you are able to see what was going on with your container, and see the outputs as well by running the following commands in the terminal


docker logs myappjava



.NET application example


Step 1: Create a Dotnet application

Create a new .NET application using the command below in the root folder in terminal:

dotnet new webapi -n myapp

This command creates a new web API project named myapp



Step 2: Create a Dockerfile for the build stage

Create a new file named Dockerfile.build in myapp folder using the following command in the terminal:

cd myapp
nano Dockerfile.build

and add the following content in the Dockerfile.build:

FROM mcr.microsoft.com/dotnet/sdk:7.0 AS build-env
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
ENTRYPOINT ["dotnet", "myapp.dll"]


This Dockerfile defines a build environment that uses the Dotnet SDK image to build the
application. The application is first restored, and then published in Release mode.
The published output is placed in the "out" directory.


Step 3: Create a Dockerfile for the runtime stage

Create a new file named Dockerfile in myapp folder using the terminal  (or you can use Google Cloud Shell Code Editor)

nano Dockerfile

and add the following content into it:




This Dockerfile defines a runtime environment that uses the Dotnet ASP.NET image to run
the application. The published output from the build stage is copied into the runtime image.

FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
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
ENTRYPOINT ["dotnet", "myapp.dll"]

Step 4: Build the Docker image

To build the Docker image, run the following command in the terminal:


docker build -t myapp .


This will build the Docker image using the Dockerfile.

Step 5: Run the Docker container

To run the Docker container, run the following command:


docker run -d -p 8080:80 --name myapp myapp


This will run the Docker container in detached mode and map port 8080 on the host to port 80 in the container.



To test out the .NET app, use the following command in terminal:
curl http://localhost:8080/WeatherForecast/


Congratulations!



You have reached the end of the Hands-on exercise 1 where you have created multistage builds fot both - Java and .NET application