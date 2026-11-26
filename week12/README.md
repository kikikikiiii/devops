# 12주차 GitHub Actions CI/CD 실습

## 1. 실습 목표
- GitHub Actions를 이용한 CI/CD 자동화
- 코드 검사 및 자동 테스트 실행
- Docker 이미지 자동 빌드 및 GHCR 업로드
- Kubernetes Deployment 업데이트
- 브랜치 보호 규칙 설정

## 2. GitHub Actions 설정
`.github/workflows/build.yml` 파일을 작성하여
GitHub에 코드를 Push하거나 PR을 생성하면
자동으로 CI 검사가 실행되도록 설정하였다.

## 3. 자동 테스트
- Ruff를 이용한 Python 코드 검사
- unittest를 이용한 Flask 애플리케이션 테스트
- Docker 이미지 빌드 및 GHCR 업로드

CI 검사에 성공하면 Docker 이미지를
GitHub Container Registry(GHCR)에 업로드하였다.

## 4. Kubernetes 배포
새롭게 빌드한 Docker 이미지를 사용하여
Kubernetes Deployment를 업데이트하였다.

`kubectl rollout status` 명령어로
배포가 정상적으로 완료되었는지 확인하였다.

## 5. CI 오류 감지 및 복구
다음 세 가지 오류를 발생시켜 CI 검사를 확인하였다.

1. Python 문법 오류
2. 정의되지 않은 변수 사용 오류
3. /healthz 응답 코드 오류 (200 → 500)

각 오류에서 CI 검사 실패를 확인하고,
`git revert`를 이용하여 복구하였다.

복구 후 CI 검사가 정상적으로 통과하는 것을 확인하였다.

## 6. 브랜치 보호 규칙
GitHub Rulesets에서 `protect-main` 규칙을 설정하였다.

- PR을 통한 병합 필수
- build 검사 통과 필수
- 강제 Push 차단
- 브랜치 삭제 제한

오류가 있는 PR #20에서 병합이 차단되는 것을 확인하였다.
오류 수정 후 CI 검사가 통과하였다.

## 7. 실습 결과
- PR #18: GitHub Actions CI/CD 구축
- PR #19: CI 오류 감지 및 복구
- PR #20: 브랜치 보호 규칙 테스트
- GitHub Actions 자동 검사 성공
- GHCR 이미지 업로드 성공
- Kubernetes 배포 업데이트 성공

## 8. 느낀 점
이번 실습을 통해 GitHub Actions를 사용하면
코드 검사와 테스트를 자동으로 실행할 수 있다는 것을 알게 되었다.

또한 브랜치 보호 규칙을 설정하면
오류가 있는 코드가 main 브랜치에 병합되는 것을
방지할 수 있다는 점을 이해하였다.
