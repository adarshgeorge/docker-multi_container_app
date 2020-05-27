## Creating Multi Container App using Flask Container and Redis

A DevOps Project to display Location of IP address.

![alt HelloWorld](https://github.com/adarshgeorge/docker-multi_container_app/blob/master/png/Search.png)



### Pre-Requisite
An Server/EC2 with docker installed. 

Userdata while launching an EC2 instance.
```
#!/bin/bash
sudo yum install docker -y
sudo service docker restart
sudo chkconfig docker on
sudo usermod -a -G docker ec2-user

```

First we need to create bridge network 

```
docker network create --driver bridge ipstack-net
```

Creating redis cache server!

```
docker run \
-d \
--name ipstack-cache \
--network ipstack-net \
--restart always \
redis:latest
```


Creating Flask WebInterface

```
docker run \
-d \
--name ipstack-app \
--network ipstack-net \
--restart always \
-p 80:8080 \
-e REDIS_HOST=ipstack-cache \
-e REDIS_CACHE=600 \
-e FLASK_PORT=8080 \
-e IPSTACK_KEY='91e59ddd65394199ad3f905d35532419' \
adarshgeorge999/ipstackapp:1
```

**Output**

![alt Search](https://github.com/adarshgeorge/docker-multi_container_app/blob/master/png/Searching.png) 



![alt Out1](https://github.com/adarshgeorge/docker-multi_container_app/blob/master/png/out1.png) 


Now again search the same IP address to verify the IP address is cached in reddis container. 

![alt Out2](https://github.com/adarshgeorge/docker-multi_container_app/blob/master/png/result.png) 


## That's it!!