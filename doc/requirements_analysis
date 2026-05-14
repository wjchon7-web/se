# One-Cart (원카트) 요구사항 분석서
**Unified Shopping Cart System (USCS)**

---

| 항목 | 내용 |
|------|------|
| 문서번호 | [원카트] 요구사항분석서_Doc002 |
| 작성일 | 2026.05.14 |
| 버전 | v1.0 |
| 작성자 | |
| 소속/팀 | |

---

## 제/개정 이력

| 버전 | 날짜 | 작성자 | 제/개정 사항 |
|------|------|--------|------------|
| 1.0 | 2026.05.14 | | 최초 작성 |

---

## 목 차

1. 서론
   - 1.1 목적 및 범위
   - 1.2 용어 정의
   - 1.3 참조 문서
2. 시스템 개요
   - 2.1 소프트웨어 컨텍스트(Context)
   - 2.2 기능 분류 및 설명
3. 요구사항 명세
   - 3.1 정적 분석 (클래스 다이어그램)
   - 3.2 CRC 카드
   - 3.3 동적 분석 (순차 다이어그램)
4. 인터페이스 분석
5. 제약사항
6. 요구사항 추적표
7. 참고문헌 및 부록

---

## 1. 서론

### 1.1 목적 및 범위

이 문서는 One-Cart(원카트) 프로젝트의 요구사항을 분석하는 문서이다. 쿠팡, 네이버쇼핑, 11번가 등 여러 이커머스 플랫폼에 흩어진 장바구니 품목을 하나의 통합 플랫폼에서 조회·관리할 수 있는 시스템에 대하여 기능 관점(유스 케이스 다이어그램 및 설명서), 구조 관점(클래스 다이어그램), 행위 관점(순차 다이어그램)의 분석 결과를 기술한다.

### 1.2 용어 정의

| 용어 | 설명 |
|------|------|
| 장바구니 동기화 | 이커머스 플랫폼의 장바구니 데이터를 크롤링 또는 API를 통해 One-Cart 시스템과 실시간으로 일치시키는 작업 |
| 크롤링 (Crawling) | 웹 페이지를 자동으로 탐색하여 필요한 데이터를 수집하는 기술. 각 플랫폼의 이용약관 및 저작권법 준수가 전제됨 |
| OAuth | 사용자 인증 정보를 직접 저장하지 않고 외부 서비스로부터 접근 권한을 위임받는 인증 프로토콜 |
| API | 응용 프로그램이 운영체제 또는 다른 프로그램의 기능을 사용할 수 있도록 제공하는 인터페이스 |
| 최저가 알림 | 동일 상품이 다른 플랫폼에서 더 저렴하게 판매되는 경우 사용자에게 알려주는 기능 |
| 공유 바구니 | 여러 플랫폼의 상품을 모아 타인과 공유할 수 있는 위시리스트 |
| FCM | Firebase Cloud Messaging. 앱 푸시 알림 발송에 사용되는 외부 API |
| SMTP | 이메일 전송에 사용되는 표준 통신 프로토콜 |

### 1.3 참조 문서

- [원카트] 요구사항정의서_Doc001.pdf

---

## 2. 시스템 개요

### 2.1 소프트웨어 컨텍스트(Context)

#### 2.1.1 Actor Table

| Actor | Role |
|-------|------|
| 사용자 | One-Cart 서비스에 가입하여 통합 장바구니 조회, 최저가 비교, 예산 관리, 위시리스트 공유 기능을 사용하는 주체 |
| 시스템 | 쇼핑몰 장바구니 데이터 동기화, 최저가 비교 정보 제공, 알림 발송 등 자동화된 처리를 수행하는 주체 |
| 외부 쇼핑몰 | 쿠팡, 네이버쇼핑, 11번가 등 OAuth API 또는 크롤링을 통해 장바구니 데이터를 제공하는 외부 시스템 |
| 외부 알림 서비스 | FCM(앱 푸시) 및 SMTP(이메일) 알림 발송을 처리하는 외부 시스템 |

#### 2.1.2 유스 케이스 다이어그램

```
┌─────────────────────────────────────────────────────────────────┐
│                     <<One-Cart System>>                         │
│                                                                 │
│          ○ 회원가입을 한다. (UC-01)                              │
│         / ○ 소셜 로그인을 한다. (UC-02)                          │
│사용자──┤  ○ 쇼핑몰을 연동한다. (UC-03)                           │
│        │  ○ 통합 장바구니를 조회한다. (UC-04)                    │
│        │  ○ 품목을 수정/삭제한다. (UC-05)                        │
│        │  ○ 최저가 정보를 확인한다. (UC-06)                      │
│        │  ○ 가격 알림을 설정한다. (UC-07)                        │
│        │  ○ 예산을 설정한다. (UC-08)                             │
│         \ ○ 공유 바구니를 생성/공유한다. (UC-09)                 │
│                                                                 │
│          ○ 장바구니 데이터를 동기화한다. (UC-10)                  │
│시스템──┤  ○ 최저가를 비교한다. (UC-11)                           │
│          ○ 알림을 발송한다. (UC-12)                              │
│                                                                 │
│외부 쇼핑몰 ── <<연동>> ── UC-03, UC-10                           │
│외부 알림 서비스 ── <<연동>> ── UC-12                              │
└─────────────────────────────────────────────────────────────────┘
```

> 참고: 실제 산출물에서는 UML 유스 케이스 다이어그램 이미지를 삽입합니다.

---

### 2.2 기능 분류 및 설명 (유스 케이스 설명서)

---

**Use Case Name**: 회원가입을 한다.  
**ID**: UC-01 | **Importance Level**: High  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 이메일과 비밀번호를 입력하여 One-Cart 서비스에 회원가입하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 서비스 이용을 위해 계정을 생성하고자 한다.

**Trigger**: 사용자가 회원가입 버튼을 누른다.

**Relationships**  
- Association: 사용자  
- Include: -  
- Extend: -  
- Generalization: -

**Normal Flow of Events**:
1. 사용자는 이메일, 비밀번호를 입력한다.
2. 사용자는 회원가입 버튼을 누른다.
3. 시스템은 입력값을 검증하고 계정을 생성한다.
4. 시스템은 가입 완료 화면으로 이동한다.

**Alternate / Exceptional Flows**:
- 2.a1: 입력 공란이 있을 경우 시스템은 실패 사유를 화면에 출력한다.
- 2.a2: 동일한 이메일이 이미 존재할 경우 시스템은 중복 오류 메시지를 출력한다.

---

**Use Case Name**: 소셜 로그인을 한다.  
**ID**: UC-02 | **Importance Level**: High  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 Google 또는 Kakao 소셜 계정으로 간편 로그인하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 별도 회원가입 없이 빠르게 로그인하고자 한다.

**Trigger**: 사용자가 소셜 로그인 버튼(Google / Kakao)을 누른다.

**Relationships**  
- Association: 사용자, 외부 소셜 인증 서비스

**Normal Flow of Events**:
1. 사용자는 소셜 로그인 버튼을 선택한다.
2. 시스템은 해당 소셜 서비스의 OAuth 2.0 인증 페이지로 리다이렉트한다.
3. 사용자는 소셜 계정으로 인증을 완료한다.
4. 시스템은 인증 토큰을 수신하고 로그인 처리 후 메인 화면으로 이동한다.

**Alternate / Exceptional Flows**:
- 3.a1: 소셜 인증 실패 시 시스템은 오류 메시지를 표시하고 로그인 화면으로 돌아간다.

---

**Use Case Name**: 쇼핑몰을 연동한다.  
**ID**: UC-03 | **Importance Level**: High  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 쿠팡, 네이버쇼핑, 11번가 등 외부 쇼핑몰 계정을 One-Cart에 연동하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 자신의 쇼핑몰 계정을 연동하여 장바구니를 통합 관리하고자 한다.

**Trigger**: 사용자가 쇼핑몰 연동 버튼을 누른다.

**Relationships**  
- Association: 사용자, 외부 쇼핑몰

**Normal Flow of Events**:
1. 사용자는 연동할 쇼핑몰을 선택한다.
2. 시스템은 해당 쇼핑몰의 OAuth 인증 절차를 안내한다.
3. 사용자는 5단계 이내의 절차로 인증을 완료한다.
4. 시스템은 인증 토큰을 저장하고 연동 완료 메시지를 표시한다.

**Alternate / Exceptional Flows**:
- 3.a1: 연동 실패 시 시스템은 실패 사유와 함께 알림을 제공한다.

---

**Use Case Name**: 통합 장바구니를 조회한다.  
**ID**: UC-04 | **Importance Level**: High  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 연동된 모든 쇼핑몰의 장바구니 품목을 하나의 통합 리스트로 조회하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 여러 쇼핑몰의 장바구니를 한 화면에서 확인하고자 한다.

**Trigger**: 사용자가 통합 장바구니 화면으로 진입한다.

**Relationships**  
- Association: 사용자  
- Include: 장바구니 데이터를 동기화한다. (UC-10)

**Normal Flow of Events**:
1. 시스템은 연동된 쇼핑몰의 최신 장바구니 데이터를 동기화한다.
2. 사용자는 플랫폼별, 카테고리별, 가격순으로 필터링 및 정렬할 수 있다.
3. 사용자는 각 품목의 상품명, 가격, 수량, 배송비, 판매 플랫폼 정보를 확인한다.
4. 시스템은 전체 예상 결제 금액(배송비 포함)을 합산하여 표시한다.

**Alternate / Exceptional Flows**:
- 1.a1: 동기화 실패 시 시스템은 오류 알림을 표시하고 마지막 동기화 데이터를 보여준다.

---

**Use Case Name**: 품목을 수정/삭제한다.  
**ID**: UC-05 | **Importance Level**: Medium  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 통합 장바구니에서 품목의 수량을 수정하거나 품목을 삭제하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 장바구니 품목을 One-Cart에서 직접 편집하고자 한다.

**Trigger**: 사용자가 수정 또는 삭제 버튼을 누른다.

**Relationships**  
- Association: 사용자

**Normal Flow of Events**:
1. 사용자는 수정 또는 삭제할 품목을 선택한다.
2. S-1 수량 수정: 사용자는 원하는 수량을 입력하고 확인 버튼을 누른다. 시스템은 변경 사항을 반영한다.
3. S-2 품목 삭제: 사용자는 삭제 버튼을 누른다. 시스템은 해당 품목을 장바구니에서 제거한다.

**Alternate / Exceptional Flows**:
- 2.a1: 수량이 0 이하일 경우 시스템은 유효성 오류 메시지를 출력한다.

---

**Use Case Name**: 최저가 정보를 확인한다.  
**ID**: UC-06 | **Importance Level**: High  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 장바구니 내 상품의 플랫폼 간 최저가 비교 정보를 확인하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 동일 상품을 더 저렴한 플랫폼에서 구매하고자 한다.

**Trigger**: 시스템이 최저가 비교 정보를 표시하거나, 사용자가 최저가 정보를 클릭한다.

**Relationships**  
- Association: 사용자  
- Include: 최저가를 비교한다. (UC-11)

**Normal Flow of Events**:
1. 시스템은 장바구니 내 상품이 다른 연동 플랫폼에서 더 저렴하게 판매되는지 비교한다.
2. 시스템은 최저가 정보를 해당 품목 옆에 표시한다.
3. 사용자는 최저가 정보를 클릭하면 해당 플랫폼의 상품 상세 페이지로 이동한다.

**Alternate / Exceptional Flows**:
- 1.a1: 비교 가능한 외부 플랫폼 데이터가 없는 경우 최저가 정보를 표시하지 않는다.

---

**Use Case Name**: 가격 알림을 설정한다.  
**ID**: UC-07 | **Importance Level**: Medium  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 특정 상품의 목표 가격을 설정하고, 가격이 해당 목표 이하로 내려가면 알림을 받는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 원하는 가격이 되었을 때 알림을 받고자 한다.

**Trigger**: 사용자가 알림 설정 버튼을 누른다.

**Relationships**  
- Association: 사용자  
- Include: 알림을 발송한다. (UC-12)

**Normal Flow of Events**:
1. 사용자는 알림을 설정할 상품을 선택하고 목표 가격을 입력한다.
2. 시스템은 설정 정보를 저장한다.
3. 시스템은 주기적으로 가격을 모니터링하여 목표 가격 이하가 되면 앱 푸시 또는 이메일로 알림을 발송한다.

**Alternate / Exceptional Flows**:
- 1.a1: 목표 가격이 현재 가격 이상으로 입력된 경우 시스템은 유효성 오류를 출력한다.

---

**Use Case Name**: 예산을 설정한다.  
**ID**: UC-08 | **Importance Level**: Medium  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 월별 쇼핑 예산을 설정하고, 장바구니 총액이 예산에 근접하거나 초과하면 경고를 받는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자: 쇼핑 지출을 예산 범위 내에서 관리하고자 한다.

**Trigger**: 사용자가 예산 설정 화면으로 진입한다.

**Relationships**  
- Association: 사용자

**Normal Flow of Events**:
1. 사용자는 월별 쇼핑 예산 금액을 입력하고 저장한다.
2. 시스템은 통합 장바구니 총액이 예산에 근접하거나 초과할 경우 경고를 표시한다.
3. 사용자는 예산 대비 현재 장바구니 총액 비율을 시각적으로 확인할 수 있다.

**Alternate / Exceptional Flows**:
- 1.a1: 예산 금액이 0 이하일 경우 시스템은 유효성 오류를 출력한다.

---

**Use Case Name**: 공유 바구니를 생성/공유한다.  
**ID**: UC-09 | **Importance Level**: Medium  
**Primary Actor**: 사용자  
**Use Case Type**: Detail, Essential

**Brief Description**: 사용자가 여러 플랫폼의 상품을 담은 공유 바구니를 생성하고 링크를 통해 타인에게 공유하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 사용자(생성자): 상품 목록을 타인과 공유하고자 한다.  
- 사용자(수신자): 공유 링크로 상품 목록을 열람하거나 자신의 장바구니에 추가하고자 한다.

**Trigger**: 사용자가 공유 바구니 생성 버튼을 누른다.

**Relationships**  
- Association: 사용자

**Normal Flow of Events**:
1. 사용자는 공유할 상품을 선택하여 공유 바구니를 생성한다.
2. 시스템은 공유 링크를 생성한다.
3. 사용자는 링크를 복사하여 타인에게 전달한다.
4. 링크를 받은 사용자는 로그인 없이 공유 바구니를 열람할 수 있다.
5. (선택) 수신자는 해당 상품을 자신의 장바구니에 추가할 수 있다.

**Alternate / Exceptional Flows**:
- 2.a1: 공유 바구니에 상품이 없는 경우 시스템은 생성을 거부하고 안내 메시지를 출력한다.

---

**Use Case Name**: 장바구니 데이터를 동기화한다.  
**ID**: UC-10 | **Importance Level**: High  
**Primary Actor**: 시스템  
**Use Case Type**: Detail, Essential

**Brief Description**: 시스템이 연동된 쇼핑몰의 장바구니 데이터를 주기적으로 수집하고 One-Cart에 반영하는 Use Case를 표현한다.

**Stakeholders and Interests**  
- 시스템: 장바구니 데이터를 최신 상태로 유지한다.

**Trigger**: 주기적 스케줄러(최대 5분 간격) 또는 사용자의 수동 동기화 요청

**Normal Flow of Events**:
1. 시스템은 연동된 쇼핑몰 목록을 조회한다.
2. 시스템은 OAuth API 또는 크롤링을 통해 각 쇼핑몰의 장바구니 데이터를 수집한다.
3. 시스템은 수집한 데이터를 One-Cart 내부 통합 포맷으로 변환하여 저장한다.

**Alternate / Exceptional Flows**:
- 2.a1: API 또는 크롤링 실패 시 시스템은 사용자에게 실패 알림을 제공한다.

---

**Use Case Name**: 최저가를 비교한다.  
**ID**: UC-11 | **Importance Level**: High  
**Primary Actor**: 시스템  
**Use Case Type**: Detail, Essential

**Brief Description**: 시스템이 장바구니 상품에 대해 외부 가격 비교 API 또는 크롤링 결과를 활용하여 최저가를 산출하는 Use Case를 표현한다.

**Trigger**: 장바구니 데이터 동기화 완료 후 자동 실행

**Normal Flow of Events**:
1. 시스템은 통합 장바구니 내 상품 목록을 조회한다.
2. 시스템은 외부 가격 비교 API 또는 크롤링을 통해 각 상품의 플랫폼별 가격 정보를 수집한다.
3. 시스템은 최저가 플랫폼 정보를 산출하여 데이터베이스에 저장한다.

---

**Use Case Name**: 알림을 발송한다.  
**ID**: UC-12 | **Importance Level**: Medium  
**Primary Actor**: 시스템  
**Use Case Type**: Detail, Essential

**Brief Description**: 시스템이 가격 알림 조건 충족 시 사용자에게 앱 푸시 또는 이메일로 알림을 발송하는 Use Case를 표현한다.

**Trigger**: 상품 가격이 사용자가 설정한 목표 가격 이하로 하락한 경우

**Normal Flow of Events**:
1. 시스템은 가격 모니터링 결과를 확인한다.
2. 조건 충족 시 FCM(앱 푸시) 또는 SMTP(이메일)를 통해 사용자에게 알림을 발송한다.

**Alternate / Exceptional Flows**:
- 2.a1: 알림 발송 실패 시 시스템은 재시도 로직을 수행한다.

---

## 3. 요구사항 명세

### 3.1 정적 분석 (클래스 다이어그램)

> 참고: 실제 산출물에서는 UML 클래스 다이어그램 이미지를 삽입합니다.

```
┌──────────────┐        ┌──────────────────┐        ┌───────────────────┐
│     User     │        │  ShoppingMallLink│        │   CartItem        │
│──────────────│1    *  │──────────────────│1    *  │───────────────────│
│- userId      │────────│- linkId          │────────│- itemId           │
│- email       │        │- platform        │        │- productName      │
│- password    │        │- oauthToken      │        │- price            │
│- createdAt   │        │- status          │        │- quantity         │
│──────────────│        │──────────────────│        │- shippingFee      │
│+ register()  │        │+ connect()       │        │- platform         │
│+ login()     │        │+ disconnect()    │        │───────────────────│
│+ socialLogin()│       │+ sync()          │        │+ update()         │
│+ findPassword()│      └──────────────────┘        │+ delete()         │
└──────────────┘                                    └───────────────────┘
       │                                                     │
       │1                                                    │*
       ▼                                            ┌────────────────────┐
┌─────────────────┐                                │  PriceComparison   │
│  BudgetSetting  │                                │────────────────────│
│─────────────────│                                │- comparisonId      │
│- budgetId       │                                │- itemId            │
│- monthlyBudget  │                                │- targetPlatform    │
│- userId         │                                │- targetPrice       │
│─────────────────│                                │────────────────────│
│+ setBudget()    │                                │+ compare()         │
│+ checkExceed()  │                                │+ getLowestPrice()  │
└─────────────────┘                                └────────────────────┘
       │
       │1
       ▼
┌──────────────────┐        ┌──────────────────┐
│   PriceAlert     │        │  SharedBasket    │
│──────────────────│        │──────────────────│
│- alertId         │        │- basketId        │
│- userId          │        │- userId          │
│- itemId          │        │- shareLink       │
│- targetPrice     │        │- createdAt       │
│──────────────────│        │──────────────────│
│+ setAlert()      │        │+ create()        │
│+ checkTrigger()  │        │+ generateLink()  │
│+ sendNotification│        │+ view()          │
└──────────────────┘        └──────────────────┘
```

---

### 3.2 CRC 카드

---

**Class Name**: User (사용자) | **ID**: 01 | **Type**: Concrete, Domain

**Description**: One-Cart 서비스에 가입하여 서비스를 이용하는 사용자를 나타낸다.

**Associated Use Case**: UC-01, UC-02, UC-03, UC-04, UC-05, UC-06, UC-07, UC-08, UC-09

| Responsibilities | Collaborators |
|-----------------|--------------|
| + register() : void | ShoppingMallLink |
| + login() : void | BudgetSetting |
| + socialLogin() : void | PriceAlert |
| + findPassword() : void | SharedBasket |

**Attributes**
- userId : String
- email : String
- password : String (암호화 저장)
- createdAt : DateTime

**Relationships**
- Aggregation (has-parts): ShoppingMallLink, BudgetSetting, PriceAlert, SharedBasket

---

**Class Name**: ShoppingMallLink (쇼핑몰 연동) | **ID**: 02 | **Type**: Concrete, Domain

**Description**: 사용자가 연동한 외부 쇼핑몰 계정 정보 및 인증 토큰을 나타낸다.

**Associated Use Case**: UC-03, UC-10

| Responsibilities | Collaborators |
|-----------------|--------------|
| + connect() : void | User |
| + disconnect() : void | CartItem |
| + sync() : void | 외부 쇼핑몰 API |
| + handleFailure() : void | |

**Attributes**
- linkId : String
- platform : String (쿠팡/네이버쇼핑/11번가 등)
- oauthToken : String
- status : Enum (CONNECTED / FAILED / DISCONNECTED)

**Relationships**
- Generalization (a-kind-of): User
- Other Associations: CartItem

---

**Class Name**: CartItem (장바구니 품목) | **ID**: 03 | **Type**: Concrete, Domain

**Description**: 각 쇼핑몰에서 동기화된 장바구니 품목 데이터를 나타낸다.

**Associated Use Case**: UC-04, UC-05, UC-06

| Responsibilities | Collaborators |
|-----------------|--------------|
| + update() : void | User |
| + delete() : void | ShoppingMallLink |
| + getTotalPrice() : int | PriceComparison |

**Attributes**
- itemId : String
- productName : String
- price : Integer
- quantity : Integer
- shippingFee : Integer
- platform : String

**Relationships**
- Aggregation (has-parts): ShoppingMallLink
- Other Associations: PriceComparison

---

**Class Name**: PriceComparison (최저가 비교) | **ID**: 04 | **Type**: Concrete, Domain

**Description**: 장바구니 품목에 대한 플랫폼 간 최저가 비교 결과를 나타낸다.

**Associated Use Case**: UC-06, UC-11

| Responsibilities | Collaborators |
|-----------------|--------------|
| + compare() : void | CartItem |
| + getLowestPrice() : int | 외부 가격 비교 API |
| + redirectToTarget() : void | |

**Attributes**
- comparisonId : String
- itemId : String
- targetPlatform : String
- targetPrice : Integer

**Relationships**
- Other Associations: CartItem

---

**Class Name**: PriceAlert (가격 알림) | **ID**: 05 | **Type**: Concrete, Domain

**Description**: 사용자가 설정한 가격 알림 조건 및 발송 이력을 나타낸다.

**Associated Use Case**: UC-07, UC-12

| Responsibilities | Collaborators |
|-----------------|--------------|
| + setAlert() : void | User |
| + checkTrigger() : bool | CartItem |
| + sendNotification() : void | 외부 알림 서비스 (FCM/SMTP) |

**Attributes**
- alertId : String
- userId : String
- itemId : String
- targetPrice : Integer
- notifyMethod : Enum (PUSH / EMAIL / BOTH)

**Relationships**
- Generalization (a-kind-of): User
- Other Associations: 외부 알림 서비스

---

**Class Name**: BudgetSetting (예산 설정) | **ID**: 06 | **Type**: Concrete, Domain

**Description**: 사용자가 설정한 월별 쇼핑 예산 정보를 나타낸다.

**Associated Use Case**: UC-08

| Responsibilities | Collaborators |
|-----------------|--------------|
| + setBudget() : void | User |
| + checkExceed() : bool | CartItem |
| + getUsageRatio() : float | |

**Attributes**
- budgetId : String
- userId : String
- monthlyBudget : Integer
- month : String (YYYY-MM)

**Relationships**
- Generalization (a-kind-of): User

---

**Class Name**: SharedBasket (공유 바구니) | **ID**: 07 | **Type**: Concrete, Domain

**Description**: 사용자가 생성한 공유 바구니 및 공유 링크 정보를 나타낸다.

**Associated Use Case**: UC-09

| Responsibilities | Collaborators |
|-----------------|--------------|
| + create() : void | User |
| + generateLink() : String | CartItem |
| + view() : void | |
| + addToMyCart() : void | |

**Attributes**
- basketId : String
- userId : String
- shareLink : String
- createdAt : DateTime
- items : List\<CartItem\>

**Relationships**
- Generalization (a-kind-of): User
- Other Associations: CartItem

---

### 3.3 동적 분석 (순차 다이어그램)

> 참고: 실제 산출물에서는 UML 순차 다이어그램 이미지를 삽입합니다. 아래는 텍스트 기반의 흐름 표현입니다.

---

#### 3.3.1 회원가입을 한다. (UC-01)

```
사용자          :User          :UserDB
  |                |               |
  |─ 회원가입 요청()→|               |
  |                |─ 중복 확인() ──→|
  |   alt [성공]   |←── success ────|
  |←── success ───|               |
  |   [공란 有]    |               |
  |←── fail ──────|               |
  |   [중복 有]    |               |
  |←── fail ──────|               |
```

---

#### 3.3.2 소셜 로그인을 한다. (UC-02)

```
사용자          :AuthService    :소셜 OAuth     :UserDB
  |                |               |               |
  |─ 소셜 로그인 요청()→|            |               |
  |                |─ OAuth 인증 요청()→|            |
  |                |←── token ─────|               |
  |   alt [성공]   |─ 사용자 조회/생성()→|           |
  |                |←── success ───────────────────|
  |←── success ───|               |               |
  |   [실패]       |               |               |
  |←── fail ──────|               |               |
```

---

#### 3.3.3 쇼핑몰을 연동한다. (UC-03)

```
사용자          :ShoppingMallLink   :외부 쇼핑몰 API
  |                |                   |
  |─ 연동 요청() ──→|                   |
  |                |─ OAuth 인증 요청()→|
  |                |←── token ─────────|
  |   alt [성공]   |                   |
  |←── success ───|                   |
  |   [실패]       |                   |
  |←── fail + 사유|                   |
```

---

#### 3.3.4 통합 장바구니를 조회한다. (UC-04)

```
사용자     :CartItem    :ShoppingMallLink   :외부 쇼핑몰 API
  |            |               |                   |
  |─ 조회 요청()→|               |                   |
  |            |─ 동기화 요청()──→|                   |
  |            |               |─ 데이터 요청() ─────→|
  |            |               |←── 장바구니 데이터 ──|
  |            |←── 동기화 완료 |                   |
  |←── 통합 목록|               |                   |
```

---

#### 3.3.5 최저가 정보를 확인한다. (UC-06)

```
사용자     :PriceComparison    :외부 가격 비교 API
  |               |                   |
  |─ 최저가 확인()→|                   |
  |               |─ 가격 조회() ──────→|
  |               |←── 가격 데이터 ────|
  |               |─ 최저가 산출()     |
  |←── 최저가 정보|                   |
  |                                   |
  | (클릭 시 해당 플랫폼 페이지로 이동) |
```

---

#### 3.3.6 가격 알림을 설정한다. (UC-07)

```
사용자     :PriceAlert    :외부 알림 서비스(FCM/SMTP)
  |              |                  |
  |─ 알림 설정()→|                  |
  |              |─ 목표가 저장()    |
  |←── success ─|                  |
  |              |                  |
  | [주기적 가격 모니터링]            |
  |              |─ 가격 확인()      |
  |   alt [목표가 이하]              |
  |              |─ 알림 발송() ─────→|
  |              |←── success ──────|
  |←── 알림 수신 |                  |
```

---

#### 3.3.7 예산을 설정한다. (UC-08)

```
사용자     :BudgetSetting    :CartItem
  |               |               |
  |─ 예산 설정()──→|               |
  |               |─ 저장()        |
  |←── success ──|               |
  |               |               |
  | [장바구니 조회 시]               |
  |               |─ 총액 조회() ──→|
  |               |←── totalPrice ─|
  |   alt [예산 초과/근접]          |
  |←── 경고 표시 ─|               |
```

---

#### 3.3.8 공유 바구니를 생성/공유한다. (UC-09)

```
사용자(생성자)  :SharedBasket    사용자(수신자)
     |               |               |
     |─ 바구니 생성()→|               |
     |               |─ 링크 생성()  |
     |←── shareLink ─|               |
     | (링크 전달)    |               |
     |               |←── 링크 접근()─|
     |               |─── 품목 목록 ──→|
     |               | (선택) 내 장바구니에 추가()
     |               |←── success ───|
```

---

## 4. 인터페이스 분석

| 분류 | 요구사항 | 연동 방식 |
|------|---------|---------|
| IR-001 | 각 쇼핑몰과의 장바구니 데이터 연동 | OAuth 2.0 API |
| IR-002 | OAuth API 미제공 쇼핑몰의 데이터 수집 | 크롤링 (이용약관 준수 범위 내) |
| IR-003 | 각 쇼핑몰 API 응답 데이터의 내부 포맷 변환 | 내부 데이터 변환 인터페이스 |
| IR-004 | 최저가 비교를 위한 외부 가격 데이터 수집 | 외부 가격 비교 API / 크롤링 |
| IR-005 | 앱 푸시 알림 발송 | FCM (Firebase Cloud Messaging) |
| IR-006 | 이메일 알림 발송 | SMTP |

---

## 5. 제약사항

| 분류 | 제약 사항 |
|------|---------|
| CR-001 | 크롤링 사용 시 각 쇼핑몰 이용약관 및 저작권법을 검토하여 법적 문제가 없는 범위 내에서만 활용한다. |
| CR-002 | 사용자 인증 정보(계정/비밀번호)를 직접 저장하지 않고 OAuth 토큰 방식으로만 관리한다. |
| CR-003 | 시스템은 개인정보보호법 및 정보통신망법에 따른 개인정보 처리방침을 준수한다. |
| CR-004 | 모든 개인정보 및 쇼핑몰 연동 정보는 AES-256 이상의 알고리즘으로 암호화하여 저장한다. |
| CR-005 | 시스템은 HTTPS를 통해서만 데이터를 전송한다. |

---

## 6. 요구사항 추적표

| 요구사항 \ 유스케이스 | UC-01 | UC-02 | UC-03 | UC-04 | UC-05 | UC-06 | UC-07 | UC-08 | UC-09 | UC-10 | UC-11 | UC-12 |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| FR-001 (이메일 회원가입) | O | | | | | | | | | | | |
| FR-002 (소셜 로그인) | | O | | | | | | | | | | |
| FR-003 (이메일 로그인) | O | | | | | | | | | | | |
| FR-004 (연동 쇼핑몰 목록 확인) | | | O | | | | | | | | | |
| FR-005 (비밀번호 재설정) | O | | | | | | | | | | | |
| FR-006 (쇼핑몰 연동) | | | O | | | | | | | O | | |
| FR-007 (장바구니 동기화) | | | | O | | | | | | O | | |
| FR-008 (쇼핑몰 연동 해제) | | | O | | | | | | | | | |
| FR-009 (연동 실패 알림) | | | O | | | | | | | O | | |
| FR-010 (통합 장바구니 조회) | | | | O | | | | | | | | |
| FR-011 (필터링/정렬) | | | | O | | | | | | | | |
| FR-012 (품목 상세 정보 확인) | | | | O | | | | | | | | |
| FR-013 (총 예상 결제 금액 표시) | | | | O | | | | O | | | | |
| FR-014 (수량 수정) | | | | | O | | | | | | | |
| FR-015 (품목 삭제) | | | | | O | | | | | | | |
| FR-016 (최저가 비교 정보 제공) | | | | | | O | | | | | O | |
| FR-017 (가격 알림) | | | | | | | O | | | | | O |
| FR-018 (최저가 클릭 시 페이지 이동) | | | | | | O | | | | | | |
| FR-019 (예산 설정) | | | | | | | | O | | | | |
| FR-020 (예산 초과 경고) | | | | | | | | O | | | | |
| FR-021 (예산 비율 시각화) | | | | | | | | O | | | | |
| FR-022 (공유 바구니 생성) | | | | | | | | | O | | | |
| FR-023 (공유 링크 생성) | | | | | | | | | O | | | |
| FR-024 (비로그인 열람) | | | | | | | | | O | | | |
| FR-025 (상품 추가) | | | | | | | | | O | | | |

---

## 7. 참고문헌 및 부록

- [원카트] 요구사항정의서_Doc001 (v2.0, 2026.05.03)
- 샘플 요구사항 분석서 — 누구나 확인 가능한 open CCTV (참조 형식)
