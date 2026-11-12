
# DevOps 11주차 - 쿠버네티스 설정과 상태 점검

## 1. 실습 목표
- ConfigMap과 Secret을 이용한 환경변수 관리
- Redis를 이용한 방명록 데이터 저장
- Deployment의 Pod 개수 조절
- Liveness Probe와 Readiness Probe 설정
- kubectl 명령어를 이용한 장애 확인 및 복구

## 2. ConfigMap과 Secret

ConfigMap을 생성하여 APP_TITLE, THEME_COLOR, REDIS_HOST, REDIS_PORT를 설정하였다.

Secret에는 REDIS_PASSWORD를 저장하였다.

Secret의 데이터는 Base64로 인코딩되며, 디코딩하면 원래 값을 확인할 수 있다. Base64는 암호화가 아니므로 Secret 파일 관리에 주의해야 한다.

## 3. Redis 연결 및 Pod 확장

Redis Deployment와 Service를 생성하고 Guestbook에 연결하였다.

실행 로그:
```text
[INFO] Redis 연결 성공: redis:6379
```

Guestbook Pod를 4개로 확장하였으며, 모든 Pod가 정상 실행되는 것을 확인하였다.

방명록에 글을 작성한 뒤 새로고침해도 글이 유지되는 것을 확인하였다.

## 4. ConfigMap 변경

APP_TITLE을 `New 방명록 (k8s)`로 변경하였다.

THEME_COLOR를 `#e11d48`로 변경하였다.

ConfigMap 변경 후 Deployment를 재시작하여 변경된 설정을 적용하였다.

## 5. Readiness Probe

Readiness Probe는 컨테이너가 사용자 요청을 처리할 준비가 되었는지 확인한다.

Redis Deployment를 0개로 줄이자 Guestbook Pod의 READY가 `1/1`에서 `0/1`로 변경되었다.

Redis를 다시 실행하자 READY가 `1/1`로 복구되었다.

## 6. Liveness Probe

Liveness Probe는 컨테이너가 정상적으로 실행 중인지 확인한다.

정상 경로인 `/healthz`를 존재하지 않는 `/devops`로 변경하였다.

그 결과 Guestbook Pod의 RESTARTS가 0에서 1 이상으로 증가하였다.

이후 경로를 `/healthz`로 복구하였고, 새로운 Pod가 정상 실행되었다.

## 7. 트러블슈팅 과정

1. `kubectl get pods`로 Pod 상태를 확인한다.
2. `kubectl describe pod`로 설정과 이벤트를 확인한다.
3. `kubectl logs`로 실행 기록과 오류 원인을 확인한다.

## 8. 실습 결과

- ConfigMap 및 Secret 설정 완료
- Redis 연결 성공
- Guestbook Pod 확장 성공
- Redis 데이터 유지 확인
- ConfigMap 변경 적용 성공
- Readiness Probe 장애 및 복구 확인
- Liveness Probe 자동 재시작 및 복구 확인

이번 실습을 통해 쿠버네티스의 설정 관리와 자동 상태 점검 기능을 이해하였다.
