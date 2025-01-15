1. What is a Dockerfile?
A. Dockerfile is text based file which consists of instructions to build the image.
2. Explain the common Dockerfile instructions.
A. 'FROM' specifies the base image in the dockerfile (FROM ubuntu:latest)
    "RUN" command executes the commands during the image build (RUN apt update -y) it'll be executed while creating image.
   "COPY" copies the file from host to container (copy file1.txt /home)
   "Add" using add we can download files from internet (add "url")
   "CMD" is used to execute commands it'll be executed while creating container.(CMD ["sh", "app.sh"]
   "ENTRYPOINT" it is used to execute a command/script.  it is used while creating container.
   "ARG" using ARG we can define variables in dockerfile.
   "ENV" used to set environment variables, these are available for image and container ( ENV java /usr/bin/jdk) we can think of it as alias for the directory.
3. What does the `WORKDIR` instruction do in a Dockerfile?
   - Answer: The `WORKDIR` instruction sets the working directory for any subsequent instructions that follow (such as `RUN`, `CMD`, `ENTRYPOINT`, etc.). If the directory doesn’t exist, Docker will create it.
     dockerfile
     WORKDIR /usr/src/app
4.How would you expose a port in a Dockerfile?
   - Answer: The `EXPOSE` instruction tells Docker that the container will listen on the specified network ports at runtime. It doesn’t actually publish the port, but serves as a hint to the container runtime.
     dockerfile
     EXPOSE 8080
5.What does the `WORKDIR` instruction do in a Dockerfile?
   - Answer: The `WORKDIR` instruction sets the working directory for any subsequent instructions that follow (such as `RUN`, `CMD`, `ENTRYPOINT`, etc.). If the directory doesn’t exist, Docker will create it.
     dockerfile
     WORKDIR /usr/src/app
6. How does the `USER` instruction work in a Dockerfile?
   - Answer: The `USER` instruction sets the user name or UID and optionally the group name or GID to use when running the image. It is important for security to avoid running containers as root.
     dockerfile
     USER node     
7. What is the purpose of the `VOLUME` instruction in a Dockerfile?
   - Answer: The `VOLUME` instruction creates a mount point with the specified path in the container. This is used for data persistence, meaning that data can be stored outside the container’s writable layer (e.g., shared with the host or another container).
     dockerfile
     VOLUME ["/data"]   
8. What is the `STOPSIGNAL` instruction used for in a Dockerfile?
   - Answer: The `STOPSIGNAL` instruction specifies the signal that Docker will send to the container to terminate it. By default, Docker uses the `SIGTERM` signal.
     dockerfile
     STOPSIGNAL SIGINT
9. Docker image-related interview scenario questions KK FUNDA
 ==================================================

 1. Scenario: You have just built a Docker image, but you realize that some unnecessary files were included. How do you reduce the size of the image?
   
   Answer:  
   - Explanation: You can reduce the size of your Docker image by optimizing the Dockerfile:
     1. Use `.dockerignore`: Exclude unnecessary files (e.g., logs, temp files, local configuration files) from the build context, just like `.gitignore`.
     2. Use multi-stage builds: In multi-stage builds, you can separate the build and runtime environments, so you only copy the necessary artifacts into the final image.
     3. Use smaller base images: Choose minimal base images, like `alpine`, instead of full-featured ones like `ubuntu`.
      #dockerfile
   # Example of multi-stage build
       FROM node:14 AS build
       WORKDIR /app
       COPYY ..
       RUN npm install && npm run build

       FROM node:14-slim
       WORKDIR /app
       COPY --from=build /app/dist /app
       CMD ["node", "app.js"]
     This Dockerfile defines a multi-stage build to create and optimize a Node.js application image. Here’s a step-by-step explanation of its components:

Stage 1: Build Stage
dockerfile
Copy code
FROM node:14 AS build
node:14: Uses the Node.js version 14 base image, which includes all necessary tools to build and develop a Node.js application.
AS build: Names this stage build. This is useful in multi-stage builds to reference this stage in subsequent stages.
dockerfile
Copy code
WORKDIR /app
WORKDIR /app: Sets the working directory inside the container to /app. Any subsequent commands will be run from this directory.
dockerfile
Copy code
COPY . .
COPY . .: Copies all files and directories from the build context (usually the current directory) into the container's /app directory.
dockerfile
Copy code
RUN npm install && npm run build
RUN npm install: Installs all Node.js dependencies as listed in the package.json file.
npm run build: Runs the build script defined in the package.json, typically used to compile or bundle the application (e.g., transpiling TypeScript, bundling assets).
Stage 2: Runtime Stage
dockerfile
Copy code
FROM node:14-slim
node:14-slim: Uses a lightweight Node.js base image with only essential runtime dependencies. This reduces the image size.
dockerfile
Copy code
WORKDIR /app
WORKDIR /app: Sets the working directory inside the container to /app.
dockerfile
Copy code
COPY --from=build /app/dist /app
COPY --from=build: Copies files from the /app/dist directory of the build stage into the /app directory of the current stage.
Purpose: This ensures only the compiled application (typically production-ready files) is included in the final image, minimizing the image size by excluding unnecessary build tools and source files.
Command to Start the Application
dockerfile
Copy code
CMD ["node", "app.js"]
CMD ["node", "app.js"]: Specifies the default command to run when the container starts. It runs the Node.js application using app.js as the entry point.
Key Advantages of This Dockerfile
Multi-Stage Build:

Separates the build and runtime environments.
Reduces the size of the final image by including only the necessary runtime dependencies.
Lightweight Runtime Image:

Uses the node:14-slim image, which is smaller than the full node:14 image.
Optimized for Production:

Only the necessary files (/app/dist) are copied to the final image, avoiding unnecessary files like source code or dev dependencies.


10. 
      
