# Docker exercise
TUT Palvelupohjaiset järjestelmät 2/2016.

## Members of the group are
* Juha Löflund, juha.suominen@student.tut.fi
* Ville Myllynen, ville.myllynen@student.tut.fi

## Purpose of this project.
* Exercise Docker (https://www.docker.com)
* Familiarize the group with using Docker swarm.
* Configure HAProxy
* Configure Logstash
* Use Rabbitmq

## Build and run
```
# Delete existing containers.
sudo docker rm -f docker-mongo
sudo docker rm -f docker-rabbitmq
sudo docker rm -f docker-frontend
sudo docker rm -f docker-logstash-q
sudo docker rm -f docker-logstash-db
sudo docker rm -f docker-haproxy

# Build images
sudo docker build frontend -t docker-frontend
sudo docker build logstashdb -t docker-logstashdb
sudo docker build logstashq -t docker-logstashq
sudo docker build haproxy -t docker-haproxy

# Run containers
sudo docker run -d --name docker-mongo -h mongohost -p 91:27017 -p 92:28017 mongo:3 mongod --rest
sudo docker run -d --name docker-rabbitmq -h rabbitmqhost rabbitmq
sudo docker run -d --name docker-frontend -h frontendhost -p 80:80 docker-frontend
sudo docker run -d --name docker-logstash-q -h interfacehost --link=docker-rabbitmq docker-logstashq
sudo docker run -d --name docker-logstash-db --link=docker-rabbitmq --link=docker-mongo docker-logstashdb
sudo docker run -d --name docker-haproxy --link=docker-mongo --link=docker-logstash-q --link=docker-frontend -p 8000:8000 docker-haproxy
```
