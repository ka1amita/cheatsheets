Docker
======

**_Docker_** is an open **platform** for

+ **developing**
+ **shipping**
+ **running**

_applications_. It enables you to **separate** your **application** from your **infrastructure**.

_Docker_ provides the ability to **package** and **run** an _application_
in a **loosely isolated** _environment_ called _Docker_ **_container_**.

The **_isolation_** and **_security_** lets you run **many** _containers_ **simultaneously**
**on** a **single _host_**.

_Containers_ are **lightweight _workspaces_** and **contain everything** needed to **run** the
_application_, like:

+ **code**
+ **binaries**
+ **scripts**
+ **runtime**
+ **dependencies**
+ any other **file system objects**

so you **don't** need to **rely** on what's **installed** on the **host**.

You can **share** _containers_ while you work,
and be sure that **everyone** you share with gets the same container that **works** in the **same
way**.

_Containers_ are great for _continuous integration_ and _continuous delivery_ (_CI/CD_) workflows.
Your developers **write code** locally and **share** their work
with their colleagues and **push** their applications into a **test** environment
using _containers_.

_Containers_ allow to dynamically **manage workloads**, **scaling up** or **tearing down**
_applications_ and _services_ in near **real time.**

## Definitions

### Docker Architecture

_Docker_ uses a **_client-server_** architecture.
The **_Docker_ _client_** sends requests to the **_Docker_ _daemon_**, which **does** the heavy
lifting of
**building**, **running**, and **distributing** the _containers_.
they can **be** on the **same system** or the _Docker client_ can connect to a **remote** _deamon_.
They **communicate** using a _REST API_, over _UNIX sockets_ or _network_ _interface_.

Another _Docker client_ is a _**Docker Compose**_, that allows you work with application consisting
of a set of containers.

#### Docker Daemon

The docker daemon `dockerd` **listens** for _Docker API_ **requests**
and **manages** _Docker_ **objects** like _images_, _containers_, _networks_ and _volumes_.

#### Docker Client

Is a primary **way** that many users **interact with _Docker_**. When you **use** a _command_
the client **sends** that _command_ to `dockerd` which **carries** them **out**.
The `docker` command uses the Docker API. The _Docker_ client can communicate with more than one
_daemon_.

#### Docker Desktop

Is an GUI application. It includes the

+ _Docker daemon_ (`dockerd`)
+ _Docker client_ (`docker`)
+ _Docker Compose_
+ _Kubernetes_
+ and more

#### Docker Registries

A **_Docker registry_** **stores** _Docker_ **_images_**.
_Docker Hub_ is a **public** _registry_ where _Docker_ looks for images **by default**.
E.g. when you use `docker pull`, `docker run` or `docker push`.
You can run your own private registry.

#### Docker Build Context

_Docker build_ **_context_** is the **set of files located** in the specified _PATH_ or _URL_.

_Docker build_ process can **access** any of the files **located** in this _context_.

#### Docker objects

##### Images

An **_image_** is a **read-only template** with **instructions** for **creating** _containers_.
_Images_ are **based** on **other** _images_ with some additional **customization**.
To build your own _image_ you can create a **_Dockerfile_**
that **defines** the steps needed to **create** the _image_ and **run** it.

Each **instruction** in a _Dockerfile_ **creates** a **layer** in the _image_. When you change and
rebuild the image,
**only** those **layers** which have **changed** are **rebuild**.
This is part of what makes _images_ so **lightweight**,**small**, and **fast**.

##### Images Best Practices

###### Caching of Dependencies

Once a _layer_ **changes**, **all downstream** _layers_ have to be **recreated** as well.

> Restructure your Dockerfile to help support the caching of the dependencies.

e.g.:

+ support the **caching** of **_dependencies_** by installing them in a separate layer from the
  application code
  (recreate them only when there was a change to the `package.json`).

###### Multi-stage Builds

advantages:

+ **separate** **build-time _dependencies_** from **runtime _dependencies_**
+ **reduce** overall _image_ **size** by **shipping** only what your app **needs to run**

Use a previous stage as a new stage:

```dockerfile
FROM alpine:latest AS builder
# ...
FROM builder AS build1
```

`COPY` from _images_ in _Docker_ registry:

```dockerfile
COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
```

###### .dockerignore

Use `.dockerignore` file to **exclude files** or **directories** from the `build` _context_

This helps to avoid unwanted files or directories to the builder **improving build speed**.

`.dockerignore` is a **_newline_-separated _list_** of file **_glob_ patterns**[^glob]

[.dockerignore](https://docs.docker.com/build/building/context/#dockerignore-files)

```dockerignore
*.md
!README*.md
README-secret.md
```

[^glob]: glob is a filename-matching pattern, https://en.wikipedia.org/wiki/Glob_(programming)

###### Multiple Dockerfiles

Use multiple Dockerfiles with different .dockerignore files by using a naming convention:

+ `<Dcokerfile name>.dockerignore` located in the same directory as the `Dockerfile`

```shell
─ docker
  ├── build.Dockerfile
  ├── build.Dockerfile.dockerignore
```

Dockerfile-specific `.dockerignore` take **precedence** over the .dockerignore file in the _root_ of
the _build context_

###### Multi-platform images

_Docker_ _images_ can **support multiple _platforms_**,
which means that a **single _image_** may **contain variants** for **different _architectures_**,
and sometimes for **different _operating systems_**.

When you run an _image_ with **multi-platform** support, _Docker_ **automatically** selects the
image that **matches** your _OS_ and _architecture_.

[Multi-platform images](https://docs.docker.com/build/building/multi-platform/)

##### Dockerfile

A **_Dockerfile_** is simply a **text-based** file with
no file extension that contains a **script** of instructions.
_Docker_ uses this script to **build** a container **image**.

##### Containers

_Docker_ **_Container_** is a **runnable** **_instance_** of an _image_.

A _container_ is a **sandboxed** **process** running on a _host_ machine
that is **isolated** from **all** other **processes** running on that _host_ machine.
That isolation leverages _Linux kernel_ **_namespaces_** and **_cgroups_**.

To summarize, a container:

+ _Docker_ **_Container_** is a **runnable** **_instance_** of an _image_.
  You can create, start, stop, move, or delete a container using the _Docker API_ or _CLI_.
+ Can **run on** **local machines**, **virtual machines**, or deployed to the **cloud**.
+ Is **portable** (and can be run on **any _OS_**).
+ Is **isolated** from other _containers_
  and runs its **own** _software_, _binaries_, _configurations_, etc.
+ You can **connect** a container to one or more networks or attach storage to it.

A running _container_ uses an isolated **_filesystem_**.
This isolated _filesystem_ is **provided by** an **_image_**,
and the _image_ must **contain** everything needed to **run** an application:

+ all _dependencies_
+ _configurations_
+ _scripts_
+ _binaries_

The _image_ also contains other **_configurations_** for the container, such as:

+ _environment variables_
+ a default _command_ to run
+ and other _metadata_

A _container_ is a relatively well **isolated** from **other** **_containers_**
and its **host** machine.
You can control how isolated a container's network, storage,
or other underlying subsystems are from other containers or from the host machine.

A _container_ is **defined** by its **_image_** as well as
any **configuration** options you **provide** to it when you **create** or **start** it.
When a container is **removed**,
any **changes** to its state that aren't stored in persistent storage **disappear**.

##### The Underlying Technology

_Docker_ is **written** in the **_Go_** programming language and
takes **advantage** of several features of the **_Linux_ _kernel_** to deliver its functionality.
Docker **uses** a **technology** called **_namespaces_**
to **provide** the loosely **isolated workspace** called the **_container_**.
When you **run** a _container_ (_image_), _Docker_ **creates** a **set of _namespaces_** **for
that _container_.**

These _namespaces_ **provide** a layer of **isolation**.
**Each aspect** of a _container_ runs in a **separate** _namespace_ and its access is **limited** to
that _namespace_ only.

##### Container orchestration

Well, what happens if the **containers die**?
Tools like _Kubernetes_, _Swarm_, _Nomad_, and _ECS_ all help solve this problem, all in slightly
different ways.

## Docker CLI Syntax

* `image`
  * `ls`
  * `tag` \<image-id> <\tag>
  * `history` \<image-id>
  * `inspect` \<image> returns details on the container image, the commands it runs, the OS and more
* `container`
  * `ls` or `ls -a`
  * `diff` \<container-id>

* `pull`
* `build` `--tag` or `-t` \<name>:\<tag> \<location>
* `start` \<container-id>
* `exec` execute command inside a container
  * MYSQL-CONTAINER-ID COMMAND
* `run`or `run` \<image> \<command>
  * `--detach` or `-d`
  * `--publish` or `-p`\<[host-ip:]host-port:docker-port>

### Snippets

#### Docker

+ any command
  + `-h` or `--help`

+ `build` **builds** _image_ from a _build context_ based on the `Dockerfile`
  + e.g. `docker build .` don't forget the location, e.g. `.`!
  + `-t` `--tag`
  + `--target <last-stage-build>` build only up to `last-stage-build`

+ `rmi <image ID>` removes image

+ `run`
  + `-d` or `--detach`
  + `-i` or `--interactive` keeps _STDIN_ open even if not attached
  + `-t`, `--tty` allocates a pseudo-_TTY_[^tty]


+ `-p` or `--publish <HOST:CONTAINER>` **creates** a **_firewall_ rule** in the host,
  **mapping** a **container port** to a **port** on the Docker **host** to the outside world.
  e.g. `127.0.0.1:3000:3000` **exposes** a container's ports to the **loopback interface**.
  Note the `EXPOSE` statement in the docker is not enough on its own!
+ `-p 0.0.0.0:3000:3000` **exposes** the container's _port_ on all interfaces of the _host_,
  making it **available** to the **outside world**.
+ `--mount type=volume,src=<volume-name>,target=</some/target>`
+ `--mount type=bind,src=</path/to/data>,target=</some/target>`
+ `-v` or `--volume </host/source>:</some/target>`
+ `-v` or `--volume <volume name>:</some/target>`

+ `ps` or `container ls`
+ `stop <the-container-id>`
+ `rm <the-container-id>`
  + `rm -f <the-container-id>`,
    e.g.`docker rm -f $(docker ps | head --lines 2 | tail --lines 1 | awk '{print $1}')`

+ `build --tag <username/name[:version]> .`
  + If you don't specify a tag version, _Docker_ uses a tag called `latest`
  + `--platform`, e.g. `--platform linux/amd64`
  + `buildx` also supports building multi-platform images.
+ `image tag <old name> <username/name[:version]>` while `latest` being the default version
+ `push getting-started <username/name>`

+ `login`
  + `login --username username --password password server`
  + `echo $DOCKER_HUB_ACCESS_TOKEN | docker login --username <username> --password-stdin docker.io`

+ `volume create <name>` but can be skipped because _Docker_ recognizes you want to use a named
  volume and creates one automatically
+ `volume inspect <name>`

+ `logs -f <container-id>`

[^tty]: https://stackoverflow.com/questions/30137135/confused-about-docker-t-option-to-allocate-a-pseudo-tty

#### Docker Compose

+ any command
  + `-h` or `--help`

+ `docker compose up`
  + `-d` or `--detach`
  + `-f` or `--follow` ?
  + `--build` **builds** _images_ before starting the _containers_

+ `docker compose down`
  + `--volumes` removes the volumes as well. By default, named volumes in your compose file are not
    removed.

+ `docker compose logs`
  + `-f` or `--follow`

## Dockerfile Syntax

[Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

* `FROM` image
* `WORKDIR`
  * `WORKDIR` /path/to/workdir
  > sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD
  * combination with `ADD`, `COPY` etc.:`ADD test.txt /absoluteDir/` or * `ADD test.txt relativeDir/`
* `COPY` source destination # relative to current position and container root
  * `COPY [--chown=<user>:<group>] [--chmod=<perms>] ["<src>",... "<dest>"]`
* `RUN` verb parameter etc
  > Allows you to **install** your **application** and **packages** required for it. It executes any command in top of current image and creates **new layer** by commiting the results.
* `CMD`
  * * `CMD ["stored","as","list"]`
  > It allows you set a **default command**, which will be **executed** only **when** you **run** a **container** without specifying a command. If container **runs with a command** then the default CMD command will be **ignored**.
* `ENTRYPOINT` ["verbs","and","parameters"]
  > Similar to CMD because it allows specify a command with parameters. Only difference is that when container runs ENTRYPOINT commands and parameters are **not ignored**.

### Snippets

## Persistence

### Volumes

**_Volume_ mounts** provide the ability to **connect** specific **_filesystem_ paths** of the
_container_ back **to** the **_host_ machine**.

### Binds

A _**Bind_ mount** lets you **share** a directory **from the host**'s filesystem into the
_container_.

You can use a _bind_ mount to mount **source code** into the _container_. Changes are **immediately
reflected** on both sides.

### Volume and Bind mounts Comparisons
The following table outlines the main differences between volume mounts and bind mounts.

 	Named volumes	Bind mounts

|                                               | Volume mounts                                    | Bind mounts                                        |
|-----------------------------------------------|--------------------------------------------------|----------------------------------------------------|
| Host location                                 | Docker chooses                                   | You decide                                         |
| Mount example (using --mount)                 | type=volume,src=my-volume,target=/usr/local/data | type=bind,src=/path/to/data,target=/usr/local/data |
| Populates new volume with container contents	 | Yes                                              | No                                                 |
| Supports Volume Drivers                       | Yes                                              | No                                                 |


1. Mount your source code into the container
1. Install all dependencies
1. Start nodemon to watch for filesystem changes

## Networking

with `--publish` or `-p` flag, e.g. `-p 8080:80/tcp -p 8080:80/udp`

If you want to **connect _container_** accessible to other containers, it **isn't necessary** to
`publish` the container's _ports_.
You can enable **inter-container communication** by **connecting** the _containers_ to the same
network,
usually a **_bridge_** **network**.
It **includes** **_DNS_** used when creating your connection string.

A _container_ has **no information** about what kind of **network** it's attached to, or whether
their **peers** are also _Docker_ workloads or not.

A _container_ only **sees** a _network_ interface with:

+ _IP address_
+ _gateway_
+ _routing table_
+ _DNS services_
+ _networking details_

Each _container_ **has** its **own _IP_** address.

+ **Assign** the _network_ **when starting** the _container_.
+ **Connect** an already **running** _container_ to a _network_.

```shell
docker run -d \
    --network todo-app --network-alias mysql \
    -v todo-mysql-data:/var/lib/mysql \
    -e MYSQL_ROOT_PASSWORD=secret \
    -e MYSQL_DATABASE=todos \
    mysql:8.0
```

```shell
docker run -dp 127.0.0.1:3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:18-alpine \
  sh -c "yarn install && yarn run dev"
```

### Multi Container apps

> Each **container** should **do one thing** and do it well.

The following are a few **reasons** to run the _container_ **separately**:

* There’s a good chance you’d have to **scale** _APIs_ and _front-ends_ **differently** than *
  *databases**.
* Separate _containers_ let you **version** and **update versions** in **isolation**.
* While you may use a container for the database locally, you may want to use a managed service for
  the database in production.
* Running **multiple processes** will require a **process manager** (the container only starts one process), which adds complexity to container startup/shutdown.

> If you place the two containers on the same network, they can talk to each other.

* Assign the network when starting the container.
* Connect an already running container to a network.

* How to Deploy a Docker container to Heroku?
  > Heroku is a popular platform with a great pipeline for deploying sites.

#### Docker Compose

_Docker_ **_Compose_** is a tool for **defining** , **sharing** and **running multi-container**
_Docker_ **applications**.

Defines the application services and stack in a `compose.yaml` **configuration file** at the **root
**
of the project **repository** (it is version controlled).

It has commands for managing the whole lifecycle of your application:

+ **`start`**, **`stop`**, **`rebuild`** services
+ **view status** of running services
+ **steam log** output of running services
+ run a **one-off command** on a service

### Connect a Debugger

[Connect a Debugger](https://docs.docker.com/language/java/develop/#connect-a-debugger)

### Secrets

[why-you-shouldnt-use-env-variables-for-secret-data](https://blog.diogomonica.com//2017/03/27/why-you-shouldnt-use-env-variables-for-secret-data/)

## Kubernetes

**_Kubernetes_** (/ˌk(j)uːbərˈnɛtɪs, -ˈneɪtɪs, -ˈneɪtiːz, -ˈnɛtiːz/, commonly abbreviated **K8s**)
is an open-source **container orchestration system** for **automating**
software **deployment**, **scaling**, and **management** of applications.

## Minikube

**_Minikube_** is **local** _Kubernetes_,
focusing on making it easy to **learn** and **develop** for _Kubernetes_.

## Troubleshooting

###### Credentials

Always be **login** to _Docker_ _registry_ (_Docker Hub_ by default) - e.g. for `docker pull` images

+ use `docker login` after setting `"credsStore": "osxkeychain"` in `~/.docker/config.json`
+ _Docker Desktop_ >`Sign in`
+ `docker login -u ka1amita --password $DOCKER_HUB_ACCESS_TOKEN docker.io`

+ check credentials by:
  1. `docker-credential-osxkeychain list` and then use the url from there like
  2. `echo "https://index.docker.io/v1/" | docker-credential-osxkeychain get`

## Material Review

* How can I create a new container image?
  > Manually installing software in a container and then committing.
  > Using Dockerfile.
  > An image is basically a diff.
* What is a parent image and how can I decide on the right one?
  > A parent image is the image that your image is based on. It refers to the contents of the FROM
  directive in the Dockerfile.
  > search on Docker Store / Docker Hub, as well as Docker Trusted Registries that you might run in
  your own environmen.
* How can I copy files to an image?
  > `COPY` keyword in Dockerfile, ...
* How can I run commands during the building process?
  > `RUN` in Dockerfile, ...
* How can I specify what commands should be ran when a container starts up?
  > `CMD` or `ENTRYPOINT` in Dockerfile, ...
* What does it mean that image files have a layered structure, and what consequences does it have?
  > Each layer is an image itself, just one without a human-assigned tag. They have auto-generated
  IDs though.
  > Each layer stores the changes compared to the image it’s based on.
  > Each instruction in a Dockerfile results in a layer. (Except for multi-stage builds, where
  usually only the layers in the final image are pushed, or when an image is squashed to a single
  layer).
  > Layers are used to avoid transferring redundant information and skip build steps which have not
  changed (according to the Docker cache).
* What is the difference between the RUN and CMD commands?
  > *see above* [Dockerfile syntax](#Dockerfile-syntax)
* What are Docker tags and what are they used for?
  > A tag points to a single Docker image.
* How can I run a container, in a way that it's immediately deleted after it's stopped running?
  > `docker run --rm IMAGE-NAME` [powercms.com](https://www.powercms.in/article/how-automatically-delete-docker-container-after-running-it)

**Images**

+ The **file system** and **configuration** of our application which are used to **create containers
  **.

**Containers**

+ Running **instances** of Docker **images** — containers run the actual applications. A container *
  *includes** an **application** and all of its **dependencies**. It **shares the kernel** with
  other containers, and runs as an **isolated process** in user space on the host OS.

**Container ID**

+ Instead of using the full container ID you can use just the first few characters, as long as they
  are enough to uniquely ID a container.

**Docker Daemon** - The background **service** running on the host that **manages** **building**, *
*running** and **distributing** Docker **containers**.

**Docker Client**

+ The **command line** tool that allows the user to interact with the Docker daemon.

**Docker Hub**

+ Store is, among other things, a registry of Docker images. You can think of the registry as a
  directory of all available Docker images.
+ To share Docker images, you have to use a Docker registry. The default registry is Docker Hub.

**Dockerfile**

+ A text file that contains all the instructions to build an image.

**Volumes**

+ A special Docker container **layer** that allows **data to persist** and be shared separately from
  the container itself. Think of volumes as a way to abstract and manage your persistent data
  separately from the application itself.

**Image Layers**

+ A Docker **image is built** up from a series of **layers**. Each layer represents an **instruction
  ** in the image’s **Dockerfile**. Each layer except the last one is **read-only**.

+ This is a powerful concept because it means that if we need to make a change to our application,
  it may only affect a single layer!

Using **Cache**

+ Docker recognized that we had already built some of these layers in our earlier image builds and
  since nothing had changed in those layers it could simply use a cached version of the layer,
  rather than pulling down code a second time and running those steps.
+ Docker is intelligent enough to build the container in the most efficient way possible, as opposed
  to repeatedly building an image from the ground up each and every time.
