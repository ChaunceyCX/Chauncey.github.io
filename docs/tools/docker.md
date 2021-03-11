# docker 常用应用启动脚本

## N1 --> openwrt --> docker

- docker-home: ``vim /etc/docker/daemon.json -> /opt/docker/``

- ``ddns-go``: docker run -d --name ddns-go --restart=always --net=host -v /opt/ddns-go:/root jeessy/ddns-go -l :9876 -f 600

- ``portainer``：docker run --restart=always -d --name portainer -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /mnt/mmcblk1p3/portainer/data:/data -v /mnt/mmcblk1p3/portainer/public:/public portainer/portainer
