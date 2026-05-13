# Persona — 디지털 명함 iOS 앱

> 나만의 명함을 만들고, QR 하나로 즉시 공유하는 스마트 명함 앱

[![Platform](https://img.shields.io/badge/Platform-iOS-1B2A4A)](https://apps.apple.com)
[![React Native](https://img.shields.io/badge/React%20Native-0.84-E9D9A8)](https://reactnative.dev)
[![Firebase](https://img.shields.io/badge/Firebase-Firestore-orange)](https://firebase.google.com)
[![Status](https://img.shields.io/badge/Status-App%20Store%20Review-green)]()

---

## 📱 프로젝트 소개

Persona는 산업경영공학과 4학년 재학 중 **기획부터 개발, 출시까지 혼자 진행한 iOS 앱**입니다.

종이 명함의 비효율을 디지털로 해결하고자 시작했습니다. 단순한 연락처 공유를 넘어, **브랜딩 가능한 명함 디자인 + QR 공유 + 지갑 관리 + AI 한줄소개 생성**까지 하나의 앱에 담았습니다.

현재 App Store 심사 제출 준비 중입니다.

---

## 🛠 기술 스택

| 분류 | 기술 |
|------|------|
| **Frontend** | React Native 0.84, TypeScript |
| **Backend** | Firebase Firestore, Firebase Auth, Firebase Functions |
| **AI** | Anthropic Claude API (한줄소개 생성) |
| **인증** | 카카오, 네이버, 구글 소셜 로그인 |
| **배포** | Firebase Hosting, App Store (심사 중) |

---

## ✨ 주요 기능

### 1. 명함 제작 (Studio)
- 11가지 그라디언트 + 6가지 글래스모피즘 스킨
- 클래식 / 좌측 / 우측 레이아웃 선택
- 프로필 이미지, 배경 이미지 커스터마이징
- GitHub, Notion, LinkedIn 등 포트폴리오 링크 임베드

### 2. QR 공유 (Share)
- QR코드로 즉시 명함 공유
- 이메일 서명 자동 생성 (이미지/텍스트)
- Zoom 배경 이미지 생성

### 3. 명함 지갑 (Wallet)
- 받은 명함 QR 스캔으로 자동 저장
- 즐겨찾기, 정렬, 검색 기능
- 명함 모을수록 획득하는 배지 시스템 (게이미피케이션)

### 4. AI 한줄소개
- Claude API를 활용한 개인화 한줄소개 생성
- 마음에 들지 않으면 재생성 가능

---

## 🏗 아키텍처

```
src/
├── screens/          # 화면 컴포넌트
│   ├── HomeScreen      # 내 명함 관리
│   ├── WalletScreen    # 받은 명함 지갑
│   ├── ShareScreen     # QR 공유
│   ├── EditorScreen    # 명함 편집 (Studio)
│   └── SettingsScreen  # 설정
├── components/       # 공통 컴포넌트
│   ├── CardPreview     # 명함 렌더링
│   ├── BadgeIcon       # 배지 SVG 컴포넌트
│   └── OrgSearchInput  # 학교/회사 검색
├── services/         # 비즈니스 로직
│   ├── firebase.ts     # Firebase 설정
│   ├── wallet.ts       # 지갑 CRUD
│   ├── theme.ts        # 디자인 시스템
│   └── settings.ts     # 앱 설정 관리
└── data/             # 로컬 데이터
    ├── universities.ts  # 한국 대학교 400개
    └── companies.ts     # 국내 기업 데이터
```

---

## 📊 개발 과정에서 해결한 문제들

### 오프라인 대응
Firestore 연결 실패 시 AsyncStorage 캐시로 fallback하여 오프라인에서도 명함 조회 가능하도록 구현했습니다.

### 성능 최적화
대학교/학과 검색 시 기존 Firestore API 호출 방식에서 **로컬 즉시 검색**으로 전환하여 응답 속도를 개선했습니다. (400개 대학, 200개 학과 로컬 데이터)

### LinearGradient 렌더링 이슈
`overflow: 'hidden'`이 LinearGradient 렌더링을 방해하는 문제를 파악하고, wrapper 구조를 분리하여 해결했습니다.

### 앱 심사 대응
Apple App Store 심사 기준에 맞춰 계정 삭제 기능, 개인정보처리방침 외부 URL, AI 데이터 전송 명시 등을 직접 구현했습니다.

---

## 🚀 실행 방법

```bash
# 패키지 설치
npm install

# iOS Pod 설치
cd ios && bundle exec pod install && cd ..

# Metro 실행
npx react-native start --reset-cache

# iOS 빌드
npx react-native run-ios
```

### 환경 변수
Firebase 설정 파일(`GoogleService-Info.plist`)과 환경 변수가 필요합니다.

---

## 📱 스크린샷

| 홈 | 명함 제작 | QR 공유 | 지갑 |
|:---:|:---:|:---:|:---:|
| 내 명함 관리 | 11가지 스타일 | QR 즉시 공유 | 받은 명함 저장 |

---

## 🔗 관련 링크

- **개인정보처리방침**: [persona-772ab.web.app/privacy.html](https://persona-772ab.web.app/privacy.html)
- **지원 페이지**: [persona-772ab.web.app](https://persona-772ab.web.app)

---

## 👨‍💻 개발자

**신재우** — 인하대학교 산업경영공학과 4학년

기획 · 디자인 · 개발 · 출시 준비 전 과정을 혼자 진행했습니다.

> 코딩 전공자가 아닌 산업공학도로서, 문제를 정의하고 실제로 작동하는 서비스로 만드는 과정에 집중했습니다.
