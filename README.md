# fastdfs-storage
基于[ygqygq2/fastdfs-nginx](https://hub.docker.com/r/ygqygq2/fastdfs-nginx/)，修复部分bug，并将其拆分成fastdfs-storage和fastdfs-tracker两个镜像，此篇文章是介绍fastdfs-storage。

### 特点
- 为storage节点整合了nginx；
- 将nginx的端口号作为环境变量，创建容器时指定；
- 支持多个group，并将group作为环境变量，创建容器时指定，且保证不同group下storage节点的nginx能够正常访问。

### 环境变量
- TRACKER_SERVER：tracker的ip及port，格式为ip:port（**必填**）；
- NGINX_PORT：nginx的端口号，默认为8080；
- PORT：storage节点的端口号，默认为23000；
- GROUP_NAME：所属group，如：group2，group3...若没有传该参数，默认为group1；

### storage.conf中的重要路径
- base_path：日志、元信息的存储路径，默认为/var/fdfs，需要将该目录挂载出来；
- store_path0：文件存储路径，store_path_count默认等于1，即只有一个存储文件的目录，默认和base_path相同，需要将该目录挂载出来；

### Demo
```
docker run -dti --network=host --name storage0 -e TRACKER_SERVER=ip:port -e NGINX_PORT=18888 -v $PWD/storage0:/var/fdfs zippenwang/fastdfs-storage
docker run -dti --network=host --name storage1 -e TRACKER_SERVER=ip:port -e NGINX_PORT=18889 -e PORT=23001 -e GROUP_NAME=group2 -v $PWD/storage1:/var/fdfs zippenwang/fastdfs-storage
```

### 注意点
- 此容器只适用于host网络，即--network=host，和宿主机共用一套网络配置；
- 一台宿主机中允许运行多个storage容器（端口号必须不同），但要求这些storage节点必须隶属于不同的group，也就是说，同一台宿主机下的同一个group中，不能出现多个storage节点；
