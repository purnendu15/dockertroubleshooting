# dockertroubleshooting
Troubleshooting experiments with Docker

# 403 Forbidden while loading a custom page to Nginx
----------------------------------------------------


1. My first testing with docker and nginx. I could not load a custom age.
Steps:
a) docker pull nginx

b)
home@home-3000-N100:~$ pwd
/home/home

c) 
host files to be loaded to webserver
home@home-3000-N100:~$ pwd
/home/home

d) start docker container
docker run -d -P --name some-nginx -v /some:/usr/share/nginx/html:ro nginx

e) docker port some-nginx
home@home-3000-N100:~$ docker port some-nginx
80/tcp -> 0.0.0.0:32774

f) Go to the browser
403 Forbidden!!

g) Problem Point: Step d)
- We should give the absolute path or the correct path
h) How to discover the problem:
- inspect the local files under “some”:
home@home-3000-N100:~$ ls -lrt some/
total 4
-rw-rw-r-- 1 home home 612 Apr 27 19:09 index.html

i) Check the same files in the nginx container

home@home-3000-N100:~$ docker exec -it some-nginx bash

root@77aed83bdcef:/usr/share/nginx/html# ls -lrt /usr/share/nginx/html
total 4
drwxr-xr-x 2 root root 4096 Apr 27 12:53 content

root@77aed83bdcef:/usr/share/nginx/html# ls /usr/share/nginx/html/content/

So we can’t find our some/index.html because while create the container
we specified a wrong mount path.

Correct command:
docker run -d -P --name some-nginx -v /home/home/some:/usr/share/nginx/html:ro nginx
docker run -d -P --name some-nginx -v some:/usr/share/nginx/html:ro nginx




