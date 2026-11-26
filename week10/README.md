# Week 10 - Kubernetes Deployment & Service

## 1. 실습 목표
Kubernetes의 Deployment와 Service를 이용하여
Guestbook 애플리케이션을 배포하고,
스케일링, 자가 치유, 롤링 업데이트, 롤백을 실습한다.

## 2. Deployment 생성
- deployment.yaml 작성
- replicas: 2 설정
- Guestbook v1 이미지 배포
- Deployment, ReplicaSet, Pod 상태 확인

## 3. Service 생성
- service.yaml 작성
- NodePort 방식 사용
- Service 포트: 80
- 컨테이너 포트: 5000
- NodePort: 30080
- 브라우저에서 Guestbook 접속 확인

## 4. 스케일링
- Pod 2개에서 4개로 확장
- 로드밸런싱 확인
- Pod를 다시 2개로 축소

## 5. 롤링 업데이트
- Guestbook v2 이미지 빌드
- GitHub Container Registry에 v2 업로드
- kubectl set image 명령어로 v1에서 v2로 변경
- ReplicaSet 변경 확인

## 6. 롤백
- kubectl rollout undo 명령어 실행
- v2에서 v1으로 복구
- 배포 기록 및 이미지 확인

## 7. 자가 치유 실습
- Pod를 5개로 확장
- 각 Pod의 hostname 확인
- Pod 1개 삭제
- 새로운 Pod 자동 생성 확인
- Pod 5개 정상 실행 확인

## 8. 실습 결과
Deployment와 Service를 이용한 배포에 성공하였다.
스케일링, 롤링 업데이트, 롤백 및
자가 치유 기능이 정상 작동하는 것을 확인하였다.

## 9. 실습 파일
- [deployment.yaml](../guestbook/k8s/deployment.yaml)
- [service.yaml](../guestbook/k8s/service.yaml)
