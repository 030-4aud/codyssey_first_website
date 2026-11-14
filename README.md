# 개발환경 구축 미션 – Docker Workstation

## 1. 프로젝트 개요

### 1-1. 프로젝트 목적
이번 프로젝트는 Linux 터미널과 Git을 활용하여 개발 환경을 구축하고, Docker의 기본 개념과 컨테이너 운영 방법을 실습하는 것을 목표로 한다.

터미널 명령어를 이용한 파일 및 디렉토리 관리, Linux 권한 설정, Docker 이미지와 컨테이너 관리, Dockerfile을 이용한 커스텀 이미지 제작, 포트 매핑, 바인드 마운트 및 볼륨 영속성 검증을 수행하였다.

또한 Git을 이용하여 프로젝트를 관리하고 GitHub 저장소와 연동하여 작업 결과를 제출할 수 있도록 구성하였다.

### 1-2. 프로젝트 주요 수행 내용
- Linux 터미널 기본 명령어 실습
- 파일 및 디렉토리 권한 확인과 변경
- Docker 설치 및 실행 상태 확인
- Docker 이미지 다운로드 및 컨테이너 실행
- Ubuntu 컨테이너 내부 명령어 실습
- NGINX 기반 커스텀 웹 이미지 제작
- 포트 매핑 및 웹 서버 접속 확인
- 바인드 마운트를 이용한 파일 변경 확인
- Docker 볼륨을 이용한 데이터 영속성 검증
- Git 초기 설정 및 GitHub 저장소 연동 시도

---

## 2. 실행 환경

| 구분 | 사용 환경 |
|---|---|
| OS | Kali Linux |
| Shell | Zsh |
| Terminal | Kali Linux 터미널 |
| 사용자 | kali |
| Docker | 28.5.2+dfsg4 |
| Docker Buildx | 0.29.1+ds1 |
| Docker Compose | 2.40.3-3 |
| Git | 2.51.0 |
| 기본 브랜치 | main |
| 웹 서버 | NGINX Alpine |
| 컨테이너 Linux | Ubuntu 24.04 |

### 2-1. Docker 버전 확인

**명령어**

```bash
docker --version
```

**실행 결과**

```text
Docker version 28.5.2+dfsg4, build 9cc6dea35e9a963f281434761c656fba4ac43aed
```

### 2-2. Git 버전 확인

**명령어**

```bash
git --version
```

**실행 결과**

```text
git version 2.51.0
```

---

## 3. 수행 항목 체크리스트

- [o] Linux 터미널 기본 명령어 실습
- [o] 파일 및 디렉토리 생성, 복사, 이동, 삭제
- [o] 파일 내용 확인 및 빈 파일 생성
- [o] 파일 권한 변경
- [o] 디렉토리 권한 변경
- [o] Docker 설치 및 버전 확인
- [o] Docker 데몬 실행 확인
- [o] Docker 이미지 다운로드 및 목록 확인
- [o] Docker 컨테이너 실행 및 종료
- [o] Docker 로그 및 리소스 확인
- [o] hello-world 실행
- [o] Ubuntu 컨테이너 내부 명령어 실행
- [o] Dockerfile 기반 커스텀 이미지 제작
- [o] NGINX 웹 서버 실행
- [o] 포트 매핑 및 curl 접속 확인
- [o] 바인드 마운트 적용 및 변경 확인
- [o] Docker 볼륨 생성 및 데이터 영속성 검증
- [o] Git 사용자 정보 설정
- [o] Git 기본 브랜치 main 설정
- [o] Git 저장소 초기화 및 첫 커밋
- [o] GitHub 인증 및 원격 저장소 Push 완료
- [o] GitHub 제출 링크 확인

---

## 4. 터미널 기본 조작 실습

### 4-1. 현재 위치 확인

**명령어**

```bash
pwd
```

**실행 결과**

```text
/home/kali
```

### 4-2. 작업 디렉토리 생성 및 이동

**명령어**

```bash
mkdir -p ~/developer-workshop
cd ~/developer-workshop
pwd
```

**실행 결과**

```text
/home/kali/developer-workshop
```

### 4-3. 숨김 파일을 포함한 목록 확인

**명령어**

```bash
ls -la
```

**실행 결과**

```text
total 8
drwxrwxr-x  2 kali kali 4096 Nov 12 22:22 .
drwx------ 23 kali kali 4096 Nov 12 22:22 ..
```

### 4-4. 디렉토리 및 파일 생성

**명령어**

```bash
mkdir practice
cd practice
touch hello.txt
```

파일과 디렉토리가 정상적으로 생성되었다.

### 4-5. 파일 복사 및 이름 변경

**명령어**

```bash
cp hello.txt copy.txt
mv copy.txt renamed.txt
ls -la
```

**실행 결과**

```text
total 8
drwxrwxr-x 2 kali kali 4096 Nov 12 22:24 .
drwxrwxr-x 3 kali kali 4096 Nov 12 22:22 ..
-rw-rw-r-- 1 kali kali    0 Nov 12 22:22 hello.txt
-rw-rw-r-- 1 kali kali    0 Nov 12 22:24 renamed.txt
```

### 4-6. 파일 이름 변경 및 삭제

**명령어**

```bash
mv renamed.txt hello2.txt
rm hello2.txt
```

파일 이름 변경과 삭제가 정상적으로 수행되었다.

### 4-7. 파일 내용 확인

**명령어**

```bash
cat hello.txt
```

**실행 결과**

```text
(출력 없음)
```

처음 파일 내용을 입력하는 과정에서 따옴표가 정상적으로 닫히지 않아 Zsh의 `dquote>` 입력 상태가 발생하였다. 이후 파일은 빈 상태로 남아 있었으며, `cat` 명령어를 이용해 내용이 없는 것을 확인하였다.

### 4-8. 프로젝트 작업 폴더 구성

**명령어**

```bash
cd ~/developer-workshop

mkdir -p docker-workstation/app
mkdir -p docker-workstation/screenshots
mkdir -p docker-workstation/logs

cd docker-workstation

touch README.md

ls -R
```

**실행 결과**

```text
.:
app  logs  README.md  screenshots

./app:

./logs:

./screenshots:
```

---

## 5. Linux 권한 실습

파일과 디렉토리의 권한을 확인하고 `chmod` 명령어를 이용하여 권한을 변경하였다.

### 5-1. 파일 권한 변경

**파일 생성**

```bash
touch permission.txt
```

**변경 전 권한 확인**

```bash
ls -l permission.txt
```

```text
-rw-rw-r-- 1 kali kali 0 Nov 12 22:27 permission.txt
```

**권한 변경**

```bash
chmod 644 permission.txt
```

**변경 후 권한 확인**

```bash
ls -l permission.txt
```

```text
-rw-r--r-- 1 kali kali 0 Nov 12 22:27 permission.txt
```

**변경 결과**

| 구분 | 권한 |
|---|---|
| 변경 전 | 664 |
| 변경 후 | 644 |

소유자는 읽기와 쓰기가 가능하며, 그룹과 기타 사용자는 읽기만 가능하도록 변경하였다.

### 5-2. 디렉토리 권한 변경

**디렉토리 생성**

```bash
mkdir permission-dir
```

**변경 전 권한 확인**

```bash
ls -ld permission-dir
```

```text
drwxrwxr-x 2 kali kali 4096 Nov 12 22:28 permission-dir
```

**권한 변경**

```bash
chmod 755 permission-dir
```

**변경 후 권한 확인**

```bash
ls -ld permission-dir
```

```text
drwxr-xr-x 2 kali kali 4096 Nov 12 22:28 permission-dir
```

**변경 결과**

| 구분 | 권한 |
|---|---|
| 변경 전 | 775 |
| 변경 후 | 755 |

소유자에게 모든 권한을 부여하고 그룹과 기타 사용자에게는 읽기 및 디렉토리 접근 권한을 부여하였다.

---

## 6. Docker 설치 및 기본 점검

### 6-1. Docker 데몬 상태 확인

**명령어**

```bash
sudo systemctl status docker
```

**실행 결과**

```text
● docker.service - Docker Application Container Engine
Loaded: loaded (/usr/lib/systemd/system/docker.service)
Active: active (running)
Main PID: 1022 (dockerd)
```

Docker 서비스가 정상적으로 실행 중인 것을 확인하였다.

### 6-2. Docker 권한 문제 해결

초기에는 일반 사용자 계정에서 Docker 데몬에 접근할 수 없어 다음과 같은 오류가 발생하였다.

```text
permission denied while trying to connect to the Docker daemon socket
at unix:///var/run/docker.sock
```

현재 사용자가 Docker 그룹에 포함되어 있지 않은 것을 확인하였다.

**명령어**

```bash
groups
```

**실행 결과**

```text
kali adm dialout cdrom floppy sudo audio dip video plugdev
users netdev scanner bluetooth lpadmin wireshark kaboxer
```

이후 다음 명령어를 실행하였다.

```bash
sudo usermod -aG docker $USER
newgrp docker
```

다시 Docker 명령어를 실행하여 정상적으로 접근되는 것을 확인하였다.

```bash
docker ps
```

```text
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

현재 실행 중인 컨테이너가 없는 빈 목록이 정상적으로 출력되었다.

### 6-3. Docker 정보 확인

**명령어**

```bash
docker info
```

초기에는 Docker Client 정보가 출력되었으나 Server 접근 권한 문제로 오류가 발생하였다.

Docker 그룹 권한을 적용한 이후에는 `docker ps`가 정상적으로 동작하는 것을 확인하였다.

---

## 7. Docker 기본 운영 명령 실습

### 7-1. 이미지 다운로드 및 목록 확인

**명령어**

```bash
docker run hello-world
docker run -it --name ubuntu-practice ubuntu:24.04 bash
docker run -d --name nginx-test -p 8085:80 nginx:alpine
docker images
```

**실행 결과**

```text
REPOSITORY       TAG       IMAGE ID
my-docker-web    1.0       d7b3a221da18
ubuntu           24.04     7da97f743f39
nginx            alpine    7cac34a4d72b
hello-world      latest    e2ac70e7319a
vulhub/struts2   2.3.30    13b410ff5928
```

Docker Hub에서 필요한 이미지가 정상적으로 다운로드되었으며, 로컬 이미지 목록에서 확인하였다.

### 7-2. 컨테이너 목록 확인

**명령어**

```bash
docker ps
docker ps -a
```

`docker ps`는 현재 실행 중인 컨테이너를 확인하는 명령어이며, `docker ps -a`는 종료된 컨테이너까지 포함하여 확인하는 명령어이다.

### 7-3. 컨테이너 로그 확인

**명령어**

```bash
docker logs nginx-test
```

**실행 결과**

```text
/docker-entrypoint.sh: Configuration complete; ready for start up
2026/11/13 03:39:19 [notice] 1#1: using the "epoll" event method
2026/11/13 03:39:19 [notice] 1#1: nginx/1.31.8
2026/11/13 03:39:19 [notice] 1#1: start worker processes
```

NGINX가 정상적으로 시작된 로그를 확인하였다.

### 7-4. 컨테이너 리소스 확인

**명령어**

```bash
docker stats --no-stream nginx-test
```

**실행 결과**

```text
CONTAINER ID   NAME        CPU %   MEM USAGE / LIMIT
e7706d22333d   nginx-test  0.00%   5.137MiB / 1.89GiB
```

컨테이너의 CPU와 메모리 사용량을 확인하였다.

### 7-5. 컨테이너 중지 및 삭제

**명령어**

```bash
docker stop nginx-test
docker rm nginx-test
```

실행 중인 NGINX 컨테이너를 정상적으로 중지하고 삭제하였다.

---

## 8. 컨테이너 실행 실습

### 8-1. hello-world 실행

**명령어**

```bash
docker run hello-world
```

**핵심 실행 결과**

```text
Hello from Docker!

This message shows that your installation appears to be working correctly.
```

Docker 클라이언트와 데몬의 통신, 이미지 다운로드, 컨테이너 생성 및 실행이 정상적으로 이루어졌다.

### 8-2. Ubuntu 컨테이너 실행

**명령어**

```bash
docker run -it --name ubuntu-practice ubuntu:24.04 bash
```

Ubuntu 컨테이너 내부로 진입하였다.

**내부 명령어**

```bash
ls
echo "Docker Container Test"
pwd
```

**실행 결과**

```text
bin  boot  dev  etc  home  lib  lib64
media  mnt  opt  proc  root  run  sbin
srv  sys  tmp  usr  var

Docker Container Test

/
```

컨테이너 내부의 디렉토리 구조와 명령어 실행을 확인하였다.

### 8-3. 컨테이너 종료 후 상태 확인

**명령어**

```bash
exit
docker ps -a
```

**실행 결과**

```text
CONTAINER ID   IMAGE          COMMAND   STATUS
2f34d13649a2   ubuntu:24.04   "bash"    Exited (0)
```

`exit` 명령어를 실행하면 현재 실행 중인 Bash 프로세스가 종료되면서 컨테이너도 종료되는 것을 확인하였다.

### 8-4. 종료된 컨테이너 재실행 및 exec 사용

**명령어**

```bash
docker start ubuntu-practice

docker exec ubuntu-practice ls

docker exec ubuntu-practice echo "Hello Again"
```

**실행 결과**

```text
ubuntu-practice

bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var

Hello Again
```

`docker start`로 종료된 컨테이너를 다시 실행하고 `docker exec`를 이용해 컨테이너 내부에서 별도의 명령어를 실행하였다.

### 8-5. 컨테이너 종료 및 삭제

```bash
docker stop ubuntu-practice
docker rm ubuntu-practice
```

**실습을 통해 알게 된 차이점**

- `docker run`: 새로운 컨테이너를 생성하고 실행한다.
- `docker start`: 기존에 생성된 컨테이너를 다시 실행한다.
- `docker exec`: 실행 중인 컨테이너 내부에서 별도의 명령을 실행한다.
- `exit`: 현재 터미널 세션의 메인 프로세스가 종료되면 컨테이너도 종료될 수 있다.
- `docker stop`: 실행 중인 컨테이너를 중지한다.
- `docker rm`: 컨테이너를 삭제한다.

이번 실습에서는 `attach`는 직접 실행하지 않았으며, `run -it`와 `exec`의 동작 차이를 확인하였다.

---

## 9. Dockerfile 기반 커스텀 이미지 제작

### 9-1. 베이스 이미지 선택

이번 실습에서는 NGINX Alpine 이미지를 베이스 이미지로 선택하였다.

```dockerfile
FROM nginx:alpine
```

NGINX는 정적 HTML 파일을 웹 브라우저에 제공할 수 있는 웹 서버이며, Alpine 기반 이미지를 사용하여 비교적 가벼운 환경을 구성하였다.

### 9-2. 프로젝트 구조

```text
docker-workstation/
│
├── Dockerfile
│
├── app/
│   └── index.html
│
├── README.md
│
├── logs/
│
└── screenshots/
```

### 9-3. Dockerfile 구성

```dockerfile
FROM nginx:alpine

COPY app/ /usr/share/nginx/html/
```

### 9-4. 커스텀 적용 내용

| 구성 요소 | 목적 |
|---|---|
| FROM nginx:alpine | 기존 NGINX 이미지 사용 |
| COPY app/ | 사용자 정의 웹 콘텐츠 복사 |
| index.html | 직접 제작한 웹페이지 제공 |

기본 NGINX 웹페이지 대신 직접 작성한 HTML 파일을 제공하도록 구성하였다.

### 9-5. 커스텀 이미지 빌드

**명령어**

```bash
docker build -t my-docker-web:1.0 .
```

**실행 결과**

```text
[+] Building 0.2s (7/7) FINISHED

[1/2] FROM docker.io/library/nginx:alpine
[2/2] COPY app/ /usr/share/nginx/html/

exporting to image
naming to docker.io/library/my-docker-web:1.0
```

커스텀 이미지가 정상적으로 빌드되었다.

### 9-6. 이미지 생성 확인

**명령어**

```bash
docker images
```

**핵심 결과**

```text
REPOSITORY       TAG       IMAGE ID
my-docker-web    1.0       ea09bf53b5d3
nginx            alpine    7cac34a4d72b
ubuntu           24.04     7da97f743f39
hello-world      latest    e2ac70e7319a
```

이후 Dockerfile을 다시 수정하고 재빌드하여 최종 이미지 ID가 다음과 같이 변경되었다.

```text
my-docker-web    1.0    d7b3a221da18
```

---

## 10. 포트 매핑 및 웹 서버 접속 확인

### 10-1. 커스텀 컨테이너 실행

**명령어**

```bash
docker run -d --name my-web -p 8080:80 my-docker-web:1.0
```

**실행 결과**

```text
```

### 10-2. 포트 매핑 확인

**명령어**

```bash
docker ps
```

**실행 결과**

```text
CONTAINER ID   IMAGE              STATUS
                my-docker-web:1.0  Up

PORTS
0.0.0.0:8080->80/tcp
```

호스트의 8080 포트가 컨테이너의 80 포트로 연결된 것을 확인하였다.

### 10-3. curl 접속 확인

**명령어**

```bash
curl http://localhost:8080
```

**핵심 실행 결과**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>My Docker Web</title>
</head>
<body>
    <div class="container">
        <h1>Bind Mount Test Success!</h1>
        <p>나만의 개발 환경 구축 성공!</p>
        <p>Docker와 NGINX를 이용한 웹 서버입니다.</p>
    </div>
</body>
</html>
```

웹 서버에 정상적으로 접속하여 사용자 정의 HTML이 반환되는 것을 확인하였다.

접속 주소:

```text
http://localhost:8080
```

---

## 11. 바인드 마운트 실습

바인드 마운트는 호스트 컴퓨터의 디렉토리를 컨테이너 내부 디렉토리와 연결하는 기능이다.

### 11-1. 바인드 마운트 컨테이너 실행

**명령어**

```bash
docker run -d \
  --name my-web-bind \
  -p 8080:80 \
  -v "$(pwd)/app:/usr/share/nginx/html:ro" \
  nginx:alpine
```

**실행 결과**

```text
7dfcedb65057ce873d32328254013e06a3d1dee3af8ab7255a6c73df3fe4d94c
```

### 11-2. 웹페이지 확인

**명령어**

```bash
curl http://localhost:8080
```

**실행 결과**

```html
<h1>Bind Mount Test Success!</h1>
<p>나만의 개발 환경 구축 성공!</p>
<p>Docker와 NGINX를 이용한 웹 서버입니다.</p>
```

### 11-3. 호스트 파일 변경 확인

호스트의 `app/index.html` 파일을 수정한 후 다시 접속하였다.

```bash
nano app/index.html

curl http://localhost:8080
```

수정된 HTML 내용이 반환되는 것을 확인하였다.

**실습 결과**

호스트에서 수정한 파일이 컨테이너 내부에 실시간으로 반영되는 것을 확인하였다.

`:ro` 옵션은 컨테이너 내부에서 마운트된 파일을 읽기 전용으로 사용하도록 설정하는 옵션이다.

---

## 12. Docker 볼륨 영속성 검증

Docker 볼륨은 컨테이너와 독립적으로 데이터를 저장할 수 있는 기능이다.

이번 실습에서는 컨테이너를 삭제한 후에도 데이터가 유지되는지 확인하였다.

### 12-1. Docker 볼륨 생성

**명령어**

```bash
docker volume create mydata
```

**실행 결과**

```text
mydata
```

### 12-2. 볼륨 목록 확인

**명령어**

```bash
docker volume ls
```

**실행 결과**

```text
DRIVER    VOLUME NAME
local     mydata
```

### 12-3. 첫 번째 컨테이너에 볼륨 연결

**명령어**

```bash
docker run -d \
  --name volume-test \
  -v mydata:/data \
  ubuntu:24.04 sleep infinity
```

**실행 결과**

```text
9baeb03df5ca9537d600a15d6d182ce2d4136f28d6dda96f73342c7cb95454f
```

### 12-4. 데이터 저장 및 확인

**명령어**

```bash
docker exec volume-test bash -c "echo 'Persistent Docker Data!' > /data/hello.txt"

docker exec volume-test cat /data/hello.txt
```

**실행 결과**

```text
Persistent Docker Data!
```

### 12-5. 첫 번째 컨테이너 삭제

**명령어**

```bash
docker rm -f volume-test
```

**실행 결과**

```text
volume-test
```

컨테이너를 삭제하였다.

### 12-6. 새로운 컨테이너에 동일한 볼륨 연결

**명령어**

```bash
docker run -d \
  --name volume-test2 \
  -v mydata:/data \
  ubuntu:24.04 sleep infinity
```

**실행 결과**

```text
004f9cd78a924fd676efaadfea2c9c3865ade52bab3af011e6c7d8d70a1dff4f
```

### 12-7. 기존 데이터 유지 여부 확인

**명령어**

```bash
docker exec volume-test2 cat /data/hello.txt
```

**실행 결과**

```text
Persistent Docker Data!
```

첫 번째 컨테이너를 삭제한 이후에도 새로운 컨테이너에서 기존 데이터가 정상적으로 출력되었다.

### 12-8. 실습 결과

Docker 컨테이너와 볼륨은 별도로 관리된다는 것을 확인하였다.

컨테이너를 삭제해도 볼륨은 유지되기 때문에 동일한 볼륨을 새로운 컨테이너에 연결하면 기존 데이터를 다시 사용할 수 있다.

마지막으로 테스트 컨테이너를 삭제하였다.

```bash
docker rm -f volume-test2
```

---

## 13. Git 설정 및 저장소 관리

### 13-1. Git 사용자 정보 설정

**명령어**

```bash
git config --global user.name "030-4aud"

git config --global user.email "<개인 이메일>"
```

### 13-2. 기본 브랜치 설정

**명령어**

```bash
git config --global init.defaultBranch main
```

### 13-3. Git 설정 확인

**명령어**

```bash
git config --list
```

**실행 결과**

```text
user.name=030-4aud
user.email=<개인정보 보호를 위해 마스킹>
init.defaultbranch=main
```

### 13-4. Git 저장소 초기화

**명령어**

```bash
cd ~/developer-workshop/docker-workstation

git init

git branch -M main
```

**실행 결과**

```text
Initialized empty Git repository in
/home/kali/developer-workshop/docker-workstation/.git/
```

### 13-5. Git 상태 확인

**명령어**

```bash
git status
```

**실행 결과**

```text
On branch main

No commits yet

Untracked files:
    Dockerfile
    README.md
    app/
    permission.txt

nothing added to commit but untracked files present
```

### 13-6. 첫 번째 커밋

**명령어**

```bash
git add .

git commit -m "Complete developer workstation mission"
```

**실행 결과**

```text
[main (root-commit) ea6c10b]
Complete developer workstation mission

5 files changed, 70 insertions(+)

create mode 100644 .gitignore
create mode 100644 Dockerfile
create mode 100644 README.md
create mode 100644 app/index.html
create mode 100644 permission.txt
```

Git 저장소 초기화와 첫 번째 커밋이 정상적으로 완료되었다.

---

## 14. GitHub 연동 및 인증

### 14-1. 원격 저장소 설정

**GitHub 저장소**

```text
https://github.com/030-4aud/codyssey_first_website
```

**명령어**

```bash
git remote add origin https://github.com/030-4aud/codyssey_first_website
```

### 14-2. 원격 저장소 확인

**명령어**

```bash
git remote -v
```

**실행 결과**

```text
origin  https://github.com/030-4aud/codyssey_first_website (fetch)
origin  https://github.com/030-4aud/codyssey_first_website (push)
```

### 14-3. GitHub Push 시도

**명령어**

```bash
git push -u origin main
```

**실행 결과**

```text
Username for 'https://github.com': 030-4aud
Password for 'https://030-4aud@github.com':

remote: Invalid username or token.
Password authentication is not supported for Git operations.

fatal: Authentication failed
```

GitHub는 일반 계정 비밀번호를 이용한 Git 작업 인증을 지원하지 않으므로 인증 오류가 발생하였다.

### 14-4. GitHub CLI 설치 시도

**명령어**

```bash
sudo apt update

sudo apt install gh -y
```

**실행 결과**

```text
Error: Unable to locate package gh
```

현재 Kali Linux의 기본 패키지 저장소에서는 해당 패키지를 찾지 못하였다.

### 14-5. 현재 진행 상태

Git 저장소 초기화, 브랜치 설정, 사용자 정보 설정, 첫 커밋까지 완료하였다.

다만 GitHub 인증 및 Push는 아직 완료하지 못하였다.

향후 GitHub CLI 설치 또는 SSH 인증 방식을 이용하여 인증을 완료하고 원격 저장소에 Push할 예정이다.

---

## 15. 문제 해결 과정

### 15-1. Zsh History 파일 오류

터미널 실행 중 다음 오류가 발생하였다.

```text
sh: corrupt history file /home/kali/.zsh_history
```

기존 History 파일을 백업하고 새로운 파일을 생성하였다.

```bash
mv ~/.zsh_history ~/.zsh_history.bak

touch ~/.zsh_history
```

기존 기록은 백업 파일에 보관하고 새로운 History 파일을 생성하였다.

### 15-2. Docker 권한 오류

일반 사용자 계정으로 Docker 데몬에 접근할 때 권한 오류가 발생하였다.

```text
permission denied while trying to connect to the Docker daemon socket
```

다음 명령어로 Docker 그룹에 사용자를 추가하였다.

```bash
sudo usermod -aG docker $USER

newgrp docker
```

이후 `docker ps` 명령어가 정상적으로 실행되는 것을 확인하였다.

### 15-3. GitHub 인증 오류

GitHub Push 과정에서 계정 비밀번호 인증을 시도하여 실패하였다.

GitHub 인증 방식을 별도로 설정한 후 다시 Push해야 한다는 점을 확인하였다.

---

## 16. 보안 및 개인정보 보호

프로젝트의 기술 문서와 로그를 공개할 때 다음 사항을 준수한다.

- GitHub 인증 토큰을 문서에 기록하지 않는다.
- 비밀번호와 개인키를 스크린샷에 노출하지 않는다.
- 인증 코드와 민감한 환경변수를 공개하지 않는다.
- Git 사용자 이메일은 공개 문서에서 마스킹한다.
- `.gitignore`를 이용하여 불필요한 파일과 민감한 파일이 커밋되지 않도록 관리한다.

인증 정보가 노출된 경우 해당 인증 정보를 폐기하고 재발급한다.

---

## 17. 최종 검증 및 증거 자료

| 검증 항목 | 검증 명령어 | 결과 |
|---|---|---|
| 현재 위치 | `pwd` | 확인 |
| 파일 목록 | `ls -la` | 확인 |
| 파일 조작 | `cp`, `mv`, `rm`, `touch` | 수행 |
| 파일 권한 | `chmod 644` | 성공 |
| 디렉토리 권한 | `chmod 755` | 성공 |
| Docker 버전 | `docker --version` | 성공 |
| Docker 서비스 | `systemctl status docker` | 실행 중 |
| 이미지 목록 | `docker images` | 확인 |
| 컨테이너 목록 | `docker ps -a` | 확인 |
| 컨테이너 로그 | `docker logs` | 확인 |
| 리소스 확인 | `docker stats` | 확인 |
| hello-world | `docker run hello-world` | 성공 |
| Ubuntu | `docker run -it ubuntu:24.04 bash` | 성공 |
| 커스텀 이미지 | `docker build` | 성공 |
| 웹 서버 실행 | `docker run` | 성공 |
| 포트 접속 | `curl localhost:8080` | 성공 |
| 바인드 마운트 | `-v` 옵션 | 성공 |
| 볼륨 영속성 | `docker volume` | 성공 |
| Git 설정 | `git config --list` | 성공 |
| Git 커밋 | `git commit` | 성공 |
| GitHub Push | `git push` | 인증 실패 |
