# Persona — 디지털 명함 iOS 앱

> **App Store 출시 완료** | React Native | Firebase | Claude AI  
> 🔗 [App Store 다운로드](https://apps.apple.com/kr/app/id6769245736) · [웹 명함 페이지](https://persona-772ab.web.app)

## 기획 배경

AI·DX가 빠르게 확산되는 시대에도 여전히 종이 명함이 
업무 현장의 표준으로 남아있다는 점에서 아이러니를 느꼈습니다.

국내 직장인 대부분이 스마트폰으로 결제·일정·업무를 처리하면서도, 
첫 만남의 정보 교환만큼은 아날로그 방식에 머물러 있습니다. 
기존 디지털 명함 앱들도 단순 연락처 저장에 그쳐 
'진짜 DX'가 이루어졌다고 보기 어려웠습니다.

명함 교환 이후의 팔로업, 맥락 관리, Apple Wallet 연동까지
명함의 전체 라이프사이클을 디지털화하고,
Claude AI를 활용해 종이 명함 OCR 스캔·대화 주제 추천까지 구현함으로써
국내 직장인의 네트워킹 경험을 실질적으로 끌어올리고자 했습니다.

---

## 프로젝트 개요

**Persona**는 종이 명함을 대체하는 스마트 디지털 명함 iOS 앱입니다.  
QR 코드 기반 공유, AI 명함 스캔, Apple Wallet 연동까지 — 명함 교환의 전 과정을 디지털화했습니다.

| 항목 | 내용 |
|------|------|
| 플랫폼 | iOS (React Native 0.84.1) |
| 출시일 | 2026년 (App Store 심사 통과) |
| 번들 ID | com.jaewoo.persona |
| App Store ID | 6769245736 |
| 백엔드 | Firebase (Firestore, Auth, Storage, Functions) |
| 웹 | Firebase Hosting (persona-772ab.web.app) |

---

## 주요 기능

### 🪪 명함 생성 & 편집
- 11가지 그라디언트 + 글래스모피즘 6종 디자인 테마
- 실시간 명함 프리뷰 (CardPreview 컴포넌트)
- 배경사진 / 프로필 사진 업로드 (Firebase Storage 연동)
- 명함 자동 업데이트 (Firestore `onSnapshot` 실시간 동기화)

### 🤖 AI 기능 (Claude API)
- **종이명함 OCR 스캔**: 카메라로 종이 명함 촬영 → Claude API가 텍스트 자동 추출 및 필드 파싱
- **AI 대화 주제 추천**: 받은 명함 기반으로 첫 대화 아이디어 추천
- Firebase Cloud Functions를 프록시로 활용해 API 키 보안 처리

### 📲 공유 & 수신
- QR 코드 생성 및 공유
- 카카오톡 공유 → 딥링크(`persona://`) → 지갑 자동 저장
- 웹 명함 페이지: `persona-772ab.web.app/card/{userId}` (Three.js 3D 렌더링)

### 👛 명함 지갑
- 받은 명함 관리 (만남 맥락 태그 + 행사 그룹핑)
- Apple Wallet 패스 생성 및 저장 (Firebase Functions 서버사이드 서명)
- 팔로업 알림 (notifee), 리마인더 날짜 직접 설정
- 명함 만료 알림, 대화 타임라인

### 🔐 인증
- 카카오 / 네이버 / 구글 / 이메일 / Apple 로그인 (5종)
- Firebase Auth 기반 통합 인증

### 🌗 기타
- 다크 / 라이트 모드 지원
- 홈화면 위젯 (App Groups 연동)
- 탭바 한글화

---

## 기술 스택

### 프론트엔드
| 기술 | 용도 |
|------|------|
| React Native 0.84.1 | iOS 앱 개발 |
| TypeScript | 타입 안정성 |
| notifee | 로컬 푸시 알림 |
| react-native-vision-camera | OCR 카메라 |

### 백엔드 / 인프라
| 기술 | 용도 |
|------|------|
| Firebase Firestore | 실시간 데이터베이스 |
| Firebase Auth | 소셜 로그인 통합 |
| Firebase Storage | 이미지 업로드 |
| Firebase Cloud Functions | Claude API 프록시, Apple Wallet 패스 생성, OG 데이터 파싱 |
| Firebase Hosting | 웹 명함 페이지 배포 |

### 외부 API / 서비스
| 서비스 | 용도 |
|--------|------|
| Claude API (Anthropic) | OCR, AI 대화 추천 |
| Apple Wallet (PassKit) | 디지털 명함 패스 |
| 카카오 SDK | 로그인, 공유, 딥링크 |
| 네이버 OAuth | 로그인 |
| Three.js | 웹 명함 3D 렌더링 |

---

## 아키텍처

```
사용자
  │
  ├── iOS 앱 (React Native)
  │     ├── 명함 생성/편집 (Firestore 저장)
  │     ├── QR 공유 → 딥링크 수신
  │     ├── OCR 스캔 → claudeProxy (Cloud Function) → Claude API
  │     └── Apple Wallet → generateWalletPass (Cloud Function)
  │
  └── 웹 (Firebase Hosting)
        └── card.html → Firestore 명함 데이터 → Three.js 렌더링
```

---

## 프로젝트 구조

```
Persona/
├── src/
│   ├── screens/
│   │   ├── HomeScreen.tsx       # 명함 목록 + ViewShot 저장
│   │   ├── ShareScreen.tsx      # 공유 + 딥링크
│   │   ├── WalletScreen.tsx     # 받은 명함 지갑
│   │   ├── CardScanScreen.tsx   # OCR 스캔
│   │   ├── EditorScreen.tsx     # 명함 편집
│   │   └── SettingsScreen.tsx   # 설정 + 프로필
│   ├── services/
│   │   ├── firebase.ts
│   │   ├── wallet.ts
│   │   ├── cards.ts
│   │   └── settings.ts
│   └── components/
│       └── CardPreview.tsx      # 명함 렌더링
├── functions/
│   ├── public/
│   │   └── card.html            # 웹 명함 페이지
│   └── functions/lib/index.js   # Cloud Functions
└── ios/
```

---

## 개발 환경

| 항목 | 버전 |
|------|------|
| macOS | 26 베타 |
| Xcode | 26.2 |
| React Native | 0.84.1 |
| Node.js | v24.14.0 |
| Ruby | 3.3.11 |
| CocoaPods | 1.16.2 |

---

## 향후 개발 계획 (v1.2)

### 기업용 팀 기능
- 팀 생성 및 초대 코드 시스템
- 관리자: 팀원 명함 브랜드 통일 (로고/색상)
- 통계 대시보드 (팀원별 조회수)
- 수익 모델: 팀 Pro 월 19,900원 / Enterprise 월 49,900원

### 장기 로드맵
- 커스텀 도메인 (persona.io)
- Android 포팅
- 다국어 지원 (영어)
- NFC 카드 하드웨어 연동

---

## 개발자

산업경영공학과 4학년  
Apple Developer Program 등록 완료 (Team ID: R9X886G4DM)  
App Store 단독 출시 및 운영 중
