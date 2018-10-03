# Docker Fundamentals

## Docker help

Is docker installed?

```powershell
PS> docker version
```

```text
Client:
 Version:      18.03.1-ce
 API version:  1.37
 Go version:   go1.9.5
 Git commit:   9ee9f40
 Built:        Thu Apr 26 07:12:48 2018
 OS/Arch:      windows/amd64
 Experimental: false
 Orchestrator: swarm

Server:
 Engine:
  Version:      18.03.1-ce
  API version:  1.37 (minimum version 1.12)
  Go version:   go1.9.5
  Git commit:   9ee9f40
  Built:        Thu Apr 26 07:22:38 2018
  OS/Arch:      linux/amd64
  Experimental: true
```

```powershell
PS> docker help
```

```text
Usage:  docker COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default
                           "C:\\Users\\david.mcfadzean\\.docker")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level
                           ("debug"|"info"|"warn"|"error"|"fatal")
                           (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default
                           "C:\\Users\\david.mcfadzean\\.docker\\ca.pem")
      --tlscert string     Path to TLS certificate file (default
                           "C:\\Users\\david.mcfadzean\\.docker\\cert.pem")
      --tlskey string      Path to TLS key file (default
                           "C:\\Users\\david.mcfadzean\\.docker\\key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  checkpoint  Manage checkpoints
  config      Manage Docker configs
  container   Manage containers
  image       Manage images
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  secret      Manage Docker secrets
  service     Manage services
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  deploy      Deploy a new stack or update an existing stack
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```

----

## Working with containers

Run a centos (linux) container with a command:

```powershell
PS> docker run centos:7 echo "hello world"
```

Did Docker download the centos image? Run it again, was it faster to execute this time?

----

Run the centos container again with a different command:

```powershell
PS> docker run centos:7 ps -ef
```

Notice only one process is running in the container (as opposed to a VM).

----

List all running containers:

```powershell
PS> docker container ls
```

Where are our containers? They must not be running.

----

List all containers with the `-a` option:

```powershell
PS> docker container ls -a
```

The containers stopped because the commands exited immediately:

```text
CONTAINER ID        IMAGE               COMMAND                CREATED              STATUS                          PORTS               NAMES
722616d58a0e        centos:7            "ps -ef"               About a minute ago   Exited (0) About a minute ago                       pensive_wing
b43a65c8b668        centos:7            "echo 'hello world'"   18 minutes ago       Exited (0) 18 minutes ago                           eloquent_einstein
```

----

To specify a container ID you only need to use enough characters to make the id unique. The inspect command provides more than you want to know in JSON format:

```powershell
docker inspect 722
```

```json
[
    {
        "Id": "722616d58a0e3f4896598f98c91427eccc3216e19b926e2597f34dbb30556829",
        "Created": "2018-07-19T13:02:22.6044313Z",
        "Path": "ps",
        "Args": [
            "-ef"
        ],
        "State": {
            "Status": "exited",
            "Running": false,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 0,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2018-07-19T13:02:23.55181Z",
            "FinishedAt": "2018-07-19T13:02:23.6258591Z"
        },
        "Image": "sha256:49f7960eb7e4cb46f1a02c1f8174c6fac07ebf1eb6d8deffbcb5c695f1c9edd5",
        "ResolvConfPath": "/var/lib/docker/containers/722616d58a0e3f4896598f98c91427eccc3216e19b926e2597f34dbb30556829/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/722616d58a0e3f4896598f98c91427eccc3216e19b926e2597f34dbb30556829/hostname",
        "HostsPath": "/var/lib/docker/containers/722616d58a0e3f4896598f98c91427eccc3216e19b926e2597f34dbb30556829/hosts",
        "LogPath": "/var/lib/docker/containers/722616d58a0e3f4896598f98c91427eccc3216e19b926e2597f34dbb30556829/722616d58a0e3f4896598f98c91427eccc3216e19b926e2597f34dbb30556829-json.log",
        "Name": "/pensive_wing",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "shareable",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                42,
                120
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/1544ac4b2408ff421d108707b3ef3c3aebf8f8156eb3ee539692c87e88d96c73-init/diff:/var/lib/docker/overlay2/7bba5573af0fb6d2c323d0edd6a320d7a50084b757ba2eb05cdcbbda47fdcbc8/diff",
                "MergedDir": "/var/lib/docker/overlay2/1544ac4b2408ff421d108707b3ef3c3aebf8f8156eb3ee539692c87e88d96c73/merged",
                "UpperDir": "/var/lib/docker/overlay2/1544ac4b2408ff421d108707b3ef3c3aebf8f8156eb3ee539692c87e88d96c73/diff",
                "WorkDir": "/var/lib/docker/overlay2/1544ac4b2408ff421d108707b3ef3c3aebf8f8156eb3ee539692c87e88d96c73/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "722616d58a0e",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "ps",
                "-ef"
            ],
            "Image": "centos:7",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.schema-version": "= 1.0     org.label-schema.name=CentOS Base Image     org.label-schema.vendor=CentOS     org.label-schema.license=GPLv2     org.label-schema.build-date=20180531"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "400ebe9b9558cfb63fd6c4bdf28bb24fb4c4c5601edf7791adc96789af7bd840",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/400ebe9b9558",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "",
            "Gateway": "",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "",
            "IPPrefixLen": 0,
            "IPv6Gateway": "",
            "MacAddress": "",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "d962935a30191715b80dc1c959d9f29f4357917bb9ce36ed7771f78b624808db",
                    "EndpointID": "",
                    "Gateway": "",
                    "IPAddress": "",
                    "IPPrefixLen": 0,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

----

## Interactive Containers

Run an ubuntu container and connect to a bash shell in interactive TTY (`-it`) mode:

```powershell
PS> docker run -it ubuntu bash
```

----

Explore the container as root:

```bash
ls -l
whoami
cat /etc/lsb-release
cd
pwd
echo "hello world" > foo.txt
cat foo.txt
exit
```

----

Check what is running in our ubuntu container:

```powershell
PS> docker exec [id] ps -ef
```

That didn't work, the container is stopped. We'll have to start it again:

```powershell
PS> docker start [id] 
PS> docker exec [id] ps -ef
```

Now we can reconnect:

```powershell
PS> docker exec -it [id] bash
```

```bash
ps faux
cd
cat foo.txt
exit
```

Notice our file is still there.

----

## Logging

Start a new centos container in the background using the `-d` (daemon) option:

```powershell
PS> docker run -d centos:7 ping 8.8.8.8
```

Note that the command outputs the full hash ID for the container.
If you don't capture it you can always see it in the container list.

----

Display the container's output using the logs command:

```powershell
PS> docker logs [id]
```

----

Display a running log using the `-f` option to follow (like `tail -f`):

```powershell
PS> docker logs -f [id]
```

----

Use `ctrl-c` to exit.

Use the `-t` option to add a timestamp and the `--tail N` option to show only the last N entries:

```powershell
PS> docker logs -t --tail 25 [id]
```

----

## Removing containers

Before we proceed let's clean up our containers. Get a list of all:

```powershell
PS> docker container ls -a
```

```text
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                         PORTS               NAMES
7dab6a4485ec        ubuntu              "bash"                 28 minutes ago      Up 11 minutes                                      lucid_leavitt
b41958650142        ubuntu              "bash"                 35 minutes ago      Up 33 minutes                                      nervous_clarke
19934dc497b3        ubuntu              "bash"                 37 minutes ago      Exited (0) 33 minutes ago                          jovial_hypatia
722616d58a0e        centos:7            "ps -ef"               About an hour ago   Exited (0) About an hour ago                       pensive_wing
b43a65c8b668        centos:7            "echo 'hello world'"   About an hour ago   Exited (0) About an hour ago                       eloquent_einstein
```

We can remove a container by name:

```powershell
PS> docker rm pensive_wing
```

...or by ID:

```powershell
PS> docker rm b43
```

----

But we can't remove a running container...

```powershell
PS> docker rm 7d
```

```text
Error response from daemon: You cannot remove a running container 7dab6a4485ec83a988090b2b069ea70e7b0b07dfedc19352bea341657f80034d. Stop the container before attempting removal or force remove
```

unless we force it:

```powershell
PS> docker rm -f 7d
```

----

A handy trick is to list all containers by ID:

```powershell
PS> docker container ls -aq
```

and pass the output of that to a remove-all command:

```powershell
PS> docker rm -f $(docker container ls -aq)
```

----

## Working with images

List the images you have installed:

```powershell
PS> docker image ls
```

----

Visit the [Docker Store](https://store.docker.com/) to explore available images.
Search for ubuntu, then pull the ubuntu 14.04 image with the 'trusty' label:

```powershell
PS> docker image pull ubuntu:trusty
```

----

Give the trusty image an alias with the `tag` command:

```powershell
PS> docker image tag ubuntu:trusty mytrusty
PS> docker image ls
```

```
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
ubuntu                     trusty              971bb384a50a        2 days ago          188MB
mytrusty                   latest              971bb384a50a        2 days ago          188MB
ubuntu                     latest              74f8760a2a8b        2 days ago          82.4MB
```

Notice that `ubuntu:trusty` and `mytrusty` have the same image ID which is different from `ubuntu:latest`.
Also notice that mytrusty was automatically assigned tag `latest` because it was not specified.

----

Test out the new image with a disposable (`--rm`) container:

```powershell
PS> docker run --rm -it mytrusty
```

```text
root@e092d412378e:/# cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=14.04
DISTRIB_CODENAME=trusty
DISTRIB_DESCRIPTION="Ubuntu 14.04.5 LTS"
root@e092d412378e:/# exit
```

Notice that it leaves no container behind after it has exited.

----

Remove the tag:

```powershell
PS> docker rmi mytrusty
PS> docker image ls
```

Notice removing the tag does not remove the image.

----

Remove the image:

```powershell
PS> docker rmi ubuntu:trusty
PS> docker image ls
```

----

## Creating images with Dockerfiles

Create a new folder called `myimage` and a text file called `Dockerfile` which includes the following instructions:

```text
FROM ubuntu:bionic

RUN apt-get update -y
RUN apt-get install -y wget
```

Create your image with the `build` command:

```powershell
PS> docker image build -t myimage .
PS> docker image ls
```

Verify your new image is in the list. How big is it (size in MB)?
Connect to your image interactively and verify that the `wget` command works as expected:

```powershell
PS> docker run --rm -it myimage
```

```bash
wget www.google.com
cat index.html
exit
```

----

1. Open `Dockerfile` and add another `RUN` step and the end to install vim.
2. Build the image again as above. Which steps are skipped because they come from the cache?
3. Build it again and notice which steps are used from the cache.
4. Swap the order of the two `RUN` commands for installing `wget` and `vim` and build the image again. Which steps are cached this time?

----

Use the `history` command to view the build cache history of your image:

```powershell
PS> docker image history myimage
```

Combine the two `RUN` commands into one, rebuild the image. How has the history changed?

----

Add the following lines to the end of your Dockerfile:

```text
RUN apt-get install -y inetutils-ping

CMD ["ping", "127.0.0.1", "-c", "5"]
```

This sets ping as the default command to run in a container created from this image, and also sets some parameters for that command.
Rebuild the image and run it with no command provided:

```powershell
PS> docker image build -t myimage .
PS> docker run --rm myimage
```

The container should automatically exit after pinging localhost 5 times.
Run it again, but this time override the default command:

```powershell
PS> docker run --rm myimage echo "hey"
```

----

Replace the `CMD` in the Dockerfile with an `ENTRYPOINT`:

```text
ENTRYPOINT ["ping"]
```

Rebuild and run with no command:

```powershell
PS> docker run --rm myimage
ping: missing host operand
Try 'ping --help' or 'ping --usage' for more information.
```

What went wrong? 
Run it again, but this time specify an argument for the ping command:

```powershell
PS> docker run --rm myimage localhost
PS> docker run --rm myimage google.ca
```

----

If `ENTRYPOINT` and `CMD` are both specified in the Dockerfile then the `CMD` will provide default arguments to the `ENTRYPOINT`.
Replace the end of your Dockerfile with the following two lines:

```text
ENTRYPOINT ["ping", "-c", "3"]
CMD ["localhost"]
```

Rebuild your image and run it with and without a parameter specified:

```powershell
PS> docker run --rm myimage
PS> docker run --rm myimage google.ca
```

----

## Multi-stage Builds

Multi-stage builds are used to reduce the size of images by removing intermediary steps that are not needed for the final build.
Make a new folder called `multi-stage` and cd into it.
Add a file `hello.c` containing the canonical Hello World program in C:

```C
#include <stdio.h>

int main (void)
{
    printf ("Hello, world!\n");
    return 0;
}
```

Add a Dockerfile with these commands:

```text
FROM alpine:3.5
RUN apk update && \
    apk add --update alpine-sdk
RUN mkdir /app
WORKDIR /app
COPY hello.c /app
RUN mkdir bin
RUN gcc -Wall hello.c -o bin/hello
CMD /app/bin/hello
```

This time we are using the alpine linux distribution which is designed for minimal size.
Build the image, test it, and investigate its size:

```powershell
PS> docker image build -t hello-large .
PS> docker run --rm hello-large
Hello, world!
PS> docker image ls
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
hello-large                latest              2322d664d6f9        10 seconds ago      189MB
```

----

Now modify it into a multi-stage build by replacing the Dockerfile with one that uses the `AS` clause to split the build into two steps:

```text
FROM alpine:3.5 AS build
RUN apk update && \
    apk add --update alpine-sdk
RUN mkdir /app
WORKDIR /app
COPY hello.c /app
RUN mkdir bin
RUN gcc -Wall hello.c -o bin/hello

FROM alpine:3.5
COPY --from=build /app/bin/hello /app/hello
CMD /app/hello
```

Again, build the image, test it, and investigate its size:

```powershell
PS> docker image build -t hello-small .
PS> docker run --rm hello-small
Hello, world!
PS> docker image ls
REPOSITORY                 TAG                 IMAGE ID            CREATED             SIZE
hello-small                latest              ee5283e8b208        18 seconds ago      4.01MB
hello-large                latest              2322d664d6f9        8 minutes ago       189MB
```

Compare the histories of the two images:

```text
PS> docker history hello-large
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
2322d664d6f9        24 minutes ago      /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/app…   0B
56fad7f7c982        24 minutes ago      /bin/sh -c gcc -Wall hello.c -o bin/hello       10.6kB
8d96a2ef31cf        24 minutes ago      /bin/sh -c mkdir bin                            0B
5819233ab494        24 minutes ago      /bin/sh -c #(nop) COPY file:4e66806a1e4d5445…   93B
e071642f0953        24 minutes ago      /bin/sh -c #(nop) WORKDIR /app                  0B
bad39bea5cc4        24 minutes ago      /bin/sh -c mkdir /app                           0B
76d1230b6800        24 minutes ago      /bin/sh -c apk update &&     apk add --updat…   185MB
a2b04ae28915        2 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>           2 weeks ago         /bin/sh -c #(nop) ADD file:fbb7c24296423cb0b…   4MB

PS> docker history hello-small
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
ee5283e8b208        16 minutes ago      /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/app…   0B
45ed0d897ba2        16 minutes ago      /bin/sh -c #(nop) COPY file:e1bb5e4622dbf3b5…   10.6kB
a2b04ae28915        2 weeks ago         /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
<missing>           2 weeks ago         /bin/sh -c #(nop) ADD file:fbb7c24296423cb0b…   4MB
```

----

## Managing images

For this exercise `[docker id]` refers to the ID you created at [Docker Hub](https://hub.docker.com/).

Use the `login` command:

```powershell
PS> docker login
Username: [docker id]
Password: [your password]
Login Succeeded
```

----

Download the latest `alpine` image from the [Docker Store](https://hub.docker.com/_/alpine/):

```powershell
PS> docker image pull alpine
```

Make a new tag for this image:


```powershell
PS> docker image tag alpine my-alpine:dev
PS> docker image ls
```

Push the image to Docker Hub:

```powershell
PS> docker image push my-alpine:dev
```

You should get an access denied error.
Retag your image to namespace it properly and try again:


```powershell
PS> docker image tag my-alpine:dev [docker id]/my-alpine:dev
PS> docker image push [docker id]/my-alpine:dev
```

Navigate to `https://hub.docker.com/r/[docker id]/my-alpine/` and verify that your image was uploaded with the proper tag.

----

## Private images (optional)

1. Go to the image repository settings and make it private.
2. Pair up with someone else and try to pull their private image.
3. Add each other as collaborators and try again.

----

# Examples

The examples are included in repo for this workshop.
Clone the git repo:

```powershell
PS> git clone https://github.com/macterra/docker-workshop.git
```

## MkDocs Container

[MkDocs](https://www.mkdocs.org/) is a static site generator that's geared toward project documentation.
Documentation source files are written in Markdown, and configured with a single [YAML](http://yaml.org/) configuration file.
If we follow some conventions it should be possible to write project documentation that is compatible with both MkDocs and BitBucket.

In this example we will containerize MkDocs so that developers don't have to worry about installing MkDocs and its dependencies (namely python).

In the cloned docker repo, change directory to `mkdocs` and examine the Dockerfile containing these commands:

```text
FROM python:3.4-alpine

EXPOSE 8000

RUN pip install --upgrade pip
RUN pip install mkdocs

WORKDIR /docroot
RUN mkdocs new .

CMD ["mkdocs", "serve", "-a", "0.0.0.0:8000"]
```

The `EXPOSE` command is there mostly as documentation, to tell the user of the image what port to map in order to access the server.

The `WORKDIR` command will create the directory and make it the current directory for the following commands.
We run `mkdocs new .` in the working directory to generate a default set of documentation files to use in case the container is run without mapping a volume (useful for testing the container).

Finally we run the command `mkdocs serve -a 0.0.0.0:8000` which tells mkdocs to run its server on port 8000 and listen on all available IP addresses for incoming requests.


Build the image and call it "mkdocs":

```powershell
PS> docker image build -t mkdocs .
```

----

## Networking

If we run the container we can see that mkdocs launches properly in the logs:

```text
PS> docker run --rm -it mkdocs
INFO    -  Building documentation...
INFO    -  Cleaning site directory
[I 180723 12:42:32 server:292] Serving on http://0.0.0.0:8000
[I 180723 12:42:32 handlers:59] Start watching changes
[I 180723 12:42:32 handlers:61] Start detecting changes
```

However in order to access the server from outside the container we must map the container port to a port on localhost.
This is done with the `-p` port mapping option.

Run the container again, mapping its internal port 8000 to localhost port 8888:

```powershell
PS> docker run --rm -it -p 8888:8000 mkdocs
```

Now you should be able to navigate to `http://localhost:8888/` and view the default mkdocs site.

----

## Volumes

The default site in the mkdocs container is useful for testing the container but we want the container to build and publish documentation for projects outside the container.

To do this we use volume mapping to inject a local directory into the container:

```powershell
PS> docker run --rm -it -p 8888:8000 -v c:\src\docker\browser:/docroot mkdocs
```

This replaces the `/docroot` folder in the container (the one initialized with the MkDocs default site in the Dockerfile) with a folder containing documentation copied from the [TBD]() project.

----

## docker-compose

Compose is a tool for defining and running multi-container Docker applications.
With Compose, you use a YAML file to configure your application’s services.
Then, with a single command, you create and start all the services from your configuration.

Change directory to `composetest` and examine the two containers defined in the docker-compose.yml file:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: "redis:alpine"
```

Notice the web container is built from the Dockerfile in the current directory:

```text
FROM python:3.4-alpine
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

The Dockerfile installs the python redis and flask packages using pip, then runs the app defined in app.py:

```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return '<h1>composetest view counter</h1>I have been seen {} times.\n'.format(count)

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```

Notice the python app connects to the redis host that is running in the other container.
Start the app in the background:

```powershell
PS> cd docker\composetest
PS> docker-compose up -d
PS> Start-Process http://localhost:5000
PS> docker-compose logs -f
```

Reload the page a few times to see the app in action.
Shutdown the app:

```powershell
PS> docker-compose down
```

----
