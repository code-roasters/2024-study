# Minikube FE Hello World

Setting up a "Hello World" application using Minikube and kubectl involves several steps. I'll guide you through a basic setup where your frontend application is containerized and deployed using Kubernetes, which Minikube and kubectl help manage. This process involves creating a simple frontend application, containerizing it, and then deploying it to a local Kubernetes cluster managed by Minikube.

Here are the general steps:

### 1. Install Minikube and kubectl

First, ensure that Minikube and kubectl are installed on your system. Minikube is a tool that lets you run Kubernetes locally, and kubectl is a command-line tool for interacting with Kubernetes clusters.

Install Minikube: Follow the instructions on the Minikube GitHub page.
Install kubectl: Instructions can be found on the Kubernetes website.

### 2. Start Minikube

Run Minikube to create a local Kubernetes cluster:

```bash
minikube start
```

### 3. Create a Simple Frontend Application

Let's say you're creating a simple HTML page as your "Hello World" app. Create an index.html file:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Hello World</title>
  </head>
  <body>
    <h1>Hello World!</h1>
  </body>
</html>
```

### 4. Dockerize Your Application

Create a Dockerfile to containerize your frontend app.

```Dockerfile
# Use an official lightweight Node.js as a parent image
FROM node:alpine

# Set the working directory
WORKDIR /usr/src/app

# Copy your application files
COPY . .

# Install a simple HTTP server to serve static content
RUN npm install -g http-server

# Make port 8080 available to the outside world
EXPOSE 8080

# Run the HTTP server
CMD ["http-server", "-p 8080"]
```

### 5. Build and Push the Docker Image

Build your Docker image and, if necessary, push it to a Docker registry. Minikube has its own Docker daemon, so you can build directly on Minikube's Docker:

```bash
Copy code
eval $(minikube docker-env)
docker build -t hello-world-frontend .
```

### 6. Create a Kubernetes Deployment

Write a Kubernetes deployment YAML file (deployment.yaml) to define your deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
name: hello-world
spec:
replicas: 1
selector:
matchLabels:
app: hello-world
template:
metadata:
labels:
app: hello-world
spec:
containers: - name: hello-world
image: hello-world-frontend
ports: - containerPort: 8080
```

### 7. Deploy to Kubernetes

Use kubectl to deploy your application:

```bash
kubectl apply -f deployment.yaml
```

### 8. Access Your Application

Expose your application to access it from a browser:

```bash
kubectl expose deployment hello-world --type=LoadBalancer --port=8080
```

Then, find the URL with:

```bash
minikube service hello-world --url
```

Notes:
This is a basic setup. For real-world applications, consider aspects like logging, monitoring, environment-specific configurations, and CI/CD integrations.
Regularly check for updates in Kubernetes, Minikube, and other tools, as best practices and commands can evolve.
