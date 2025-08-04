# Chapter 3 - Lab 02: Deployment로 ddoddo-market 3티어 앱 배포하기

## 🎯 Lab 목표

`ch2`에서 개별 Pod로 배포했던 `ddoddo-market` 애플리케이션을 **Deployment**를 사용하여 3티어(frontend, backend, database) 아키텍처 전체를 안정적으로 배포합니다. 이번 실습을 통해 여러분은 여러 컴포넌트로 구성된 실제 애플리케이션을 쿠버네티스에서 '관리'하는 방법을 익히고, Deployment의 자동 복구 및 스케일링 기능을 경험하게 됩니다.

**⚠️ 중요:** 이번 실습의 목표는 각 컴포넌트를 **안정적으로 띄워두는 것**에 있습니다. 아직 Service를 배우지 않았기 때문에 컴포넌트 간의 통신은 여전히 불가능합니다. `backend`는 `database`에 연결하지 못해 에러 로그를 출력할 수 있으며, `frontend`는 `backend`에 연결하지 못할 것입니다. 이는 정상적인 과정이며, "왜 Deployment만으로는 부족한가?"에 대한 해답을 다음 챕터에서 찾게 될 것입니다.

---

## 💻 해결 과제

`ddoddo-market` 애플리케이션을 구성하는 3개의 티어(Database, Backend, Frontend)를 각각 **Deployment**를 사용하여 쿠버네티스 클러스터에 배포하세요. 아래의 요구사항을 만족하는 3개의 Deployment YAML 파일을 `ch3/labs/lab02` 디렉토리에 직접 작성해주세요.

> ❗️ YAML 파일에 사용되는 모든 이미지는 `ch2/labs`에서 빌드하고 푸시했던 본인의 이미지를 사용해야 합니다. (예: `your-username/ddoddo-frontend:v1`, `your-username/ddoddo-backend:v1`)

### 과제 1: Database Deployment

-   **파일명:** `database-deployment.yaml`
-   **Deployment 이름:** `ddoddo-db-deployment`
-   **Pod 복제본(Replicas):** 1개
-   **컨테이너 이미지:** `postgres:15` (공식 이미지 사용)
-   **필요한 환경 변수:** `POSTGRES_USER`, `POSTGRES_PASSWORD`, `POSTGRES_DB` (값은 자유롭게 지정)
-   **Pod 레이블:** `app: ddoddo-db`

### 과제 2: Backend Deployment

-   **파일명:** `backend-deployment.yaml`
-   **Deployment 이름:** `ddoddo-backend-deployment`
-   **Pod 복제본(Replicas):** 2개
-   **컨테이너 이미지:** `your-username/ddoddo-backend:v1` (본인의 이미지 주소로 변경)
-   **필요한 환경 변수:** `ch2/labs`에서 설정했던 `ddoddo-market` 백엔드 애플리케이션 실행에 필요한 모든 환경 변수를 찾아 설정하세요. (`PGHOST`는 아직 연결할 수 없으므로 `dummy-db-host`와 같은 임의의 값을 넣으세요.)
-   **Pod 레이블:** `app: ddoddo-backend`

### 과제 3: Frontend Deployment

-   **파일명:** `frontend-deployment.yaml`
-   **Deployment 이름:** `ddoddo-frontend-deployment`
-   **Pod 복제본(Replicas):** 3개
-   **컨테이너 이미지:** `your-username/ddoddo-frontend:v1` (본인의 이미지 주소로 변경)
-   **필요한 환경 변수:** `ch2/labs`에서 설정했던 `ddoddo-market` 프론트엔드 애플리케이션 실행에 필요한 모든 환경 변수를 찾아 설정하세요. (`REACT_APP_API_URL`은 아직 연결할 백엔드 주소가 없으므로 `http://dummy-backend:8080`과 같은 임의의 값을 넣으세요.)
-   **Pod 레이블:** `app: ddoddo-frontend`

---

## ✅ 최종 확인

모든 YAML 파일을 `kubectl apply`로 배포한 후, 아래 명령어를 통해 각 Deployment가 선언한 `replicas` 수만큼 Pod들을 성공적으로 생성하고 `Running` 상태로 유지하는지 확인하세요.

```bash
kubeclt get deployments
kubeclt get pods -l app=ddoddo-db
kubeclt get pods -l app=ddoddo-backend
kubeclt get pods -l app=ddoddo-frontend
```