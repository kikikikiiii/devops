cat > README.md <<'EOF'
# 5주차 - Docker

## 1. 학습 목표

Docker의 기본 개념을 이해하고 nginx 컨테이너를 실행한다.

## 2. Docker 기본 명령어

| 명령어 | 설명 |
|---|---|
| docker --version | Docker 버전 확인 |
| docker run hello-world | Docker 실행 환경 테스트 |
| docker ps | 실행 중인 컨테이너 확인 |
| docker ps -a | 전체 컨테이너 확인 |
| docker images | 이미지 목록 확인 |
| docker logs web | 컨테이너 로그 확인 |
| docker stop web | 컨테이너 정지 |
| docker rm -f web | 컨테이너 삭제 |

## 3. nginx 컨테이너 실행

docker run -d -p 8080:80 --name web nginx

- -d: 백그라운드 실행
- -p 8080:80: 호스트 8080 포트를 컨테이너 80 포트에 연결
- --name web: 컨테이너 이름 지정

브라우저에서 http://localhost:8080 접속을 확인했다.

## 4. 실습 결과

- Docker 버전 확인 성공
- hello-world 실행 성공
- nginx 웹서버 실행 성공
- curl 명령어로 HTTP 응답 확인
- Docker 이미지와 컨테이너의 차이 이해

## 5. 배운 점

Docker를 이용하면 프로그램을 직접 설치하지 않고도 컨테이너로 실행할 수 있다.

또한 컨테이너 내부에서 파일을 수정하더라도 컨테이너를 삭제하고 다시 생성하면 수정한 내용이 사라진다는 것을 확인했다.
EOF

## 6. 혼자서 해보기 - nginx 컨테이너 3개

Docker를 이용해 nginx 컨테이너 3개를 각각 다른 포트로 실행했다.

| 컨테이너 | 호스트 포트 | 웹페이지 |
|---|---|---|
| nginx1 | 8091 | Welcome to nginx1 |
| nginx2 | 8092 | Welcome to nginx2 |
| nginx3 | 8093 | Welcome to nginx3 |

각 컨테이너의 index.html 파일을 수정하여 서로 다른 페이지를 표시했다.

curl 명령어로 세 페이지의 응답을 확인했으며,
실행 결과는 curl-nginx-results.txt에 저장했다.

컨테이너는 서로 격리되어 있어 내부 포트 80을 동일하게 사용할 수 있지만,
호스트 포트는 서로 다르게 지정해야 한다.
