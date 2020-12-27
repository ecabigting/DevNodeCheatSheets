# Setting up MongoDB in Linux
*   Install via package management system
        +   Get a public GPG key first 
            >   `wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add -`
        +   Create a list file for MongoDB. 
            >   For Ubuntu 20.04 (Focal) `echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/4.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.4.list`
        +   Reload local package database
            >   `sudo apt-get update`
        +   Install the MongoDB packages
            >   `sudo apt-get install -y mongodb-org`
        +   Verify that MongoDB has started successfully
            >   `sudo systemctl status mongod`
        +   If you get the following error:
            >   ● mongod.service - MongoDB Database Server            
            >   Loaded: loaded (/lib/systemd/system/mongod.service; enabled; vendor preset: enabled)            
            >   Active: failed (Result: exit-code) since Sat 2020-12-26 21:19:42 +04; 6min ago            
            >   Docs: https://docs.mongodb.org/manual            
            >   Process: 6013 ExecStart=/usr/bin/mongod --config /etc/mongod.conf (code=exited, status=14)            
            >   Main PID: 6013 (code=exited, status=14)            
        +   Run the following commands(skip these commands if you did not get the above errors):                
            -   `chown -R mongodb:mongodb /var/lib/mongodb`                        
            -   `chown mongodb:mongodb /tmp/mongodb-27017.sock`
            -   `sudo service mongod restart`
            -   then check the status again using `sudo systemctl status mongod`
        +   You can optionally ensure that MongoDB will start following a system reboot by issuing the following command:
            >   `sudo systemctl enable mongod`
        +   NOTES**:
            +   Data Directory : `/var/lib/mongodb`
            +   Log Directory : `/var/log/mongodb`
            +   Config File:    `/etc/mongod.conf`
*   Installing MongoDB Compass
    +   Client tool provided by the MongoDB team to help see MongoDB database visuall.
        >   `https://www.mongodb.com/try/download/compass`
*   Using Mongoose
    +   To connect to MongoDB from node, we will need a lib. Mongoose is the officially supported Node.js package
    +   To install run `npm install mongoose`
*   Usefull link learning about Query documents
    >   https://docs.mongodb.com/manual/tutorial/query-documents/