# Elasticsearch Management Stack

이 저장소는 아래 두 프로젝트를 한 번에 실행하기 위한 launcher 저장소입니다.

- frontend: elasticsearch-management-tool
- api: elasticsearch-management-tool-api

## 1. clone

```bash
git clone --recurse-submodules <YOUR_REPOSITORY_URL>
cd elasticsearch-management-stack
```

이미 clone 한 뒤라면:

```bash
git submodule update --init --recursive
```

## 2. env 파일 생성

```bash
cp .env.frontend.example .env.frontend
cp .env.api.example .env.api
```

필요한 값 수정:

- `.env.frontend`
- `.env.api`

예를 들어 Elasticsearch가 로컬에 떠 있다면:

```env
ELASTICSEARCH_URL=http://host.docker.internal:9200
ES_HOST=http://host.docker.internal:9200
```

## 3. 실행

```bash
docker compose up --build
```

## 4. 접속

- Frontend: http://localhost:3000
- API: http://localhost:8000

## 5. 중지

```bash
docker compose down
```

---

## 9. 자주 발생하는 문제

### 문제 1) 프론트에서 API 호출이 안 됨

원인:

- `CLUSTER_API_URL`에 `localhost`를 넣은 경우

해결:

```env
CLUSTER_API_URL=http://api:8000
```

---

### 문제 2) Elasticsearch 연결 실패

원인:

- 컨테이너 안에서 `localhost:9200`은 컨테이너 자기 자신

해결:

- `host.docker.internal:9200` 사용
- 또는 실제 Elasticsearch 서버 주소 사용
- 또는 Elasticsearch도 compose에 같이 올리기

---

### 문제 3) API가 환경변수를 못 읽음

원인:

- API 코드가 `src/resources/.env`만 읽도록 고정된 경우

해결:

- `env_settings.py`에서 `env_file` 제거
- compose의 `env_file:`로 관리

---

## 10. 요약

```bash
git clone --recurse-submodules <YOUR_REPOSITORY_URL>
cp .env.frontend.example .env.frontend && cp .env.api.example .env.api
docker compose up --build
```
