# 13주차 - GitOps 및 모니터링

## 1. 실습 목표

- Argo CD를 활용한 GitOps 자동 배포
- selfHeal과 prune 동작 확인
- Git을 이용한 롤백
- CI/CD 파이프라인과 Argo CD 연동
- Prometheus와 Grafana를 활용한 모니터링

## 2. Argo CD 설치 및 설정

- Argo CD 설치 및 관리자 로그인
- argocd-application.yaml 작성
- GitHub 저장소의 main 브랜치 연동
- 자동 동기화, selfHeal, prune 설정

실행 결과: Healthy, Synced 상태 확인

## 3. GitOps 실습

### Replica 변경
- Deployment의 replicas를 4개에서 2개로 변경
- PR 생성 및 Merge
- Argo CD를 통한 자동 반영 확인

### selfHeal
- kubectl 명령어로 replicas를 10개로 변경
- Argo CD가 Git에 기록된 2개로 자동 복구하는 것을 확인

### 롤백
- git revert를 이용하여 이전 설정으로 복구
- guestbook Pod 4개가 정상 실행되는 것을 확인

### prune
- hello ConfigMap 생성 및 GitHub 반영
- Git에서 hello.yaml 삭제
- Argo CD가 ConfigMap을 자동 삭제하는 것을 확인

## 4. CI/CD 이미지 배포

- guestbook 웹 페이지 제목을 CI/CD로 변경
- GitHub Actions를 통한 이미지 빌드 및 GHCR 게시
- deployment.yaml의 이미지 태그 변경
- PR 및 Merge를 통한 자동 배포
- 브라우저에서 변경된 제목 확인

## 5. 모니터링 환경 구축

### Metrics Server

```bash
minikube addons enable metrics-server
kubectl top nodes
kubectl top pods
```

노드와 Pod의 CPU 및 메모리 사용량을 확인하였다.

### Prometheus 및 Grafana

```bash
sudo snap install helm --classic
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
kubectl get pods -n monitoring
kubectl port-forward -n monitoring svc/monitoring-grafana 3000:80
```

- 모니터링 Pod 6개 정상 실행 확인
- Grafana 관리자 로그인 성공
- guestbook Pod의 CPU 및 메모리 사용량 확인

## 6. 부하 테스트

```bash
kubectl run load --image=busybox --restart=Never -- \
  sh -c "while true; do wget -q -O /dev/null http://guestbook:9090/; done"

kubectl top pods
```

### 실행 결과

| Pod | CPU | Memory |
|---|---:|---:|
| guestbook-1 | 5m | 28Mi |
| guestbook-2 | 13m | 27Mi |
| guestbook-3 | 17m | 26Mi |
| guestbook-4 | 7m | 27Mi |
| load | 368m | 5Mi |
| redis | 16m | 5Mi |

Grafana에서 CPU 사용량 증가를 확인하였다.

부하 테스트 완료 후 load Pod를 삭제하였다.

```bash
kubectl delete pod load
```

## 7. 로그 확인

```bash
kubectl logs -l app=guestbook --tail=20
kubectl logs -l app=guestbook -f --prefix
kubectl logs deploy/guestbook --since=5m
kubectl logs -l app=guestbook | grep -i error
```

HTTP 요청과 healthz, readyz의 정상 응답(200)을 확인하였다.

## 8. 트러블슈팅

- Helm 설치 중 Ubuntu 비밀번호 인증 오류 발생
- WSL 배포판 확인 후 해당 Ubuntu 계정의 비밀번호 재설정
- Helm 및 모니터링 도구 설치 성공
- guestbook 서비스 포트에 맞게 부하 테스트 주소를 9090번 포트로 수정

## 9. 실습 결과 및 느낀 점

GitOps를 활용하면 클러스터를 직접 수정하지 않고도
GitHub의 변경 사항을 기반으로 자동 배포할 수 있다는 것을 배웠다.

또한 selfHeal과 prune을 통해 클러스터의 상태를 관리하고,
Prometheus와 Grafana를 활용하여 CPU 및 메모리 사용량을
확인할 수 있었다.
