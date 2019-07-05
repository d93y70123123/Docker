# Docker
#### Docker是一種容器的虛擬化技術，一般會使用虛擬機是因為需要不同的環境作業或是需要測試用的環境，而容器就好像是為了這個而生出來的，所以簡單來認識一下差異：　　

|    功能    |     Docker    |         VM         |
|:---------:|:--------------:|:------------------:|
|  啟動速度  |   **非常快**   |         慢         |
|  容量大小  |   **非常小**   |         大         |
|   效能     |  **接近原生**  |        慢          |
| 開啟數量   |   **非常多**   | 一般來說10個就很多了 |  
|建置相同環境|   **非常快**    |        慢          |  

## 簡單介紹一下Docker的概念  
Docker可以分成Image、Container、Registry：  

Image    ：建立容器的基底，映像檔是唯獨的!!!  
Container：所有操作都在裏頭執行  
Registry ：類似Github，只不過是用來儲存映像檔的  

## 安裝Docker-ce
ce與ee的差別，免費使用板與企業版的差別吧!!  
這邊安裝的是CE喔  
```bash
wget https://download.docker.com/linux/centos/docker-ce.repo -P /etc/yum.repos.d/
yum install docker-ce docker-ce-cli containerd.io
systemctl start docker
systemctl enable docker
```

## 常用指令，這邊我想分成針對Image還有Container的
## [Image]  
search  
```
看到 [STARS] 那個欄位，這代表的是使用最多的映像檔，選擇第一個下載
# docker search centos    *加上 [ -f is-official=true ] 就可以指定官方版的映像檔了
NAME                               DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
centos                             The official build of CentOS.                   5434                [OK]
ansible/centos7-ansible            Ansible on Centos7                              121                                     [OK]
jdeathe/centos-ssh                 CentOS-6 6.10 x86_64 / CentOS-7 7.6.1810 x86…   110                                     [OK]
consol/centos-xfce-vnc             Centos container with "headless" VNC session…   91                                      [OK]
centos/mysql-57-centos7            MySQL 5.7 SQL database server                   57
imagine10255/centos6-lnmp-php56    centos6-lnmp-php56                              57                                      [OK]
tutum/centos                       Simple CentOS docker image with SSH access      44
centos/postgresql-96-centos7       PostgreSQL is an advanced Object-Relational …   37
kinogmt/centos-ssh                 CentOS with SSH                                 27                                      [OK]
......
......

```

pull：要抓image可以從直接docker hub上抓下來  
```
# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
8ba884070f61: Pull complete
Digest: sha256:a799dd8a2ded4a83484bbae769d97655392b3f86533ceb7dd96bbac929809f3c
Status: Downloaded newer image for centos:latest

# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
centos              latest              9f38484d220f        3 months ago        202MB
```

create：建立容器但不啟動  
```
建立
# docker create -it --name centos7-1 centos
234999445b69ef107f4ffcf8f10bac9de1070d1a2f0a722b0051d92230381cd4

列出正在執行的容器，這邊要注意的是STATUS是Created，是剛建立而不是啟動中喔!!!
# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                  PORTS               NAMES
234999445b69        centos              "/bin/bash"         3 seconds ago       Created                                     centos7-1
```

run：可以直接建立且執行容器，通常這個比較常用  
```
使用run之後會直接進入容器喔!!若要幫容器開port就必須要在建立的時候就增加。
# docker run -it -p 80:8080 --name centos7-2 centos
[root@7c5ca9df4fd1 /]#

# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS                  NAMES
7c5ca9df4fd1        centos              "/bin/bash"         About a minute ago   Up About a minute   0.0.0.0:80->8080/tcp   centos7-2
234999445b69        centos              "/bin/bash"         26 minutes ago       Up 26 minutes                              centos7-1
```

build  
* 使用build前，必須先移動到該dockerfile下  

rmi  
* 刪除映像檔前，必須把容器先殺光  
```
# docker rmi centos:latest
Untagged: centos:latest
Untagged: centos@sha256:b5e66c4651870a1ad435cd75922fe2cb943c9e973a9673822d1414824a1d0475
Deleted: sha256:9f38484d220fa527b1fb19747638497179500a1bed8bf0498eb788229229e6e1
Deleted: sha256:d69483a6face4499acb974449d1303591fcbb5cdce5420f36f8a6607bda11854
```
push  

## [Container]  
start：啟動容器 
```
# docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                   PORTS               NAMES
8d5712257957        centos              "/bin/bash"         7 seconds ago       Created                                      centos7-1

# docker start centos7-1
centos7-1

# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
8d5712257957        centos              "/bin/bash"         58 seconds ago      Up 2 seconds                            centos7-1

```

stop：停止啟動中的容器  
```
# docker stop centos7-1
centos7-1
```

exec：在容器中執行指令  
```
# docker exec centos7-1 ls
anaconda-post.log
bin
dev
etc
...
....

# docker exec -it centos7-1 ls
anaconda-post.log  dev  home  lib64  mnt  proc  run   srv  tmp  var
bin                etc  lib   media  opt  root  sbin  sys  usr


## docker exec -it centos7-1 bash
[root@8d5712257957 /]#

[root@8d5712257957 /]# exit
exit
[root@localhost ~]#
```
rm：刪除容器  
```
*刪除前記得先停止容器喔!!!
# docker stop testimg
testimg

# docker rm testimg
testimg
```

commit：整合容器裡的資料到映像檔  
```
# docker commit centos7-1 testimg
sha256:d1bfe959c714391e8e4818204ca3c15a110e10d127a0c7379ffe71c7264b4f40

# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
testimg             latest              d1bfe959c714        4 seconds ago       202MB
centos              latest              9f38484d220f        3 months ago        202MB
hello-world         latest              fce289e99eb9        6 months ago        1.84kB
```
logs：訊息將繼續從容器的STDOUT和STDERR流式傳輸新輸出，簡單來說若有程式執行就會輸出出來。  
```
# docker logs -f --until=2s centos7-1
[root@8d5712257957 /]#
( 要離開的話 Ctrl+c 就可以離開了 )
```
inspect：查看容器的結構資訊  
```
# docker inspect centos7-1
[
    {
        "Id": "8d5712257957661db6f63e78543eedebc59a945ebf57c960635d62bd774f8840",
        "Created": "2019-07-05T12:53:28.895040109Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
....
....

```
* 若要離開通常都會想到exit，但容器必須要敲打鍵盤的[ctrl]+[p] & [ctrl]+[q]，才能離開



先把覺得有用的網址記錄下來  
* Docker-toturial：https://github.com/twtrubiks/docker-tutorial  
* Docker-基本教學：https://ithelp.ithome.com.tw/articles/10199339?sc=rss.qu  
* Run & exec：https://www.jinnsblog.com/2018/10/docker-container-command.html  
