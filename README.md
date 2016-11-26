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

### Build and run frontend.
Change directory to the frontend folder.
Run command "sudo docker build . --tag customimage:2" without outer quotation characters.
This will build the container.

To run it, run command "sudo docker run -d --name frontend -p 80:80 customimage:2" without outer quotation characters.
This will set it to run on the specified ports (please make sure the ports are available).

If you make changes to the frontend, you need to rebuild and restart it.
Run command "sudo docker rm -f frontend; sudo docker build . --tag customimage:2; sudo docker run -d --name frontend -p 80:80 customimage:2" without outer quotation characters.
This will remove existing frontend container, rebuild it and start it.

### Build and run HAProxy.
Change directory to the haproxy folder.
Run command "sudo docker build . --tag customhaproxy:2" without outer quotation characters.
This will build the container.

To run it, run command "sudo docker run -d --name haproxy -p 8000:8000 customhaproxy:2" without outer quotation characters.
This will set it to run on the specified ports (please make sure the ports are available).

If you make changes to the haproxy, you need to rebuild and restart it.
Run command "sudo docker rm -f haproxy; sudo docker build . --tag customhaproxy:2; sudo docker run -d --name haproxy -p 8000:8000 customhaproxy:2" without outer quotation characters.
This will remove existing haproxy container, rebuild it and start it.

### Build and run Logstash.
Change directory to the logstash folder.
Run command "sudo docker build . --tag customlogstash:2" without outer quotation characters.
This will build the container.

To run it, run command "sudo docker run -d --name logstash -p 2000:2000 customlogstash:2" without outer quotation characters.
This will set it to run on the specified ports (please make sure the ports are available).

If you make changes to the logstash, you need to rebuild and restart it.
Run command "sudo docker rm -f logstash; sudo docker build . --tag customlogstash:2; sudo docker run -d --name logstash -p 2000:2000 customlogstash:2" without outer quotation characters.
This will remove existing logstash container, rebuild it and start it.

### Build and run MongoDB.
Run command "sudo docker run -d --name mongo -p 91:27017 -p 92:28017 mongo:3 mongod --rest" without outer quotation characters.

If you make changes, you can remove and restart it with following command.
Run command "sudo docker rm -f mongo; sudo docker run -d --name mongo -p 91:27017 -p 92:28017 mongo:3 mongod --rest" without outer quotation characters.


