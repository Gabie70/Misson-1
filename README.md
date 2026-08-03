1주차: 개발 환경 구축 보고서
(스크린샷 추가)
1. 프로젝트 개요
본 과제의 목표는 개발 환경을 직접 구축하고, 터미널, Git, Docker를 활용하여 재현 가능한 실행 환경을 만드는 것이다.

2. 실습 환경
OS: Windows 11
Shell: Git Bash
Docker: 29.2.0
Git: 2.53.0
작성일: 2026-03-31
3. Phase 1: 터미널 기초 + 권한 실습
3-1. Git 설정 확인
git config --list
설명: 사용자 정보(user.name, user.email)가 정상 등록된 것을 확인했다.

git config

3-2. 현재 위치 확인
pwd
설명: 현재 작업 디렉토리 위치를 확인하였다.

pwd

3-3. 파일 및 폴더 생성
mkdir app docs
touch README.md test.txt
ls -la
설명: 폴더와 파일을 생성하고 숨김 파일 포함 목록을 확인하였다.

ls -la

3-4. 파일 복사 및 이름 변경
cp test.txt copy.txt
mv copy.txt renamed.txt
설명: 파일을 복사하고 이름을 변경하였다.

rename

3-5. 파일 삭제
rm renamed.txt
설명: 파일 삭제 후 목록에서 사라진 것을 확인하였다.

remove

3-6. 권한 변경
chmod 644 test.txt
chmod 755 app
ls -la
설명: 파일과 폴더 권한을 변경하고 결과를 확인하였다.

chmod

3-7. 폴더 구조 확인
ls
설명: 전체 프로젝트 구조를 확인하였다.

folder

3-8. Git 초기화 및 연결
git init
git remote add origin <repository-url>
설명: Git 저장소를 초기화하고 GitHub와 연결하였다.

git init

4. 권한 숫자 해석
표기	숫자	권한 설명
rw-r--r--	644	소유자(읽기/쓰기), 그룹/기타(읽기만)
rwxr-xr-x	755	소유자(모든 권한), 그룹/기타(읽기/실행)
5. Git 파일 추적 상태 확인
문제 상황
git status
출력:

Untracked files:
  images/project-structure.png
원인
새로 생성한 파일은 Git이 자동으로 추적하지 않는다.
해결 방법
git add images/project-structure.png
git commit -m "Add: project structure screenshot"
git push origin main
설명
git add: 파일을 추적 대상으로 등록
git commit: 변경사항 저장
git push: 원격 저장소에 업로드
실행 결과
git untracked

6. Phase 4: Docker hello-world 실행
실행 명령
docker run hello-world
출력 결과
Hello from Docker!
This message shows that your installation appears to be working correctly.
설명
Docker Hub에서 이미지를 다운로드하여 실행하였다.
Docker 엔진이 정상적으로 동작함을 확인하였다.
동작 과정 이해
Docker client → daemon 요청
daemon → Docker Hub 이미지 다운로드
컨테이너 생성 및 실행
결과 출력
실행 결과
docker hello world

7. 프로젝트 구조
설명: 최종 작업 디렉토리 구조

project structure

8. ubuntu 컨테이너 실행 및 내부 진입
실행 명령
docker run -it --name ubuntu-test ubuntu bash
내부 명령 실행
ls
echo "hello ubuntu"
pwd
종료 후 상태 확인
exit
docker ps -a
설명
ubuntu 이미지를 다운로드하고 컨테이너를 실행하였다.
컨테이너 내부에서 ls, echo, pwd 명령을 실행하였다.
exit 후 docker ps -a를 통해 컨테이너 상태가 Exited로 변경된 것을 확인하였다.
전체 실행 과정을 하나의 흐름으로 확인하였다.
실행 결과
ubuntu container full process

attach와 exec 차이
attach: 실행 중인 컨테이너의 표준 입출력에 연결
9. Dockerfile 기반 커스텀 웹 서버 이미지 제작
베이스 이미지 선택
nginx:alpine 이미지를 베이스로 선택하였다.
정적 HTML 파일만 교체하는 방식으로 커스텀 이미지를 제작하였다.
Dockerfile
FROM nginx:alpine

LABEL org.opencontainers.image.title="codyssey-web"
ENV APP_ENV=dev

COPY app/index.html /usr/share/nginx/html/index.html
웹 서버 소스 코드
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <title>Codyssey Week 1</title>
</head>
<body>
  <h1>안녕하세요, Codyssey 1주차 과제입니다.</h1>
  <p>Dockerfile 기반 커스텀 이미지 실행 성공</p>
</body>
</html>
빌드 및 실행 명령
docker build -t codyssey-web:1.0 .
docker run -d -p 8080:80 --name codyssey-web-container codyssey-web:1.0
docker ps
docker logs codyssey-web-container
설명
NGINX 웹 서버 이미지를 기반으로 커스텀 이미지를 생성하였다.
COPY 명령어를 통해 HTML 파일을 컨테이너 내부로 복사하였다.
-p 8080:80 옵션으로 로컬 포트와 컨테이너 포트를 연결하였다.
브라우저 접속을 통해 웹 서버 정상 실행을 확인하였다.
실행 결과
1) 이미지 빌드 성공
docker build

2) 컨테이너 실행 및 포트 매핑 확인
docker run

3) 컨테이너 로그 확인
docker logs

4) 브라우저 접속 결과
browser

10. 바인드 마운트 반영 확인
실행 및 문제 확인
bind mount run

문제 상황
Git Bash 환경에서 경로 변환 문제로 인해 /usr/share/nginx/html 접근 실패 발생
해결 결과
bind mount success

설명
로컬 app 폴더를 컨테이너에 연결
브라우저에서 수정한 HTML이 정상 출력됨을 확인
11. Docker 볼륨 영속성 검증
실행 명령
docker volume create mydata
docker run -d --name vol-test -v mydata:/data ubuntu sleep infinity
docker exec -it vol-test bash -lc "echo hi > /data/hello.txt && cat /data/hello.txt"
docker rm -f vol-test
docker run -d --name vol-test2 -v mydata:/data ubuntu sleep infinity
docker exec -it vol-test2 bash -lc "cat /data/hello.txt"
설명
Docker 볼륨 mydata를 생성하였다.
첫 번째 컨테이너에서 /data/hello.txt 파일을 생성하고 데이터를 저장하였다.
컨테이너를 삭제한 뒤 동일한 볼륨을 연결한 새로운 컨테이너를 실행하였다.
삭제 이후에도 데이터가 유지되는 것을 확인하였다.
실행 결과
1) 볼륨 생성
docker volume create

2) 데이터 생성 및 확인
docker volume before delete

3) 컨테이너 삭제 후 데이터 유지 확인
docker volume after delete

exec: 실행 중인 컨테이너에 새로운 프로세스를 실행

실습에서는 exec 방식이 더 안전하고 직관적이었다.

🔧 6-1. Docker 버전 확인 (평가기준 3 보완)
docker --version
설명:

Docker 설치 여부 및 버전을 확인하였다.
본 실습에서는 Docker 29.2.0 환경에서 진행하였다.
📁 7-1. 프로젝트 구조 설계 기준
설명:

app: 웹 서버에서 실행될 HTML 파일을 저장하는 디렉토리
docs: 문서 및 보고서 관리용 디렉토리
images: 실습 증거 스크린샷을 저장하는 디렉토리
README.md: 전체 실습 과정을 정리한 문서
→ 기능별 역할 분리를 기준으로 디렉토리를 구성하였다.

🔁 11-1. 재현 가능한 실행 절차 정리
1) 이미지 빌드
docker build -t codyssey-web:1.0 .
2) 컨테이너 실행
docker run -d -p 8080:80 --name codyssey-web-container codyssey-web:1.0
3) 브라우저 확인
http://localhost:8080
4) 볼륨 실행
docker volume create mydata
docker run -d --name vol-test -v mydata:/data ubuntu sleep infinity
→ 위 과정을 통해 동일한 환경을 누구나 재현할 수 있다.

⚙️ 12-1. 이미지 vs 컨테이너 차이
구분	설명
이미지	실행을 위한 “설계도” (읽기 전용)
컨테이너	이미지를 기반으로 실행된 “실행 인스턴스”
관점별 차이
빌드: 이미지 생성 과정
실행: 컨테이너 실행
변경: 컨테이너 내부 변경은 이미지에 자동 반영되지 않음
🌐 12-2. 컨테이너 포트 접근 구조
설명:

컨테이너 내부 포트(80)는 외부에서 직접 접근할 수 없다.
Docker는 독립된 네트워크 공간을 사용하기 때문이다.
해결 방법
docker run -p 8080:80
→ 호스트(로컬) 포트 8080과 컨테이너 포트 80을 연결하여 접근 가능하게 만든다.

📍 12-3. 절대 경로 vs 상대 경로
구분	설명
절대 경로	/usr/share/nginx/html처럼 루트 기준 경로
상대 경로	현재 위치 기준 경로 (./app)
선택 기준
컨테이너 내부: 절대 경로 사용 (명확성)
로컬 개발 환경: 상대 경로 사용 (유연성)
🚨 13-1. 포트 충돌 문제 해결 절차
문제:

이미 사용 중인 포트를 사용할 경우 컨테이너 실행 실패
진단 순서
docker ps
netstat -ano | findstr :8080
해결 방법
기존 컨테이너 종료
docker rm -f <컨테이너명>
또는 다른 포트 사용
docker run -p 8081:80
💾 13-2. 데이터 유실 방지 전략
문제:

컨테이너 삭제 시 내부 데이터는 기본적으로 사라짐
해결 방법
Docker Volume 사용
docker volume create mydata
장점
컨테이너 삭제 후에도 데이터 유지
여러 컨테이너에서 공유 가능
→ 실제 운영 환경에서는 반드시 볼륨 또는 외부 저장소를 사용해야 한다
