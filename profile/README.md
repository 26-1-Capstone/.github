## 🎬 시연 동영상(수정)

- <img src="https://cdn-icons-png.flaticon.com/512/727/727245.png" width="18"/> 
  [안드로이드 시연 영상](https://youtube.com/shorts/nsMjESt-zVo?feature=share)

- [<img src="https://cdn-icons-png.flaticon.com/512/727/727245.png" width="18"/> 
  아이폰 시연 영상 보기](https://youtu.be/Mm-R8tdzca0)

  

- <img src="https://cdn-icons-png.flaticon.com/512/727/727245.png" width="18"/> 
  [리액트(PC) 시연 영상](영상링크)

<div align="center">

# 📚 NutriShare - 공동구매 쇼핑몰 프로젝트

</div>

## 📖 목차

- [🎯 프로젝트 소개](#-프로젝트-소개)
- [🏠 홈페이지](#-홈페이지)
- [📌 기능 구성도](#-기능-구성도)
- [📌 API 명세서](#-api-명세서)
- [💻 코드](#-코드)
- [📱 App 설치](#-app-설치)
- [🎬 시연 동영상](#-시연-동영상)
- [🏆 작년 우수팀과의 비교표](#-작년-우수팀과의-비교표)
- [👥 팀원 소개](#-팀원-소개)

## 🎯 프로젝트 소개

NutriShare는 생필품 등 상품을 **공동구매**로 모집하고, **장바구니·주문·결제·마이페이지**까지 이어지는 흐름을 제공하는 쇼핑/공구 서비스입니다. 웹(React)과 모바일 앱이 동일한 백엔드 API(`api/v1`)를 사용하는 구조입니다.

### 핵심 기능

- 공동구매 모집·참여·목록/상세 조회
- 장바구니 담기 및 주문/결제 시스템(시뮬레이션 결제 플로우)
- 공동구매 참여 기준 리뷰 작성 및 조회
- JWT 기반 사용자 인증(액세스 토큰 + 리프레시 재발급), 카카오·구글 OAuth 로그인
- 관리자 물품 등록/삭제 및 이미지 관리(AWS S3) — **백엔드/API 상세는 저장소 기준**
- **Android, iOS, 데스크탑(웹)** 에서 이용 가능한 크로스플랫폼 구성

## 🏠 홈페이지

| 구분 | URL / 설명 |
|------|------------|
| **운영 API (백엔드)** | Base URL: `http://3.36.139.67`，REST 프리픽스: **`/api/v1`** (환경변수 `VITE_API_BASE_URL`와 동일 호스트) |
| **OAuth 로그인 시작** | 백엔드 호스트 기준 `http://3.36.139.67/oauth2/authorization/{kakao\|google}` (프론트 `LoginPage`에서 동일 호스트로 조합) |
| **웹(React) 배포** | 프로젝트 루트 `vercel.json`에서 `/api/*`를 위 백엔드로 프록시하도록 설정되어 있음 → **Vercel에 연결된 실제 도메인**이 웹 주소(배포 후 대시보드에서 확인). 저장소에는 고정 도메인 문자열이 없음 |
| **로컬 개발** | 저장소 루트에서 `npm install` 후 `npm run dev` (Vite). API는 `.env`의 `VITE_API_BASE_URL` 또는 프록시 설정에 맞출 것 |

**웹 주요 화면(라우트)**  
로그인 `/login`, OAuth 콜백 `/login/callback`, 홈 `/`, 검색 `/search`, 상품 상세 `/products/:id`, 공구 목록·상세 `/groups`, `/groups/:id`, 공구 생성 `/groups/new`(로그인 필요), 장바구니 `/cart`, 결제 `/checkout`, 주문 완료 `/orders/:id/complete`, 마이페이지 `/mypage`, 프로필 수정 `/mypage/edit`.

## 📌 기능 구성도

### 시스템 관점

```mermaid
flowchart LR
  subgraph client["클라이언트"]
    WEB["React 웹\n(Vite)"]
    AND["Android 앱"]
    IOS["iOS 앱"]
  end

  subgraph api["Spring Boot\nNutriShare API"]
    AUTH["인증·회원\nJWT / OAuth2"]
    CAT["상품·검색"]
    GB["공동구매"]
    CART["장바구니"]
    ORD["주문"]
    PAY["결제 확정"]
    MYP["마이페이지·리뷰"]
  end

  subgraph data["데이터·외부"]
    MYSQL[("MySQL")]
    REDIS[("Redis")]
    S3["AWS S3\n(상품 이미지 등)"]
    OAUTH["Kakao / Google"]
  end

  WEB --> api
  AND --> api
  IOS --> api
  AUTH --> OAUTH
  AUTH --> REDIS
  CAT --> MYSQL
  GB --> MYSQL
  CART --> MYSQL
  ORD --> MYSQL
  PAY --> MYSQL
  MYP --> MYSQL
  CAT --> S3
