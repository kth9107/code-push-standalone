# React Native CodePush 구축 가이드

## 프로젝트 개요

### 목표
- React Native 앱의 **무중단 업데이트 시스템** 구축
- 앱스토어 승인 과정 없이 **즉시 배포 가능한 환경** 조성
- 내부 시스템용 **보안성 강화**

### 핵심 기능
- **JWT 토큰 기반 인증** - 보안성 확보
- **MS CodePush 서버 자체 구축** - 완전한 제어권
- **Oracle DB 연동** - 엔터프라이즈급 데이터 관리
- **Redis TLS 암호화** - 데이터 전송 보안

### 주요 성과
- **배포 시간 96% 단축** (24시간 → 5분)
- **앱스토어 의존성 완전 제거**
- **엔터프라이즈급 보안 시스템 구축**

---

## 시스템 아키텍처

```
React Native App → JWT Auth → CodePush Server → Oracle DB
                                    ↓
                               Redis (TLS)
```

### 주요 구성 요소

#### 인프라
- Linux 서버 (CentOS/RHEL)
- Node.js 22.x
- PM2 프로세스 관리
- Azurite (로컬 Blob Storage)

#### 보안
- Redis TLS 암호화
- SSL/TLS 인증서
- JWT 토큰 인증
- GitHub OAuth 연동

---

## 1. 서버 설치 과정

### 1.1 필수 구성 요소 설치

```bash
# Azurite 설치 및 실행
sudo npm install -g azurite
mkdir ~/azurite_data
nohup azurite --location ~/azurite_data > ~/azurite.log 2>&1 &

# Redis 설치 및 설정
sudo yum install -y redis
sudo systemctl enable redis
sudo systemctl start redis

# 내부 IP 테스트를 위한 Redis 설정 수정
sudo vi /etc/redis/redis.conf
# bind 127.0.0.1 → bind 0.0.0.0
# protected-mode yes → protected-mode no
sudo systemctl restart redis

# Node.js 설치
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs

# Git 설치
sudo yum install -y git
```

### 1.2 CodePush 서버 클론 및 설정

```bash
git clone https://github.com/microsoft/code-push-server.git
cd code-push-server/api
cp .env.example .env
npm install
npm run build
```

---

## 2. 환경 설정

### 2.1 .env 파일 설정

```env
PORT=3333
EMULATED=true
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
DB_CONNECTION_STRING=sqlite:///path/to/db.sqlite
SERVER_URL=http://localhost:3333
HTTPS=false
```

### 2.2 GitHub OAuth 설정

```env
GITHUB_CLIENT_ID=Ov23liTH2E0SQSDoMgPJ
GITHUB_CLIENT_SECRET=7d31e6fb55c6024c8dc12276cf83d9159d70fac6
# 리다이렉트 URI
REDIRECT_URI=http://codepush.local:8443/auth/callback/github
```

### 2.3 호스트 파일 설정

Windows의 경우 `C:\Windows\System32\drivers\etc\hosts`에 추가:

```
192.168.1.43    codepush.local
```

---

## 3. Redis TLS 보안 설정

### 3.1 인증서 체계 구성

```
/etc/redis/tls/
├── root/                 # Root CA
│   ├── rootCA.crt
│   └── rootCA.key
├── intermediate/         # 중간 CA
│   ├── intermediate.crt
│   └── intermediate.key
├── server/               # Redis 서버용
│   ├── server.crt
│   └── server.key
├── client/               # 클라이언트용
│   ├── client.crt
│   └── client.key
└── fullchain.crt         # 전체 체인
```

### 3.2 Root CA 생성

```bash
sudo mkdir -p /etc/redis/tls/{root,intermediate,client,server}

# Root CA 키 생성
sudo openssl genrsa -out /etc/redis/tls/root/rootCA.key 4096

# Root CA 인증서 생성
sudo openssl req -x509 -new -nodes -key /etc/redis/tls/root/rootCA.key \
  -sha256 -days 3650 \
  -subj "/CN=redis-root-ca" \
  -out /etc/redis/tls/root/rootCA.crt
```

### 3.3 Intermediate CA 생성

```bash
# 키 생성
sudo openssl genrsa -out /etc/redis/tls/intermediate/intermediate.key 4096

# CSR 생성
sudo openssl req -new -key /etc/redis/tls/intermediate/intermediate.key \
  -subj "/CN=redis-intermediate" \
  -out /etc/redis/tls/intermediate/intermediate.csr

# 확장 설정 파일 생성
cat <<EOF > /etc/redis/tls/intermediate/intermediate-ext.cnf
[ v3_ca ]
basicConstraints = CA:TRUE
keyUsage = keyCertSign, cRLSign
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
EOF

# 인증서 서명
sudo openssl x509 -req \
  -in /etc/redis/tls/intermediate/intermediate.csr \
  -CA /etc/redis/tls/root/rootCA.crt \
  -CAkey /etc/redis/tls/root/rootCA.key \
  -CAcreateserial \
  -out /etc/redis/tls/intermediate/intermediate.crt \
  -days 365 \
  -extfile /etc/redis/tls/intermediate/intermediate-ext.cnf \
  -extensions v3_ca
```

### 3.4 서버 인증서 생성

```bash
# 키 생성
sudo openssl genrsa -out /etc/redis/tls/server/server.key 2048

# CSR 생성
sudo openssl req -new -key /etc/redis/tls/server/server.key \
  -subj "/CN=redis-server" \
  -out /etc/redis/tls/server/server.csr

# 확장 설정 파일 생성
cat <<EOF > /etc/redis/tls/server/server-ext.cnf
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = redis.local
IP.1 = 192.1.1.1
EOF

# 인증서 서명
sudo openssl x509 -req -in /etc/redis/tls/server/server.csr \
  -CA /etc/redis/tls/intermediate/intermediate.crt \
  -CAkey /etc/redis/tls/intermediate/intermediate.key \
  -CAcreateserial \
  -out /etc/redis/tls/server/server.crt \
  -days 825 \
  -extfile /etc/redis/tls/server/server-ext.cnf \
  -extensions v3_req
```

### 3.5 클라이언트 인증서 생성

```bash
# 키 생성
sudo openssl genrsa -out /etc/redis/tls/client/client.key 2048

# CSR 생성
sudo openssl req -new -key /etc/redis/tls/client/client.key \
  -subj "/CN=redis-client" \
  -out /etc/redis/tls/client/client.csr

# 확장 설정 파일 생성
cat <<EOF > /etc/redis/tls/client/client-ext.cnf
[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth
EOF

# 인증서 서명
sudo openssl x509 -req -in /etc/redis/tls/client/client.csr \
  -CA /etc/redis/tls/intermediate/intermediate.crt \
  -CAkey /etc/redis/tls/intermediate/intermediate.key \
  -CAcreateserial \
  -out /etc/redis/tls/client/client.crt \
  -days 825 \
  -extfile /etc/redis/tls/client/client-ext.cnf \
  -extensions v3_req
```

### 3.6 Full Chain 생성 및 권한 설정

```bash
# Full chain 생성
sudo sh -c 'cat /etc/redis/tls/intermediate/intermediate.crt /etc/redis/tls/root/rootCA.crt > /etc/redis/tls/fullchain.crt'

# 인증서 검증
sudo openssl verify -CAfile /etc/redis/tls/fullchain.crt /etc/redis/tls/client/client.crt
sudo openssl verify -CAfile /etc/redis/tls/fullchain.crt /etc/redis/tls/server/server.crt

# 그룹 생성 및 권한 설정
getent group redis_tls || sudo groupadd redis_tls
sudo usermod -aG redis_tls dev
sudo chown -R root:redis_tls /etc/redis/tls
sudo chmod -R 750 /etc/redis/tls
sudo chmod 640 /etc/redis/tls/*/*.key
```

### 3.7 Redis 설정

```conf
# /etc/redis/redis.conf
bind 127.0.0.1 192.1.1.1
port 0
tls-port 6379
tls-cert-file /etc/redis/tls/server/server.crt
tls-key-file /etc/redis/tls/server/server.key
tls-ca-cert-file /etc/redis/tls/root/rootCA.crt
tls-auth-clients no
loglevel notice
```

### 3.8 Redis TLS 연결 테스트

```bash
redis-cli \
  --tls \
  --cert /etc/redis/tls/client/client.crt \
  --key /etc/redis/tls/client/client.key \
  --cacert /etc/redis/tls/root/rootCA.crt \
  -h 192.1.1.1 -p 6379
```

---

## 4. Oracle DB 연동

### 4.1 주요 테이블 구조

```sql
-- 1. 계정 테이블
CREATE TABLE codepush_accounts (
    id VARCHAR2(50) PRIMARY KEY,
    email VARCHAR2(100) NOT NULL,
    name VARCHAR2(100),
    github_id VARCHAR2(100)
);

-- 2. 앱 테이블
CREATE TABLE codepush_apps (
    id VARCHAR2(50) PRIMARY KEY,
    name VARCHAR2(100) NOT NULL,
    created_time NUMBER,
    owner_account_id VARCHAR2(50),
    CONSTRAINT fk_apps_account FOREIGN KEY (owner_account_id) REFERENCES codepush_accounts(id)
);

-- 3. 배포 테이블
CREATE TABLE codepush_deployments (
    id VARCHAR2(50) PRIMARY KEY,
    app_id VARCHAR2(50),
    name VARCHAR2(100),
    deployment_key VARCHAR2(100),
    CONSTRAINT fk_deployment_app FOREIGN KEY (app_id) REFERENCES codepush_apps(id)
);

-- 4. 패키지 테이블
CREATE TABLE codepush_packages (
    id VARCHAR2(50) PRIMARY KEY,
    deployment_id VARCHAR2(50),
    label VARCHAR2(50),
    package_hash VARCHAR2(100),
    app_version VARCHAR2(50),
    blob_url VARCHAR2(200),
    manifest_blob_url VARCHAR2(200),
    is_disabled NUMBER(1),
    is_mandatory NUMBER(1),
    release_method VARCHAR2(50),
    rollout NUMBER(5),
    description VARCHAR2(500),
    uploaded_at NUMBER,
    CONSTRAINT fk_package_deployment FOREIGN KEY (deployment_id) REFERENCES codepush_deployments(id)
);

-- 5. 액세스 키
CREATE TABLE codepush_access_keys (
    id VARCHAR2(50) PRIMARY KEY,
    name VARCHAR2(100),
    description VARCHAR2(200),
    account_id VARCHAR2(50),
    expires NUMBER,
    CONSTRAINT fk_accesskey_account FOREIGN KEY (account_id) REFERENCES codepush_accounts(id)
);

-- 6. 배포 통계
CREATE TABLE codepush_deployment_metrics (
    deployment_id VARCHAR2(50),
    label VARCHAR2(50),
    active NUMBER,
    downloaded NUMBER,
    installed NUMBER,
    failed NUMBER,
    created_time NUMBER,
    PRIMARY KEY (deployment_id, label),
    FOREIGN KEY (deployment_id) REFERENCES codepush_deployments(id)
);
```

---

## 5. CLI 설정 및 앱 등록

### 5.1 CodePush CLI 설치

```bash
git clone https://github.com/microsoft/code-push-server.git
cd code-push-server/cli
npm install
npm link

# 버전 확인
code-push-standalone
```

### 5.2 사용자 등록 및 로그인

```bash
# GitHub 등록 (도메인 필수, 브라우저에서 직접 접속)
# http://codepush.local:8443/auth/register/github

# CLI 로그인
code-push-standalone login http://codepush.local:8443

# 수동 사용자 등록 (필요시)
INSERT INTO users (username, email, accessToken, linkedProviders)
VALUES ('github_id', 'mail@mail.com', 'fh7cbqnLb0518_SD0SPjJfj9evj34kwOm2Cpbx', 'github');

INSERT INTO AccessKeys (name, value, description, userId)
VALUES (
  'Login-ManualKey',
  'fh7cbqnLb0518_SD0SPjJfj9evj34kwOm2Cpbx',  
  'Manually issued key',
  1
);
```

### 5.3 앱 등록 및 배포 키 확인

```bash
# 앱 등록
code-push-standalone app add icoopWms android

# 배포 키 확인
code-push-standalone deployment ls icoopWms -k
```

### 5.4 배포 키 예시

```
┌────────────┬───────────────────────────────┐
│ Name       │ Deployment Key                │
├────────────┼───────────────────────────────┤
│ Production │ EWookX3FMdx5GV6ZnnL-F6vKRfGi1 │
├────────────┼───────────────────────────────┤
│ Staging    │ sSOhYcSnq-Nvbl4LvYZGNdeCMo4o1 │
└────────────┴───────────────────────────────┘
```

---

## 6. React Native 앱 연동

### 6.1 CodePush 라이브러리 설치

```bash
npm install react-native-code-push --save
```

### 6.2 Android 설정

#### strings.xml 설정

```xml
<!-- android/app/src/main/res/values/strings.xml -->
<string name="CodePushServerUrl">http://codepush.local:3333</string>
<string name="CodePushDeploymentKey">YOUR_DEPLOYMENT_KEY</string>
```

#### settings.gradle 설정

```gradle
include ':app', ':react-native-code-push'
project(':react-native-code-push').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-code-push/android/app')
```

### 6.3 iOS 설정 (참고)

```xml
<!-- Info.plist -->
<key>CodePushServerURL</key>
<string>http://localhost:3333</string>
<key>CodePushDeploymentKey</key>
<string>YOUR_DEPLOYMENT_KEY</string>
```

### 6.4 배포 명령어

```bash
# Staging 환경에 배포
code-push-standalone release-react icoopWms android -d Staging

# Production 환경에 배포
code-push-standalone release-react icoopWms android -d Production \
  --description "버그 수정" --targetBinaryVersion "1.0"

# 서버 URL 지정 배포
code-push-standalone release-react icoopWms android \
  --serverUrl=http://codepush.local:3333 \
  --description "버그 수정" \
  --targetBinaryVersion "1.0"
```

---

## 7. 모니터링 및 로깅

### 7.1 배포 상태 추적

```bash
curl -X POST http://192.1.1.1:3333/v0.1/public/codepush/report_status/deploy \
  -H "Content-Type: application/json" \
  -d '{
    "appVersion": "1.0",
    "deploymentKey": "Vu-_Y3SBOa937skg0RlVOveIu_qG1",
    "clientUniqueId": "fcfb2fe86a91c611",
    "label": "v1",
    "status": "DeploymentSucceeded"
  }'
```

### 7.2 로그 확인 방법

```bash
# Android 로그 확인
adb logcat | grep CodePush

# React Native 로그 확인
adb logcat *:S ReactNative:V ReactNativeJS:V

# 서버 로그 확인
sudo pm2 logs codepush

# 자바스크립트 번들 저장소 확인
ls -lh /usr/local/src/code-push/code-push-server/api/.code-push/blobStore
```

### 7.3 PM2로 서버 실행

```bash
cd /usr/local/src/code-push/api
sudo pm2 start bin/script/server.js --name codepush --node-args="--env-file=.env"
```

---

## 8. 트러블슈팅

### 8.1 주요 이슈 및 해결 방법

#### 앱 등록 오류
- **문제**: CLI를 통한 앱 등록 실패
- **해결**: 데이터베이스에 직접 INSERT 쿼리 실행

#### TLS 인증서 오류
- **문제**: Redis TLS 연결 실패
- **해결**: 인증서 체인 검증 및 권한 설정 재확인

```bash
# 인증서 검증
openssl verify -CAfile /etc/redis/tls/fullchain.crt /etc/redis/tls/client/client.crt
openssl verify -CAfile /etc/redis/tls/fullchain.crt /etc/redis/tls/server/server.crt
```

#### 번들 다운로드 실패
- **문제**: 앱에서 업데이트 파일 다운로드 실패
- **해결**: 방화벽 포트 개방 및 네트워크 설정 점검

### 8.2 필수 포트 설정
- CodePush 서버: 3333
- Redis TLS: 6379
- GitHub OAuth: 8443

---

## 9. 주요 기능 및 성과

### 9.1 핵심 기능

#### 무중단 배포
- 앱 재시작 없이 업데이트
- JavaScript 번들만 교체

#### 점진적 롤아웃
- 사용자 비율별 단계적 배포
- A/B 테스트 지원

#### 롤백 지원
- 문제 발생시 즉시 이전 버전으로 복구
- 배포 히스토리 관리

#### 배포 통계
- 다운로드, 설치, 실패율 추적
- 실시간 모니터링

### 9.2 성과 지표

| 항목 | 이전 | 현재 | 개선율 |
|------|------|------|--------|
| 배포 시간 | 24시간 | 5분 | 96% 단축 |
| 배포 빈도 | 월 1회 | 주 2-3회 | 300% 증가 |
| 버그 수정 시간 | 1-2주 | 즉시 | 실시간 |
| 앱스토어 의존성 | 100% | 0% | 완전 제거 |

### 9.3 비즈니스 임팩트
- 앱스토어 리뷰 대기시간 제거
- 긴급 버그 수정 시간 최소화
- 사용자 피드백 즉시 반영 가능
- A/B 테스트 및 실험 용이

---

## 10. 향후 개선 계획

### 10.1 기술적 개선
- **고가용성**: 로드밸런서 및 다중 서버 구성
- **Docker 컨테이너화**: 배포 및 확장성 개선
- **모니터링 강화**: Prometheus + Grafana 도입
- **자동화**: CI/CD 파이프라인 구축

### 10.2 운영 개선
- **백업 자동화**: DB 및 파일 정기 백업
- **에러 추적**: Sentry 연동
- **성능 최적화**: CDN 도입 검토
- **보안 강화**: Let's Encrypt 자동 갱신

### 10.3 기능 확장
- iOS 지원 확장
- 웹 관리 대시보드 개발
- 사용자별 권한 관리 시스템
- 배포 승인 워크플로우 구축

---

## 11. 환경 변수 참고

### 11.1 주요 환경 변수

| 변수명 | 설명 | 예시 값 |
|--------|------|---------|
| PORT | CodePush 서버 포트 | 3333 |
| EMULATED | Azurite 사용 여부 | true |
| REDIS_HOST | Redis 호스트 주소 | 127.0.0.1 |
| REDIS_PORT | Redis 포트 번호 | 6379 |
| SERVER_URL | CodePush 서버 URL | http://localhost:3333 |
| HTTPS | HTTPS 활성화 여부 | false |

### 11.2 운영 고려사항

#### 보안
- 로컬 개발 환경에서는 HTTPS가 필수가 아니지만, 프로덕션 환경에서는 반드시 HTTPS 활성화
- Let's Encrypt 등 무료 SSL 서비스 활용 권장

#### 데이터 백업
- SQLite 데이터베이스와 Redis 데이터 주기적 백업
- 자바스크립트 번들 파일 백업

#### 성능 최적화
- 프로덕션 환경에서는 SQLite 대신 Oracle/PostgreSQL 사용 권장
- Redis 캐싱 레이어 적극 활용

#### 네트워크 설정
- 방화벽 규칙 확인 및 필요 포트 개방
- 내부 네트워크 보안 정책 준수

---

## 결론

React Native CodePush 자체 서버 구축을 통해 다음과 같은 성과를 달성했습니다:

### 핵심 성과
- **배포 시간 96% 단축** (24시간 → 5분)
- **앱스토어 의존성 완전 제거**
- **엔터프라이즈급 보안 시스템 구축**
- **완전한 자체 제어 환경 조성**

### 기술적 성취
- **MS CodePush 서버 완전 구축**
- **JWT + TLS 보안 체계 구현**
- **SQLite → Oracle DB 마이그레이션**
- **무중단 배포 시스템 실현**

이 프로젝트를 통해 모바일 앱 배포 프로세스의 혁신적 개선을 달성하고, 향후 더 나은 개발 및 운영 환경의 기반을 마련했습니다.
