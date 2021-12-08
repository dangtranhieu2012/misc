This folder contains the Dockerfile for building a Yocto build environment which include possibility of running Docker
inside Docker (through the host's Docker socket).

To build the Docker image, run:

```
docker build --no-cache --build-arg "host_gid=$(id -g)" --build-arg "host_uid=$(id -u)" --build-arg "host_docker_gid=$(cut -d: -f3 < <(getent group docker))" --build-arg 'git_username="Your Name Here"' --build-arg 'git_email=your_email_address@here.com' -t yocto-build-env:latest .
```

Once built, the Docker image can be entered and used with:

```
docker run --privileged -v /dev:/dev -v /var/run/docker.sock:/var/run/docker.sock -v /path/to/yocto:/path/to/yocto -it yocto-build-env:latest /bin/bash
```

Feel free to ignore ```--privileged -v /dev:/dev``` if you don't need to have access to device nodes (or want to
finetune access to the device nodes on the host), this is useful for when you want to prepare use things like loopback
mount for example.

Also feel free to remove the bind mount of Docker socket if you don't need to use Docker inside the Yocto build
environment container.

Note that due to Yocto extensive use of links with absolute paths, the bind mount path needs to be the same inside the
Docker container as on the host (hence /path/to/yocto:/path/to/yocto).
