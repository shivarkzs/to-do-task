![todo app](https://user-images.githubusercontent.com/82657136/116808188-118daf00-ab55-11eb-88c0-a7d9ee612b5a.jpg)




                               --------Frontend.server--------

The frontend is the service in To-do to serve the web content over Nginx.

To Install Nginx.

# apt-get update
# apt-get install nginx -y

This service is written in NodeJS, Hence need to install NodeJS in the system.

# curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
# apt-get install -y nodejs
# ape-get npm install -you
# cd /var/www/html
# mkdir todo
# git clone https://github.com/zelar-soft-todoapp/frontend.git
# cd /var/www/html/todo/frontend 
# cd /etc/nginx
# cd site-available
# vi default
#    root /var/www/html/todo/frontend/dist;
# nginx -t
# systemctl restart nginx

 Update Login and todo Ip address.
 
# cd /var/www/html/vue/frontend
# cd config
# vi index.js

Finally restart the service once to effect the changes.
# systemctl daemon-reload
# systemctl restart nginx
# npm start



                                   ------Login.server-------

This service is responsible for showing the login in portal.

This service is written in Go language , Hence need to install 'go' in the system.

# apt update
# wget https://dl.google.com/go/go1.14.1.linux-amd64.tar.gz
# tar -xvzf go1.14.1.linux-amd64.tar.gzgo version
# rm go1.14.1.linux-amd64.tar.gz
# mkdir -p $HOME/go/{bin,src,pkg}
# export "PATH=$PATH:/usr/local/go/bin"
# export "GOPATH=$HOME/go"
# export "GOBIN=$GOPATH/bin"
# source ~/.profile
# go env
# cd $HOME/go/src 
# mkdir new_go_module_srk
# cd new_go_module_srk
# git clone https://github.com/zelar-soft-todoapp/login.git
# cd login
# export GOPATH=/go
# go get
# go build
# ./login

Now, lets set up the service with systemctl.

# vi /etc/systemd/system/login.service
                   
	[Unit]
	Description= login.service
	
	[Service]
	User=root
	Environment=USERS_API_ADDRESS=http://172.31.18.126:8080
	Environment=AUTH_API_PORT=8080
	ExecStart=/root/go/src/new_go_module_srk/login/login
	Restart=always
	Syslogidentifier=login
	
	[Install]
	WantedBy=multi-user.target


# systemctl daemon-reload
# systemctl enable login.service
# systemctl start login.service
# systemctl status login.service

                                         --------users.server--------
										 										 
Todo-Users

Users service is responsible for finding the users by username and password.
First you need to downgrade java version from  java-8

# apt update
# apt-get install openjdk-8-jdk
# java -version

Now, go and fetch git code

# git clone https://github.com/zelar-soft-todoapp/users.git
# cd users
# cd
# apt-get install maven -y
# mvn clean package
# cd target
# java -jar users-api-0.0.1.jar

Start service now

# vi /etc/systemd/system/users.service

	[Unit]
	Description = users.service

	[Service]
	User=root
	Environment=SERVER_PORT=8080
	ExecStart=java -jar /root/users/target/users-api-0.0.1.jar
	SyslogIdentifier=user

	[Install]
	WantedBy=multi-user.target
	
# systemctl daemon-reload
# systemctl enable users.service
# systemctl start users.service
# systemctl status users.service




                                --------Todo server--------------
								
     This service is responsible for Todo Service in Todo app.

     This service is written in NodeJS, Hence need to install NodeJS in the system.
	 
# apt-get update
# apt-get install nodejs -y
# apt-get install npm -y
# git clone https://github.com/zelar-soft-todoapp/todo.git
# cd todo/
# npm install
# npm start
	 
     Now, lets set up the service with systemctl.
     
# cd /etc/systemd/system
# vi todo.service
	 
	    [UNIT]
		Description = Todo Service

		[Service]
		Environment=REDIS_HOST=172.31.25.65
		ExecStart=/bin/node /root/todo/server.js
		SyslogIdentifier=todo
		[Install]
		WantedBy=multi-user.target
	 
	 
# systemctl daemon-reload
# systemctl restart todo.service
# systemctl status todo.service
     
	 
	 
	                             -------Redis server-------
								 
# Redis
# apt-get update
# apt-get install redis-server
# sed -i -e 's/127.0.0.1/0.0.0.0/' /etc/redis/redis.conf
# systemctl enable redis
# systemctl start redis
# systemctl status redis

.......................................
