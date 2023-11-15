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