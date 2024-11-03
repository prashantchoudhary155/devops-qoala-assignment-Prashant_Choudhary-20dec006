# Screenshots

## Screenshot of the application running in the browser.
![assignment123](https://github.com/user-attachments/assets/d09dbed8-6435-425f-ad54-4cea995523f1)

## Screenshot showing Nginx (or web server) access logs that confirm a successful request.
![ass123](https://github.com/user-attachments/assets/08174270-ed34-4c0e-b488-6b22c36fbf2d)



# Issues Identified and Resolution Steps

During the setup and testing of the multi-container Docker environment, several issues were identified across different files, including misconfigurations and missing files. Here’s a detailed report on each issue encountered and the steps taken to resolve them, with specific file references and line changes.

## docker-compose.yml
### Issues:
- Nginx was not correctly forwarding requests to the Flask application.
- Port conflicts on port 80 due to permissions on the host system.

### Resolution Steps:
- **Update Port Mappings:** Changed the exposed Nginx port to 8080 instead of 80 to avoid permission conflicts.
- **Network Configuration:** Ensured both services shared an internal network by defining the network in the docker-compose.yml file.

## nginx.conf
### Issues:
- Nginx was not configured to forward requests to the Flask application running on port 8000.
- The configuration file lacked a static file routing setup for testing direct access.

### Resolution Steps:
- **Proxy Pass Configuration:** Added a reverse proxy configuration in nginx.conf to forward requests from Nginx (port 8080) to Flask (port 8000).
- **Static File Routing:** Configured Nginx to serve static files from the html directory directly under the /static/ endpoint.

## index.html
### Purpose:
- index.html was added to the html folder to verify that Nginx can serve static content independently.

## Python Dockerfile (python/Dockerfile)
### Issues:
- The Python Dockerfile was incomplete, lacking dependency installations and a properly exposed port.

### Resolution Steps:
- **Install Dependencies:** Added a RUN command to install dependencies from requirements.txt.
- **Expose Port:** Exposed port 8000 for internal communication within the Docker network.
- **CMD Instruction:** Set the command to run app.py using Flask.

## Nginx Dockerfile (nginx/Dockerfile)
### Purpose:
- The Nginx Dockerfile was set up to include a custom nginx.conf and copy static files.

### Resolution Steps:
- **Copy Configuration:** Replaced the default Nginx configuration with nginx.conf.
- **Copy Static Files:** Added a command to copy index.html from the html folder.

## app.py (Python Flask Application)
### Issues:
- Minor adjustments were required to set the host and port properly for Docker deployment.

### Resolution Steps:
- **Set Host and Port:** Configured Flask to run on 0.0.0.0 (allowing all incoming connections) and port 8000.



# Summary of Changes
The following adjustments resolved all issues and ensured the multi-container setup worked as expected:
- **docker-compose.yml:** Adjusted port mappings and added network configuration.
- **nginx.conf:** Configured proxy pass and static file routing.
- **index.html:** Added for static content testing, accessible at /static/index.html.
- **Python Dockerfile:** Completed the setup with dependencies, exposed ports, and run command.
- **Nginx Dockerfile:** Added custom configuration and static file serving setup.
