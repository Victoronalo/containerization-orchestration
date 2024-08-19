# containerization-orchestration

Project: Containerization and Container Orchestration
Objective: To develop a simple static website (HTML and CSS) for a company landing page. and containerize the application using Docker and deploy it to Kubernete cluster and access it through Nginx.



-First task is to create a new project directory namely
       * /c/Users/Adeola Onalo/containerization


--I proceeded to create the docker file by running the command 
           *touch docker-file


-I proceeded to wrie the docker file by using the below 

* # Use an official Node.js runtime as a parent image
FROM node:14

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy package.json and package-lock.json files to the working directory
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code to the working directory
COPY . .

# Expose the port the app runs on
EXPOSE 8080

# Define the command to run your app
CMD [ "node", "app.js" ]



Explanation of Each Instruction
•	FROM node:14: Specifies the base image to use. Here, it’s an official Node.js image with version 14.
•	WORKDIR /usr/src/app: Sets the working directory inside the container.
•	COPY package*.json ./: Copies the package files to the working directory.
•	RUN npm install: Installs the Node.js dependencies defined in package.json.
•	COPY . .: Copies the rest of your application’s code into the container.
•	EXPOSE 8080: Documents that the container listens on port 8080.
•	CMD [ "node", "app.js" ]: Defines the command to run the application.
-To build docker image, I used the command
         * docker build -t my-node-app .
-To run the image I used the command 
          * docker run -p 8080:8080 my-node-app


-Then inside the directory, I created a new index.html file. The file below.


* <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Simple HTML Page</title>
</head>
<body>
    <header>
        <h1>Welcome to My Website</h1>
    </header>
    <main>
        <p>This is a simple HTML page created as an example. You can add more content here.</p>
        <a href="https://www.example.com">Visit Example.com</a>
    </main>
    <footer>
        <p>&copy; 2024 My Simple HTML Page</p>
    </footer>
</body>
</html>




-CODE EXPLANATION

•	- <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Simple HTML Page</title>
</head>
<body>
    <header>
        <h1>Welcome to My Website</h1>
    </header>
    <main>
        <p>This is a simple HTML page created as an example. You can add more content here.</p>
        <a href="https://www.example.com">Visit Example.com</a>
    </main>
    <footer>
        <p>&copy; 2024 My Simple HTML Page</p>
    </footer>
</body>
</html> 



-I then created style.css file.as seen below.


* /* Reset some default styles */
body, h1, h2, h3, p, ul, li {
    margin: 0;
    padding: 0;
}

/* Basic body styling */
body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    color: #333;
    background-color: #f4f4f4;
}

/* Header styling */
header {
    background-color: #333;
    color: #fff;
    padding: 1em 0;
    text-align: center;
}

header h1 {
    margin: 0;
}

/* Navigation styling */
nav ul {
    list-style-type: none;
    padding: 0;
}

nav ul li {
    display: inline;
    margin-right: 10px;
}

nav a {
    color: #fff;
    text-decoration: none;
}

/* Main content styling */
main {
    padding: 1em;
}

h1, h2, h3 {
    color: #333;
}

/* Footer styling */
footer {
    background-color: #333;
    color: #fff;
    text-align: center;
    padding: 1em 0;
    position: fixed;
    width: 100%;
    bottom: 0;
}

/* Responsive design */
@media (max-width: 600px) {
    nav ul li {
        display: block;
        margin-bottom: 5px;
    }
}



-CODE DETAIL:
•  Reset Styles: Removes default margins and paddings from common elements to ensure consistent styling across different browsers.
•  Body: Sets a basic font, line height, text color, and background color for the entire page.
•  Header: Styles the header with a background color, text color, and padding. Centers the text and removes margins from the heading.
•  Navigation: Removes list styles from the navigation, sets items to display inline, and styles links.
•  Main Content: Adds padding around the main content and sets a color for headings.
•  Footer: Styles the footer with a background color, text color, and ensures it is fixed to the bottom of the page.
•  Responsive Design: Adjusts the navigation styling for smaller screens by stacking the items vertically.




-I logged into the docker hub by using the command
          *docker login



-I proceeded to tag my image using the command
           *docker tag nginx:latest victoronalo/repository:tag 



-To push the image I used the command below
            *docker push victoronalo/repository:tag



-To install kind I used the below command
       *curl -Lo ./kind https://kind.sigs.k8s.io/dl/latest/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind



-To verify kind version I used the command 
        *kind version



-To create kind cluster I ran the command
           *kind create cluster




-To create a kubernete YAML file I used the below.
          * apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
  labels:
    app: my-app
spec:
  replicas: 4
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
        ports:
        - containerPort: 80  


- Explanation:
•	apiVersion: apps/v1: Specifies the version of the Kubernetes API to use.
•	kind: Deployment: Indicates that this YAML file describes a Deployment resource.
•	metadata: Contains metadata about the Deployment, such as its name and labels.
•	spec: Defines the desired state for the Deployment.
o	replicas: The number of replicas you want to run.
o	selector: Defines how the Deployment finds which Pods to manage.
o	template: Describes the Pods that will be created.
	metadata: Labels for the Pods.
	spec: The specification for the Pods, including containers.
	containers: List of containers in the Pod.
	name: Name of the container.
	image: The Docker image to use (replace with your image).
	ports: Ports that the container exposes (adjust as needed).




-To apply YAML file I used the command 
* kubectl apply -f /c/Users/Adeola Onalo/containerization.yaml


-I proceeded to run check status of the deployment by running the below command
           * kubectl get deployments



-To check status of pods I used the command
       * kubectl get pods   



-To port forward I ran the command to get the IP address
    *ifconfig



-The application port is 80



-I entered the GUI by running the IP address and it worked.



This project has given me more insight into container orchestration and its mode of configuration and management.
Though the process was tough, with the help of the internet and the team, I was able to complete the project.
 



