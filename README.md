## Creating Multi Container App using Flask Container and Redis

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
-e IPSTACK_KEY='91e59ddd65394199ad3f905d35532479' \
adarshgeorge999/ipstackapp:1
```

