# Mission 1 - 개발 환경 세팅

> **제출용 README 템플릿**
>
> **사용 방법**
> - `(스크린샷)` 위치에 캡처 이미지를 삽입합니다.
> - Docker/Git 버전과 실제 명령 결과는 본인 환경에 맞게 수정합니다.

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

```bash
pwd
```

**설명:** 현재 작업 중인 디렉터리 위치를 확인하였다.

(스크린샷)

## 5-2. 파일 목록 확인

```bash
ls -la
```

**설명:** 숨김 파일을 포함한 전체 목록을 확인하였다.

(스크린샷)

## 5-3. 파일 및 폴더 생성

```bash
mkdir app docs screenshots
touch README.md Dockerfile app/index.html
ls -la
```

**설명:** 프로젝트에 필요한 폴더와 파일을 생성하고 생성 결과를 확인하였다.

(스크린샷)

## 5-4. 디렉터리 이동

```bash
cd app
pwd
cd ..
```

**설명:** 디렉터리를 이동하고 현재 위치를 확인하였다.

(스크린샷)

## 5-5. 파일 복사

```bash
cp README.md README-copy.md
```

**설명:** 파일 복사를 수행하였다.

(스크린샷)

## 5-6. 파일 이동 및 이름 변경

```bash
mv README-copy.md docs/README-copy.md
```

**설명:** 파일을 이동하였다.

(스크린샷)

## 5-7. 파일 삭제

```bash
rm docs/README-copy.md
```

**설명:** 파일을 삭제하였다.

(스크린샷)

## 5-8. 파일 내용 확인

```bash
cat README.md
```

**설명:** 파일 내용을 확인하였다.

(스크린샷)

---

# 6. 파일 권한 실습

## 권한 확인

```bash
ls -l
```

**설명:** 파일 및 디렉터리 권한을 확인하였다.

(스크린샷)

## 파일 권한 변경

```bash
chmod 644 test.txt
```

**설명:** 파일 권한을 644로 변경하였다.

(스크린샷)

## 디렉터리 권한 변경

```bash
chmod 755 sample
```

**설명:** 디렉터리 권한을 755로 변경하였다.

(스크린샷)

---

# 7. Docker 설치 및 점검

## Docker 버전

```bash
docker --version
```

**설명:** Docker 설치 여부를 확인하였다.

(스크린샷)

## Docker 정보

```bash
docker info
```

**설명:** Docker Engine이 정상 실행 중인지 확인하였다.

(스크린샷)

---

# 8. Docker 기본 명령

## hello-world 실행

```bash
docker run hello-world
```

**설명:** Docker 컨테이너 실행을 확인하였다.

(스크린샷)

## Ubuntu 컨테이너

```bash
docker run -it ubuntu
ls
echo Hello
```

**설명:** Ubuntu 컨테이너 내부 명령을 실행하였다.

(스크린샷)

## 이미지 목록

```bash
docker images
```

**설명:** Docker 이미지 목록을 확인하였다.

(스크린샷)

## 컨테이너 목록

```bash
docker ps
docker ps -a
```

**설명:** 실행 중인 컨테이너와 전체 컨테이너를 확인하였다.

(스크린샷)

## 로그 확인

```bash
docker logs <container_name>
```

**설명:** 컨테이너 로그를 확인하였다.

(스크린샷)

## 리소스 확인

```bash
docker stats
```

**설명:** CPU 및 메모리 사용량을 확인하였다.

(스크린샷)

---

# 9. Dockerfile 기반 커스텀 이미지

## 베이스 이미지

- nginx:latest

**설명:** Nginx 이미지를 기반으로 커스텀 이미지를 제작하였다.

## Dockerfile

```dockerfile
FROM nginx:latest
COPY app/index.html /usr/share/nginx/html/index.html
```

(스크린샷)

## 이미지 빌드

```bash
docker build -t mission-1web .
```

**설명:** Dockerfile을 이용해 이미지를 생성하였다.

(스크린샷)

## 컨테이너 실행

```bash
docker run -d -p 8080:80 --name mission1-container mission-1web
```

**설명:** 포트 매핑을 적용하여 컨테이너를 실행하였다.

(스크린샷)

## 브라우저 확인

http://localhost:8080

**설명:** 포트 매핑이 정상 동작하는 것을 확인하였다.

(스크린샷)

---

# 10. Bind Mount

실행 명령

```bash
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

```bash
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

- 문제:
- 원인:
- 확인:
- 해결:

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

---

# 18. 요구사항 충족표

|요구사항|README 위치|
|---|---|
|터미널 명령|5장|
|권한 실습|6장|
|Docker|7~11장|
|Dockerfile|9장|
|포트 매핑|9장|
|Bind Mount|10장|
|Volume|11장|
|Git/GitHub|12~13장|
|트러블슈팅|15장|
