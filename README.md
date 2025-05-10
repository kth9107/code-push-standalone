# Self-Hosted CodePush Server

## 개요

이 프로젝트는 Microsoft의 CodePush 오픈소스를 기반으로 구축한 **사내망 독립 실행형 OTA 업데이트 서버**입니다. 외부 Azure 서비스 없이도 `code-push-standalone` 명령어만으로 배포가 가능하며, React Native 앱의 OTA 업데이트를 내부 인프라에서 직접 관리할 수 있도록 설계되었습니다.

## 특징

- CodePush 서버를 로컬 또는 내부망에 설치 및 실행
- Azure Storage 없이 JSON 또는 커스텀 DB(Storage)로 대체 가능
- 인증 기능 포함 (GitHub OAuth 기반 커스터마이징 경험 포함)
- `code-push-standalone` CLI로 배포 수행 가능
- React Native 앱과 연동하여 실시간 배포 테스트 완료

## 사용 기술

- Node.js (Express)
- SQLite → Oracle로 마이그레이션 대응
- Redis (TLS 인증 연동)
- React Native (배포 테스트용 클라이언트 앱)
- PM2 (서버 프로세스 관리)

## 실행 방법

```bash
# 1. 의존성 설치
npm install

# 2. 환경변수 설정
cp .env.example .env
# 필요한 값 직접 설정

# 3. 서버 실행
npm run build
code-push-standalone

## 참조
https://drive.google.com/file/d/15FHVEbaaC5ZxAym8weB8774Ec6GtFKl8/view?usp=drive_link
