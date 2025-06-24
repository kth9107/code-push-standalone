<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React Native CodePush 구축 가이드</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            overflow: hidden;
            color: #f8fafc;
        }

        .presentation-container {
            width: 100vw;
            height: 100vh;
            position: relative;
        }

        .slide {
            width: 100%;
            height: 100%;
            position: absolute;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 60px;
            opacity: 0;
            transform: translateX(100%);
            transition: all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94);
            background: linear-gradient(135deg, #1e293b 0%, #334155 100%);
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.25);
        }

        .slide.active {
            opacity: 1;
            transform: translateX(0);
        }

        .slide.prev {
            transform: translateX(-100%);
        }

        /* Title Slide */
        .title-slide {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 50%, #334155 100%);
            color: #f8fafc;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .title-slide::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.03'%3E%3Ccircle cx='30' cy='30' r='2'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
            z-index: 0;
        }

        .title-slide .content {
            position: relative;
            z-index: 1;
        }

        .title-slide h1 {
            font-size: 4rem;
            font-weight: 800;
            margin-bottom: 1.5rem;
            background: linear-gradient(135deg, #60a5fa 0%, #34d399 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            animation: fadeInUp 1s ease;
        }

        .title-slide h2 {
            font-size: 1.75rem;
            font-weight: 400;
            margin-bottom: 3rem;
            opacity: 0.8;
            animation: fadeInUp 1s ease 0.3s both;
        }

        .title-slide .author {
            font-size: 1.25rem;
            font-weight: 500;
            padding: 2rem;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 16px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            animation: fadeInUp 1s ease 0.6s both;
        }

        .author-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 2rem;
            margin-top: 1rem;
        }

        .author-item {
            text-align: center;
        }

        .author-label {
            font-size: 0.875rem;
            color: #94a3b8;
            margin-bottom: 0.5rem;
        }

        .author-value {
            font-weight: 600;
            color: #f8fafc;
        }

        /* Content Slides */
        .content-slide {
            text-align: left;
            max-width: 1400px;
            width: 100%;
        }

        .content-slide h1 {
            font-size: 3rem;
            font-weight: 700;
            color: #f8fafc;
            margin-bottom: 3rem;
            position: relative;
            padding-bottom: 1rem;
        }

        .content-slide h1::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100px;
            height: 4px;
            background: linear-gradient(135deg, #60a5fa 0%, #34d399 100%);
            border-radius: 2px;
        }

        .content-slide h2 {
            font-size: 2rem;
            font-weight: 600;
            color: #e2e8f0;
            margin: 2rem 0 1rem 0;
        }

        .content-slide h3 {
            font-size: 1.5rem;
            font-weight: 600;
            color: #cbd5e1;
            margin: 1.5rem 0 0.75rem 0;
        }

        .content-slide p {
            font-size: 1.125rem;
            line-height: 1.75;
            margin-bottom: 1.5rem;
            color: #94a3b8;
        }

        .content-slide ul {
            margin-left: 2rem;
            margin-bottom: 1.5rem;
        }

        .content-slide li {
            font-size: 1.125rem;
            line-height: 1.75;
            margin-bottom: 0.75rem;
            color: #94a3b8;
        }

        .content-slide li strong {
            color: #f8fafc;
            font-weight: 600;
        }

        /* Code blocks */
        .code-block {
            background: #0f172a;
            border: 1px solid #334155;
            border-radius: 12px;
            padding: 2rem;
            margin: 2rem 0;
            overflow-x: auto;
            position: relative;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.25);
        }

        .code-block::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 40px;
            background: #1e293b;
            border-radius: 12px 12px 0 0;
            border-bottom: 1px solid #334155;
        }

        .code-block::after {
            content: '';
            position: absolute;
            top: 12px;
            left: 16px;
            width: 12px;
            height: 12px;
            border-radius: 50%;
            background: #ef4444;
            box-shadow: 20px 0 0 #f59e0b, 40px 0 0 #22c55e;
        }

        .code-block pre {
            margin: 0;
            padding-top: 1.5rem;
            font-family: 'JetBrains Mono', 'Fira Code', 'Consolas', monospace;
            font-size: 0.9rem;
            line-height: 1.6;
            color: #e2e8f0;
            white-space: pre-wrap;
            word-wrap: break-word;
        }

        .code-inline {
            background: rgba(99, 102, 241, 0.1);
            color: #a5b4fc;
            padding: 0.25rem 0.5rem;
            border-radius: 6px;
            font-family: 'JetBrains Mono', 'Fira Code', 'Consolas', monospace;
            font-size: 0.875rem;
            border: 1px solid rgba(99, 102, 241, 0.2);
        }

        /* Highlight boxes */
        .highlight-box {
            background: rgba(34, 197, 94, 0.1);
            border: 1px solid rgba(34, 197, 94, 0.3);
            border-left: 4px solid #22c55e;
            padding: 1.5rem;
            margin: 2rem 0;
            border-radius: 8px;
            backdrop-filter: blur(10px);
        }

        .highlight-box strong {
            color: #22c55e;
        }

        .warning-box {
            background: rgba(245, 158, 11, 0.1);
            border: 1px solid rgba(245, 158, 11, 0.3);
            border-left: 4px solid #f59e0b;
            padding: 1.5rem;
            margin: 2rem 0;
            border-radius: 8px;
            backdrop-filter: blur(10px);
        }

        .warning-box strong {
            color: #f59e0b;
        }

        .success-box {
            background: rgba(56, 178, 172, 0.1);
            border: 1px solid rgba(56, 178, 172, 0.3);
            border-left: 4px solid #38b2ac;
            padding: 1.5rem;
            margin: 2rem 0;
            border-radius: 8px;
            backdrop-filter: blur(10px);
        }

        .success-box strong {
            color: #38b2ac;
        }

        /* Two column layout */
        .two-column {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 3rem;
            width: 100%;
        }

        /* Navigation */
        .nav-container {
            position: fixed;
            bottom: 30px;
            left: 50%;
            transform: translateX(-50%);
            display: flex;
            gap: 1rem;
            z-index: 1000;
        }

        .nav-btn {
            background: rgba(15, 23, 42, 0.9);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #f8fafc;
            padding: 12px 24px;
            border-radius: 12px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 500;
            transition: all 0.3s ease;
        }

        .nav-btn:hover {
            background: rgba(15, 23, 42, 0.95);
            border-color: rgba(96, 165, 250, 0.3);
            transform: translateY(-2px);
        }

        .nav-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }

        .slide-counter {
            background: rgba(15, 23, 42, 0.9);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(255, 255, 255, 0.1);
            color: #f8fafc;
            padding: 12px 24px;
            border-radius: 12px;
            font-size: 1rem;
            font-weight: 500;
        }

        /* Architecture diagram */
        .architecture-diagram {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 3rem 0;
            padding: 3rem 2rem;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 16px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
        }

        .arch-component {
            text-align: center;
            padding: 2rem 1.5rem;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 12px;
            border: 1px solid rgba(255, 255, 255, 0.1);
            min-width: 160px;
            transition: all 0.3s ease;
        }

        .arch-component:hover {
            transform: translateY(-5px);
            box-shadow: 0 20px 40px -10px rgba(0, 0, 0, 0.3);
        }

        .arch-component-title {
            font-size: 1.125rem;
            font-weight: 600;
            color: #f8fafc;
            margin-bottom: 0.5rem;
        }

        .arch-component-desc {
            font-size: 0.875rem;
            color: #94a3b8;
        }

        .arch-arrow {
            font-size: 2rem;
            color: #60a5fa;
            margin: 0 1rem;
        }

        /* Table styles */
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 2rem 0;
            background: rgba(255, 255, 255, 0.03);
            border-radius: 12px;
            overflow: hidden;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        th, td {
            padding: 1rem 1.5rem;
            text-align: left;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        th {
            background: rgba(96, 165, 250, 0.1);
            color: #f8fafc;
            font-weight: 600;
            font-size: 1rem;
        }

        td {
            color: #94a3b8;
            font-size: 0.9rem;
        }

        tr:hover {
            background: rgba(255, 255, 255, 0.02);
        }

        /* Animations */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(40px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .fade-in {
            animation: fadeInUp 0.8s ease;
        }

        /* Features grid */
        .features-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 2rem;
            margin: 2rem 0;
        }

        .feature-card {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 12px;
            padding: 2rem;
            backdrop-filter: blur(10px);
            transition: all 0.3s ease;
        }

        .feature-card:hover {
            transform: translateY(-5px);
            border-color: rgba(96, 165, 250, 0.3);
        }

        .feature-title {
            font-size: 1.25rem;
            font-weight: 600;
            color: #f8fafc;
            margin-bottom: 1rem;
        }

        .feature-desc {
            font-size: 1rem;
            color: #94a3b8;
            line-height: 1.6;
        }

        /* Metrics */
        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 2rem;
            margin: 2rem 0;
        }

        .metric-card {
            background: rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 12px;
            padding: 2rem;
            text-align: center;
            backdrop-filter: blur(10px);
        }

        .metric-value {
            font-size: 2.5rem;
            font-weight: 700;
            color: #60a5fa;
            margin-bottom: 0.5rem;
        }

        .metric-label {
            font-size: 1rem;
            color: #94a3b8;
        }

        /* Responsive */
        @media (max-width: 768px) {
            .slide {
                padding: 30px 20px;
            }
            
            .title-slide h1 {
                font-size: 2.5rem;
            }
            
            .content-slide h1 {
                font-size: 2rem;
            }
            
            .two-column {
                grid-template-columns: 1fr;
                gap: 2rem;
            }

            .architecture-diagram {
                flex-direction: column;
                gap: 1rem;
            }

            .arch-arrow {
                transform: rotate(90deg);
                margin: 1rem 0;
            }

            .author-grid {
                grid-template-columns: 1fr;
                gap: 1rem;
            }
        }
    </style>
</head>
<body>
    <div class="presentation-container">
        <!-- Slide 1: Title -->
        <div class="slide active title-slide">
            <div class="content">
                <h1>React Native CodePush</h1>
                <h2>자체 서버 구축 및 무중단 배포 시스템</h2>
                <div class="author">
                    <div class="author-grid">
                        <div class="author-item">
                            <div class="author-label">개발자</div>
                            <div class="author-value">김개발</div>
                        </div>
                        <div class="author-item">
                            <div class="author-label">기간</div>
                            <div class="author-value">2024년</div>
                        </div>
                        <div class="author-item">
                            <div class="author-label">기술스택</div>
                            <div class="author-value">React Native, MS CodePush, Oracle, Redis</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!-- Slide 2: 프로젝트 개요 -->
        <div class="slide content-slide">
            <h1>프로젝트 개요</h1>
            
            <h2>목표</h2>
            <ul>
                <li>React Native 앱의 <strong>무중단 업데이트 시스템</strong> 구축</li>
                <li>앱스토어 승인 과정 없이 <strong>즉시 배포 가능한 환경</strong> 조성</li>
                <li>내부 시스템용 <strong>보안성 강화</strong></li>
            </ul>

            <h2>핵심 기능</h2>
            <div class="features-grid">
                <div class="feature-card">
                    <div class="feature-title">JWT 토큰 기반 인증</div>
                    <div class="feature-desc">보안성 확보를 위한 토큰 기반 인증 시스템</div>
                </div>
                <div class="feature-card">
                    <div class="feature-title">MS CodePush 서버 자체 구축</div>
                    <div class="feature-desc">완전한 제어권을 가진 독립적인 배포 서버</div>
                </div>
                <div class="feature-card">
                    <div class="feature-title">Oracle DB 연동</div>
                    <div class="feature-desc">엔터프라이즈급 데이터 관리 시스템</div>
                </div>
                <div class="feature-card">
                    <div class="feature-title">Redis TLS 암호화</div>
                    <div class="feature-desc">데이터 전송 구간 보안 강화</div>
                </div>
            </div>

            <div class="success-box">
                <strong>성과:</strong> 배포 시간 24시간 → 5분으로 단축 (96% 개선), 앱스토어 의존성 완전 제거
            </div>
        </div>

        <!-- Slide 3: 시스템 아키텍처 -->
        <div class="slide content-slide">
            <h1>시스템 아키텍처</h1>
            
            <div class="architecture-diagram">
                <div class="arch-component">
                    <div class="arch-component-title">React Native App</div>
                    <div class="arch-component-desc">모바일 애플리케이션</div>
                </div>
                <div class="arch-arrow">→</div>
                <div class="arch-component">
                    <div class="arch-component-title">JWT Auth</div>
                    <div class="arch-component-desc">인증 시스템</div>
                </div>
                <div class="arch-arrow">→</div>
                <div class="arch-component">
                    <div class="arch-component-title">CodePush Server</div>
                    <div class="arch-component-desc">배포 서버</div>
                </div>
                <div class="arch-arrow">→</div>
                <div class="arch-component">
                    <div class="arch-component-title">Oracle DB</div>
                    <div class="arch-component-desc">데이터베이스</div>
                </div>
            </div>

            <h2>주요 구성 요소</h2>
            <div class="two-column">
                <div>
                    <h3>인프라</h3>
                    <ul>
                        <li>Linux 서버 (CentOS/RHEL)</li>
                        <li>Node.js 22.x</li>
                        <li>PM2 프로세스 관리</li>
                        <li>Azurite (로컬 Blob Storage)</li>
                    </ul>
                </div>
                <div>
                    <h3>보안</h3>
                    <ul>
                        <li>Redis TLS 암호화</li>
                        <li>SSL/TLS 인증서</li>
                        <li>JWT 토큰 인증</li>
                        <li>GitHub OAuth 연동</li>
                    </ul>
                </div>
            </div>
        </div>

        <!-- Slide 4: 서버 설치 과정 -->
        <div class="slide content-slide">
            <h1>서버 설치 과정</h1>
            
            <h2>1. 필수 구성 요소 설치</h2>
            <div class="code-block">
                <pre>```bash
# Azurite 설치 및 실행
sudo npm install -g azurite
mkdir ~/azurite_data
nohup azurite --location ~/azurite_data > ~/azurite.log 2>&1 &

# Redis 설치 및 설정
sudo yum install -y redis
sudo systemctl enable redis
sudo systemctl start redis

# Node.js 설치
curl -fsSL https://rpm.nodesource.com/setup_22.x | sudo bash -
sudo yum install -y nodejs
```</pre>
            </div>

            <h2>2. CodePush 서버 클론 및 설정</h2>
            <div class="code-block">
                <pre>```bash
git clone https://github.com/microsoft/code-push-server.git
cd code-push-server/api
cp .env.example .env
npm install
npm run build
```</pre>
            </div>

            <div class="highlight-box">
                <strong>팁:</strong> .env 파일에서 EMULATED=true로 설정하여 로컬 Azurite 사용
            </div>
        </div>

        <!-- Slide 5: 환경 설정 -->
        <div class="slide content-slide">
            <h1>환경 설정</h1>
            
            <h2>.env 파일 주요 설정</h2>
            <div class="code-block">
                <pre>```env
PORT=3333
EMULATED=true
REDIS_HOST=127.0.0.1
REDIS_PORT=6379
DB_CONNECTION_STRING=sqlite:///path/to/db.sqlite
SERVER_URL=http://localhost:3333
HTTPS=false
```</pre>
            </div>

            <h2>GitHub OAuth 설정</h2>
            <div class="code-block">
                <pre>```env
GITHUB_CLIENT_ID=Ov23liTH2E0SQSDoMgPJ
GITHUB_CLIENT_SECRET=7d31e6fb55c6024c8dc12276cf83d9159d70fac6
REDIRECT_URI=http://codepush.local:8443/auth/callback/github
```</pre>
            </div>

            <div class="warning-box">
                <strong>주의:</strong> 프로덕션 환경에서는 반드시 HTTPS를 활성화하고 실제 도메인 사용
            </div>
        </div>

        <!-- Slide 6: Redis TLS 보안 설정 -->
        <div class="slide content-slide">
            <h1>Redis TLS 보안 설정</h1>
            
            <h2>인증서 체계 구성</h2>
            <div class="code-block">
                <pre>```
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
```</pre>
            </div>

            <h2>Redis 설정</h2>
            <div class="code-block">
                <pre>```conf
port 0
tls-port 6379
tls-cert-file /etc/redis/tls/server/server.crt
tls-key-file /etc/redis/tls/server/server.key
tls-ca-cert-file /etc/redis/tls/root/rootCA.crt
tls-auth-clients no
```</pre>
            </div>

            <div class="success-box">
                <strong>보안 강화:</strong> TLS 암호화로 데이터 전송 구간 보안 확보
            </div>
        </div>

        <!-- Slide 7: Oracle DB 연동 -->
        <div class="slide content-slide">
            <h1>Oracle DB 연동</h1>
            
            <h2>주요 테이블 구조</h2>
            <table>
                <thead>
                    <tr>
                        <th>테이블명</th>
                        <th>역할</th>
                        <th>주요 필드</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>codepush_accounts</td>
                        <td>사용자 계정</td>
                        <td>id, email, name, github_id</td>
                    </tr>
                    <tr>
                        <td>codepush_apps</td>
                        <td>앱 정보</td>
                        <td>id, name, owner_account_id</td>
                    </tr>
                    <tr>
                        <td>codepush_deployments</td>
                        <td>배포 환경</td>
                        <td>id, app_id, deployment_key</td>
                    </tr>
                    <tr>
                        <td>codepush_packages</td>
                        <td>패키지 정보</td>
                        <td>id, label, package_hash, blob_url</td>
                    </tr>
                    <tr>
                        <td>codepush_deployment_metrics</td>
                        <td>배포 통계</td>
                        <td>deployment_id, active, downloaded, installed</td>
                    </tr>
                </tbody>
            </table>

            <div class="highlight-box">
                <strong>특징:</strong> SQLite에서 Oracle로 마이그레이션하여 엔터프라이즈급 안정성 확보
            </div>
        </div>

        <!-- Slide 8: CLI 설정 및 앱 등록 -->
        <div class="slide content-slide">
            <h1>CLI 설정 및 앱 등록</h1>
            
            <h2>CodePush CLI 설치</h2>
            <div class="code-block">
                <pre>```bash
git clone https://github.com/microsoft/code-push-server.git
cd code-push-server/cli
npm install
npm link
```</pre>
            </div>

            <h2>로그인 및 앱 등록</h2>
            <div class="code-block">
                <pre>```bash
# 서버 로그인
code-push-standalone login http://codepush.local:8443

# 앱 등록
code-push-standalone app add icoopWms android

# 배포 키 확인
code-push-standalone deployment ls icoopWms -k
```</pre>
            </div>

            <h2>배포 키 예시</h2>
            <div class="code-block">
                <pre>```
┌────────────┬───────────────────────────────┐
│ Name       │ Deployment Key                │
├────────────┼───────────────────────────────┤
│ Production │ EWookX3FMdx5GV6ZnnL-F6vKRfGi1 │
├────────────┼───────────────────────────────┤
│ Staging    │ sSOhYcSnq-Nvbl4LvYZGNdeCMo4o1 │
└────────────┴───────────────────────────────┘
```</pre>
            </div>
        </div>

        <!-- Slide 9: React Native 앱 연동 -->
        <div class="slide content-slide">
            <h1>React Native 앱 연동</h1>
            
            <h2>CodePush 라이브러리 설치</h2>
            <div class="code-block">
                <pre>```bash
npm install react-native-code-push --save
```</pre>
            </div>

            <h2>Android 설정</h2>
            <div class="code-block">
                <pre>```xml
<!-- android/app/src/main/res/values/strings.xml -->
<string name="CodePushServerUrl">http://codepush.local:3333</string>
<string name="CodePushDeploymentKey">YOUR_DEPLOYMENT_KEY</string>
```</pre>
            </div>

            <h2>배포 명령어</h2>
            <div class="code-block">
                <pre>```bash
# Staging 환경에 배포
code-push-standalone release-react icoopWms android -d Staging

# Production 환경에 배포
code-push-standalone release-react icoopWms android -d Production \
  --description "버그 수정" --targetBinaryVersion "1.0"
```</pre>
            </div>

            <div class="success-box">
                <strong>핵심:</strong> 앱스토어 승인 없이 JavaScript 번들만 업데이트 가능
            </div>
        </div>

        <!-- Slide 10: 주요 기능 및 성과 -->
        <div class="slide content-slide">
            <h1>주요 기능 및 성과</h1>
            
            <div class="two-column">
                <div>
                    <h2>핵심 기능</h2>
                    <div class="features-grid">
                        <div class="feature-card">
                            <div class="feature-title">무중단 배포</div>
                            <div class="feature-desc">앱 재시작 없이 업데이트</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">점진적 롤아웃</div>
                            <div class="feature-desc">사용자 비율별 단계적 배포</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">롤백 지원</div>
                            <div class="feature-desc">문제 발생시 즉시 이전 버전으로 복구</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">배포 통계</div>
                            <div class="feature-desc">다운로드, 설치, 실패율 추적</div>
                        </div>
                    </div>
                </div>
                <div>
                    <h2>성과 지표</h2>
                    <div class="metrics-grid">
                        <div class="metric-card">
                            <div class="metric-value">96%</div>
                            <div class="metric-label">배포 시간 단축</div>
                        </div>
                        <div class="metric-card">
                            <div class="metric-value">5분</div>
                            <div class="metric-label">현재 배포 시간</div>
                        </div>
                        <div class="metric-card">
                            <div class="metric-value">주 2-3회</div>
                            <div class="metric-label">배포 빈도</div>
                        </div>
                        <div class="metric-card">
                            <div class="metric-value">즉시</div>
                            <div class="metric-label">핫픽스 가능</div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="highlight-box">
                <strong>비즈니스 임팩트:</strong>
                <ul>
                    <li>앱스토어 리뷰 대기시간 제거</li>
                    <li>긴급 버그 수정 시간 최소화</li>
                    <li>사용자 피드백 즉시 반영 가능</li>
                    <li>A/B 테스트 및 실험 용이</li>
                </ul>
            </div>
        </div>

        <!-- Slide 11: 모니터링 및 로깅 -->
        <div class="slide content-slide">
            <h1>모니터링 및 로깅</h1>
            
            <h2>배포 상태 추적</h2>
            <div class="code-block">
                <pre>```bash
curl -X POST http://121.67.133.137:3333/v0.1/public/codepush/report_status/deploy \
  -H "Content-Type: application/json" \
  -d '{
    "appVersion": "1.0",
    "deploymentKey": "Vu-_Y3SBOa937skg0RlVOveIu_qG1",
    "clientUniqueId": "fcfb2fe86a91c611",
    "label": "v1",
    "status": "DeploymentSucceeded"
  }'
```</pre>
            </div>

            <h2>로그 확인 방법</h2>
            <div class="code-block">
                <pre>```bash
# Android 로그 확인
adb logcat | grep CodePush

# React Native 로그 확인
adb logcat *:S ReactNative:V ReactNativeJS:V

# 서버 로그 확인
sudo pm2 logs codepush
```</pre>
            </div>

            <div class="success-box">
                <strong>운영 안정성:</strong> PM2를 통한 프로세스 관리 및 자동 재시작
            </div>
        </div>

        <!-- Slide 12: 트러블슈팅 -->
        <div class="slide content-slide">
            <h1>트러블슈팅</h1>
            
            <h2>주요 이슈 및 해결 방법</h2>
            
            <h3>1. 앱 등록 오류</h3>
            <div class="warning-box">
                <strong>문제:</strong> CLI를 통한 앱 등록 실패<br>
                <strong>해결:</strong> 데이터베이스에 직접 INSERT 쿼리 실행
            </div>

            <h3>2. TLS 인증서 오류</h3>
            <div class="warning-box">
                <strong>문제:</strong> Redis TLS 연결 실패<br>
                <strong>해결:</strong> 인증서 체인 검증 및 권한 설정 재확인
            </div>

            <h3>3. 번들 다운로드 실패</h3>
            <div class="warning-box">
                <strong>문제:</strong> 앱에서 업데이트 파일 다운로드 실패<br>
                <strong>해결:</strong> 방화벽 포트 개방 및 네트워크 설정 점검
            </div>

            <h2>연결 테스트</h2>
            <div class="code-block">
                <pre>```bash
# Redis TLS 연결 테스트
redis-cli --tls --cert /etc/redis/tls/client/client.crt \
  --key /etc/redis/tls/client/client.key \
  --cacert /etc/redis/tls/root/rootCA.crt \
  -h 121.67.133.137 -p 6379
```</pre>
            </div>
        </div>

        <!-- Slide 13: 향후 개선 계획 -->
        <div class="slide content-slide">
            <h1>향후 개선 계획</h1>
            
            <div class="two-column">
                <div>
                    <h2>기술적 개선</h2>
                    <div class="features-grid">
                        <div class="feature-card">
                            <div class="feature-title">고가용성</div>
                            <div class="feature-desc">로드밸런서 및 다중 서버 구성</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">Docker 컨테이너화</div>
                            <div class="feature-desc">배포 및 확장성 개선</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">모니터링 강화</div>
                            <div class="feature-desc">Prometheus + Grafana 도입</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">자동화</div>
                            <div class="feature-desc">CI/CD 파이프라인 구축</div>
                        </div>
                    </div>
                </div>
                <div>
                    <h2>운영 개선</h2>
                    <div class="features-grid">
                        <div class="feature-card">
                            <div class="feature-title">백업 자동화</div>
                            <div class="feature-desc">DB 및 파일 정기 백업</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">에러 추적</div>
                            <div class="feature-desc">Sentry 연동</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">성능 최적화</div>
                            <div class="feature-desc">CDN 도입 검토</div>
                        </div>
                        <div class="feature-card">
                            <div class="feature-title">보안 강화</div>
                            <div class="feature-desc">Let's Encrypt 자동 갱신</div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="highlight-box">
                <strong>다음 단계:</strong>
                <ul>
                    <li>iOS 지원 확장</li>
                    <li>웹 관리 대시보드 개발</li>
                    <li>사용자별 권한 관리 시스템</li>
                    <li>배포 승인 워크플로우 구축</li>
                </ul>
            </div>
        </div>

        <!-- Slide 14: 결론 -->
        <div class="slide title-slide">
            <div class="content">
                <h1>결론</h1>
                <h2>React Native CodePush 프로젝트 성공</h2>
                
                <div style="font-size: 1.25rem; margin-top: 3rem; text-align: left; max-width: 900px;">
                    <h3 style="color: #f8fafc; margin-bottom: 1.5rem;">핵심 성과</h3>
                    <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 1rem; margin-bottom: 2rem;">
                        <div style="background: rgba(34, 197, 94, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(34, 197, 94, 0.3);">
                            배포 시간 96% 단축 (24시간 → 5분)
                        </div>
                        <div style="background: rgba(34, 197, 94, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(34, 197, 94, 0.3);">
                            앱스토어 의존성 완전 제거
                        </div>
                        <div style="background: rgba(34, 197, 94, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(34, 197, 94, 0.3);">
                            엔터프라이즈급 보안 시스템 구축
                        </div>
                        <div style="background: rgba(34, 197, 94, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(34, 197, 94, 0.3);">
                            완전한 자체 제어 환경 조성
                        </div>
                    </div>
                    
                    <h3 style="color: #f8fafc; margin-bottom: 1.5rem;">기술적 성취</h3>
                    <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 1rem;">
                        <div style="background: rgba(96, 165, 250, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(96, 165, 250, 0.3);">
                            MS CodePush 서버 완전 구축
                        </div>
                        <div style="background: rgba(96, 165, 250, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(96, 165, 250, 0.3);">
                            JWT + TLS 보안 체계 구현
                        </div>
                        <div style="background: rgba(96, 165, 250, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(96, 165, 250, 0.3);">
                            SQLite → Oracle DB 마이그레이션
                        </div>
                        <div style="background: rgba(96, 165, 250, 0.1); padding: 1rem; border-radius: 8px; border: 1px solid rgba(96, 165, 250, 0.3);">
                            무중단 배포 시스템 실현
                        </div>
                    </div>
                </div>
                
                <div class="author" style="margin-top: 3rem;">
                    <strong>감사합니다!</strong><br>
                    질문 및 토론 환영합니다
                </div>
            </div>
        </div>
    </div>

    <!-- Navigation -->
    <div class="nav-container">
        <button class="nav-btn" id="prevBtn" onclick="changeSlide(-1)">← 이전</button>
        <div class="slide-counter">
            <span id="currentSlide">1</span> / <span id="totalSlides">14</span>
        </div>
        <button class="nav-btn" id="nextBtn" onclick="changeSlide(1)">다음 →</button>
    </div>

    <script>
        let currentSlideIndex = 0;
        const slides = document.querySelectorAll('.slide');
        const totalSlides = slides.length;
        
        document.getElementById('totalSlides').textContent = totalSlides;

        function showSlide(index) {
            slides.forEach((slide, i) => {
                slide.classList.remove('active', 'prev');
                if (i === index) {
                    slide.classList.add('active');
                } else if (i < index) {
                    slide.classList.add('prev');
                }
            });
            
            document.getElementById('currentSlide').textContent = index + 1;
            document.getElementById('prevBtn').disabled = index === 0;
            document.getElementById('nextBtn').disabled = index === totalSlides - 1;
        }

        function changeSlide(direction) {
            const newIndex = currentSlideIndex + direction;
            if (newIndex >= 0 && newIndex < totalSlides) {
                currentSlideIndex = newIndex;
                showSlide(currentSlideIndex);
            }
        }

        // 키보드 네비게이션
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowRight' || e.key === ' ') {
                e.preventDefault();
                changeSlide(1);
            } else if (e.key === 'ArrowLeft') {
                e.preventDefault();
                changeSlide(-1);
            } else if (e.key === 'Home') {
                e.preventDefault();
                currentSlideIndex = 0;
                showSlide(currentSlideIndex);
            } else if (e.key === 'End') {
                e.preventDefault();
                currentSlideIndex = totalSlides - 1;
                showSlide(currentSlideIndex);
            }
        });

        // 초기 설정
        showSlide(0);

        // 터치/스와이프 지원 (모바일)
        let startX = 0;
        let endX = 0;

        document.addEventListener('touchstart', (e) => {
            startX = e.touches[0].clientX;
        });

        document.addEventListener('touchend', (e) => {
            endX = e.changedTouches[0].clientX;
            handleSwipe();
        });

        function handleSwipe() {
            const threshold = 50;
            const diff = startX - endX;
            
            if (Math.abs(diff) > threshold) {
                if (diff > 0) {
                    // 왼쪽으로 스와이프 - 다음 슬라이드
                    changeSlide(1);
                } else {
                    // 오른쪽으로 스와이프 - 이전 슬라이드
                    changeSlide(-1);
                }
            }
        }

        // 풀스크린 모드 지원
        document.addEventListener('keydown', (e) => {
            if (e.key === 'F11') {
                e.preventDefault();
                if (!document.fullscreenElement) {
                    document.documentElement.requestFullscreen();
                } else {
                    document.exitFullscreen();
                }
            }
        });

        // 슬라이드 애니메이션 개선
        function addFadeInAnimation() {
            const activeSlide = document.querySelector('.slide.active');
            if (activeSlide) {
                const elements = activeSlide.querySelectorAll('h1, h2, h3, p, ul, .code-block, .highlight-box, .warning-box, .success-box, .feature-card, .metric-card');
                elements.forEach((el, index) => {
                    el.style.opacity = '0';
                    el.style.transform = 'translateY(20px)';
                    setTimeout(() => {
                        el.style.transition = 'all 0.6s cubic-bezier(0.25, 0.46, 0.45, 0.94)';
                        el.style.opacity = '1';
                        el.style.transform = 'translateY(0)';
                    }, index * 50);
                });
            }
        }

        // 슬라이드 변경시 애니메이션 적용
        const originalShowSlide = showSlide;
        showSlide = function(index) {
            originalShowSlide(index);
            setTimeout(addFadeInAnimation, 200);
        };

        // 초기 애니메이션 적용
        setTimeout(addFadeInAnimation, 500);
    </script>
</body>
</html>
