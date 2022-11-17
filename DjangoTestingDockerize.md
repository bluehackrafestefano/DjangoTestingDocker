- What is Docker
Docker makes development efficient and predictable. It is working in my computer, but what about after deployment to the cloud?
Docker takes away repetitive configuration tasks and is used throughout the development lifecycle for fast, easy and portable application development.

- Build
Get a head start on your coding by leveraging Docker images to efficiently develop your own unique applications on any OS.  Create your multi-container application using Docker Compose.

- Share
Leverage Docker Trusted Content, including Docker Official Images and images from Docker Verified Publishers from the Docker Hub repository.
Innovate by collaborating with team members and other developers and by easily publishing images to Docker Hub.

- Run
Deliver multiple applications hassle free and have them run the same way on all your environments including design, testing, staging and production – desktop or cloud-native.
Deploy your applications in separate containers independently and in different languages. Reduce the risk of conflict between languages, libraries or frameworks.

- Prepare Django project and push to github.

### Dockerize Django

- Create Dockerfile inside working directory;
```dockerfile
# Select a base image which suits your usecase
FROM python:3.10.8-slim-bullseye

# PYTHONDONTWRITEBYTECODE=1 env prevents Python from copying pyc files 
# to the container. 
ENV PYTHONDONTWRITEBYTECODE=1

# PYTHONUNBUFFERED=1 ensures that Python output is logged to 
# the terminal, making it possible to monitor Django logs in 
# realtime.
ENV PYTHONUNBUFFERED=1

WORKDIR /code

# Docker checks if layers can be reused, if it finds that there are 
# no changes to the requirements.txt file, it will jump straight 
# to the COPY instruction, which will be resolved in a matter of seconds. 
# With this tiny change, we speed up a lot the build process: No more 
# waiting for minutes between builds each time that we modify something 
# in our code.
COPY requirements.txt /code/requirements.txt

# Adding --no-cache-dir to the pip install command saves an 
# additional disk space, as this prevents pip from caching 
# downloads and caching wheels locally. Since you won't need 
# to install requirements again after the Docker image has 
# been created, this can be added to the pip install command.
RUN pip install -r requirements.txt --no-cache-dir

# Copy all the working directory to the container. Optionally a
# `.dockerignore` file can be used not to copy unrelated things
# to the continer and keep it smaller in size and less vulnerable
COPY . /code/

CMD [ "python", "manage.py", "runserver", "0.0.0.0:8000" ]
```

- Build image using Dockerfile on the current directory;
```docker
docker build -t django-testing .
```

- Make a container from the image you just created with the tag `django-testing`;
```docker
docker run -d -p 8000:8000 django-testing
```

- Go to the Docker Desktop and open your app on the browser. Also you can simply go to the endpoint you published on your docker run command which is port 8000.

- See your application up and running. But if you test the app you will see you did not apply migrations and create db tables. So open a terminal and migrate db tables.


### Dockerize DataBase

- In most cases, we use Postgresql for db layer. We can dockerize db too. 


### .dockerignore

- A standard .dockerignore file may be;
```
*.pyc
*.pyo
*.mo
*.db
*.css.map
*.egg-info
*.sql.gz
.cache
.project
.idea
.pydevproject
.idea/workspace.xml
.DS_Store
.git/
.sass-cache
.vagrant/
__pycache__
dist
docs
env
logs
src/{{ project_name }}/settings/local.py
src/node_modules
web/media
web/static/CACHE
stats
Dockerfile
```

### Best practices for containerizing Python applications

- Use explicit and deterministic Docker base image tags for containerized Python applications.
- Separate dependencies from source code.
- Use Python WSGI for production.
- Run containers with least possible privilege (and never as root).
- Handle unhealthy states of your application. 
- Find and fix security vulnerabilities in your Python Docker application image.