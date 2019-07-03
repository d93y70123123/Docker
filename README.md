# Docker
##　Docker是一種容器的虛擬化技術，一般會使用虛擬機是因為需要不同的環境作業或是需要測試用的環境，而容器就好像是為了這個而生出來的，所以簡單來認識一下差異：　　

|    功能    |     Docker    |         VM         |
|:---------:|:--------------:|:------------------:|
|  啟動速度  |   **非常快**   |         慢         |
|  容量大小  |   **非常小**   |         大         |
|   效能     |  **接近原生**  |        慢          |
| 開啟數量   |   **非常多**   | 一般來說10個就很多了 |  
|建置相同環境|   **非常快**    |        慢          |  

## 簡單介紹一下Docker的概念，Docker可以分成Image、Container、Registry：  
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
[Image]  
create|run  
build  
search  
pull  
rmi  
push  

[Container]  
run  
start  
exec  
commit  
* 若要離開通常都會想到exit，但容器必須要敲打鍵盤的[ctrl]+[p] & [ctrl]+[q]，才能離開



先把覺得有用的網址記錄下來  
* Docker-toturial：https://github.com/twtrubiks/docker-tutorial  
* Docker-基本教學：https://ithelp.ithome.com.tw/articles/10199339?sc=rss.qu  
* Run & exec：https://www.jinnsblog.com/2018/10/docker-container-command.html  
