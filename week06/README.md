# DevOps 6주차 - Dockerfile

## 1. 실습 목표
- Dockerfile을 이용한 이미지 빌드
- Docker 빌드 캐시 확인
- 환경 변수 설정 및 변경
- GHCR과 Docker Hub에 이미지 업로드
- 멀티 플랫폼 이미지 빌드

## 2. Dockerfile 주요 명령어
- FROM: 베이스 이미지 지정
- WORKDIR: 작업 디렉터리 설정
- COPY: 파일 복사
- RUN: 이미지 빌드 중 명령어 실행
- ENV: 환경 변수 설정
- EXPOSE: 컨테이너 포트 정보 명시
- CMD: 컨테이너 시작 시 실행할 명령어

## 3. Docker 이미지
- GHCR v1: ghcr.io/kikikikiiii/guestbook:v1
- GHCR v2: ghcr.io/kikikikiiii/guestbook:v2
- GHCR 멀티 플랫폼: ghcr.io/kikikikiiii/guestbook:multi
- Docker Hub: choinarin/guestbook:v1

## 4. 빌드 캐시
requirements.txt를 먼저 복사하고 패키지를 설치하면
HTML 파일을 수정해도 패키지 설치 단계의 캐시를 재사용할 수 있다.

## 5. 환경 변수 우선순위
docker run -e 옵션으로 지정한 값이
Dockerfile의 ENV 기본값보다 우선한다.

## 6. 개인 과제
- 방명록 제목을 '나린이의 방명록'으로 변경
- 테마 색상을 분홍색으로 변경
- HTML 안내 문구 수정
- guestbook:v2 이미지 빌드 및 GHCR 업로드

## 7. 실습 결과
Dockerfile을 이용해 방명록 이미지를 직접 생성하고
GitHub Container Registry와 Docker Hub에 업로드하였다.

## 8. 추가 실습
- 친구 이미지 교환 및 커스터마이징: 미진행
