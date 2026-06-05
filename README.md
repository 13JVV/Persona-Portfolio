# Persona — 디지털 명함 앱

> 나만의 명함을 만들고, QR로 공유하고, 받은 명함을 스마트하게 관리하는 iOS 앱

[![App Store](https://img.shields.io/badge/App_Store-출시됨-0D96F6?logo=apple&logoColor=white)](https://apps.apple.com/kr/app/id6769245736)
[![Platform](https://img.shields.io/badge/Platform-iOS-lightgrey?logo=apple)](https://reactnative.dev)
[![React Native](https://img.shields.io/badge/React_Native-0.84-61DAFB?logo=react&logoColor=white)](https://reactnative.dev)
[![Firebase](https://img.shields.io/badge/Firebase-12.x-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com)
[![Claude API](https://img.shields.io/badge/Claude_API-Sonnet_·_Haiku-D97706)](https://anthropic.com)

<br/>

## 프로젝트 소개

**Persona**는 종이 명함을 완전히 대체하는 iOS 디지털 명함 앱입니다.

명함을 직접 디자인하고 QR 코드 하나로 즉시 공유할 수 있으며, 카메라로 찍은 종이 명함은 AI가 자동으로 인식·저장합니다. 받은 명함에는 만난 날짜·장소를 기록하고, 팔로업 알림과 AI 대화 주제 추천으로 관계를 이어갑니다.

2025년 App Store 출시 후 실사용자 대상으로 운영 중인 개인 프로덕트입니다.

**App Store:** https://apps.apple.com/kr/app/id6769245736

<br/>

## 주요 기능

### 명함 생성 & 커스터마이징
- 이름·직함·소속·연락처·SNS(Instagram, GitHub, 포트폴리오) 정보 입력
- **13종 카드 스킨** — Navy, Cream, Mono, Minimal, SerifPress + 글래스모피즘 6종
- 서체(Georgia, System), 글씨 크기 6단계 실시간 미리보기 조절
- 최대 5개의 Persona(명함 프로필) 생성 및 전환

### QR 공유 & 스캔
- 명함 정보를 QR 코드로 인코딩, 1탭으로 즉시 공유
- QR 스캔 → 자동으로 상대 명함 저장
- **딥링크** (`persona://card?...`) 지원 — 링크 탭만으로 명함 수신
- 카카오톡 공유, Firebase Hosting 랜딩 페이지 경유 딥링크

### AI 명함 인식
- 실제 종이 명함을 카메라로 촬영
- **Claude Sonnet 비전 모델**이 이미지에서 이름·직함·연락처 등 자동 추출 및 JSON 파싱
- AbortController 기반 20초 타임아웃, 단계별 진행 상태 표시

### 지갑 (Wallet)
- 받은 명함 카테고리 분류(친구·직장·고객 등) 및 이름 검색
- **만남 컨텍스트** — 만난 날짜·장소·메모 기록
- **AI 대화 주제 추천** (Claude Haiku) — 명함 정보 기반 아이스브레이킹 주제 생성
- **팔로업 리마인더** — 교환 후 N일이 지나면 알림

### Apple Wallet 연동
- 명함을 `.pkpass`로 생성하여 Apple Wallet에 추가
- Cloud Functions + PassKit 서버사이드 생성, P12 인증서 Firebase Secrets 관리

### iOS 위젯
- 홈 화면·잠금화면 위젯: 활성 명함의 이름·직함·QR 코드 표시
- **App Groups + WidgetKit**으로 앱↔위젯 실시간 데이터 동기화

### 알림
- 팔로업 알림: 명함 교환 3일 후 로컬 알림
- 장기 미연락 알림: 3개월 이상 연락 없는 명함 자동 감지

### 소셜 로그인 & 인증
- **Kakao · Naver** (Custom Token via Cloud Functions)
- **Google · Apple · Email/Password**
- Firebase Auth + AsyncStorage 영속화로 자동 로그인

<br/>

## 기술 스택

| 분류 | 기술 |
|---|---|
| **프레임워크** | React Native 0.84.1, React 19, TypeScript |
| **백엔드** | Firebase Auth, Cloud Firestore, Cloud Storage, Cloud Functions (Node.js) |
| **AI** | Anthropic Claude API — Sonnet (명함 Vision 인식), Haiku (대화 주제 생성) |
| **소셜 로그인** | Kakao, Naver, Google, Apple Sign In, Email |
| **네이티브 연동** | Apple Wallet (PassKit), iOS Widget (WidgetKit + App Groups) |
| **UI / 애니메이션** | React Native Animated API (useNativeDriver), react-native-linear-gradient, react-native-svg |
| **QR** | react-native-qrcode-svg, react-native-camera-kit |
| **알림** | @notifee/react-native (로컬 트리거 알림) |
| **딥링크** | React Native Linking API + Firebase Hosting 랜딩 페이지 |

<br/>

## 아키텍처 개요

```
┌──────────────────────────────────────────────────────────────┐
│                         iOS App                              │
│                                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌────────────┐  │
│  │  Home    │  │  Wallet  │  │  Share   │  │  Editor /  │  │
│  │ (내 명함) │  │ (받은명함)│  │  (QR공유) │  │  Settings  │  │
│  └──────────┘  └──────────┘  └──────────┘  └────────────┘  │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐   │
│  │       커스텀 네비게이터 (Animated API + screenRef)     │   │
│  │   탭 전환: fade + translateX  /  오버레이: slide-up   │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                              │
│  ┌─────────────┐  ┌───────────────┐  ┌──────────────────┐  │
│  │ ThemeContext │  │ Cards/Wallet  │  │ Notifications /  │  │
│  │ (테마·폰트)  │  │ Service       │  │ WidgetBridge     │  │
│  └─────────────┘  └───────────────┘  └──────────────────┘  │
└──────────────────────────┬───────────────────────────────────┘
                           │
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
    ┌────────────┐  ┌────────────┐  ┌────────────────┐
    │  Firebase  │  │ Claude API │  │ Cloud Functions │
    │  Auth +    │  │ (Vision +  │  │ (Wallet Pass,  │
    │  Firestore │  │  Chat)     │  │ SNS Auth Proxy)│
    └────────────┘  └────────────┘  └────────────────┘
```

<br/>

## 프로젝트 구조

```
Persona/
├── App.tsx                       # 루트 컴포넌트, 커스텀 네비게이터
├── src/
│   ├── screens/
│   │   ├── HomeScreen.tsx        # 내 명함 뷰어 및 카드 선택기
│   │   ├── WalletScreen.tsx      # 받은 명함 관리 (검색·분류·메모)
│   │   ├── ShareScreen.tsx       # QR 생성 및 공유
│   │   ├── EditorScreen.tsx      # 명함 편집기
│   │   ├── CardScanScreen.tsx    # AI 명함 인식 (Claude Vision)
│   │   ├── QRScanScreen.tsx      # QR 스캔
│   │   ├── SettingsScreen.tsx    # 설정 (테마·폰트·알림 등)
│   │   ├── LoginScreen.tsx       # 소셜 로그인 5종
│   │   └── OnboardingScreen.tsx  # 최초 명함 생성 플로우
│   ├── services/
│   │   ├── firebase.ts           # Firebase 초기화 (AsyncStorage 영속화)
│   │   ├── ThemeContext.tsx      # 전역 테마 · 폰트스케일 Context
│   │   ├── theme.ts              # 색상 토큰, 카드 스킨 13종, 타입 스케일
│   │   ├── cards.ts              # 내 명함 CRUD (Firestore)
│   │   ├── wallet.ts             # 지갑 명함 CRUD (Firestore)
│   │   ├── settings.ts           # 설정값 AsyncStorage 관리
│   │   ├── FollowUpNotification.ts  # 팔로업·장기미연락 알림 스케줄러
│   │   └── WidgetDataBridge.ts   # App Groups 통한 위젯 데이터 동기화
│   └── components/
│       ├── CardPreview.tsx       # 명함 렌더링 컴포넌트 (글래스모피즘 포함)
│       ├── Toast.tsx             # 전역 토스트 메시지 시스템
│       ├── TutorialTooltip.tsx   # 온보딩 말풍선 (AsyncStorage 기반 1회 표시)
│       └── PermissionPrompt.tsx  # 카메라·알림 권한 요청 UI
└── functions/
    ├── src/index.ts              # Cloud Functions: Wallet Pass 생성, Claude Proxy, SNS Auth
    └── public/card.html          # 딥링크 랜딩 페이지 (카카오 공유 → 앱 이동)
```

<br/>

## 핵심 엔지니어링 구현

### 1. 커스텀 애니메이션 네비게이터

React Navigation 없이 `Animated API` + `useCallback` + `useRef`로 직접 구현한 화면 전환 시스템. `useNativeDriver: true`로 네이티브 스레드에서 60fps 보장.

- **탭 전환**: 60ms 페이드 아웃 → 이동 방향에 따라 ±24px translateX spring → 180ms 페이드 인
- **오버레이 화면** (스캔·카드추가): translateY 48px → spring 슬라이드 업
- **stale closure 방지**: `screenRef`로 최신 화면 상태 참조, 중복 전환 차단

```typescript
const navigateTo = useCallback((newScreen: string) => {
  if (newScreen === screenRef.current) return;
  screenRef.current = newScreen;

  Animated.timing(fadeAnim, { toValue: 0, duration: 60, useNativeDriver: true })
    .start(() => {
      setScreen(newScreen);
      // 전환 타입별 slideX / slideY spring 분기
      Animated.parallel([...]).start();
    });
}, []);
```

### 2. AI 명함 인식 파이프라인

```
카메라 촬영 → RNFS base64 인코딩 → Firebase Auth 토큰 검증
→ Cloud Functions Claude Proxy → Sonnet Vision API
→ JSON 파싱 (name, role, org, phone, email, …) → Firestore 저장
```

- **AbortController** 기반 20초 타임아웃으로 무한 대기 방지
- 단계별 진행 상태 표시: `"이미지 준비 중..."` → `"AI가 분석 중이에요..."`
- 황금색 스캔라인 애니메이션으로 촬영 가이드 제공

### 3. Firebase Auth 자동 로그인 안정화

**문제**: `SplashScreen`의 2.5초 타이머가 Firebase가 AsyncStorage 세션을 복원하기 전에 발화할 경우, stale closure의 `user = null`로 인해 로그인 화면으로 강제 이동하는 race condition.

**해결**: `userRef`로 항상 최신 인증 상태를 참조하도록 분리.

```typescript
// 타이머 만료 시점에 최신 user를 읽도록 ref 사용
const userRef = useRef<User | null>(null);
useEffect(() => { userRef.current = user; }, [user]);

// onDone: stale closure 없이 항상 최신 상태 참조
<SplashScreen onDone={() => navigateTo(userRef.current ? 'home' : 'login')} />
```

### 4. 전역 폰트 스케일 시스템

`ThemeContext`에 `fontScale`(0.82× ~ 1.41×) 통합, AsyncStorage 영속화. 설정 화면의 실시간 미리보기 모달에서 6단계 선택.

### 5. Apple Wallet 패스 서버사이드 생성

Cloud Functions에서 `passkit-generator`로 `.pkpass` 동적 생성. P12 인증서는 Base64 인코딩하여 Firebase Secrets에 저장, 런타임에 복호화.

<br/>

## 릴리즈 노트

| 버전 | 주요 변경사항 |
|---|---|
| **v1.1** | Apple Wallet 연동, iOS 홈·잠금화면 위젯, 만남 컨텍스트 기록, 팔로업 알림 고도화, Apple Sign In, 명함 스킨 리뉴얼 |
| **v1.0** | 명함 생성·공유·AI 스캔, 지갑, 카카오·네이버·Google 로그인, App Store 출시 |

<br/>

## 개발 환경 세팅

```bash
# 1. 의존성 설치
yarn install
cd ios && bundle exec pod install && cd ..

# 2. iOS 실행
npx react-native run-ios --device
```

> **Note**: `GoogleService-Info.plist`, SNS 앱 키 등 민감 정보는 `.gitignore` 처리되어 있어 별도 설정이 필요합니다.

<br/>

## 개발자

**신재우**

- Email: sjwtlswodn@naver.com
- App Store: [Persona](https://apps.apple.com/kr/app/id6769245736)

<br/>

---

*React Native + Firebase 풀스택, Anthropic Claude API 연동, Apple 네이티브 기능(Wallet · Widget) 통합을 포함한 개인 프로덕트입니다.*
