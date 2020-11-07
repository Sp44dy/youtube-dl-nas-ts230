[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](https://raw.githubusercontent.com/edebont/youtube-dl-nas/master/LICENSE)
![Docker Pulls Shield](https://img.shields.io/docker/pulls/edebont/youtube-dl-nas.svg?style=flat-square)
# youtube-dl-nas

simple youtube download micro web queue server. 
To prevent a queue attack when using NAS as a server, a making account was created when creating a docker container, and a new UI was added.
This Queue server based on python3 and debian Linux.
https://hub.docker.com/r/modenaf360/youtube-dl-nas/

- web server : [`bottle`](https://github.com/bottlepy/bottle) 
- youtube-dl : [`youtube-dl`](https://github.com/rg3/youtube-dl).
- base : [`python queue server`](https://github.com/manbearwiz/youtube-dl-server).
- websocket : [`bottle-websocket`](https://github.com/zeekay/bottle-websocket).

![screenshot1](https://github.com/hyeonsangjeon/youtube-dl-nas/blob/master/pic/youtube-dl-server-login.png?raw=true)

### Update Info
- 2020.11.08 : Copied and modified image from https://github.com/hyeonsangjeon/youtube-dl-nas to build for ARM (Raspberry Pi) 

### How to use this image

#### To run docker, excute this command in a ternimal:
The docker volume parameter `-v` is used by the queue operation to process the downloaded mount path to the host server

- downloaded docker volume path :  '/downfolder'  
- MY_ID, MY_PW : Required variables when you login validation, The start character  '!' '$' '&' is does not apply.
- TZ :  optional variable.

## docker options are as follows,

|Variables      |Description                                                   |
|---------------|--------------------------------------------------------------|
|-v  host:guest         |/downfolder (do not change guest location. expose volume mount to host server)                            |  
|-d           |background run                                                | 
|-p   host:guest        |expose port conainer core-os port to your os (port fowarding) |
|-e MY_ID          |using it login id, linux environment variables, do not use start character   '!' '$' '&'                                |
|-e MY_PW           |using it login password, linux environment variables ,  do not use start character   '!' '$' '&'                                  |
|-e TZ           |take it to user country, linux environment variables                                   |
|-e APP_PORT           |optinal variable. default is 8080   |

##### To run docker, excute this command in a ternimal:
```shell
docker run -d --name youtube-dl -e MY_ID=youtube -e MY_PW=1234  -v /volume2/youtube-dl:/downfolder -p 8080:8080 modenaf360/youtube-dl-nas
```

##### If want set TimeZone, example using in Europe web content 
```shell
docker run -d --name youtube-dl -e TZ=Europe/Amsterdam -e MY_ID=youtube -e MY_PW=1234 -v /volume2/youtube-dl:/downfolder -p 8080:8080 edebont/youtube-dl-nas
```

##### example, how to using docker host network and changing the application port :
```shell
# use --net=host -e APP_PORT=custom_port
docker run -d --name youtube-dl --net=host -e APP_PORT=9999 -e MY_ID=youtube -e MY_PW=1234  -v /volume2/youtube-dl:/downfolder edebont/youtube-dl-nas
```


#### Request restful API & Response
```shell
curl -X POST http://localhost:8080/youtube-dl/rest \
  -d '{
	"url":"https://www.youtube.com/watch?v=s9mO5q6GiAc",
	"resolution":"best", 
	"id":"iamgroot",
	"pw":"1234"
}'
```
```shell
{
    "success": true,
    "msg": "download has started",
    "Remaining downloading count": "7"
}
```

 If you want to get into docker container os, excute this command `[1]` :
```console
docker exec -i -t youtube-dl /bin/bash
```

