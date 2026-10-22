# Week 07 - Docker Compose

## 1. Docker Compose 구성
- Nginx: 외부 요청을 웹 서버에 전달
- Web: Flask 방명록 애플리케이션
- Redis: 방명록 데이터 저장
- Redis와 Web에 Healthcheck 적용

## 2. 웹 서버 확장
docker compose up -d --scale web=3

- Nginx 1개, Web 3개, Redis 1개 실행
- 새로고침 시 Pod/Host 값이 변경되는 것을 확인
- 웹 컨테이너 하나를 중지해도 서비스 접속 가능

## 3. 데이터 영속성
- docker compose down: 컨테이너와 네트워크를 제거하지만 볼륨은 유지
- docker compose down -v: 컨테이너, 네트워크와 볼륨을 제거
- Redis 볼륨을 유지하면 방명록 데이터도 유지됨

## 4. 환경 변수 보안
- .env에 Redis 비밀번호 저장
- .gitignore에 .env를 등록하여 GitHub 업로드 방지
- .env.example은 설정 예시로 공유

## 5. Watch 실습
- templates/index.html 수정
- Docker Compose Watch가 변경 사항을 감지
- 웹 이미지 자동 재빌드 및 컨테이너 재생성 확인
