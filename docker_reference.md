**Accessing a running container**
`docker exec -it <container_id> /bin/bash`
or
`docker exec -it <container_name> /bin/bash`

**Start a container with volume and destroy when exited**
`docker run --rm -it -p 8080:8080 -v $(pwd):/home/app/src mepler/client1 /bin/bash`

**Networking: Expose, -p/-P, and 'link'**
[When to use what](https://www.ctl.io/developers/blog/post/docker-networking-rules/)
