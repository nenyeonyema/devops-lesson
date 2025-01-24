# Use the official Jenkins LTS image from Docker Hub
FROM jenkins/jenkins:lts

# Switch to root user to install dependencies
USER root

# Install necessary packages (optional, adjust as needed)
RUN apt-get update && \
    apt-get install -y git docker.io && \
    apt-get clean

# Switch back to Jenkins user
USER jenkins

# Expose necessary ports
EXPOSE 8080 50000

# Set Jenkins home directory
VOLUME /var/jenkins_home

# Default command to run Jenkins
CMD ["jenkins"]