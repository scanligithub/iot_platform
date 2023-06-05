nanopi core  install docker and docker-compose
1、修改成国内  源
    文件  /etc/apt/sources.list.d/nodesource.list 内容：

deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe
deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe
deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe

2、sudo apt-get update
3、sudo apt-get install docker.io
4、安装 docker-compose 
      将 https://github.com/docker/compose 的合适的发布版下载到
      /usr/local/bin/  目录中，将docker-compose-linux-armv7 改名为docker-compose  并赋予可执行权限。

df -hl

# ChirpStack Docker example

docker run hello-world
docker login
docker tag hello-world:latest scanlidocker/loraserver-app:v0.21
docker push scanlidocker/loraserver-app:v0.21

systemctl restart docker

docker tag scanlidocker/chirpstack-application-server chirpstack-application-server

docker tag scanlidocker/chirpstack-network-server:v0.21 chirpstack-network-server

docker tag scanlidocker/chirpstack-gateway-bridge:v0.21 chirpstack-gateway-bridge
df -hl
apt install docker.io -y

docker run -d -p 1880:1880 -p 5000:5000/udp -p 6000:6000/udp -p 1700:1700/udp --restart=always -v node_red_data:/data --name lora_nodered nodered/node-red
docker run -d -p 1880:1880 -p 2000:2000/udp -p 3000:3000/udp --restart=always -v node_red_data:/data --name lora_nodered nodered/node-red

docker run -d -p 1880:1880 --restart=always -v node_red_data:/data --name lora_nodered nodered/node-red
docker run -d -p 9000:9000  --restart=always  -v /var/run/docker.sock:/var/run/docker.sock  --name lora_portainer  scanlidocker/portainer:v0.1
docker run -d -p 9000:9000  --restart=always  -v /var/run/docker.sock:/var/run/docker.sock  --name portainer  portainer/portainer

dd if=/dev/zero of=/dev/mmcblk2p2 bs=1024 seek=8 count=1

nmcli connection modify 'Wired connection 1' connection.autoconnect yes ipv4.method manual ipv4.address 192.168.14.14/24 ipv4.gateway 192.168.14.1 ipv4.dns 114.114.114.114






This repository contains a skeleton to setup the [ChirpStack](https://www.chirpstack.io)
open-source LoRaWAN Network Server stack using [Docker Compose](https://docs.docker.com/compose/).

**Note:** Please use this `docker-compose.yml` file as a starting point for testing
but keep in mind that for production usage it might need modifications. 

## Directory layout

* `docker-compose.yml`: the docker-compose file containing the services
* `docker-compose-env.yml`: alternate docker-compose file using environment variables, can be run with the docker-compose `-f` flag
* `configuration/chirpstack*`: directory containing the ChirpStack configuration files, see:
    * https://www.chirpstack.io/gateway-bridge/install/config/
    * https://www.chirpstack.io/network-server/install/config/
    * https://www.chirpstack.io/application-server/install/config/
    * https://www.chirpstack.io/geolocation-server/install/config/
* `configuration/postgresql/initdb/`: directory containing PostgreSQL initialization scripts

## Configuration

The ChirpStack stack components components are pre-configured to work with the provided
`docker-compose.yml` file and defaults to the EU868 LoRaWAN band. Please refer
to the `configuration/chirpstack-network-server/examples` directory for more configuration
examples.

# Data persistence

PostgreSQL and Redis data is persisted in Docker volumes, see the `docker-compose.yml`
`volumes` definition.

## Requirements

Before using this `docker-compose.yml` file, make sure you have [Docker](https://www.docker.com/community-edition)
installed.

## Usage

To start the ChirpStack open-source LoRaWAN Network Server stack, simply run:

```bash
$ docker-compose up
```

**Note:** during the startup of services, it is normal to see the following errors:

* ping database error, will retry in 2s: dial tcp 172.20.0.4:5432: connect: connection refused
* ping database error, will retry in 2s: pq: the database system is starting up


After all the components have been initialized and started, you should be able
to open http://localhost:8080/ in your browser.

### Add Network Server

When adding the Network Server in the ChirpStack Application Server web-interface
(see [Network Servers](https://www.chirpstack.io/application-server/use/network-servers/)),
you must enter `chirpstack-network-server:8000` as the Network Server `hostname:IP`.
