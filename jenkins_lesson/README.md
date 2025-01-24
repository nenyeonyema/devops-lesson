**Step-by-Step Instructions to Run the Jenkins Container**
Create the Docker image from the Dockerfile:

```
docker build -t my-jenkins .
Run the Jenkins container:
```

```
docker run -d -p 8080:8080 -p 50000:50000 --name jenkins my-jenkins
-d runs the container in detached mode (in the background).
-p 8080:8080 maps the Jenkins web interface port to your local machine.
-p 50000:50000 maps the JNLP agent connection port.
--name jenkins assigns a name to the running container.
```

Check the running container status:
`docker ps`

View the logs to verify Jenkins is running:
`docker logs -f jenkins`


Exec into the running Jenkins container:
`docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword`
The command will output the initial admin password. Use it to unlock Jenkins when you access it via http://localhost:8080 in your browser.

