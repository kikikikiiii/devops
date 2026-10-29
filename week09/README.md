
# 9주차 - Kubernetes 실습

## 1. 실습 목표
- Minikube를 이용해 로컬 Kubernetes 클러스터 구성
- kubectl을 이용한 Pod 및 Deployment 관리
- Guestbook 애플리케이션 배포
- Pod 자동 복구(Self-healing) 확인

## 2. Kubernetes 환경 구성
Docker Desktop과 WSL Ubuntu 환경에서 Minikube를 실행했다.

```bash
minikube start --driver=docker
minikube status
kubectl get nodes
```

`kubectl get nodes` 명령어로 minikube 노드가 Ready 상태임을 확인했다.

## 3. Nginx Pod 실습
```bash
kubectl run mynginx --image=nginx
kubectl get pods
kubectl describe pod mynginx
kubectl logs mynginx
kubectl port-forward mynginx 8080:80
```

브라우저에서 http://localhost:8080 에 접속해 Nginx 기본 페이지가 나타나는 것을 확인했다.

## 4. Deployment 및 스케일링
```bash
kubectl create deployment mynginx --image=nginx --replicas=3
kubectl get deployments
kubectl scale deployment mynginx --replicas=5
kubectl scale deployment mynginx --replicas=2
```

Deployment를 이용해 Pod 개수를 3개에서 5개로 늘리고 다시 2개로 줄였다.

Pod를 삭제했을 때 Deployment가 새로운 Pod를 자동 생성하는 것도 확인했다.

## 5. Guestbook 애플리케이션 배포
6주차에 제작한 Guestbook Docker 이미지를 사용했다.

```bash
kubectl create deployment guestbook --image=ghcr.io/kikikikiiii/guestbook:v1
kubectl get pods -w
kubectl get deployments
kubectl logs -l app=guestbook
kubectl port-forward deployment/guestbook 8080:5000
```

브라우저에서 http://localhost:8080 에 접속해 InhaTC DevOps 방명록이 정상적으로 실행되는 것을 확인했다.

Redis 환경변수가 없어 메모리 모드로 실행되었으며, Flask 서버는 5000번 포트를 사용했다.

## 6. Self-healing 실습
기존 Guestbook Pod를 삭제했다.

```bash
kubectl delete pod guestbook-6c88d7cd69-glm8r
kubectl get pods
```

삭제 후 새로운 Pod `guestbook-6c88d7cd69-cqqff`가 자동 생성되었고, `1/1 Running` 상태를 확인했다.

이를 통해 Kubernetes Deployment의 자동 복구 기능을 확인했다.

## 7. 실습 환경 정리
```bash
kubectl delete deployment guestbook
minikube stop
```

## 8. 실습 결과 및 느낀 점
이번 실습을 통해 Kubernetes에서 Pod와 Deployment를 관리하는 방법을 배웠다.

특히 Pod를 삭제해도 Deployment가 자동으로 새로운 Pod를 생성하는 Self-healing 기능을 직접 확인할 수 있었다.

또한 기존에 Docker로 제작했던 Guestbook 애플리케이션을 Kubernetes 환경에 배포하고 브라우저에서 실행해 보았다.
  
