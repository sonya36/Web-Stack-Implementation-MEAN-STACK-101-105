# Documentation on Web-Stack-Implementation-MEAN-STACK-101-105
The MEAN stack is a JavaScript-based framework for developing scalable web applications. The term MEAN is an acronym for MongoDB, Express, Angular, and Node — the four key technologies that make up the layers of the technology stack.

MongoDB: A NoSQL, object-oriented database designed for use with cloud applications
Express(.js): A web application framework for Node(.js) that supports interactions between the front end (e.g., the client side) and the database
Angular(.js): Often referred to as the “front end"; a client-side JavaScript framework used to create dynamic web applications to work with interactive user interfaces
Node(.js): The premier JavaScript web server used to build scalable network applications

## MEAN-Stack-101 : EC2 instace and Virtual Ubuntu Server
AWS account was already created in previously done stack projects. I have created another EC2 instance for MEAN Stack Implentation.

### Created New EC2 instance and Setting up Ubuntu 
- First, created an ec2 instance named it as "Mean_stack_instance" in a region "Stockholm" with instance type "t3.micro", AMI (Amazon Machine Image ) as "ubuntu", at first selected security group having inbound rules for (SSH,HTTP,HTTPS), later on added port 3300 and all other required configuration was selected as default here.
 ![EC2 Instance](./images/t3_micro.png)
 ![EC2 Instance](./images/securitygroup.png)
 ![EC2 Instance](./images/EC2.png)
- Latest version of ubuntu was selected which is "Ubuntu Server 22.04 LTS (HVM)". An AMI is a template that contains the software configuration (operating system, application server, and applications) required to launch your instance.
 ![Ubuntu AMI](./images/AMI_ubuntu.png)
- Private key was generated and named it as : "mean_stack_private" and downloaded ".pem" file.

### Connecting virtual server to EC2 instance
Used the same private key previously downloaded to connect to EC2 instace via ssh :
- Created security group configuration adding ssh and updated this configuration to my ec2 instance to access  TCP port 22.
- Changed the permission for "mern-stack-private.pem" file as :

  ```
    chmod 400 "mean_stack_private.pem"
  ```
- Connected to the instance as
  ```
    ssh -i "mean_stack_private.pem" ubuntu@ec2-16-16-167-154.eu-north-1.compute.amazonaws.com
  ```
  This get changes as you stopped the ec2 instance and run again.
  ![Ubuntuip](./images/ubuntuip.png)

### Conclusion 
Linux Server in the cloud was created.


## MEAN-Stack-102 : Installing Node.js
Node.js is used to set up Express routes and AngularJS controllers.
- Updated and Upgraded ubuntu EC2 instance :
  ```
    sudo apt update
    sudo apt upgrade
  ```
    ![UbuntuUpdate](./images/apt.png)
- Added certificates :
  ```
    sudo apt -y install curl dirmngr apt-transport-https lsb-release ca-certificates
    curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
  ```
- Installed NodeJS :
  ```
    sudo apt install -y nodejs
  ```
## MEAN-Stack-103 : Installing MongoDB

- Imported Mongodb Repository key
  ```
    curl -fsSL https://pgp.mongodb.com/server-7.0.asc | sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg --dearmor
  ```

- Installed mongodb :
  ```
    echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
  ```
  ```
  sudo apt update
  sudo apt install -y mongodb-org
  ```
- Checked the version :
  ```
    mongod --version
  ```
- Started the server :
  ```
    sudo systemctl start mongod
  ```
- Checked the status :
  ```
    sudo systemctl status mongod
  ```
- Installed npm (node package manager) :
  ```
    sudo apt install -y npm 
  ```
- Installed body-parser package :
  ```
    sudo npm install body-parser
  ```
- Created  s folder named 'Books' :
  ```
    mkdir Books && cd Books 
  ```
- In the Books directory, initialzed npm project :
  ```
    npm init
  ```
- Added a file to it, opened the file and wrote the code in the file :
  ```
    vi server.js
  ```
  ```
    var express = require('express');
    var bodyParser = require('body-parser');
    var mongoose = require('mongoose');
    var app = express();

    // Connect to MongoDB
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost, {
    useNewUrlParser: true,
    useUnifiedTopology: true
    });

    // Handle connection events
    mongoose.connection.on('connected', function() {
    console.log('Mongoose connected to ' + dbHost);
    });

    mongoose.connection.on('error', function(err) {
    console.log('Mongoose connection error: ' + err);
    });

    mongoose.connection.on('disconnected', function() {
    console.log('Mongoose disconnected');
    });

    // Middleware
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    app.use(bodyParser.urlencoded({ extended: true }));

    // Routes
    require('./apps/routes')(app);

    // Start server
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
    console.log('Server up: http://localhost:' + app.get('port'));
    });
  ```
- 







