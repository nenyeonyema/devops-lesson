**Step-by-Step Instructions to Run the Jenkins Container**
Create the Docker image from the Dockerfile:

`docker build -t my-jenkins . `

Run the Jenkins container:
`docker run -d -p 8080:8080 -p 50000:50000 --name jenkins my-jenkins`
-d runs the container in detached mode (in the background).
-p 8080:8080 maps the Jenkins web interface port to your local machine.
-p 50000:50000 maps the JNLP agent connection port.
--name jenkins assigns a name to the running container.


Check the running container status:
`docker ps`

View the logs to verify Jenkins is running:
`docker logs -f jenkins`


Exec into the running Jenkins container:
`docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword`
The command will output the initial admin password. Use it to unlock Jenkins when you access it via http://localhost:8080 in your browser.

**Steps to Create a Docker Network for Communication**
Step 1: Create a docker network
`docker network create jenkins-network`

Step 2: Start the Jenkins Master in the Network
`docker run -d --network jenkins-network -p 8080:8080 -p 50000:50000 --name jenkins-master my-jenkins`

Step 3: Run a Jenkins Agent Container
`docker run -d --network jenkins-network --name jenkins-agent jenkins/inbound-agent`
The agent container will wait for instructions from the master.
You can configure the agent in Jenkins UI by going to Manage Jenkins > Manage Nodes and Clouds > New Node, then add the agent with JNLP or SSH options.


**Accessing the Jenkins Agent (SSH or Docker Exec)**
If you need to manually interact with the Jenkins agent container to troubleshoot or run commands, you can do it in two ways:

Option 1: Using Docker Exec
You can directly execute commands inside the running agent container:
`docker exec -it jenkins-agent bash`
Once inside, you can check logs, install dependencies, or troubleshoot.

Option 2: Using SSH (If Configured)
If your Jenkins agent is configured with SSH, you can connect to it like this:

`ssh jenkins@agent-ip`
You'll need to configure SSH access first by adding credentials in the Jenkins master.

6. Verifying the Connection Between Master and Agent
Once your agent container is running, go to the Jenkins dashboard and:

Navigate to "Manage Jenkins" > "Manage Nodes and Clouds."
Check if the agent is connected and ready.
You can run a job to verify the communication.
