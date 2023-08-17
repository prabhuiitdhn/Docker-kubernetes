Tutorial link:

https://www.youtube.com/watch?v=Wf2eSG3owoA&ab_channel=freeCodeCamp.org

Docker is for packaging the build-content in a image[container.] & later this container will ne running somewhere which will orcheraste somewhere is known as Kubernetes.
Kubernetes will orcherests the multiple container which will talk to each other.
It helps/maintain to communicate between all the container using Kubernetes.

Docker is a tool for running applications in an issolated environemt. SImilar to virtual machine and but it is faster and does not required more memory for ruuning it.
it is standard for software deployment, Used for packaging applications.

conatainer: It is an abstraction at the app layer that packages the code and dependencies together. Multiple container can run on the same machien and share the OS kernel with other container, each running isolated appication
Virtual machine: It includes full copy of OS and required so much of memory than docker.
Uisng docker, It can run container in seconds instaed of miniutes, less resources and uses less memory and not reuired full operation system
testing can be done locally.

command:Open terminal
docker
docker --version
docker ps //It will work when docker demon will run or docker starts

docker images: snapsshot/template of creating env of your choice.
It contains os, software and app code.
conatner is running instance of an image


docker pull docker-image-name
docker pull nginx
docker images//check if the images is being downloaded.

run container for this images:
docker run nginx:latest //latest is the tag

check what are the conatiner ruuning on nginx
docker container ls
output:
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS     NAMES
c277c15a3761   nginx:latest   "/docker-entrypoint.…"   31 seconds ago   Up 30 seconds   80/tcp    magical_beaver

docker run -d nginx:latest//it will run conatiner without interruption//detach mode
docker ps //check container rumnning
docker stop containerID //it will stop the container running in docker.

docker run -d -p 8080:80 nginx:latest //start & run the docker in port 8080 with detach mode
[manage to map docker port 8080 to container port 80]
It can also map to other port
docker run -d -p 3000:80 nginx:latest
docker run -d -p 3000:80 -p 8080:80 nginx:latest //mapping to more than one port
docker stop docker-conatiner-name//it can also stopped ruuninng container.
docker start docker-conatiner-name # can start again the container
docker ps --help //can show what we do want to see about docker


docker ps -a //show all the cinatiner.
docker rm conatinerID/name # it will stope the conatienr.

docker rm -aq //shows all conatiner running
docker rm $(docker ps -aq) # this will remove all conatiner running

# naming container
docker run --name test_name_for_container -d -p 3000:80 -p 8080:80 nginx:latest
docker start test_name_for_container //can start this conatiner
docker stop test_name_for_container //can stop the container.

# if we want to format docker naming
format="ID\t{{.ID}}\nNAME\t{{.Names}}\nIMAGE\t{{.Image}}\nPORTS\t{{.Ports}}\nCOMMAND\t{{.Command}}\nCREATED\t{{.CreatedAt}}\nSTATUS\t{{.Status}}\n"
export format
docer ps --format=format

# docker volume
it is used for sharing the data, files and folder
It is used for sharing data between hots and container or between containers
need to create a volume which can used for sharing the data with conatiner and host to conatiner.

docker run --name some-nginx -v /some/source/content:/usr/share/destination/nginx/html:ro -d nginx
docker run --name some-nginx-name -v C:\Users\uib43225\PycharmProjects\DSAlgo\PracticeMLOps\Docker-Kubernetes\index.html:/usr/share/nginx/html:ro -d -p 8080:80 nginx
docker exec -it some-nginx-name bash //It will allow to work on this container to work on
docker exect -it container_id bash //It will redirect to conatienr terminal
 Now can check the file inside terminal: ls or cat file_name-inside_there
 we can also add any number of file related to website inside the conatiner terminal and all and have a wesite running on conatiner/local host

# Sharing files/data between containers
docker run --name website -d -p 8081:80 nginx # one container running
docker run --name website-copy -d -p 8080:80 nginx # another container running

Output: # running containers
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                  NAMES
8172cb0bb1b9   nginx     "/docker-entrypoint.…"   58 seconds ago       Up 58 seconds       0.0.0.0:8081->80/tcp   website
ef4e2abbe702   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp   website-copy

# build own images using dockerfile
we can create own docker-image using dockerfile which can run the container.
dockerfile: will conatins list of steps for creating a docker images

https://docs.docker.com/engine/reference/builder/#from
docker file: Inside root folder,
FROM baseimage//it is used for creating image from base image
add . //will add all the file
add file_name//used for adding required file
run pip install numpy //it will install numpy inside the container.

docker build --tag conatiner_name:latest whereisdockerfile
docker build --tag nginx:latest C:\Users\uib43225\PycharmProjects\DSAlgo\PracticeMLOps\Docker-Kubernetes\
# after building this, we can check how many docker are there/created
docker image ls

    REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
    nginx        latest    c7f2d364a9ea   About a minute ago   187MB
    nginx        <none>    af62dd757b58   11 hours ago         187MB

docker run --name new_image -p 8081:80 -d nginx:latest # THis is for running latest docker image which is being vcreated now

# dockerignore file
It helps to exclude/ignore the file which is not needed to work with docker, or in the docker image

# Caching and layers in docker
we can take advantages of caching when we have to create dockerfile again and again for the same with slight modification
so, basically arrange the command which is changing frequently and remaining will cache
needed to check while writing the dockerfile, which is needed to be arranged.

# improve image size of the images
using aplinelinux distribution, size can be reduced.
use the tage of alpineVersion of base image, which will have smaller size of creating docker image
docker pull nginx:alpine3.18-perl //It reduces the size by 187MB to 78MB

use alpine version to build the image: chaneg the base image in dockerfile, choose base image of dockerimage is alpine version of
    FROM nginx:alpine3.18-perl
    add . /usr/share/nginx/html

# importants of tags, version and tagging
-allows to control the image version
-avoids breaking changes
change docker file for tags and versioning.
    FROM nginx:1.17.2-alpine
    add . /usr/share/nginx/html

# tagging own images while creating
docker tag from_tag_image to To_imageVersion
docker tag ngx:latest ngx:version_1

# Pushing container into container registry
    Docker registry:
    which used to store and lets us distribute the docker images in a server side application and ci/cd pipelines
    Also, it runs the applications using kubernetes, aws, googlecloud

    two types of docker registry
    1. private
    2. public:

    we do have docker registry
    1. docker Hub
    2. quay.io
    3. Amazon ECR
    In docker hub, create a new repository and use following command to push the local images in dockerhub
    for docker repository

    docker image ls //which will show all the images locally
    docker tag existing_image:version new_image:version
    ex: docker tag nginx:latest test_nginx:2.0

    create repository and having the same name of image local and push it to docker hub

    docker push prabhuiitdhn/test_repo:tagname
    [after this it can shared with anyone who wants to use]

    # pushing the repo we have sent, we can also see into the docker hub https://hub.docker.com/repository/docker/prabhuiitdhn/test_repo/general
    docker pull prabhuiitdhn/test-repo:latest

    It can be run after pulling it using below command.
    docker image ls # for cheking whether it is being clone properly or not?
    docker run --name test_repo_pulled -p 9000:80 -d  prabhuiitdhn/test-repo

# docker inspect
    it is for debugging the container. It will show all the required information about the container.
    docker inspect container_id or container_name

    output:[
    {
        "Id": "sha256:c7f2d364a9ea9ac064e1e3014237cd47ed2c596ac1cc77b023a3c254b5ee97b8",
        "RepoTags": [
            "nginx:latest",
            "ngx:1",
            "test_nginx:1",
            "test_repo:1"
        ],
        "RepoDigests": [],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2023-08-16T10:56:21.262257696Z",
        "Container": "",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.25.2",
                "NJS_VERSION=0.8.0",
                "PKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 186645222,
        "VirtualSize": 186645222,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/41bd9a1498419233360cabbf0cb8c68bfccfaf69de8322b6368ed431f807f6ad/diff:/var/lib/docker/overlay2/cdf5285a910af545d7eb8d23d2c3bcd2200d619b5ec24eb5ae722e1b993717e2/diff:/var/lib/docker/overlay2/2075cc69bb33ce83e1810d93b3efac5599a8760c7512b5abb048931bf24564d9/diff:/var/lib/docker/overlay2/6d6e72c623f211914b2e0c8560408a07a817873a57b55bd42360ced298d7c40c/diff:/var/lib/docker/overlay2/d1ff86b5cf31a9d80989be53f7ff758cd75cf6238eb6df3f7068fa4aeb68aaa9/diff:/var/lib/docker/overlay2/0ed054541398e4e9ecd45cbf40affb35ae000ec91100a8911d684071d91a6d32/diff:/var/lib/docker/overlay2/769202ba0d12df064509004a0f6d0a93bdce44d696a42e44a28ee5b5299c6aac/diff",
                "MergedDir": "/var/lib/docker/overlay2/4bqwoh0b59cd684m8cxxrlu5g/merged",
                "UpperDir": "/var/lib/docker/overlay2/4bqwoh0b59cd684m8cxxrlu5g/diff",
                "WorkDir": "/var/lib/docker/overlay2/4bqwoh0b59cd684m8cxxrlu5g/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:c6e34807c2d51444c41c15f4fda65847faa2f43c9b4b976a2f6f476eca7429ce",
                "sha256:e17417668755d2962593694352a319f7d6f83ff69372756f82714c06edc27d93",
                "sha256:d9979288f61897b93d85b8e7d4ed168163a9fb990b489cd1f047f98a42b83f2f",
                "sha256:5a2a0d1ab8feecef3ae829a3f93ad607bbaa92db1dae1b3789bb50c3ad5dec26",
                "sha256:103ee56c0db53973efe374c6da88a4a414e71270ecb96c43f4aec681166b494e",
                "sha256:021a192bcc442a3c9e89a20500dc21eac2a4a84c2ea4edd86caac27cd6ce08c3",
                "sha256:68e722008ce0dffc886c2a47ae3b984c7b66a5386e0772f86f3209e6031c419d",
                "sha256:4d0fede4b64cfe704231bb00d5041590ea46d25138057d573853130cfe57f72a"
            ]
        },
        "Metadata": {
            "LastTagTime": "2023-08-16T12:49:09.019493057Z"
        }
    }
]

# Docker logs
 docker logs container_id
 docker logs -f container_id //-f is used for following the logs
 docker logs --help

# docker execute
 docker exec --help // it is used for debugging the execution.








