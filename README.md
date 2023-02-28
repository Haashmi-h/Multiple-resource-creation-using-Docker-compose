# Multiple-resource-creation-using-Docker-compose
Docker compose method to deploy multiple resources and host a CMS application.
</br>
</br>


![Untitled Diagram drawio (4)](https://user-images.githubusercontent.com/117455666/221953874-94201265-98ab-4e98-8eac-fbf98ec3509d.png)

</br>
Here, we deploy multiple following resources together via Docker compose: </br> Then a Wordpress website is configured using all of these.

##### Custom network:
When you start Docker, a default bridge network (also called bridge) is created automatically, and newly-started containers connect to it unless otherwise specified.</br>
</br>

We can also create user-defined custom bridge networks. User-defined bridge networks are superior to the default bridge network.</br>
Here also, we are deploying the CMS application in such a custom bridge network.</br>
The "docker-compose.yaml" mainly includes:

##### 2 Volumes to bind mount:
Bind mounts have limited functionality compared to volumes. </br>
When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its absolute path on the host machine.
1. For database.
2. For website files.

##### 2 containers:
1. One composer is for setting up the database server. </br>
We use "Mysql 5.6" docker image to install, then create database setting datadirectory "/var/lib/mysql" using bindmount option.
2. One composer is to install wordpress from docker image "Wordpress", then saving website contents through bind mount on "/var/www/html" folder and database details from the first container as datbase host.</br>

We are creating both containers in the same custom network.
</br>

To check if the configurations are properly setup, the following command is used:
`docker-compose config`

</br>
Once this is properly executed without any issue, docker compose file can be executed using the command:

`docker-compose up -d`
</br>

***Result***

```sh
[+] Running 22/22
 ? web Pulled                                                                                                                                  21.2s
   ? 8740c948ffd4 Pull complete                                                                                                                 5.1s
   ? 1873be858264 Pull complete                                                                                                                 5.2s
   ? 7ce6a163d8c1 Pull complete                                                                                                                11.1s
   ? 008a172010ba Pull complete                                                                                                                11.2s
   ? d15353ae3d77 Pull complete                                                                                                                12.2s
   ? 223eb1888c0f Pull complete                                                                                                                12.3s
   ? 83374c2a967a Pull complete                                                                                                                12.4s
   ? 8adb6fee4c96 Pull complete                                                                                                                12.6s
   ? 04e8dd13d367 Pull complete                                                                                                                12.7s
   ? 08c657b572e7 Pull complete                                                                                                                13.5s
   ? 4e2a69062e74 Pull complete                                                                                                                13.6s
   ? f340310b8889 Pull complete                                                                                                                13.7s
   ? c2839f599e98 Pull complete                                                                                                                13.7s
   ? 07cf3f6c92fa Pull complete                                                                                                                14.9s
   ? a61869359229 Pull complete                                                                                                                15.9s
   ? a84ad5ffd8f9 Pull complete                                                                                                                16.0s
   ? c7fdd14fc94d Pull complete                                                                                                                16.0s
   ? f7d19cc4ffaa Pull complete                                                                                                                16.1s
   ? 1b81deb9db45 Pull complete                                                                                                                18.0s
   ? db8f5037e095 Pull complete                                                                                                                18.1s
   ? 34b5166230af Pull complete                                                                                                                18.2s 
[+] Running 3/3
 ? Network web-project_wpntw  Created                                                                                                           0.1s  
 ? Container wpdb             Started                                                                                                           1.2s  
 ? Container wpfiles          Started                                                                                                           1.2s
```
</br>
</br>

To know the container status:-
`docker container ls -a`

```sh
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS         PORTS                               NAMES
cb83d22df8d3   mysql:5.6          "docker-entrypoint.s…"   6 seconds ago   Up 4 seconds   3306/tcp                            wpdb
a2491aeb639b   wordpress:latest   "docker-entrypoint.s…"   6 seconds ago   Up 4 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   wpfiles
```


</br>
</br>
Thus the wordpress website can be optained by host machine's IP/ hostname with port "80".
</br>


***Website preview:***
</br>

![image](https://user-images.githubusercontent.com/117455666/221946350-6ec26eb6-e433-4c77-aa31-6d0c47ae088a.png)

