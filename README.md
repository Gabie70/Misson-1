# Mission 1 - Docker Basic Practice

## Overview

이번 미션에서는 Docker와 Git의 기본 사용법을 실습하였다.

주요 목표는 다음과 같다.

- Linux 기본 명령어 학습
- Docker Image 생성
- Docker Container 실행
- Dockerfile 작성
- Port Mapping 이해
- Docker Volume을 이용한 데이터 영속성 확인
- Bind Mount를 이용한 실시간 파일 공유
- GitHub를 통한 프로젝트 관리

---

# Development Environment

| Item | Value |
|------|------|
| OS | macOS |
| Terminal | zsh |
| Editor | Visual Studio Code |
| Docker | Docker Engine (OrbStack) |
| Architecture | x86_64 |

---

# Project Structure

```
Misson-1
├── app
│   └── index.html
├── Dockerfile
├── README.md
├── docs
└── screenshots
```

---

# 1. Docker Installation

## Purpose

Docker가 정상적으로 설치되었는지 확인한다.

## Command

```bash
docker --version
```

Result

```
Docker version 28.5.2
```

Docker Engine이 정상적으로 설치된 것을 확인하였다.

---

## Docker Engine 확인

```bash
docker info
```

확인 내용

- Docker Engine 정상 실행
- OrbStack 사용
- x86_64 Architecture

---

# 2. Docker Test

## Purpose

Docker가 정상적으로 Container를 실행할 수 있는지 확인한다.

## Command

```bash
docker run hello-world
```

Result

```
Hello from Docker!
```

### What I Learned

hello-world 이미지는 Docker 설치 여부를 확인하는 가장 기본적인 테스트 이미지이다.

Docker Client와 Docker Engine이 정상적으로 통신하는 것을 확인하였다.

---

# 3. Dockerfile

## Purpose

사용자 정의 Docker 이미지를 생성하기 위해 Dockerfile을 작성하였다.

## Dockerfile

```dockerfile
FROM nginx:latest

COPY app/index.html /usr/share/nginx/html/index.html
```

### What I Learned

- FROM은 사용할 Base Image를 지정한다.
- COPY는 로컬 파일을 컨테이너 내부로 복사한다.

---

# 4. Build Docker Image

## Purpose

Dockerfile을 이용하여 새로운 이미지를 생성한다.

## Command

```bash
docker build -t mission-1web .
```

이미지 확인

```bash
docker images
```

Result

```
mission-1web
```

### What I Learned

Docker Image는 Container를 생성하기 위한 설계도(template) 역할을 한다.

---

# 5. Run Container

## Purpose

생성한 이미지를 실행하여 웹 서버를 확인한다.

## Command

```bash
docker run -d -p 8080:80 --name mission1-container mission-1web
```

### Explanation

| Option | Meaning |
|--------|---------|
| -d | Background 실행 |
| -p | Port Mapping |
| --name | Container 이름 지정 |

브라우저에서

```
http://localhost:8080
```

접속하여 웹페이지가 정상적으로 표시되는 것을 확인하였다.

### What I Learned

Host의 8080 Port를 Container의 80 Port와 연결하면 브라우저를 통해 Container 내부의 웹 서버에 접속할 수 있다.

---

# 6. Docker Basic Commands

다음 명령어를 실습하였다.

| Command | Description |
|----------|-------------|
| docker ps | 실행 중인 Container 확인 |
| docker ps -a | 전체 Container 확인 |
| docker images | Image 목록 확인 |
| docker stop | Container 중지 |
| docker start | Container 시작 |
| docker rm | Container 삭제 |
| docker logs | 로그 확인 |
| docker exec | Container 내부 명령 실행 |

### What I Learned

Container의 생성, 실행, 중지, 삭제 과정을 이해하였다.

---

# 7. Ubuntu Container

## Purpose

Linux Container 내부에서 기본 명령어를 실습한다.

## Command

```bash
docker run -it --name ubuntu-test ubuntu bash
```

실습한 명령어

```bash
pwd
ls
echo "Hello from Ubuntu"
whoami
```

### Result

```
/
root
```

### What I Learned

Docker Container 내부에서도 일반 Linux 환경처럼 명령어를 사용할 수 있다는 것을 확인하였다.

---

# 8. Docker Volume

## Purpose

Container를 삭제해도 데이터가 유지되는지 확인한다.

Volume 생성

```bash
docker volume create my-volume
```

Container 생성

```bash
docker run -it --name ubuntu-volume -v my-volume:/data ubuntu bash
```

파일 생성

```bash
echo "Docker Volume Test" > /data/test.txt
```

확인

```bash
cat /data/test.txt
```

Container 삭제

```bash
docker rm -f ubuntu-volume
```

새 Container 생성

```bash
docker run -it --name ubuntu-volume2 -v my-volume:/data ubuntu bash
```

파일 확인

```bash
cat /data/test.txt
```

### Result

```
Docker Volume Test
```

### What I Learned

Container는 삭제되었지만 Volume은 삭제되지 않았기 때문에 데이터가 유지되었다.

Docker Volume은 영구 데이터를 저장하기 위해 사용된다.

---

# 9. Bind Mount

## Purpose

Host의 파일을 Container와 공유한다.

## Command

```bash
docker run -d \
-p 8080:80 \
--name mission-bind \
-v "$(pwd)/app:/usr/share/nginx/html" \
nginx:latest
```

VS Code에서

```
app/index.html
```

파일을 수정한 후

브라우저를 새로고침하였다.

### Result

이미지를 다시 Build하지 않아도 수정 내용이 즉시 반영되었다.

### What I Learned

Bind Mount는 Host와 Container가 동일한 파일을 공유하므로 개발 과정에서 매우 효율적이다.

---

# 10. Git

프로젝트를 GitHub Repository에 업로드하였다.

```bash
git add .
git commit -m "Complete Docker Mission 1"
git push -u origin main
```

### What I Learned

Git은 로컬 버전 관리를 담당하고 GitHub는 원격 저장소 역할을 한다.

---

# Problems & Solutions

| Problem | Solution |
|----------|----------|
| Docker 설치 안됨 | Docker Desktop(OrbStack) 설치 후 해결 |
| Dockerfile Parse Error | 잘못 입력한 `</> dockerfile` 제거 |
| Port 8080 already allocated | 기존 Container 확인 후 중지 또는 삭제 |
| Git Push 실패 | Commit 생성 후 Push 수행 |
| GitHub 인증 실패 | Personal Access Token 사용 |

---

# Conclusion

이번 미션을 통해 Docker의 기본 개념(Image, Container, Dockerfile, Port Mapping)을 이해하였다.

또한 Docker Volume과 Bind Mount의 차이를 직접 실습하며 데이터 영속성과 Host-Container 파일 공유 방식을 확인하였다.

Git과 GitHub를 이용하여 프로젝트를 관리하고 원격 저장소에 업로드하는 과정까지 수행하였다.