# ProjectView 설치

## 설치 경로 준비
임시 디렉토리와 설치 디렉토리를 생성한다.

```
projectview@projectview:~$ sudo mkdir -p /app/installer
projectview@projectview:~$ sudo mkdir -p /app/projectview
projectview@projectview:~$ sudo chown -R projectview:projectview /app

```


## 설치 파일 준비
임시 디렉토리에 설치 파일을 복사한다.

```
projectview@projectview:~/다운로드$ cd projectview
projectview@projectview:~/다운로드/projectview$ ls
projectview-installer-v2.0.5.tar
projectview@projectview:~/다운로드/projectview$ cp *.tar /app/installer
projectview@projectview:~/다운로드/projectview$ cd /app/installer
projectview@projectview:/app/installer$ ls
projectview-installer-v2.0.5.tar
```

## 설치 파일 해제
설치 파일 아카이브를 해제한다.

```
projectview@projectview:/app/installer$ ls
projectview-installer-v2.0.5.tar
projectview@projectview:/app/installer$ tar -xvf *.tar
./
./.git/
./.git/branches/
./.git/description
./.git/hooks/
./.git/hooks/applypatch-msg.sample
... 생략 ...
./data/database/postgresql.conf
./data/database/postmaster.opts
./docker-compose.yml
./install.sh
./projectview-v2.0.5.tgz

```


## 설정 파일 편집
설정 파일을 vi 편집기나 Visual Studio Code로 편집한다.

```
projectview@projectview:/app/installer$ ls
LICENSE                              common.sh                   data                projectview-installer-v2.0.5.tar
Projectview_Open_Source_License.doc  config.env                  docker-compose.yml  projectview-v2.0.5.tgz
README.md                            create_nginx_log_rotate.sh  install.sh
projectview@projectview:/app/installer$ vi config.env

```

파일 내용을 필요에 따라 현행화한다.

* PROJECTVIEW_VERSION: 설치 파일 버전
* INSTALL_TARGET_PATH: 설치 디렉토리

```
# ProjectView Version - Use for docker image version to load and docker compose
PROJECTVIEW_VERSION=v2.0.5

# Install target directory path
INSTALL_TARGET_PATH=/app/projectview

# Path to install postgresql database data files
DATABASE_DATA_PATH=${INSTALL_TARGET_PATH}/postgresql/data

# Path of attach files, logs
PROJECTVIEW_ATTACH_FILE_PATH=${INSTALL_TARGET_PATH}/uploadFiles
PROJECTVIEW_LOG_PATH=${INSTALL_TARGET_PATH}/logs

```

## 설치 스크립트 실행
설치 스크립트에 실행 권한을 부여한 후 실행한다.

```
projectview@projectview:/app/installer$ ls -al
total 8414684
drwxr-xr-x 4 projectview projectview       4096  4월 11 18:20 .
drwxr-xr-x 4 projectview projectview       4096  4월 11 18:08 ..
drwxr-xr-x 8 projectview projectview       4096  3월 27 16:13 .git
-rw-r--r-- 1 projectview projectview       1801  3월 27 16:13 LICENSE
-rw-r--r-- 1 projectview projectview    3200000  3월 27 16:13 Projectview_Open_Source_License.doc
-rw-r--r-- 1 projectview projectview         57  3월 27 16:13 README.md
-rw-r--r-- 1 projectview projectview       3373  3월 27 16:13 common.sh
-rw-r--r-- 1 projectview projectview        429  3월 27 16:13 config.env
-rw-r--r-- 1 projectview projectview        520  3월 27 16:13 create_nginx_log_rotate.sh
drwxr-xr-x 5 projectview projectview       4096  3월 27 16:13 data
-rw-r--r-- 1 projectview projectview       7191  3월 27 16:13 docker-compose.yml
-rw-r--r-- 1 projectview projectview       3181  3월 27 16:13 install.sh
-rwxr-xr-x 1 projectview projectview 4337397760  4월 11 17:57 projectview-installer-v2.0.5.tar
-rw-r--r-- 1 projectview projectview 4275975470  3월 27 16:21 projectview-v2.0.5.tgz
projectview@projectview:/app/installer$ chmod u+x install.sh
projectview@projectview:/app/installer$ ./install.sh 


--- ProjectView installation is started..----

[Step 1]: checking if docker is installed ...

Note: docker version: 26.0.0

[Step 2]: checking docker-compose is installed ...

Note: Docker Compose version v2.25.0

[Step 3]: loading ProjectView docker images ...
➜ Load projectview v2.0.4
open ./projectview-v2.0.4.tgz: no such file or directory
projectview@projectview:/app/installer$ vi config.env
projectview@projectview:/app/installer$ ./install.sh 

--- ProjectView installation is started..----

[Step 1]: checking if docker is installed ...

Note: docker version: 26.0.0

[Step 2]: checking docker-compose is installed ...

Note: Docker Compose version v2.25.0

[Step 3]: loading ProjectView docker images ...
... 생략 ...

Loaded image: projectview/base-frontend:rhel-8.9-1107.1705420509-nginx-1.25.3
a92c9b75e345: Loading layer [==================================================>]  3.072kB/3.072kB
76c48870636f: Loading layer [==================================================>]  80.59MB/80.59MB
3f4dda9d2290: Loading layer [==================================================>]  6.144kB/6.144kB
Loaded image: projectview/pms-server-frontend:v2.0.5


[Step 4]: preparing environment ...
➜ Install target path : /app/projectview
➜ Database install path : /app/projectview/postgresql/data
➜ Directory for database data is created. - /app/projectview/postgresql/data
➜ Copy initial database files.
➜ Directory for attach files is created. - /app/projectview/uploadFiles
➜ Directory for log files is created. - /app/projectview/logs
➜ Copy nginx conf files.
➜ Copy cert files.
➜ Copy service scripts.


Note: stopping existing ProjectView instance ...
Warning: No resource found to remove for project "projectview".


[Step 5]: starting ProjectView ...
WARN[0000] /app/installer/docker-compose.yml: `version` is obsolete 
[+] Running 11/12
 ⠧ Network projectview_promise-network  Created                                                                   212.7s 
 ✔ Container pms-redis                  Healthy                                                                    27.2s 
 ✔ Container pms-postgresql             Healthy                                                                    27.2s 
 ✔ Container pms-manual                 Healthy                                                                   181.8s 
 ✔ Container pms-server-backend         Healthy                                                                   139.2s 
 ✔ Container ph-server-backend          Healthy                                                                   171.1s 
 ✔ Container pms-multi-dashboard        Healthy                                                                   170.6s 
 ✔ Container pms-batch                  Healthy                                                                   187.9s 
 ✔ Container pms-mail                   Healthy                                                                   171.1s 
 ✔ Container ph-server-frontend         Healthy                                                                   186.5s 
 ✔ Container pms-server-frontend        Healthy                                                                   186.5s 
 ✔ Container pms-nginx                  Healthy                                                                   202.1s 
✔ --- ProjectView has been installed and started successfully.----

```
설치 완료 후 자동으로 실행된다.

## 설치 결과 확인
설치 디렉토리를 확인한다.

```
projectview@projectview:/app/projectview$ ls
backup-projectview.sh  conf        docker-compose.yml  postgresql               startup-projectview.sh
cert                   config.env  logs                shutdown-projectview.sh  uploadFiles

```

### 디렉토리
* cert : 도메인 인증서 디렉토리
* conf : 웹서버 설정 파일 디렉토리
* logs : 서비스 별 로그 파일 디렉토리
* postgresql : 데이터베이스 데이터 디렉토리
* uploadFiles : 첨부 파일 디렉토리

### 파일
* config.env : ProjectView 설정 파일
* docker-compose.yml : docker compose 설정 파일
* shutdown-projectview.sh : ProjectView 서비스 종료 스크립트 파일
* startup-projectview.sh : ProjectView 서비스 시작 스크립트 파일

## 실행 프로세스 확인
실행된 docker 프로세스를 확인한다.
```
projectview@projectview:/app/projectview$ docker ps
CONTAINER ID   IMAGE                                                             COMMAND                   CREATED             STATUS                       PORTS                                                                                  NAMES
8966615b8065   projectview/base-frontend:rhel-8.9-1107.1705420509-nginx-1.25.3   "/usr/local/nginx/sb…"   About an hour ago   Up 59 minutes (healthy)      0.0.0.0:443->443/tcp, :::443->443/tcp                                                  pms-nginx
bddee6b15225   projectview/ph-server-frontend:v2.0.5                             "/usr/local/nginx/sb…"   About an hour ago   Up 59 minutes (healthy)      0.0.0.0:8020->8020/tcp, :::8020->8020/tcp                                              ph-server-frontend
f3d595bf75fa   projectview/pms-server-frontend:v2.0.5                            "/usr/local/nginx/sb…"   About an hour ago   Up 59 minutes (healthy)      0.0.0.0:8090->8090/tcp, :::8090->8090/tcp                                              pms-server-frontend
19654ef23787   projectview/pms-batch:v2.0.5                                      "/bin/sh -c 'exec ja…"   About an hour ago   Up About an hour (healthy)   0.0.0.0:8081->8081/tcp, :::8081->8081/tcp                                              pms-batch
be6aa42ad016   projectview/pms-mail:v2.0.5                                       "/bin/sh -c 'exec ja…"   About an hour ago   Up About an hour (healthy)   0.0.0.0:8050->8050/tcp, :::8050->8050/tcp                                              pms-mail
20fc2cc468e5   projectview/pms-multi-dashboard:v2.0.5                            "/bin/sh -c 'exec ja…"   About an hour ago   Up About an hour (healthy)   0.0.0.0:8060->8060/tcp, :::8060->8060/tcp                                              pms-multi-dashboard
8fad26f4b067   projectview/ph-server-backend:v2.0.5                              "java -jar /promise-…"   About an hour ago   Up About an hour (healthy)   0.0.0.0:8010->8010/tcp, :::8010->8010/tcp                                              ph-server-backend
381e2b1bb0c1   projectview/pms-server-backend:v2.0.5                             "/bin/sh -c 'exec ja…"   About an hour ago   Up About an hour (healthy)   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp                                              pms-server-backend
c44734e168d9   projectview/pms-manual:v2.0.5                                     "/usr/local/nginx/sb…"   About an hour ago   Up About an hour (healthy)   80/tcp, 5432/tcp, 8080-8081/tcp, 8090/tcp, 0.0.0.0:8088->8088/tcp, :::8088->8088/tcp   pms-manual
f0df3a8b86e8   projectview/postgres:13.13                                        "docker-entrypoint.s…"   About an hour ago   Up About an hour (healthy)   5432/tcp, 0.0.0.0:35432->35432/tcp, :::35432->35432/tcp                                pms-postgresql
0eab33a92799   projectview/redis:7.2.3                                           "docker-entrypoint.s…"   About an hour ago   Up About an hour (healthy)   0.0.0.0:36379->6379/tcp, :::36379->6379/tcp                                            pms-redis

```

## 서비스 종료
실행 중인 서비스를 종료한다.
```
projectview@projectview:/app/projectview$ ls
backup-projectview.sh  cert  conf  config.env  docker-compose.yml  logs  postgresql  shutdown-projectview.sh  startup-projectview.sh  uploadFiles
projectview@projectview:/app/projectview$ ./shutdown-projectview.sh 
[+] Running 12/12
 ✔ Container pms-batch                  Removed                                                                                                                  0.4s 
 ✔ Container pms-nginx                  Removed                                                                                                                  0.3s 
 ✔ Container ph-server-frontend         Removed                                                                                                                  0.5s 
 ✔ Container pms-server-frontend        Removed                                                                                                                  0.7s 
 ✔ Container pms-manual                 Removed                                                                                                                  0.4s 
 ✔ Container pms-mail                   Removed                                                                                                                  0.6s 
 ✔ Container pms-multi-dashboard        Removed                                                                                                                  0.5s 
 ✔ Container ph-server-backend          Removed                                                                                                                  0.7s 
 ✔ Container pms-server-backend         Removed                                                                                                                  0.7s 
 ✔ Container pms-postgresql             Removed                                                                                                                  0.4s 
 ✔ Container pms-redis                  Removed                                                                                                                  0.5s 
 ✔ Network projectview_promise-network  Removed                                                                                                                  0.4s 

```

## 서비스 시작
서비스를 시작한다.
```
projectview@projectview:/app/projectview$ ls
backup-projectview.sh  cert  conf  config.env  docker-compose.yml  logs  postgresql  shutdown-projectview.sh  startup-projectview.sh  uploadFiles
projectview@projectview:/app/projectview$ ./startup-projectview.sh 
Warning: No resource found to remove for project "projectview".
WARN[0000] /app/projectview/docker-compose.yml: `version` is obsolete 
[+] Running 11/12
 ⠼ Network projectview_promise-network  Created                                                                                                                139.4s 
 ✔ Container pms-postgresql             Healthy                                                                                                                 16.0s 
 ✔ Container pms-manual                 Healthy                                                                                                                108.6s 
 ✔ Container pms-redis                  Healthy                                                                                                                 16.0s 
 ✔ Container pms-mail                   Healthy                                                                                                                 47.7s 
 ✔ Container ph-server-backend          Healthy                                                                                                                 62.7s 
 ✔ Container pms-batch                  Healthy                                                                                                                124.3s 
 ✔ Container pms-server-backend         Healthy                                                                                                                107.7s 
 ✔ Container pms-multi-dashboard        Healthy                                                                                                                 47.7s 
 ✔ Container ph-server-frontend         Healthy                                                                                                                123.5s 
 ✔ Container pms-server-frontend        Healthy                                                                                                                123.5s 
 ✔ Container pms-nginx                  Healthy                                                                                                                139.2s 

```

## 웹 브라우저로 서비스 확인

아래 URL로 첫 페이지가 표시되는지 확인한다.
보안 경고를 무시하고 첫 페이지(로그인 페이지)가 보이는지 확인한다.

* https://localhost/

## 서버 IP 주소 확인

클라이언트에서 서버로 접속하기 위한 IP 주소를 확인한다.

```
projectview@projectview:~$ ifconfig
br-4e362cda3ae4: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.30.1  netmask 255.255.255.0  broadcast 192.168.30.255
        inet6 fe80::42:e0ff:fe68:fda8  prefixlen 64  scopeid 0x20<link>
        ether 02:42:e0:68:fd:a8  txqueuelen 0  (Ethernet)
        RX packets 477  bytes 40616 (39.6 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 408  bytes 66863 (65.2 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:17:96:da:7e  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp3s0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        ether 98:83:89:43:66:98  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 413978  bytes 29534429 (28.1 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 413978  bytes 29534429 (28.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth57b0db0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::d063:faff:fe67:dbbd  prefixlen 64  scopeid 0x20<link>
        ether d2:63:fa:67:db:bd  txqueuelen 0  (Ethernet)
        RX packets 10510  bytes 2917047 (2.7 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 15620  bytes 1511974 (1.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth664b285: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::2cdf:ebff:fe7c:21c1  prefixlen 64  scopeid 0x20<link>
        ether 2e:df:eb:7c:21:c1  txqueuelen 0  (Ethernet)
        RX packets 35148  bytes 3275279 (3.1 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 27209  bytes 19124436 (18.2 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth76005c5: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::fc98:c3ff:febd:6a0e  prefixlen 64  scopeid 0x20<link>
        ether fe:98:c3:bd:6a:0e  txqueuelen 0  (Ethernet)
        RX packets 11575  bytes 1095648 (1.0 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 8987  bytes 2681052 (2.5 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth8d1ad6f: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::5ca1:60ff:fe42:5029  prefixlen 64  scopeid 0x20<link>
        ether 5e:a1:60:42:50:29  txqueuelen 0  (Ethernet)
        RX packets 35  bytes 2546 (2.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 952  bytes 98387 (96.0 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

veth9b516c0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::4df:46ff:fe41:b14f  prefixlen 64  scopeid 0x20<link>
        ether 06:df:46:41:b1:4f  txqueuelen 0  (Ethernet)
        RX packets 35  bytes 2546 (2.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 939  bytes 97145 (94.8 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethabc5e6f: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::386e:b5ff:fe15:7f82  prefixlen 64  scopeid 0x20<link>
        ether 3a:6e:b5:15:7f:82  txqueuelen 0  (Ethernet)
        RX packets 122  bytes 15466 (15.1 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1034  bytes 119484 (116.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethaf99464: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::c71:25ff:feaf:c1d2  prefixlen 64  scopeid 0x20<link>
        ether 0e:71:25:af:c1:d2  txqueuelen 0  (Ethernet)
        RX packets 26259  bytes 19022984 (18.1 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 36013  bytes 3357393 (3.2 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethb95a21b: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::38e2:5cff:fec9:4789  prefixlen 64  scopeid 0x20<link>
        ether 3a:e2:5c:c9:47:89  txqueuelen 0  (Ethernet)
        RX packets 35  bytes 2546 (2.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 953  bytes 98457 (96.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethbdb219b: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::e4f7:8cff:fef8:9164  prefixlen 64  scopeid 0x20<link>
        ether e6:f7:8c:f8:91:64  txqueuelen 0  (Ethernet)
        RX packets 3320  bytes 335644 (327.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 3250  bytes 420325 (410.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethf81e2fe: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::60a2:1dff:fe03:87ca  prefixlen 64  scopeid 0x20<link>
        ether 62:a2:1d:03:87:ca  txqueuelen 0  (Ethernet)
        RX packets 50  bytes 4103 (4.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 953  bytes 99988 (97.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethf84072a: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::d0eb:1dff:feb4:686d  prefixlen 64  scopeid 0x20<link>
        ether d2:eb:1d:b4:68:6d  txqueuelen 0  (Ethernet)
        RX packets 35  bytes 2546 (2.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 953  bytes 98477 (96.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.247  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::e7c7:e170:7fb3:6716  prefixlen 64  scopeid 0x20<link>
        ether f8:59:71:72:7c:1b  txqueuelen 1000  (Ethernet)
        RX packets 46354  bytes 50962225 (48.6 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 15844  bytes 3553817 (3.3 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

위 예에서는 IP 주소가 아래와 같다.
* 192.168.0.247