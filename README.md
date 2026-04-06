# AirSpotter Pro Plus

AirSpotter의 운영형 Flask 프로젝트입니다. 기존 기능을 유지하면서 아래를 추가했습니다.

- 영상 댓글 수정/삭제
- 알림 시스템(좋아요, 댓글, DM, 신고, 관리자 조치)
- S3 / Cloudflare R2 실제 업로드 전환
- 관리자 유저 정지/해제
- PostgreSQL + Redis + Socket.IO 배포형 구조
- 업로드 파일 삭제 연동
- health check (`/healthz`)

## 주요 기능

### 계정/운영
- 회원가입 / 로그인 / 로그아웃
- 비밀번호 해시 저장
- 관리자 생성 CLI
- 관리자 대시보드 `/ops`
- 유저 정지 / 정지 해제
- 신고 처리 / 신고 영상 삭제

### 영상
- 영상 업로드 / 수정 / 삭제
- 자동 제목 생성
- 희귀도 계산
- 좋아요
- 댓글 작성 / 수정 / 삭제
- 신고
- 조회수 집계
- 로컬 저장 또는 S3/R2 저장

### 커뮤니티/소셜
- 공항 페이지
- 공항 채팅(Socket.IO)
- 크루 생성/가입/채팅
- 짧은 SNS
- DM
- 알림 목록 / 읽음 처리

### 수집/게임화
- XP / 레벨
- 미션 / 뱃지
- 도감 / 포인트 / 사운드 / 캘린더 / 랭킹

## 설치 라이브러리

로컬 가상환경 기준:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## 빠른 실행 (SQLite 로컬 테스트)

```bash
cp .env.example .env
# .env에서 STORAGE_BACKEND=local 유지
python -m flask --app wsgi:app init-db
python -m flask --app wsgi:app seed
python -m flask --app wsgi:app create-admin
python wsgi.py
```

## Docker 실행 (실사용 권장 기본형)

```bash
cp .env.example .env
docker compose up --build
```

초기화:

```bash
docker compose exec web python -m flask --app wsgi:app init-db
docker compose exec web python -m flask --app wsgi:app seed
docker compose exec web python -m flask --app wsgi:app create-admin
```

## PostgreSQL 설정

`.env.example`의 `DATABASE_URL`이 이미 PostgreSQL 기준입니다.

예:
```env
DATABASE_URL=postgresql+psycopg://airspotter:airspotter@postgres:5432/airspotter
```

## Cloudflare R2 / S3 업로드 설정

`.env`에서:

```env
STORAGE_BACKEND=s3
S3_BUCKET=your-bucket
S3_REGION=auto
S3_ENDPOINT_URL=https://<accountid>.r2.cloudflarestorage.com
S3_ACCESS_KEY_ID=...
S3_SECRET_ACCESS_KEY=...
S3_PUBLIC_BASE_URL=https://cdn.example.com
```

- `S3_PUBLIC_BASE_URL`에는 퍼블릭 버킷 URL 또는 CDN URL을 넣는 것이 가장 안정적입니다.
- 설정 후 업로드되는 새 파일부터 R2/S3로 올라갑니다.

## 실사용 전 권장 추가 작업

- CSRF 보호 추가(Flask-WTF)
- Alembic 마이그레이션 정식 운영
- 백업 정책
- HTTPS/Nginx 리버스프록시
- 로그 수집/모니터링(Sentry, Loki 등)
- 이미지/영상 썸네일 비동기 처리

## 주의

이 프로젝트는 "거의 실사용 가능한 운영형 베이스"입니다. 다만 대규모 트래픽 서비스 수준의 완전 상용 운영을 위해서는 인프라/보안/배치 처리 보강이 더 필요합니다.


## New in this build
- Automatic thumbnail generation from uploaded video when no thumbnail is provided
- Live ADS-B list views for 20 major airports
- Brighter editorial UI refresh
