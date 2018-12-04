# fastdfs-storage
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
