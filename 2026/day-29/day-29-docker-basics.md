# Day 29 – Docker Basics

## What is Docker
Docker is a container platform used to package applications with their dependencies.

## Containers vs Virtual Machines
Containers share host OS while VMs run full OS.

## Docker Architecture
Docker Client → Docker Daemon → Images → Containers → Registry

## Commands Practiced

docker run hello-world

docker run -d -p 8080:80 nginx

docker run -it ubuntu

docker ps

docker ps -a

docker stop <container>

docker rm <container>

docker logs <container>

docker exec -it <container> bash
