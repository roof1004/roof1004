<div align="center">

# 안녕하세요, 류진환(Roof)입니다 👋

**설계부터 고민하는 백엔드 개발자**
도메인 규칙을 문서로 확정하고, 동시성·보안·데이터 정합성까지 코드로 책임집니다.

[![Portfolio](https://img.shields.io/badge/Portfolio-roof1004.github.io-222?style=flat-square&logo=githubpages&logoColor=white)](https://roof1004.github.io)
[![Email](https://img.shields.io/badge/Email-your.email@example.com-EA4335?style=flat-square&logo=gmail&logoColor=white)](mailto:y0204jin@gmail.com)

</div>

---

## 🛠 Tech Stack

**Backend**
![Java](https://img.shields.io/badge/Java_21/25-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![Spring Security](https://img.shields.io/badge/Spring_Security-6DB33F?style=flat-square&logo=springsecurity&logoColor=white)
![JPA](https://img.shields.io/badge/JPA-59666C?style=flat-square&logo=hibernate&logoColor=white)
![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=flat-square&logo=nestjs&logoColor=white)
![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)

**Database / Infra**
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)
![Flyway](https://img.shields.io/badge/Flyway-CC0200?style=flat-square&logo=flyway&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![OpenAPI](https://img.shields.io/badge/OpenAPI-6BA539?style=flat-square&logo=openapiinitiative&logoColor=white)

**Frontend / Mobile**
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![React Native](https://img.shields.io/badge/Expo-000020?style=flat-square&logo=expo&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat-square&logo=nextdotjs&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)

---

## 🚀 Projects

### ⚽ 에어리어 리그 (area_league) — 아마추어 축구 리그·매칭 앱 · 1인 개발
> 시 단위 85개 리그의 순위, 경기 일정·매칭, 용병 모집까지 다루는 상용 출시 목표 iOS/Android 앱

`NestJS` `Prisma 6` `PostgreSQL` `Docker` `jose(JWT)` `OpenAPI` `Expo`

- **규모** — 백엔드 15개 도메인 · OpenAPI **122개 오퍼레이션** · Prisma **43개 모델** · 커밋 93개 (전부 본인)
- **스펙 우선 개발** — OpenAPI 스펙을 먼저 확정하고 Redocly lint로 검증, 스펙에서 타입을 생성해 서버·앱 계약을 맞춤
- **인증 설계**
  - Refresh 토큰 **회전 + 재사용 탐지 시 전 세션 폐기**, access는 무상태·HS256 고정으로 alg 다운그레이드 차단
  - 가입·재인증·번호변경용 단기 토큰 5종을 `ConsumedTokenJti` 테이블로 **단회 소비** (insert unique 충돌 = 재사용 거부)
  - 카카오·구글·네이버·Apple 소셜 검증기를 공통 인터페이스 뒤에 provider별로 구현
- **개인정보 보호**
  - 전화번호는 **E.164 정규화 → HMAC(도메인 접두사 분리) + AES-256-GCM** 저장, 원문은 요청 처리 중 메모리에만 존재
  - SMS 본인인증: 3분 만료, 5회 실패 시 30분 잠금(423), 번호당 10회·IP당 20회/일 제한 — IP도 원문 대신 해시로 카운트
  - 차단된 사용자 프로필은 403이 아닌 **404로 은폐**해 존재 여부 노출 차단
- **도메인 로직**
  - 경기 결과 **양 팀 상호검증** — 팀별 제출을 대조해 승인 / 불일치 재입력 / 관리자 중재로 분기, 승인 시 시즌 기록을 한 트랜잭션에서 증분
  - 순위 정렬기를 순수 함수로 분리 — 승점 → 골득실 → 다득점 → **동률 그룹 내 승자승** → 가나다
  - **멱등성 키 인터셉터** (유저·경로·키 스코프, 같은 키·다른 본문은 422)
  - 시즌 종료·리마인더·만료 정리 등 **배치 작업**을 재실행해도 안전하게 설계
- **보안 운영** — Gitleaks pre-commit, 노출된 시크릿 4종 교체, env 스키마를 **fail-closed**로 수정(NODE_ENV가 .env에만 있을 때 fail-open 되던 문제)

### 🥚 하루도감 (day_log) — 습관 인증으로 캐릭터를 키우는 앱 · 백엔드 1인 개발
> 하루 습관을 인증하면 인증 패턴(규칙성·시간대·완주력·집중도·강도)에 따라 알이 특정 종으로 부화하는 자기관리 게임

`Java 25` `Spring Boot 4.1` `Spring Security` `JPA` `MySQL 8.4` `Flyway` `JWT`

- **규모** — 16개 도메인 패키지 · Java 파일 327개 · Flyway 마이그레이션 20개 · 기능 브랜치 → PR 30건
- **동시성 제어**
  - 인증·부화가 겹칠 때 알 XP와 재화가 이중 지급되던 문제 → 활성 알을 **비관적 락(PESSIMISTIC_WRITE)으로 트랜잭션 첫 접근에 조회**하도록 순서 재배치
  - 재화 지갑은 **원장(ledger) 기반** + 비관적 락, `@Version` 낙관적 락은 안전망으로 유지, 락 충돌은 500 대신 409로 매핑
- **종 판정 알고리즘**
  - 인증 기록에서 5개 축 지표를 계산해 종별 점수 산출 → **점수 비례 확률**로 종 선택 (설정값으로 순위 가중 모드와 전환 가능)
  - 종마다 **시그니처 게이트**를 두어, 핵심 지표가 0인데 범용 축 점수로 당선되던 문제 해결 (의도 일치 10/12 → 11/12)
  - DB 없이 돌아가는 **12개 패턴 밸런스 리포트 생성기**와 회귀 테스트로 규칙 변경 검증
- **XP·인증 규칙** — CHECK/PHOTO/STEP/COUNT/집중 타이머 5가지 인증, 습관별·유저별 **일일 XP 2중 상한**, 인증 취소는 역거래로 XP·재화 회수
- **집중 타이머 부정 방지** — 클라이언트가 보고한 시간을 **서버 경과시간 + 버퍼**로 상한
- **하루 경계** — KST 새벽 5시 기준 `service_date`를 UTC 저장 DB의 생성 컬럼과 앱 코드에서 동일하게 계산
- **식별자** — 외부 노출 ID를 **UUIDv7 / BINARY(16)** `public_id`로 분리
- **출시 필수 기능** — Refresh 토큰 해시 저장·회전, 유예 기간 방식 회원 탈퇴, 차단·신고, 약관 동의 이력, 푸시 토큰 등록

### 🏆 레시비 (recibi.kr) — AI 바이브톤 최우수상 · 백엔드 담당
> 유튜브 레시피 URL을 넣으면 "사 먹는 가격 vs 직접 만드는 재료비"를 비교해 주는 서비스

`Next.js Route Handlers` `TypeScript` `Supabase(PostgreSQL · RLS)` `Gemini`

- **레시피 추출 파이프라인** (PoC → 제품 이식, 약 7,000줄) — 설명란에 재료가 있는 영상이 20%뿐인 것을 확인하고 **설명란 → 고정 댓글** 폴백 사슬로 설계, 어느 단계에서 가져왔는지 trail로 기록
- **가격 3계층** — ① KAMIS 실시세(하루 1회 캐시) · 시드 스냅샷 → ② 네이버 쇼핑 API / Gemini 검색 그라운딩(중앙값 선택, 출처 없는 값은 버림) → ③ 사용자 직접 입력
- **재료명 정규화 3단계** — 규칙 → DB 별칭 캐시 → 남은 것만 LLM 일괄 호출, 결과를 캐시에 저장해 **이름당 LLM 1회**로 비용 고정
- **단위 환산** — 큰술·컵·개·모 등 조리 단위를 g/ml로 환산하고, 영상 설명란에 적힌 계량 기준("1컵은 180ml 종이컵")을 찾아 기본값보다 우선 적용
- **팬트리 추천** — LLM은 메뉴 추론만, 금액은 가격 파이프라인에서만 계산 (스키마에 가격 필드를 두지 않아 LLM이 금액을 지어내지 못하게 함)
- Supabase 스키마·**RLS 정책**·마스터 데이터 마이그레이션, 카카오 로그인·탈퇴, 북마크 API

### 🚨 런포유 (Run For You) — 긴급 A/S 배정 B2B SaaS · 5인 팀
> 프랜차이즈 카페·무인매장 설비 고장 시 최적의 기사를 실시간 배정하는 플랫폼

`Spring Boot` `JPA` `MySQL` `Redis` `SSE`

- **18개 테이블 ERD 단독 설계** + LMS 확장 도메인(19~28번 테이블)
- **가중치 기반 기사 배정 알고리즘** — 거리 30% · 전문분야 25% · 평점 20% · 가용성 15% · 긴급도 10%
- **SSE 실시간 알림**과 중복 배정 방지용 **Redis 분산 락** 아키텍처 설계

### 🎯 라인업 (Lineup) — AI 채용공고 추천
`Spring Boot` `JPA` `Flyway` `Next.js`

- 프로필-공고 적합도 **가중치 모델** 설계, Spring Boot + Next.js 모노레포
- 북마크 **수직 슬라이스**(Flyway 마이그레이션 → API → 화면), 인증 기능 구현

### 💬 ErrorLog — 개발자 소셜 플랫폼 (멋쟁이사자처럼 팀 프로젝트)
`Java 21` `Spring Boot` `JPA` `MySQL`

- **좋아요·댓글·팔로우** 도메인 담당, 팀원 코드와 **Spring Security 통합** 및 머지 후 버그 수정

---

## 📌 Activities & Awards

| 기간 | 내용 |
|---|---|
| 2026.09 | 🏆 **AI 바이브톤 최우수상** (레시비) |
| 2026.07 | 네이버 부스트캠프 **AI Agent Challenge** |
| 2026.02 ~ 07 | 멋쟁이사자처럼 **백엔드 Java 과정** |
| — | **NYPC Code Battle 39위** |

## 🎓 Education

- 전남대학교 컴퓨터공학과 (4학년 휴학 중)
- 덕수고등학교 컴퓨터정보과 졸업

---

<div align="center">

**결정은 문서로, 규칙은 코드로, 검증은 테스트로.**
AI를 설계 파트너로 쓰되, 모든 결정의 근거를 직접 설명할 수 있게 개발합니다.

</div>
