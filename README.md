★# Mission 1 - 개발 환경 세팅
---

# 1. 프로젝트 개요

## 프로젝트 목표

본 미션에서는 Windows 환경에서 개발 워크스테이션을 구축하고 Docker와 Git을 활용하여 개발 환경을 구성하였다.

주요 수행 내용

- Docker Desktop 설치
- Git 설치 및 GitHub 연동
- VS Code 개발환경 구축
- Dockerfile 작성
- Docker 커스텀 이미지 생성
- 컨테이너 실행 및 포트 매핑 확인
- Bind Mount 및 Docker Volume 실습
- Git을 이용한 버전 관리

---

# 2. 실행 환경

|항목|내용|
|---|---|
|OS|Windows 11|
|Terminal|PowerShell|
|Editor|Visual Studio Code|
|Docker|Docker Desktop (버전 입력)|
|Git|(버전 입력)|

**설명:** 본 미션을 수행한 개발 환경이다.

(스크린샷)

---

# 3. 프로젝트 구조

```text
Mission-1
├── app
├── docs
├── screenshots
├── Dockerfile
└── README.md
```

**설명:** 프로젝트 디렉터리 구조이다.

(스크린샷)

---

# 4. 수행 체크리스트

- [ ] 터미널 명령 실습
- [ ] 파일 권한 실습
- [ ] Docker 설치
- [ ] Docker 기본 명령
- [ ] hello-world 실행
- [ ] Ubuntu 컨테이너 실습
- [ ] Dockerfile 작성
- [ ] 커스텀 이미지 생성
- [ ] 포트 매핑
- [ ] Bind Mount
- [ ] Docker Volume
- [ ] Git 설정
- [ ] GitHub 연동

---

# 5. 터미널 명령 실습

## 5-1. 현재 위치 확인

```powershell
pwd
```

**설명:** 현재 작업 중인 디렉터리 위치를 확인하였다.

스크린샷

## 5-2. 파일 목록 확인

```powershell
dir -Force
dir
```

**설명:** 숨김 파일을 포함한 전체 목록을 확인하였다(dir-Force) / (dir; 일반확인)

(스크린샷)

## 5-3. 파일 및 폴더 생성

```powershell
mkdir app docs test-floder 
ni test.txt
dir
```

**설명:** 프로젝트에 필요한 폴더와 파일을 생성하고 생성 결과를 확인하였다.

(스크린샷)

## 5-4. 디렉터리 이동

```powershell
cd test-folder
pwd
cd ..
```

**설명:** 디렉터리를 이동하고 현재 위치를 확인하였다.

(스크린샷)

## 5-5. 파일 복사

```powershell
Copy-Item test.txt test-copy.txt
dir
```

**설명:** 기존 파일을 복사하여 동일한 내용을 가진 새로운 파일을 생성하였다.

(스크린샷)

## 5-6. 파일 이동 및 이름 변경

```powershell
Rename-Item test-copy.txt test2.txt
Move-Item test2.txt test-folder
dir
dir test-folder
```

**설명:** 파일 이름을 변경한 후 다른 디렉터리로 이동하고 결과를 확인하였다.

(스크린샷)

## 5-7. 파일 내용 확인

```powershell
Set-Content test.txt "Hello Mission 1"
Get-Content test.txt
```

**설명:** 테스트 파일에 내용을 입력한 후 파일 내용을 확인하였다.

(스크린샷)


## 5-8. 파일 삭제

```powershell
cd test-folder
Remove-Item test2.txt
dir
cd ..
```

**설명:** 파일을 삭제하였다.

(스크린샷)


---

# 6. 파일 권한 실습

## 6-1. 권한 확인

```bash
mkdir permission-test
cd permission-test
touch test.txt
mkdir sample
ls -l
```

**설명:** Ubuntu 컨테이너 내부에서 권한 실습용 디렉터리와 테스트 파일을 생성하고 기본 권한을 확인하였다.

(스크린샷)

## 6-2. 파일/디렉터리 권한 변경

```bash
chmod 600 test.txt
ls -l test.txt

chmod 644 test.txt
ls -l test.txt

chmod 700 sample
ls -ld sample

chmod 755 sample
ls -ld sample
```

**설명:** Ubuntu 컨테이너에서 파일 권한을 600으로 변경한 후 644로 다시 변경하여 권한 변화를 확인하고, 디렉터리 권한을 700으로 변경한 후 755로 다시 변경하였다.

(스크린샷)
[권한변경 결과]
- `600` : 소유자만 읽기 및 쓰기가 가능하며, 그룹과 다른 사용자는 접근할 수 없다.
- `644` : 소유자는 읽기/쓰기가 가능하고, 그룹과 다른 사용자는 읽기만 가능하다.
- `700` : 디렉터리 소유자만 접근할 수 있다.
- `755` : 소유자는 모든 권한을 가지며, 그룹과 다른 사용자는 읽기 및 실행 권한을 가진다.

---

# 7. Docker 설치 및 점검

## 7-1. Docker 버전

```powershell
docker --version
```

**설명:** Docker 설치 여부와 버전을 확인했다.

(스크린샷)

## 7-2. Docker 정보

```powershell
docker info
```

**설명:** Docker Engine이 정상적으로 실행 중인지 확인하고 시스템 정보를 조회하였다.

(스크린샷)

## 7-3. Docker 이미지·컨테이너 확인

```powershell
docker images
docker ps -a
```

**설명:** (이미지) 로컬 시스템에 저장된 Docker 이미지를 확인하였다. Ubuntu 이미지가 정상적으로 저장되어 있는 것을 확인하였다.
(컨테이너) 실행 중인 Docker 컨테이너 목록을 확인했다.

(스크린샷)

---

# 8. Docker 기본 명령

## 8-1. 컨테이너 로그 확인

```powershell
docker logs ubuntu-permission
```

**설명:** 컨테이너 로그를 확인하였다.

(스크린샷)


## 8-2. Docker 리소스 사용량 확인

```powershell
docker stats
```

**설명:** CPU 및 메모리 사용량을 확인하였다.

(스크린샷)

## 8-3. 컨테이너 중지 및 상태 확인

```powershell
docker stop ubuntu-permission
docker ps -a
```

**설명:** 실행 중인 Ubuntu 컨테이너를 중지하고 상태가 Exited로 변경된 것을 확인하였다.

(스크린샷)

## 8-4. hello-world 실행

```powershell
docker run hello-world
```

**설명:**  `hello-world` 이미지를 실행하여 Docker Engine이 정상적으로 동작하는 것을 확인하였다.

(스크린샷)

## 8-5. Ubuntu 컨테이너 실행 및 내부 명령 실행

```powershell
docker start ubuntu-permission
docker exec -it ubuntu-permission bash
echo "Hello Docker"
pwd
ls
```

**설명:** 기존 Ubuntu 컨테이너를 시작한 후 `docker exec` 명령으로 내부에 접속하여 문자열 출력, 현재 위치 확인, 파일 목록 확인 명령을 실행하였다.


(스크린샷)




---

# 9. Dockerfile 기반 커스텀 이미지

## 9-1. Dockerfile 확인

```dockerfile
FROM nginx:latest
COPY app/index.html /usr/share/nginx/html/index.html
```

**설명:** Nginx 공식 이미지를 베이스 이미지로 사용하고, 직접 작성한 index.html 파일을 Nginx 기본 웹 경로로 복사하도록 Dockerfile을 구성하였다.

(스크린샷)

## 9-2. index.html 확인


**설명:** Nginx 컨테이너에서 제공할 정적 웹 페이지 소스코드를 확인하였다.

(스크린샷)


## 9-3. Docker 이미지 빌드

```powershell
docker build -t mission-1web .
```

**설명:** Dockerfile을 기반으로 `mission-1web`이라는 이름의 커스텀 이미지를 생성하였다.

(스크린샷)


## 9-4. Docker 이미지 생성 확인

```powershell
docker images .
```

**설명:** mission-1web 커스텀 이미지가 정상적으로 생성된 것을 확인하였다.


## 9-5 컨테이너 실행(포트매핑)

```powershell
docker run -d -p 8080:80 --name mission1-container mission-1web
```

**설명:** mission-1web 이미지를 기반으로 컨테이너를 백그라운드에서 실행하고, 호스트의 8080 포트를 컨테이너의 80 포트와 연결하였다. docker ps 결과에서 컨테이너 상태가 Up이며 0.0.0.0:8080->80/tcp 포트 매핑이 적용된 것을 확인하였다.

(스크린샷)

## 9-6. 브라우저 확인

http://localhost:8080

**설명:** 포트 매핑이 정상 동작하는 것을 확인하였다.

(스크린샷)

---

# 10. Bind Mount

실행 명령

```powershell
docker run ...
```

**설명:** 호스트 파일 변경이 컨테이너에 즉시 반영되는 것을 확인하였다.

(스크린샷)

---

# 11. Docker Volume

생성 → 연결 → 데이터 생성 → 컨테이너 삭제 → 재실행 → 데이터 유지 확인

**설명:** 컨테이너 삭제 후에도 데이터가 유지되는 것을 확인하였다.

(스크린샷)

---

# 12. Git 설정

```powershell
git config --global user.name
git config --global user.email
git config --global init.defaultBranch main
git config --list
```

**설명:** Git 사용자 정보 및 기본 브랜치를 설정하였다.

(스크린샷)

---

# 13. GitHub 및 VS Code 연동

- GitHub 로그인
- 저장소 연결
- Push 성공

**설명:** VS Code와 GitHub를 연동하였다.

(스크린샷)

---

# 14. 검증 방법

|검증 항목|명령어|증거|
|---|---|---|
|Docker 설치|docker --version|(스크린샷)|
|Docker 실행|docker info|(스크린샷)|
|이미지|docker images|(스크린샷)|
|컨테이너|docker ps|(스크린샷)|
|로그|docker logs|(스크린샷)|
|포트 매핑|http://localhost:8080|(스크린샷)|
|Volume|docker volume|(스크린샷)|

---

# 15. 트러블슈팅

## 사례 1

- 문제: docker build 명령 실행 시 다음과 같은 오류 발생
```text
ERROR: docker: 'docker buildx build' requires 1 argument
```
- 원인: `docker build -t mission-1web` 명령에서 **빌드 대상 경로(PATH)** 를 지정하지 않아 Docker가 Dockerfile의 위치를 찾지 못함
- 확인: 명령어 마지막에 현재 디렉터리를 의미하는 `.`(점)이 누락된 것을 확인
- 해결: 명령어 마지막에 현재 디렉터리를 의미하는 `.`을 추가하여 다시 실행하고、이미지가 정상적으로 생성되는 것을 확인함

## 사례 2

- 문제:
- 원인:
- 확인:
- 해결:

---

# 16. 학습 내용

- 절대경로와 상대경로
- 파일 권한(r/w/x)
- 755, 644 의미
- Dockerfile
- 포트 매핑
- Docker Volume
- Git과 GitHub 차이

---

# 17. 보안

- 개인정보 마스킹
- 토큰/비밀번호 제거
- 재현 가능한 절차 작성

