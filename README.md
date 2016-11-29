# Docker exercise
TUT Palvelupohjaiset järjestelmät 2/2016.

## Members of the group are
* Juha Löflund, 218619, juha.suominen@student.tut.fi
* Ville Myllynen, 234334, ville.myllynen@student.tut.fi

## Purpose of this project.
* Exercise Docker (https://www.docker.com)
* Familiarize the group with using Docker swarm.
* Configure HAProxy
* Configure Logstash

## Build and run
```
# Frontend
sudo docker build frontend -t myfrontend
sudo docker run -d --name frontend -p 80:80 myfrontend

# Haproxy
sudo docker build haproxy -t myhaproxy
sudo docker run -d --name haproxy -p 8000:8000 myhaproxy

# Logstash
sudo docker build logstash -t mylogstash
sudo docker run -d --name logstash -p 2000:2000 mylogstash

# MongoDB
sudo docker run -d --name mongo -p 91:27017 -p 92:28017 mongo:3 mongod --rest

# Remove, rebuild and restart all.
sudo docker rm -f frontend; sudo docker build frontend -t myfrontend; sudo docker run -d --name frontend -p 80:80 myfrontend;
sudo docker rm -f haproxy; sudo docker build haproxy -t myhaproxy; sudo docker run -d --name haproxy -p 8000:8000 myhaproxy;
sudo docker rm -f logstash; sudo docker build logstash -t mylogstash; sudo docker run -d --name logstash -p 2000:2000 mylogstash;
sudo docker rm -f mongo; sudo docker run -d --name mongo -p 91:27017 -p 92:28017 mongo:3 mongod --rest;
```

## Architecture
### HAProxy
* HAProxy routes all incoming requests according to the url path.
* "/" => frontend web application.
* "/interface" => logstash
* "/ordersDB" => MongoDB

### Frontend
Frontend has two buttons: "Add order" and "Fetch Orders"
#### "Add order"
* "Add order" will take text input and send the data to logstash.
* Logstash renames JSON data "message" field (containing the input data) to "name" field.
* Logstash adds new field "ordered" with value "true" to the JSON data.
* The message is then routed to the database URL where new database entry is created.
#### "Fetch orders"
* "Fetch orders" will query the database directly using the corresponding URL to get all entries with "ordered" value being true
* Results are shown as a list.

### Logstash
Logstash is configured to get inputs to specified URL and send data to MongoDB.

### MongoDB
Contains all data sent from the application.

## Usage
This chapter describes how to setup this docker project.
Note that this project has HAProxy mapped to port 8000. This means it is not executable in Virtual machine provided by the course.

### Install Docker
From docker webpage, follow the instructions to install docker on your computer.

