# Part 3: Docker Volumes
### Create a new Docker volume named "my_volume".

`docker volume create my_volume` will register a persistent volume managed by Docker.
- It won’t be tied to any specific container until you mount it.
- Volumes survive container removal, so your data stays safe unless you explicitly delete the volume.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker volume create my_volume
my_volume
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker volume ls
DRIVER    VOLUME NAME
local     5023314c9c2e425d35a9dc5bd5e0de1ffdadabacb2b8359f2c74de20d94275f3
local     my_volume
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Create a new Docker container using the "nginx" image and mount the "my_volume"

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker run -d --name my_nginx -v my_volume:/usr/share/nginx/html -p 8080:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
1efc53192927: Pull complete
04bdbc7ea6b5: Pull complete
807ab930f3a0: Pull complete
f7e1db57b58b: Pull complete
8020170dc65f: Pull complete
947d22046304: Pull complete
c329293b1d90: Download complete
a1b85ab28c90: Download complete
Digest: sha256:f9ea18bfa4fad859e1ed38259d711da7ccad2c3516e875cec3351a57c859f571
Status: Downloaded newer image for nginx:latest
58989acbb0a659d0d757a3134491a9fc5beef873d8d013d13fb60f1df74b53f9
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Verify that the "nginx" default page is accessible on your host machine at http://localhost:8080.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED             STATUS          PORTS                                     NAMES
58989acbb0a6   nginx                                 "/docker-entrypoint.…"   51 seconds ago      Up 50 seconds   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp   my_nginx
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   About an hour ago   Up 50 minutes   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   dicebootcamp26
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ curl localhost:8080
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy,
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Create a new file named "index.html" on your host machine and add some text to it.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ touch index.html
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ echo "Test File for assignment1 part3" >> index.html
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Copy the "index.html" file from your host machine to the "my_volume" volume using the "docker cp" command

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker cp ./index.html my_nginx:/usr/share/nginx/html/index.html
Successfully copied 32B (transferred 2.05kB) to my_nginx:/usr/share/nginx/html/index.html
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Verify that the "index.html" file is accessible on your host machine at http://localhost:8080

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ curl localhost:8080
Test File for assignment1 part3
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Stop and remove the container. 

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker stop  my_nginx
my_nginx
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker rm  my_nginx
my_nginx
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Create a new Docker container using the "httpd" image and mount the "my_volume" volume to the container's "/usr/local/apache2/htdocs" directory. 

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker run -d --name my_httpd -v my_volume:/usr/local/apache2/htdocs -p 8081:80 httpd
Unable to find image 'httpd:latest' locally
latest: Pulling from library/httpd
d19953f56d19: Pull complete
62aa6b76421e: Pull complete
4f4fb700ef54: Pull complete
31730ab78c03: Pull complete
0bdb50ccb97e: Pull complete
2e5e9538f241: Download complete
ae5940b419b0: Download complete
Digest: sha256:c842ac797bd1fe8b3cd26d002ac9d948aa2814097544600600b35aef42e6785a
Status: Downloaded newer image for httpd:latest
576997ec232a37f7603c8ce2eaeafb8818490761060270dfeace2147ee2fdccc
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Verify that the "httpd" default page is accessible on your host machine at http://localhost:8081.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker ps
CONTAINER ID   IMAGE                                 COMMAND                  CREATED             STATUS          PORTS                                     NAMES
576997ec232a   httpd                                 "httpd-foreground"       34 seconds ago      Up 33 seconds   0.0.0.0:8081->80/tcp, [::]:8081->80/tcp   my_httpd
739bb30b0736   highspeedali/dicebootcamp26:assign1   "uv run main.py --po…"   About an hour ago   Up 54 minutes   0.0.0.0:80->8080/tcp, [::]:80->8080/tcp   dicebootcamp26
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ curl localhost:8081
Test File for assignment1 part3
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Create a new file named "about.html" on your host machine and add some text to it. 

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ touch about.html
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ echo "about.html created for assignment 1 part 3" > about.html
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Copy the "about.html" file from your host machine to the "my_volume" volume using the "docker cp" command.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker cp ./about.html my_httpd:/usr/local/apache2/htdocs/about.html
Successfully copied 43B (transferred 2.05kB) to my_httpd:/usr/local/apache2/htdocs/about.html
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Verify that the "about.html" file is accessible on your host machine at http://localhost:8081/about.html.

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ curl localhost:8081/about.html
about.html created for assignment 1 part 3
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Stop and remove the container. 

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker stop  my_httpd
my_httpd
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker rm  my_httpd
my_httpd
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Verify that the "index.html" and "about.html" files are still available in the "my_volume" volume. 

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker volume ls
DRIVER    VOLUME NAME
local     5023314c9c2e425d35a9dc5bd5e0de1ffdadabacb2b8359f2c74de20d94275f3
local     my_volume
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
buntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker volume inspect my_volume
[
    {
        "CreatedAt": "2026-10-10T00:30:41Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/my_volume/_data",
        "Name": "my_volume",
        "Options": null,
        "Scope": "local"
    }
]
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ ls /var/lib/docker/volumes/my_volume/_data
ls: cannot access '/var/lib/docker/volumes/my_volume/_data': Permission denied
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ sudo ls /var/lib/docker/volumes/my_volume/_data
[sudo] password for ubuntu:
50x.html  about.html  index.html
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```

### Cleanup: Remove the "my_volume" volume. 

```
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker volume rm my_volume
my_volume
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$ docker volume ls
DRIVER    VOLUME NAME
local     5023314c9c2e425d35a9dc5bd5e0de1ffdadabacb2b8359f2c74de20d94275f3
ubuntu@ubuntu:~/dicebootcamp26-assignment1-part3$
```
