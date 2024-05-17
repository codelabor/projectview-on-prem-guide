## 1-2 ProjectView 설치
### ProjectView 설치

#### 설치 디렉토리 생성
임시 디렉토리와 설치 디렉토리를 생성하고 소유권을 설정한다.

```
projectview@projectview:~$ sudo mkdir -p /app/installer
projectview@projectview:~$ sudo mkdir -p /app/projectview
projectview@projectview:~$ sudo chown -R projectview:projectview /app

```

변경된 소유권을 확인한다.
```
projectview@projectview:~$ cd /app
projectview@projectview:/app$ ls -al
total 16
drwxr-xr-x  4 projectview projectview 4096  5월 17 13:59 .
drwxr-xr-x 21 root        root        4096  5월 17 13:59 ..
drwxr-xr-x  2 projectview projectview 4096  5월 17 13:59 installer
drwxr-xr-x  2 projectview projectview 4096  5월 17 13:59 projectview
projectview@projectview:/app$ 

```

#### 설치 파일 준비
임시 디렉토리에 설치 파일을 복사한다.

```
projectview@projectview:~/다운로드$ cd projectview
projectview@projectview:~/다운로드/projectview$ ls
projectview-installer-버전.tar
projectview@projectview:~/다운로드/projectview$ cp *.tar /app/installer
projectview@projectview:~/다운로드/projectview$ cd /app/installer
projectview@projectview:/app/installer$ ls
projectview-installer-버전.tar
projectview@projectview:/app/installer$ 
```

#### 설치 파일 해제
설치 파일 아카이브를 해제한다.

```
projectview@projectview:/app/installer$ tar -xvf projectview-installer-*.tar
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
./projectview-버전.tgz

```

#### 설정 파일 편집
설정 파일을 vi 편집기나 Visual Studio Code로 편집한다.

```
projectview@projectview:/app/installer$ vi config.env

```

파일 내용을 필요에 따라 현행화한다.

* PROJECTVIEW_VERSION: 설치 파일 버전 (예: v0.0.0, v0.0.0.0)
* INSTALL_TARGET_PATH: 설치 디렉토리

```
# ProjectView Version - Use for docker image version to load and docker compose
PROJECTVIEW_VERSION=버전

# Install target directory path
INSTALL_TARGET_PATH=/app/projectview

# Path to install postgresql database data files
DATABASE_DATA_PATH=${INSTALL_TARGET_PATH}/postgresql/data

# Path of attach files, logs
PROJECTVIEW_ATTACH_FILE_PATH=${INSTALL_TARGET_PATH}/uploadFiles
PROJECTVIEW_LOG_PATH=${INSTALL_TARGET_PATH}/logs
```

#### 설치 스크립트 실행
설치 스크립트에 실행 권한을 확인한다.

```
projectview@projectview:/app/installer$ ls -al
total 8418444
drwxr-xr-x 4 projectview projectview       4096  5월 17 14:44 .
drwxr-xr-x 4 projectview projectview       4096  5월 17 13:59 ..
drwxr-xr-x 8 projectview projectview       4096  5월 16 16:15 .git
-rw-r--r-- 1 projectview projectview       1801  5월 16 16:15 LICENSE
-rw-r--r-- 1 projectview projectview    3200000  5월 16 16:15 Projectview_Open_Source_License.doc
-rw-r--r-- 1 projectview projectview         57  5월 16 16:15 README.md
-rw-r--r-- 1 projectview projectview       3373  5월 16 16:15 common.sh
-rw-r--r-- 1 projectview projectview        430  5월 17 14:43 config.env
-rw-r--r-- 1 projectview projectview        520  5월 16 16:15 create_nginx_log_rotate.sh
drwxr-xr-x 5 projectview projectview       4096  5월 16 16:15 data
-rw-r--r-- 1 projectview projectview       7191  5월 16 16:15 docker-compose.yml
-rw-r--r-- 1 projectview projectview       3181  5월 16 16:15 install.sh
-rwxrwxrwx 1 projectview projectview 4339322880  5월 17 11:30 projectview-installer-버전.tar
-rw-r--r-- 1 projectview projectview 4277902642  5월 16 16:22 projectview-버전.tgz
projectview@projectview:/app/installer$ 

```
설치 스크립트에 실행 권한을 부여하고 확인한다.

```
projectview@projectview:/app/installer$ chmod u+x install.sh
projectview@projectview:/app/installer$ ls -al
total 8418444
drwxr-xr-x 4 projectview projectview       4096  5월 17 14:44 .
drwxr-xr-x 4 projectview projectview       4096  5월 17 13:59 ..
drwxr-xr-x 8 projectview projectview       4096  5월 16 16:15 .git
-rw-r--r-- 1 projectview projectview       1801  5월 16 16:15 LICENSE
-rw-r--r-- 1 projectview projectview    3200000  5월 16 16:15 Projectview_Open_Source_License.doc
-rw-r--r-- 1 projectview projectview         57  5월 16 16:15 README.md
-rw-r--r-- 1 projectview projectview       3373  5월 16 16:15 common.sh
-rw-r--r-- 1 projectview projectview        430  5월 17 14:43 config.env
-rw-r--r-- 1 projectview projectview        520  5월 16 16:15 create_nginx_log_rotate.sh
drwxr-xr-x 5 projectview projectview       4096  5월 16 16:15 data
-rw-r--r-- 1 projectview projectview       7191  5월 16 16:15 docker-compose.yml
-rwxr--r-- 1 projectview projectview       3181  5월 16 16:15 install.sh
-rwxrwxrwx 1 projectview projectview 4339322880  5월 17 11:30 projectview-installer-버전.tar
-rw-r--r-- 1 projectview projectview 4277902642  5월 16 16:22 projectview-버전.tgz
projectview@projectview:/app/installer$ 

```

설치 스크립트를 실행한다.
```
projectview@projectview:/app/installer$ sudo ./install.sh
[sudo] password for projectview: 

--- ProjectView installation is started..----

[Step 1]: checking if docker is installed ...

Note: docker version: 26.1.2

[Step 2]: checking docker-compose is installed ...

Note: Docker Compose version v2.27.0

[Step 3]: loading ProjectView docker images ...
➜ Load projectview 버전
af20dc84a450: Loading layer [==================================================>]  212.8MB/212.8MB
ab85d2a43d1e: Loading layer [==================================================>]   2.56kB/2.56kB
...생략...
f75f216b926a: Loading layer [==================================================>]  6.144kB/6.144kB
Loaded image: projectview/pms-server-frontend:버전


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
[+] Running 12/12
 ✔ Network projectview_promise-network  Created                                                              0.2s 
 ✔ Container pms-manual                 Healthy                                                            205.5s 
 ✔ Container pms-redis                  Healthy                                                            205.5s 
 ✔ Container pms-postgresql             Healthy                                                            205.5s 
 ✔ Container pms-server-backend         Healthy                                                            202.5s 
 ✔ Container pms-batch                  Healthy                                                            202.4s 
 ✔ Container ph-server-backend          Healthy                                                            202.4s 
 ✔ Container pms-multi-dashboard        Healthy                                                            202.4s 
 ✔ Container pms-mail                   Healthy                                                            202.4s 
 ✔ Container pms-server-frontend        Healthy                                                            202.2s 
 ✔ Container ph-server-frontend         Healthy                                                            202.2s 
 ✔ Container pms-nginx                  Healthy                                                            217.0s 
✔ --- ProjectView has been installed and started successfully.----
projectview@projectview:/app/installer$ 

```

설치 완료 후 자동으로 실행된다.

#### 설치 결과 확인
설치 디렉토리를 확인한다.

```
rojectview@projectview:/app/projectview$ ls -al
total 52
drwxr-xr-x 7 projectview projectview 4096  5월 17 17:40 .
drwxr-xr-x 4 projectview projectview 4096  5월 17 13:59 ..
-rwxr--rwx 1 root        root        1261  5월 17 17:10 backup-projectview.sh
drwxr-xr-x 2 root        root        4096  5월 17 14:52 cert
drwxr-xr-x 6 root        root        4096  5월 17 14:52 conf
-rw-r--r-- 1 root        root         430  5월 17 14:52 config.env
-rw-r--r-- 1 root        root        7473  5월 17 14:52 docker-compose.yml
drwxr-xr-x 8 root        root        4096  5월 17 14:52 logs
drwxr-xr-x 3 root        root        4096  5월 17 14:52 postgresql
-rwxr--rwx 1 root        root          59  5월 17 14:52 shutdown-projectview.sh
-rwxr--rwx 1 root        root         126  5월 17 14:52 startup-projectview.sh
drwxr-xr-x 3 root        root        4096  5월 17 17:20 uploadFiles

```

소유권을 변경한다.
```
projectview@projectview:/app/projectview$ sudo chown -R projectview:projectview .
```
변경된 소유권을 확인한다.
```
projectview@projectview:/app/projectview$ ls -al
total 52
drwxr-xr-x 7 projectview projectview 4096  5월 17 17:40 .
drwxr-xr-x 4 projectview projectview 4096  5월 17 13:59 ..
-rwxr--rwx 1 projectview projectview 1261  5월 17 17:10 backup-projectview.sh
drwxr-xr-x 2 projectview projectview 4096  5월 17 14:52 cert
drwxr-xr-x 6 projectview projectview 4096  5월 17 14:52 conf
-rw-r--r-- 1 projectview projectview  430  5월 17 14:52 config.env
-rw-r--r-- 1 projectview projectview 7473  5월 17 14:52 docker-compose.yml
drwxr-xr-x 8 projectview projectview 4096  5월 17 14:52 logs
drwxr-xr-x 3 projectview projectview 4096  5월 17 14:52 postgresql
-rwxr--rwx 1 projectview projectview   59  5월 17 14:52 shutdown-projectview.sh
-rwxr--rwx 1 projectview projectview  126  5월 17 14:52 startup-projectview.sh
drwxr-xr-x 3 projectview projectview 4096  5월 17 17:20 uploadFiles
projectview@projectview:/app/projectview$ 
```

##### 디렉토리
* cert : 도메인 인증서 디렉토리
* conf : 웹서버 설정 파일 디렉토리
* logs : 서비스 별 로그 파일 디렉토리
* postgresql : 데이터베이스 데이터 디렉토리
* uploadFiles : 첨부 파일 디렉토리

##### 파일
* config.env : ProjectView 설정 파일
* docker-compose.yml : docker compose 설정 파일
* shutdown-projectview.sh : ProjectView 서비스 종료 스크립트 파일
* startup-projectview.sh : ProjectView 서비스 시작 스크립트 파일

#### 실행 프로세스 확인
실행된 docker 프로세스를 확인한다.
```
projectview@projectview:/app/projectview$ sudo docker ps
CONTAINER ID   IMAGE                                                             COMMAND                   CREATED          STATUS                    PORTS                                                                                  NAMES
5be1bfd07397   projectview/base-frontend:rhel-8.9-1107.1705420509-nginx-1.25.3   "/usr/local/nginx/sb…"   17 minutes ago   Up 13 minutes (healthy)   0.0.0.0:443->443/tcp, :::443->443/tcp                                                  pms-nginx
975e2f1fa16a   projectview/pms-server-frontend:버전                          "/usr/local/nginx/sb…"   17 minutes ago   Up 13 minutes (healthy)   0.0.0.0:8090->8090/tcp, :::8090->8090/tcp                                              pms-server-frontend
583e9a701169   projectview/ph-server-frontend:버전                           "/usr/local/nginx/sb…"   17 minutes ago   Up 13 minutes (healthy)   0.0.0.0:8020->8020/tcp, :::8020->8020/tcp                                              ph-server-frontend
a03357555f60   projectview/pms-mail:버전                                     "/bin/sh -c 'exec ja…"   17 minutes ago   Up 14 minutes (healthy)   0.0.0.0:8050->8050/tcp, :::8050->8050/tcp                                              pms-mail
fce21eb2a91f   projectview/pms-multi-dashboard:버전                          "/bin/sh -c 'exec ja…"   17 minutes ago   Up 14 minutes (healthy)   0.0.0.0:8060->8060/tcp, :::8060->8060/tcp                                              pms-multi-dashboard
5f6024b076ab   projectview/ph-server-backend:버전                            "java -jar /promise-…"   17 minutes ago   Up 14 minutes (healthy)   0.0.0.0:8010->8010/tcp, :::8010->8010/tcp                                              ph-server-backend
1a421e7ed635   projectview/pms-batch:버전                                    "/bin/sh -c 'exec ja…"   17 minutes ago   Up 14 minutes (healthy)   0.0.0.0:8081->8081/tcp, :::8081->8081/tcp                                              pms-batch
48d50969344b   projectview/pms-server-backend:버전                           "/bin/sh -c 'exec ja…"   17 minutes ago   Up 16 minutes (healthy)   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp                                              pms-server-backend
001f3acf52b2   projectview/pms-manual:버전                                   "/usr/local/nginx/sb…"   17 minutes ago   Up 17 minutes (healthy)   80/tcp, 5432/tcp, 8080-8081/tcp, 8090/tcp, 0.0.0.0:8088->8088/tcp, :::8088->8088/tcp   pms-manual
1782c5b9c49c   projectview/postgres:13.13                                        "docker-entrypoint.s…"   17 minutes ago   Up 17 minutes (healthy)   5432/tcp, 0.0.0.0:35432->35432/tcp, :::35432->35432/tcp                                pms-postgresql
f7b8fcf3a4c2   projectview/redis:7.2.3                                           "docker-entrypoint.s…"   17 minutes ago   Up 17 minutes (healthy)   0.0.0.0:36379->6379/tcp, :::36379->6379/tcp                                            pms-redis
projectview@projectview:/app/projectview$ 

```

#### 서비스 종료
실행 중인 서비스를 종료한다.
```
projectview@projectview:/app/projectview$ sudo ./shutdown-projectview.sh 
[+] Running 12/12
 ✔ Container pms-batch                  Removed                                                              0.4s 
 ✔ Container pms-nginx                  Removed                                                              0.3s 
 ✔ Container pms-manual                 Removed                                                              0.7s 
 ✔ Container pms-server-frontend        Removed                                                              0.6s 
 ✔ Container ph-server-frontend         Removed                                                              0.4s 
 ✔ Container pms-multi-dashboard        Removed                                                              0.5s 
 ✔ Container ph-server-backend          Removed                                                              0.8s 
 ✔ Container pms-mail                   Removed                                                              0.6s 
 ✔ Container pms-server-backend         Removed                                                              0.7s 
 ✔ Container pms-redis                  Removed                                                              0.5s 
 ✔ Container pms-postgresql             Removed                                                              0.4s 
 ✔ Network projectview_promise-network  Removed                                                              0.4s 
projectview@projectview:/app/projectview$ 

```

#### 서비스 시작
서비스를 시작한다.
```
projectview@projectview:/app/projectview$ sudo ./startup-projectview.sh 
Warning: No resource found to remove for project "projectview".
WARN[0000] /app/projectview/docker-compose.yml: `version` is obsolete 
[+] Running 12/12
 ✔ Network projectview_promise-network  Created                                                              0.1s 
 ✔ Container pms-redis                  Healthy                                                            125.3s 
 ✔ Container pms-manual                 Healthy                                                            125.3s 
 ✔ Container pms-postgresql             Healthy                                                            125.3s 
 ✔ Container ph-server-backend          Healthy                                                            125.3s 
 ✔ Container pms-server-backend         Healthy                                                            125.3s 
 ✔ Container pms-batch                  Healthy                                                            125.3s 
 ✔ Container pms-multi-dashboard        Healthy                                                            125.3s 
 ✔ Container pms-mail                   Healthy                                                            125.3s 
 ✔ Container pms-server-frontend        Healthy                                                            125.3s 
 ✔ Container ph-server-frontend         Healthy                                                            125.3s 
 ✔ Container pms-nginx                  Healthy                                                            140.2s 
projectview@projectview:/app/projectview$ 
```

#### 서버 IP 주소 확인

클라이언트에서 서버로 접속하기 위한 IP 주소를 확인한다.

```
projectview@projectview:/app/projectview$ ifconfig

... 생략 ...

enp3s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.30.61  netmask 255.255.255.0  broadcast 192.168.30.255
        inet6 fe80::5ecf:d7:6773:319  prefixlen 64  scopeid 0x20<link>
        ether 98:83:89:43:66:98  txqueuelen 1000  (Ethernet)
        RX packets 302  bytes 70684 (69.0 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 88  bytes 11438 (11.1 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

... 생략 ...

projectview@projectview:/app/projectview$ 

```

#### Hosts 설정
ProjectView 설치형은 site.projectview.io 도메인을 사용한다.
개인 PC의 hosts 파일에 도메인 정보가 추가 되어야 한다.

ProjectView가 설치된 장비의 IP가 아래와 같다고 가정한다.
* 192.168.30.61

아래 파일을 관리자 권한으로 편집한다.
* /etc/hosts

```
projectview@projectview:~$ sudo vi /etc/hosts
```

아래와 같이 IP 주소를 'site.projectview.io' URL을 등록한다.
```
127.0.0.1       localhost
127.0.1.1       projectview
192.168.30.61  site.projectview.io

... 생략 ...
```

### Chrome 설정
ProjectView는 https 통신을 위해 사설 인증서를 사용한다.
ProjectView의 사설인증서를 신뢰할 수 r있는 인증서로 등록해야 한다.

#### 인증서 등록

* Chrome을 실행한다.
* URL에 'chrome://settings'를 입력하고 해당 페이지로 이동한다.
* 왼쪽에서 '개인 정보 보호 및 보안'을 클릭한다.
* '보안'을 클릭한다.
* '고급'까지 스크롤한다.
* '인증서 관리'를 클릭한다.
* '인증 기관' 탭을 선택한다.
* '가져오기'를 클릭한다.
* 제공된 파일 중 아래 인증서를 선택한다.

```
projectview@projectview:~/다운로드/certificate$ ls
'SDS.ACT Root Certificate.crt'
```

* 모든 선택 사항을 설정해서 등록한다. 
* 등록 후 인증 기관 목록에 아래 내용이 표시된다.

```
... 생략 ...
org-SDS
        SDS.ACT Root Certificate
```

###### 웹 브라우저로 서비스 확인

아래 URL로 첫 페이지가 표시되는지 확인한다.

* https://site.projectview.io


#### 프로젝트 관리자 등록

로그인 창에서 '회원가입'을 클릭하여 프로젝트 관리자를 등록한다.
가입이 완료되면 등록된 관리자로 자동 로그인된다.
로그인 후 '가입된 프로젝트가 없습니다.'라는 표시가 나오면 정상이다. 
프로젝트 관리자는 로그아웃한다.

#### 시스템 관리자 로그인
시스템 관리자로 로그인한다.

* ID: system.admin 
* Password: system.admin01!

로그인에 성공하면 비밀번호를 재설정한다.

**Warning**
변경된 비밀번호를 잊지 않도록 주의한다.

##### 프로젝트 생성 및 관리자 지정

* '코드 관리 > 회사코드'에서 고객사 코드를 추가한다.
* '코드 관리 > 회사코드'에서 협력사 코드를 추가한다.
* '프로젝트 목록'에서 프로젝트를 생성한다.
* 프로젝트 추가 화면에서 앞서 가입한 프로젝트 관리자를 지정한다.

#### 프로젝트 관리자 로그인

프로젝트 관리자로 로그인한다.
로그인 후 '프로젝트 첫 방문입니다.'라는 표시가 나오면 정상이다.

매뉴얼을 참고하여 프로젝트를 관리한다.

* 관리자 매뉴얼 : https://site.projectview.io/projectview-manual/admin-manual/
* 사용자 매뉴얼 : https://site.projectview.io/projectview-manual/user-manual/