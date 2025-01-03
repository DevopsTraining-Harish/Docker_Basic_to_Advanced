# Docker Volumes

## Problem Statement

It is a very common requirement to persist the data in a Docker container beyond the lifetime of the container. However, the file system
of a Docker container is deleted/removed when the container dies. 

# Why Docker Volumes?

Docker volumes are a way to persist data outside the container's lifecycle. When a container is deleted, its writable layer is also removed, which means any data not stored in volumes is lost. 
Volumes provide:

Data Persistence: Store data even when containers are removed.
Sharing Data: Share data between multiple containers.
Performance: Volumes can be optimized for performance compared to bind mounts.
Isolation: Volumes abstract storage management away from the host system.

# Types of Docker Volumes
Docker provides three main types of volumes:

# Anonymous Volumes

Automatically created by Docker when no specific volume name is provided.
Managed entirely by Docker.
# Named Volumes

Created explicitly by the user with a name.
Docker manages the storage location, but you control the name and lifecycle.
# Bind Mounts

Maps a specific host directory to a container directory.
Gives direct access to the host filesystem.
Requires absolute paths and doesn't integrate well with Docker's volume management.

# Differences Between Volume Types


# Feature	      AnonymousVolume	NamedVolume	BindMount
ManagedDocker	       Yes	          Yes	      No
  Data Sharing	       No (usually)	Yes	      Yes
  Flexibility	         Minimal	    High	    High
PathControl	    Docker-defined-path	Docker-defined-path	User-specified-absolute-path


Example: Using Volumes
Let’s create an example where a named volume is used to persist database data.

# Scenario
A MySQL container needs persistent storage for its data.

Steps
Create a Named Volume:

docker volume create my-db-data
Run a MySQL Container with the Volume:

docker run -d \
  --name my-mysql \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -v my-db-data:/var/lib/mysql \
  mysql:8.0
-v my-db-data:/var/lib/mysql: Maps the named volume my-db-data to the MySQL data directory inside the container.

Inspect the Volume: Check the volume details:

docker volume inspect my-db-data
Test Persistence:

Stop and remove the container:

docker rm -f my-mysql
Re-run the container with the same volume:

docker run -d \
  --name my-mysql \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -v my-db-data:/var/lib/mysql \
  mysql:8.0
Data will still be intact because it’s stored in the volume.
# Bind Mount Example
To use a bind mount instead:

docker run -d \
  --name my-nginx \
  -v /path/on/host:/usr/share/nginx/html \
  nginx:alpine
This mounts /path/on/host from the host system to /usr/share/nginx/html in the container, allowing you to directly modify files on the host and see changes in the container.

# Use Cases for Volumes
Database Containers: Persist database files for backups and consistency.
Web Applications: Share configuration files between multiple containers.
Logging: Save logs outside the container for analysis.
Volumes are critical for ensuring data durability and enabling complex multi-container applications. Let me know if you want a deeper dive!


## Solution

There are 2 different ways how docker solves this problem.

1. Volumes
2. Bind Directory on a host as a Mount
   

### Volumes 

Volumes aims to solve the same problem by providing a way to store data on the host file system, separate from the container's file system, 
so that the data can persist even if the container is deleted and recreated.

![image](https://user-images.githubusercontent.com/43399466/218018334-286d8949-d155-4d55-80bc-24827b02f9b1.png)


Volumes can be created and managed using the docker volume command. You can create a new volume using the following command:

```
docker volume create <volume_name>
```

Once a volume is created, you can mount it to a container using the -v or --mount option when running a docker run command. 

For example:

```
docker run -it -v <volume_name>:/data <image_name> /bin/bash
```

This command will mount the volume <volume_name> to the /data directory in the container. Any data written to the /data directory
inside the container will be persisted in the volume on the host file system.

### Bind Directory on a host as a Mount

Bind mounts also aims to solve the same problem but in a complete different way.

Using this way, user can mount a directory from the host file system into a container. Bind mounts have the same behavior as volumes, but
are specified using a host path instead of a volume name. 

For example, 

```
docker run -it -v <host_path>:<container_path> <image_name> /bin/bash
```

## Key Differences between Volumes and Bind Directory on a host as a Mount

Volumes are managed, created, mounted and deleted using the Docker API. However, Volumes are more flexible than bind mounts, as 
they can be managed and backed up separately from the host file system, and can be moved between containers and hosts.

In a nutshell, Bind Directory on a host as a Mount are appropriate for simple use cases where you need to mount a directory from the host file system into
a container, while volumes are better suited for more complex use cases where you need more control over the data being persisted
in the container.
