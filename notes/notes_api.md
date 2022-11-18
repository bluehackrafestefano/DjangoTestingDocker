docker-compose down -v
docker volume rm $(docker volume ls -q)
docker-compose up -d --build
docker-compose exec web python manage.py migrate --noinput
docker-compose exec web python manage.py collectstatic --no-input --clear
docker-compose logs web
docker system prune
docker system prune -a

docker build -t flightapp-api .
docker login
docker tag flightapp-api <dockerhubusername>/flightapp-api:first_push
docker push <dockerhubusername>/flightapp-api:first_push
docker push stefanorafe/flightapp-client:first_push


http://nginx.org/en/docs/http/ngx_http_core_module.html#alias


docker exec -it <container_id> python manage.py shell

The gunicorn should be used to serve the python "application" itself, while the static files are served by a static file server ( such as Nginx ).



from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    # ... the rest of your URLconf goes here ...
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)


nginx -v
nginx -h
nginx -t  # for testing especially configuration errors
systemctl status nginx
sudo su -
systemctl start nginx
systemctl reload nginx
systemctl stop nginx

/etc/nginx   configurations
conf.d , sites-available , sites-enabled  --> configurations
/var/log/nginx/  --> log files
/var/www/  --> files served html static etc
/var/www/mysite/

/etc/nginx/conf.d/mysite.conf

location directives:
server {
    location [mofifier] location_definition {}
}

for i in {1..10}; do curl localhost > /dev/null; done  --> test requesting the page to create logs

checking the ports:
sudo lsof -i :80 -i :443 | grep nginx
sudo apt install net-tools
sudo netstat -plan | grep nginx

tail -f /var/logs/nginx/*.log

load ballancer settings
```
# Balance requests across three servers in a group named "uuid"
# Use the same IP address for each server: 127.0.0.1
# Use a different port for each server: 9001, 9002, and 9003
upstream uuid {
    server 127.0.0.1:9001;
    server 127.0.0.1:9002;
    server 127.0.0.1:9003;
}

server {
    listen 80;

    # Add a location directive here to serve requests from "/uuid"
    # Inside the location, connect to the group named "uuid"
    location /uuid {
        proxy_pass http://uuid/;
    }
}
```

### dockerhub 
L^^hvk2$2N_x_Vc


### ecr
terraform init
terraform fmt  Format your Terraform configuration files using the HCL language standard.
terraform fmt --recursive — Also formats files in subdirectories
terraform get -update — Checks the versions of the already installed modules against the available modules and installs the newer versions if available
terraform validate — Validates the configuration files in your directory and does not access any remote state or services.
terraform plan -out=<path> — Save the plan file to a given path. Can then be passed to the terraform apply command.
terraform apply -auto-approve — Apply changes without having to interactively type ‘yes’ to the plan. Useful in automation CI/CD pipelines.
terraform destroy -auto-approve — Destroys the infrastructure without having to interactively type ‘yes’ to the plan. Useful in automation CI/CD pipelines


### K8s
terraform apply
aws eks --region $(terraform output -raw region) update-kubeconfig \
    --name $(terraform output -raw cluster_name)
kubectl cluster-info
kubectl get nodes
terraform destroy -auto-approve


## Best practices
Use explicit and deterministic Docker base image tags for containerized Python applications.
Separate dependencies from source code.
Use Python WSGI for production.
Run containers with least possible privilege (and never as root).
Handle unhealthy states of your application. 
Find and fix security vulnerabilities in your Python Docker application image.


## Volumes
docker run -d \
  -it \
  --name devtest \
  -v "$(pwd)"/target:/app \
  nginx:latest


## Using hadolint
iwr -useb get.scoop.sh | iex
scoop install hadolint
hadolint <Dockerfile>
docker run --rm -i hadolint/hadolint < Dockerfile