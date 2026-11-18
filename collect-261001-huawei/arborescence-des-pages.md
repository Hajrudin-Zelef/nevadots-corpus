# Summary

**Sub-menu:** `/container`

Packages requied: `container`

A container is MikroTik's implementation of Linux containers, allowing users to run containerized environments within RouterOS. The container feature works in the latest MikroTik RouterOS v7.x version. Containers are compatible with images from Docker Hub, GCR, Quay, or other providers, as well as those built on other devices, using the same formats supported by these providers. While RouterOS uses different syntax compared to Docker, it still achieves similar functionality.

# Disclaimer

you need physical access to your RouterOS device to enable support for the container feature, it is disabled by default;

- once the container feature is enabled, containers can be added/configured/started/stopped/removed remotely!
- if your RouterOS device is compromised, containers can be used to easily install malicious software in your RouterOS device and over network;
- your RouterOS device is as secure as anything you run in container;
- if you run container, there is no security guarantee of any kind;
- running a 3rd party container image on your RouterOS device could open a security hole/attack vector/attack surface;
- an expert with knowledge how to build exploits will be able to jailbreak/elevate to root;

## **Security risks:**

- When a security expert publishes his exploit research - anyone can apply such an exploit;
- Someone can build a container image that can use the exploit AND provide a Linux root shell;
- By using a root shell someone may leave a permanent backdoor/vulnerability in your RouterOS system even after the container image is removed and the container feature disabled;
- If a vulnerability is injected into the primary or secondary RouterBOOT (or vendor pre-loader), then even Netinstall may not be able to fix it;

# Requirements

Container package is compatible with **arm, arm64** and **x86** architectures. Using of remote-image (similar to docker pull) functionality requires a lot of free space in main memory, 16MB SPI flash boards may use pre-build images on USB or other disk media.

- An external disk supporting at least 100MB/s sequential read/write speed and 10K random iops recommended. When using slower disks, container extraction times may become longer.
- Container package needs to be installed
- For devices with EN7562CT CPU like the hEX Refresh, only arm32v5 container images are supported, meaning a limited number of containers can be run.

# Properties

| Property | Description | 
|---|---|
| **auto-restart-interval**  (string; Default: ) | Specify an interval at which Container will be restarted on Container failure. Example: 10s | 
| **cmd** (string; Default: ) | The main purpose of a CMD is to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an ENTRYPOINT instruction as well. | 
| **comment** (*string* ; Default: ) | Short description | 
| **dns** (string; Default: ) | If container needs different DNS, it can be configured here | 
| **domain-name** (string; Default: ) |  | 
| **entrypoint (** string; Default:**)** | An ENTRYPOINT allows to specify executable to run when starting container. Example: /bin/sh | 
| **envlist** (*string* ; Default: ) | list of environmental variables (configured under */container envs* ) to be used with container | 
| **file** (*string; Default:* ) | container *tar.gz tarball if the container is imported from a file | 
| **hostname** (*string; Default:* ) | Assigning a hostname to a container helps in identifying and managing the container more easily | 
| **interface** (*string; Default:* ) | veth interface to be used with the container | 
| **logging** (*string; Default:* ) | if set to yes, all container-generated output will be shown in the RouterOS log | 
| **start-on-boot** (*string; Default:* ) | if set to yes, the container will be started automatically on device start-up. | 
| **mountlists** (*string; Default:* ) | mounts from /container/mounts/ sub-menu to be used with this container | 
| **mount** (*string; Default:* ) | specify directory to be used as a mount | 
| **remote-image** (*string; Default:* ) | the container image name to be installed if an external registry is used (configured under /container/config set registry-url=...) | 
| **root-dir** (*string; Default:* ) | used to save container store outside main memory | 
| **stop-signal** (*string; Default: 15* ) | Type of Linux signal to send when container was not stopped after 10 seconds | 
| **workdir** (*string* ; Default: ) | the working directory for cmd entrypoint | 
| **devices**  (*string* ; Default: ) | passes through physical device to the container | 
| **cpu-list**  (*string* ; Default: ) | specifies which CPU cores the container is allowed to run on | 
| **user**  (*string* ; Default: ) | sets the user and group the container process runs as before execution. | 
| **memory-high**  (*int* ; Default: ) | RAM usage limit in bytes for a specific container | 
| **memory-max** (*int* ; Default: ) | max RAM usage limit in bytes per container (The container process will be terminated if the memory-max value is smaller than the container memory-current.) | 

**Menu specific commands**

| Property | Description | 
|---|---|
| **update** | Updates the container image. Automatically pulls from container repository and extracts it, replacing the original image. | 
| **kill** | Kills the specified running container. | 
| **repull** | Re-pulls/extracts the container image. | 
| **shell**  | Enters the container shell of a running container. | 
| **run** | Starts the container and enters it's shell. Useful if container shuts down after running. | 

# Container configuration

| Property | Description | 
|---|---|
| **registry-url** | external registry url from where the container will be downloaded (default : https://lscr.io/ ) | 
| **tmpdir** | container extraction directory | 
| **memory-high** | RAM usage limit in bytes 1 - unlimited | 
| **username** | Specifies the username for authentication ( starting from ROS 7.8) | 
| **password** *sensitive* | Specifies the password for authentication ( starting from ROS 7.8) | 

# Examples

## Running Pi-hole

### Prerequisites

  1. RouterOS device with RouterOS v7.4beta or later and **installed Container package -** How to install packages
  2. Physical access to a device to enable container mode - will be explained down bellow
  3. Attached HDD, SSD or USB drive for storage - formatted with a filesystem supported by RouterOS - How to format/manage disks
2. RouterOS device with RouterOS v7.4beta or later and 

### Steps to run Pi-hole

1. Enable Container mode and follow the instructions the command gives you (read more about Device-mode). You will need to confirm the device-mode with a press of the reset button, or a cold reboot (if using Containers on x86):
2. Create a new veth interface and assign an IP address in a range that is unique in your network:The following configuration is equivalent to "bridge" networking mode in other Container engines such as Docker. It is possible to create a "host" equivalent configuration as well.
3. Create a new bridge that is going to be used for your Containers and assign the same IP address that was used for the veth interface's gateway:
4. Add the veth interface to your newly created bridge:
5. Create a NAT for outgoing traffic:
6. Create environment variables for the Container:
7. Create mounted volumes for the Container:`src=` points to RouterOS location (could also be`src=disk1/etc_pihole` if, for example, You decide to put configuration files on external USB media),`dst=` points to defined location (consult containers manual/wiki/github for information on where to point). If`src` directory does not exist on first time use then it will be populated with whatever container have in`dst` location.It is highly recommended to place any Container volume on an attached disk to your RouterOS device. Avoid placing Container volumes on the built-in storage.
8. Configure to use a specific Container repository, for example, to use Docker.io:
9. Add a Containter:If You wish to see container output in `/log print` , then add`logging=yes` when creating a Container, root-dir should point to an external drive. It's not recommended to use internal storage for Containers.Adding a Containter will start downloading or extracting it, the Container itself will not be started after it has been added, you need to start it manually for the first time after it has been downloaded/extracted.
10. Check the status of your Container and wait until downloading/extracting has been finished and the `status=stopped` :
11. Start the Containter:
12. Create a port forwarding for your Container:
13. You should be able to access the Pi-hole web panel by navigating to `http://192.168.88.1/admin/` in your web browser.
14. To start using Pi-hole on your devices, change their DNS configuration to use `192.168.88.1` as your DNS server.

## Adding a Container image

There are multiple ways you can get a Container image running on your RouterOS device. Check the examples below.

### Option A: Get an image from an external library

Set registry-url (for downloading containers from Docker registry) and set extract directory (tmpdir) to attached USB media:

pull image:

The image will be automatically pulled and extracted to root-dir, status can be checked by using

### Option B: Import image from PC

Your can use your PC running either Docker or Podman to download your required container image and save it to an archive. We recommend using Podman since it is easier to build and download containers for specific architectures using Podman.

1. Download your required image based on the architecture of your RouterOS device:
2. Save the container image to an archive:
3. Upload the archive to your RouterOS device, for example:You can also use Winbox to upload files!
4. Create a Container on your RouterOS device using the uploaded container image archive file:

### Option C: Build an image on PC

You can build your own Containers and use them on your RouterOS device. While you can build Containers using Docker, we recommend using Podman since it is easier to build Containers for a specific architecture using Podman.

1. Get source files for your required Container image, for example by using git:
2. Build the Container image by specifying the Dockerfile or Containerfile and the target archiceture:
3. Save the container image to an archive:
4. Upload the archive to your RouterOS device, for example:You can also use Winbox to upload files!
5. Create a Container on your RouterOS device using the uploaded container image archive file:

#### Alternative: Using Docker to build Container images

To use Dockerfile and make your own docker package - docker needs to be installed as well as buildx or other builder toolkit.

Easiest way is to download and install Docker Engine:

https://docs.docker.com/engine/install/

After install check if extra architectures are available:

should return:

If not - install extra architectures:

pull or create your project with Dockerfile included and build, extract image (adjust --platform if needed):

Upload *pihole.tar* to Your RouterOS device.

Images and objects on the Linux system can be `pruned` 

Create a container from the tar image

## Networking examples

### Bridge with NAT

In this networking setup, all Containers use the same veth interface and communicate with each other without any Firewall restrictions, but you need to forward ports in order to allow access to a Container's port.

For example, a database Container needs to communicate with a web application Container, the web application needs the port `80` to be exposed to the world, but the database Container does not need any ports to be exposed to the world.

- The network configuration:
- The database Container configuration:
- The webapp Container configuration:

In this example, the `pgadmin` port 80 is accessible to everyone, but the `postgres` port 5432 is not accessible to everyone, it can only be accessed through either `pgadmin` as `127.0.0.1` or through the RouterOS device running the Containter as `172.17.0.2` .

### Isolated Containers

In this networking setup, you have multiple Containers and you want to make sure that some of them can communicate without Firewall restrictions, but some need to be isolated from other Containers. For example, you might want to create two database Containers and isolate them.

- The network configuration:
- The first and second database Container configuration:
- The first and second webapp Container configuration:

In this example, `pgadmin1` is able to reach `postgres1` , but is not able to reach `postgres2` . Similarly `pgadmin2` is able to reach `postgres2` , but is not able to reach `postgres1` . 

### Container in Layer2 network

In this networking setup, your Container is directly attached to a Layer2 network with other physical network devices. This networking setup is equivalent to "host" networking mode on other Container engines such as Docker.

In this networking setup, all the ports on your Container are exposed. This is considered insecure, but does slightly improve the Container's networking performance.

- The networking configuration:
- In case your RouterOS device has services running on the same port, you need to disable them:
- The webapp configuration:

In this example, `pgadmin` Container does not need port forwarding, but all other ports that the Container is using are now accessible to others on the same Layer2 network. This type of setup should only be used when your application requires that the Container has an IP address in the same Layer2 network such as application that use broadcast traffic for service discovery (in most cases such requirements can still be bypassed by using NAT).

## IPv4 and IPv6 for Container

In this networking setup your Container will be able to communicate over IPv4 and IPv6. The solution is based on Bridge with NAT networking setup.

- The network configuration:
- The webapp Container configuration:

# Tips and tricks

- Containers use up a lot of disk space, USB/SATA, NVMe attached media is highly recommended. For devices with USB ports - USB to SATA adapters can be used with 2.5" drives - for extra storage and faster file operations.
- RAM usage can be limited by using:

this will soft limit RAM usage - if a RAM usage goes over the high boundary, the processes of the cgroup are throttled and put under heavy reclaim pressure.

- For starting containers after router reboot use start-on-boot option (starting from 7.6beta6)
- It is possible to get to **running** container shell:
- It's possible to set the container timeout in seconds, otherwise it will stay open indefinitely:
- Enable logging to get output from container:
- Some containers will require additional privileges in order to be able to run properly:
- You can execute commands inside a Container with a specific user and without invoking /bin/sh:
- Starting from 7.11beta5 version multiple addresses and ipv6 addresses can be added:
- Active running /dev/ nodes to container: