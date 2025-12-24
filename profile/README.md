# 🎫 Tickatch
<img width="693" height="349" alt="image" src="https://github.com/user-attachments/assets/1356b265-f1cd-48b0-ba04-6773eff51da3" />

**Ticket + Catch**

대규모 트래픽 환경에서 안정적인 예약 처리를 보장하는 티켓팅 플랫폼

---

## 📋 프로젝트 소개

Tickatch는 대규모 트래픽이 발생하는 티켓팅 환경을 가정하여, **안정적인 예약 처리를 보장**할 수 있는 시스템 구축을 목표로 한 프로젝트입니다.

도메인별 기능을 마이크로서비스로 분리하여 독립적인 배포 및 확장이 가능하도록 설계하고, 분산 환경에서 발생할 수 있는 **데이터 정합성 문제를 최소화**하기 위해 비동기 메시징 기반의 구조를 적용하였습니다.

이를 통해 높은 동시 접속 상황에서도 안정적으로 동작하는 **확장 가능하고, 신뢰성 있는 티켓팅 시스템**을 구현하였습니다.

---

## 🎯 프로젝트 목표

- 예매/티켓 발행과 같은 **기본 기능** 제공
- **대용량 트래픽 처리** 능력 확보
- 기존 티켓팅 서비스와 동일한 기능을 제공하면서도 **사용자 친화적인 기능** 제공
- MSA 기반의 **확장 가능한 아키텍처** 구현

---

## 🛠️ 기술 스택

### Backend

| 분류 | 기술 |
|------|------|
| **Framework** | Spring Boot 3.x, Spring Cloud |
| **Language** | Java 21 |
| **Database** | PostgreSQL 16 (pgvector) |
| **Messaging** | RabbitMQ, Kafka |
| **Security** | Spring Security, JWT (RS256), BCrypt |
| **OAuth** | OAuth 2.0 (Kakao, Naver, Google) |

### Frontend

| 분류 | 기술 |
|------|------|
| **Framework** | Next.js 15 (App Router) |
| **Language** | TypeScript |
| **Styling** | Tailwind CSS v4 |
| **State** | React Context API, useSyncExternalStore |
| **Authentication** | HttpOnly Cookie 기반 토큰 관리 |

### Infrastructure

| 분류 | 기술 |
|------|------|
| **Container** | Docker, Docker Compose |
| **Service Discovery** | Eureka Server (HA) |
| **Configuration** | Spring Cloud Config |
| **Proxy** | NGINX |

### Observability

| 분류 | 기술 |
|------|------|
| **Logging** | ELK Stack (Elasticsearch, Logstash, Kibana) |
| **Tracing** | Zipkin |
| **Metrics** | Prometheus, Grafana |
| **Alerting** | Alertmanager |

---

## 🏗️ 시스템 아키텍처

### Infrastructure Layer

```
┌─────────────────────────────────────────────────────────────────────┐
│                         NGINX (Reverse Proxy)                       │
└─────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────┐
│                       Gateway Server                                │
└─────────────────────────────────────────────────────────────────────┘
                                    │
         ┌──────────────────────────┼──────────────────────────┐
         ▼                          ▼                          ▼
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│  Eureka Server  │      │  Config Server  │      │   Config Repo   │
│     (HA x2)     │      │                 │      │     (Git)       │
└─────────────────┘      └─────────────────┘      └─────────────────┘
```

### Service Layer

```
┌───────────────────────────────────────────────────────────────────────────┐
│                           Business Services                               │
├─────────────┬─────────────┬─────────────┬─────────────┬───────────────────┤
│    User     │    Auth     │   Product   │   Arthall   │   Reservation     │
│   Service   │   Service   │   Service   │   Service   │     Service       │
├─────────────┼─────────────┼─────────────┼─────────────┼───────────────────┤
│ Reservation │   Ticket    │   Payment   │ Notification│  Notification     │
│    Seat     │   Service   │   Service   │   Service   │  Sender Service   │
├─────────────┴─────────────┴─────────────┴─────────────┴───────────────────┤
│                              Log Service                                  │
└───────────────────────────────────────────────────────────────────────────┘
                                    │
         ┌──────────────────────────┼──────────────────────────┐
         ▼                          ▼                          ▼
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│    RabbitMQ     │      │      Kafka      │      │   PostgreSQL    │
│   (Messaging)   │      │   (Streaming)   │      │   (pgvector)    │
└─────────────────┘      └─────────────────┘      └─────────────────┘
```

### Observability Layer

```
┌───────────────────────────────────────────────────────────────────┐
│                         Observability                             │
├──────────────┬──────────────┬──────────────┬──────────────────────┤
│  ELK Stack   │    Zipkin    │  Prometheus  │       Grafana        │
│  (Logging)   │  (Tracing)   │  (Metrics)   │    (Dashboard)       │
└──────────────┴──────────────┴──────────────┴──────────────────────┘
```

---

## 📦 리포지토리 구성

### 🔧 Infrastructure

| Repository | Description | Language |
|------------|-------------|----------|
| [infrastructure](https://github.com/your-org/infrastructure) | 프로젝트 인프라 (Docker Compose 기반) | - |
| [service-infrastructure](https://github.com/your-org/service-infrastructure) | 서비스 전용 인프라 파일 | - |
| [eureka-server](https://github.com/your-org/eureka-server) | 서비스 디스커버리 서버 | Java |
| [config-server](https://github.com/your-org/config-server) | 중앙 설정 관리 서버 | Java |
| [config-repo](https://github.com/your-org/config-repo) | 공통 설정 저장소 | - |
| [gateway-server](https://github.com/your-org/gateway-server) | API 게이트웨이 서버 | Java |
| [common-lib](https://github.com/your-org/common-lib) | 공통 라이브러리 | Java |

### 🎭 Business Services

| Repository | Description | Language |
|------------|-------------|----------|
| [user-service](https://github.com/your-org/user-service) | 사용자 서비스 | Java |
| [auth-service](https://github.com/your-org/auth-service) | 인증 서비스 | Java |
| [product-service](https://github.com/your-org/product-service) | 상품 관련 서비스 | Java |
| [arthall-service](https://github.com/your-org/arthall-service) | 아트홀 서비스 | Java |
| [reservation-service](https://github.com/your-org/reservation-service) | 예매 서비스 | Java |
| [reservation-seat-service](https://github.com/your-org/reservation-seat-service) | 예매 좌석 서비스 | Java |
| [ticket-service](https://github.com/your-org/ticket-service) | 티켓 서비스 | Java |
| [payment-service](https://github.com/your-org/payment-service) | 결제 서비스 | Java |
| [notification-service](https://github.com/your-org/notification-service) | 알림 서비스 | Java |
| [notification-sender-service](https://github.com/your-org/notification-sender-service) | 알림 발송 서비스 | Java |
| [log-service](https://github.com/your-org/log-service) | 로그 서비스 | Java |

### 🌐 Frontend & Testing

| Repository | Description | Language |
|------------|-------------|----------|
| [tickatch_web](https://github.com/your-org/tickatch_web) | 웹 프론트엔드 | TypeScript |
| [test-service](https://github.com/your-org/test-service) | 부하 테스트 서비스 | Java |
| [project-interface](https://github.com/your-org/project-interface) | 프로젝트 인터페이스 템플릿 | - |

---

## 📚 상세 문서

### Infrastructure

#### Common Library

| 항목 | 내용 |
|------|------|
| **목적** | MSA 전반에서 공통으로 사용하는 핵심 인프라 라이브러리 |
| **핵심 기능** | API 응답 표준화, 전역 예외 처리, 인증/인가, 분산 추적, 로깅, JPA Auditing, 이벤트 처리 |

**주요 기능:**

- `ApiResponse`, `PageResponse` 기반 응답 표준화
- `ErrorCode` + `BusinessException` 예외 체계
- X-User-Id 기반 인증, Role 기반 권한 제어
- HTTP / Feign / RabbitMQ / Scheduler TraceId 전파
- MDC + AOP 기반 요청/메서드 자동 로깅
- `DomainEvent` → `IntegrationEvent` 변환 및 재시도 지원
- OpenAPI 3.0 + JWT 인증 스키마

---

#### Eureka Server

| 항목 | 내용 |
|------|------|
| **목적** | 서비스 디스커버리 서버 |
| **구성** | Eureka Server 2대 이중화 (HA) |
| **역할** | 마이크로서비스 등록/조회, 인스턴스 상태 감시, 동적 위치 탐색 |

**주요 특징:**

- 상호 Peer 등록으로 단일 장애 지점(SPOF) 제거
- Context Path 분리 (`/eureka1`, `/eureka2`)
- Actuator 기반 헬스 체크 및 메트릭 제공
- Zipkin, Logstash 연동 지원

---

#### Config Server

| 항목 | 내용 |
|------|------|
| **목적** | 중앙 설정 관리 서버 |
| **구성** | Spring Cloud Config Server |
| **역할** | Git 기반 설정 관리, 환경별 설정 분리, 무중단 설정 갱신 |

**주요 특징:**

- 서비스별/환경별 설정 분리 (local, dev, prod)
- RabbitMQ 기반 Bus Refresh 지원
- 민감 정보는 환경 변수로만 주입

---

#### Config Repository

| 항목 | 내용 |
|------|------|
| **목적** | 중앙 설정 저장소 (Git) |
| **역할** | 설정 버전 관리, 변경 이력 추적 |

**구조:**

```
config-repo/
├── common/              # 모든 서비스 공통 설정
├── {service}/           # 서비스별 기본 설정
└── {service}-{profile}/ # 환경별 설정
```

**설계 원칙:**

- 민감 정보는 절대 커밋하지 않음
- 모든 시크릿은 환경 변수 주입
- 공통 → 서비스 → 환경 순 우선순위

---

#### Infrastructure Repository

| 항목 | 내용 |
|------|------|
| **목적** | 통합 인프라 리포지토리 |
| **구성** | Docker Compose 기반 |
| **역할** | 서비스 디스커버리, 설정 관리, 메시징, 로깅, 모니터링 통합 |

**인프라 구성:**

| 영역 | 구성 요소 |
|------|-----------|
| **Core** | Eureka Server (HA), Config Server, NGINX |
| **Messaging** | RabbitMQ, Kafka |
| **Observability** | ELK Stack, Zipkin, Prometheus, Grafana, Alertmanager |

**설계 특징:**

- Docker Compose 단일 명령으로 전체 인프라 기동
- 데이터 볼륨 분리로 영속성 보장
- 헬스 체크 기반 의존성 순서 관리

---

#### Service Infrastructure Repository

| 항목 | 내용 |
|------|------|
| **목적** | 비즈니스 마이크로서비스 실행 환경 |
| **구성** | PostgreSQL(pgvector) + Spring Boot 서비스 |
| **역할** | 도메인 서비스 통합 운영 |

**데이터베이스 설계:**

- 서비스별 스키마 분리 (Schema per Service)
- pgvector 기반 좌석 위치 벡터 저장
- IVFFlat 인덱스 활용 좌석 거리 연산 최적화

**리소스 최적화:**

- JVM 힙 최소화 (128–256MB)
- G1GC + 짧은 GC Pause 설정
- 단일 호스트 기준 약 4.5GB 메모리 사용

---

### Business Services

#### User Service

| 항목 | 내용 |
|------|------|
| **목적** | 사용자 관리 전용 마이크로서비스 |
| **책임** | Customer/Seller/Admin 프로필 및 상태 관리 |

**설계 핵심:**

| 특징 | 설명 |
|------|------|
| **Bounded Context 분리** | Customer / Seller / Admin 독립 Aggregate |
| **계층 구조** | Presentation → Application(CQRS) → Domain → Infrastructure |
| **엔티티 상속** | Time → Audit → BaseUser 단계적 상속 |

**이벤트 기반 연동:**

- `UserStatusChangedEvent` → Auth Service 전달 (RabbitMQ)
- `UserLogEvent` → Log Service 전달

**비즈니스 규칙:**

| 사용자 유형 | 규칙 |
|------------|------|
| **Customer** | VIP 등급 하향 불가, 진행 중 예매 시 탈퇴 불가 |
| **Seller** | 승인(APPROVED) 상태에서만 공연 등록·정산 정보 수정 가능 |
| **Admin** | 마지막 ADMIN 삭제/비활성화 불가 |

---

#### Auth Service

| 항목 | 내용 |
|------|------|
| **목적** | 인증(Authentication) 전담 마이크로서비스 |
| **책임** | 회원가입, 로그인, JWT 토큰 발급·검증, OAuth 소셜 로그인 |

**설계 핵심:**

| 특징 | 설명 |
|------|------|
| **도메인 분리** | Auth 도메인(계정) / Token 도메인(토큰) 분리 |
| **복합 유니크** | `email + userType` 복합 키로 동일 이메일 다중 가입 지원 |
| **JWT 보안** | RS256 비대칭키, JWKS 엔드포인트 제공 |

**토큰 관리 정책:**

| 토큰 유형 | 유효 기간 |
|----------|-----------|
| Access Token | 5분 |
| Refresh Token | 1시간 / 30일(유지 옵션) |

**OAuth 지원:**

- Kakao / Naver / Google
- CUSTOMER 사용자만 허용
- 계정 연동 / 해제 기능

**이벤트 처리:**

| 방향 | 이벤트 |
|------|--------|
| **수신 (User → Auth)** | 사용자 정지/활성화/탈퇴 → Auth 상태 동기화 |
| **발행 (Auth → Log)** | 회원가입, 로그인, 로그아웃, 토큰 갱신 등 |

---

#### Gateway Server

<!-- TODO: 추후 작성 예정 -->

---

#### Product Service

공연/전시 상품 도메인을 담당하는 마이크로서비스입니다. 공연 등록부터 심사, 예매 오픈, 판매 종료까지 **상품 라이프사이클 전반**을 책임집니다.

**핵심 책임:**

- 공연/전시 상품의 전체 라이프사이클 관리
- 관리자 심사 기반 판매 승인 프로세스
- 좌석 등급(VIP/R/S 등)과 가격·잔여석 요약 관리
- 스케줄 기반 자동 상태 전이
- 상품 취소·좌석 변동을 이벤트로 외부 서비스에 전파

**설계 핵심:**

| 특징 | 설명 |
|------|------|
| **Aggregate 중심** | Product가 상태·정책의 단일 주인 |
| **명시적 상태 머신** | ProductStatus 기반 유효 전이만 허용 |
| **CQRS 분리** | Command/Query 서비스 분리 |
| **이벤트 기반** | Reservation/Seat 서비스와 비동기 통신 |
| **운영 자동화** | 스케줄러로 판매 시작·종료 자동 처리 |

**상품 상태 흐름:**

```
DRAFT → PENDING → APPROVED → SCHEDULED → ON_SALE → CLOSED → COMPLETED
                                ↓ (어떤 상태에서도)
                            CANCELLED
```

**도메인 모델:**

| 모델 | 역할 |
|------|------|
| **Product** | Aggregate Root, 상품 정책과 상태의 기준 소스 |
| **SeatGrade** | 등급별 가격, 총 좌석, 잔여 좌석 관리 |

**이벤트 연동:**

| 방향 | 이벤트 |
|------|--------|
| **발행** | 상품 취소 → Reservation / ReservationSeat 서비스 전파 |
| **수신** | 좌석 예약/해제 → 잔여석 요약 갱신 |
| **발행** | 모든 주요 행위 → Log Service |

**동시성·정합성:**

- 좌석 차감 시 비관적 락 적용
- 좌석 등급 합계와 총 좌석 수 일관성 검증
- 스케줄러 전이는 `REQUIRES_NEW` 트랜잭션으로 격리

---

#### Arthall Service
아트홀·스테이지·스테이지 좌석 구조 도메인을 담당하는 마이크로서비스입니다.  
예매 및 티켓 도메인의 기준이 되는 **공간 구조와 좌석 배치 정보**를 책임집니다.

---

### 핵심 책임

- 아트홀, 스테이지, 좌석의 전체 라이프사이클 관리
- 예매·티켓 서비스가 참조하는 좌석 구조의 기준 데이터 제공
- 좌석의 논리적 위치(row/col)와 실제 공간 좌표(vector) 관리
- 상위 도메인 삭제 시 하위 도메인 연쇄 삭제 처리
- 아트홀/스테이지/좌석 상태 변경 이력 로그 발행

---

### 설계 핵심

| 특징 | 설명 |
|---|---|
| 도메인 분리 | ArtHall / Stage / StageSeat 독립 Aggregate |
| 명시적 상태 관리 | 각 도메인별 Status 기반 유효 전이만 허용 |
| 좌표 설계 | 논리 좌표(row/col)와 물리 좌표(vector) 분리 |
| Cascade 제어 | DB Cascade 미사용, 애플리케이션 레벨 연쇄 삭제 |
| CQRS 분리 | Command / Query 분리 |
| 로그 중심 | 주요 상태 변경·삭제 행위 Log Service로 전파 |

---

### 도메인 상태 흐름

**ArtHall / Stage**

ACTIVE → INACTIVE
↓
(soft delete)

**StageSeat**

ACTIVE → INACTIVE
↓
(soft delete)

---

### 도메인 모델

| 모델 | 역할 |
|---|---|
| ArtHall | Aggregate Root, 공연장 구조와 운영 상태의 기준 소스 |
| Stage | 아트홀 내 개별 스테이지 관리 |
| StageSeat | 좌석 위치·상태·배치 정보 관리 |

---

### 이벤트 연동

| 방향 | 이벤트 |
|---|---|
| 발행 | 아트홀/스테이지/좌석 상태 변경 → Log Service |
| 발행 | 연쇄 삭제(CASCADE_DELETED) → Log Service |

---

### 외부 서비스 연동

| 서비스 | 연동 목적 |
|---|---|
| Log Service | 도메인 행위 이력 기록 |

---

### 동시성·정합성

- 좌석 구조는 예매·티켓 도메인의 **단일 기준 소스(Single Source of Truth)**
- 삭제 및 상태 변경은 트랜잭션 내에서 처리
- 연쇄 삭제는 명시적 코드 흐름으로 제어하여 확장 가능성 확보

---

#### Reservation Service
티켓 예매 플랫폼의 **예매 트랜잭션 라이프사이클**을 관리하는 마이크로서비스입니다. 예매 생성부터 결제 동기화, 만료 처리까지 **예매 상태 전 과정**을 책임집니다.

**핵심 책임:**
- 예매 트랜잭션의 전체 라이프사이클 관리 (생성 → 결제 → 확정/취소/만료)
- 결제 서비스와의 상태 동기화 처리
- 좌석 선점·해제 조율 (ReservationSeat Service 연동)
- 결제 미진행 예매의 자동 만료 처리 (10분 기준)
- 예매 확정 시 알림·티켓 발행 트리거
- 상품 취소 시 관련 예매 일괄 취소

**설계 핵심:**
| 특징 | 설명 |
|------|------|
| **Aggregate 중심** | Reservation이 예매 상태·정책의 단일 주인 |
| **명시적 상태 머신** | ReservationStatus 기반 유효 전이만 허용 |
| **결제 동기화** | Payment 서비스 결과에 따른 상태 자동 전환 |
| **이벤트 기반** | Product 서비스와 비동기 통신 |
| **자동 만료** | 스케줄러로 10분 초과 예매 자동 EXPIRED 처리 |

**예매 상태 흐름:**
```
INIT → PENDING_PAYMENT → CONFIRMED
                ↓ (결제 실패)
        PAYMENT_FAILED → CANCELED
        
INIT/PENDING_PAYMENT (10분 초과) → EXPIRED
```

**도메인 모델:**
| 모델 | 역할 |
|------|------|
| **Reservation** | Aggregate Root, 예매 상태와 정책의 기준 소스 |
| **Reserver** | 예매자 정보 (ID, 이름) |
| **ProductInfo** | 상품·좌석 스냅샷 (productId, seatId, price 등) |

**이벤트 연동:**
| 방향 | 이벤트 |
|------|--------|
| **발행** | 예매 확정 → Notification Service (알림 발송) |
| **수신** | 상품 취소 → 관련 예매 일괄 취소 처리 |
| **발행** | 모든 주요 행위 → Log Service |

**외부 서비스 연동:**
| 서비스 | 연동 시점 | 목적 |
|--------|----------|------|
| **Seat** | 예매 생성 시 | 좌석 선점 요청 |
| **Seat** | 예매 취소/만료 시 | 선점 해제 요청 |
| **Payment** | 예매 확정 후 취소 시 | 환불 요청 |
| **Ticket** | 예매 확정 후 취소 시 | 티켓 취소 요청 |
| **Product/User** | 예매 생성 시 | 상품·사용자 정보 검증 |

**동시성·정합성:**
- 예매 생성 시 좌석 선점으로 동시 예약 방지
- 결제 결과 수신 시 멱등성 보장 (중복 처리 방지)
- 만료 스케줄러는 `REQUIRES_NEW` 트랜잭션으로 격리
- 예매 취소 시 좌석·티켓·결제 일관성 보장 (분산 트랜잭션 패턴)

---

#### Reservation Seat Service

<!-- TODO: 추후 작성 예정 -->

---

#### Ticket Service
예매 확정 후 **티켓 발행과 사용 처리**를 담당하는 마이크로서비스입니다. 티켓 발행부터 사용, 취소까지 **티켓 라이프사이클 전반**을 책임집니다.

**핵심 책임:**
- 티켓의 전체 라이프사이클 관리 (발행 → 사용/취소/만료)
- 예매 확정(CONFIRMED) 상태 검증 후 티켓 발행
- 예매 취소 시 티켓 자동 취소 처리
- 티켓 사용 처리 (공연 입장 시)
- 티켓 발행 시 알림 발송 트리거
- 중복 발행 방지 (기존 티켓 취소 후 재발행)

**설계 핵심:**
| 특징 | 설명 |
|------|------|
| **Aggregate 중심** | Ticket이 티켓 상태·정책의 단일 주인 |
| **명시적 상태 머신** | TicketStatus 기반 유효 전이만 허용 |
| **예매 동기화** | Reservation 서비스 상태와 연동 |
| **이벤트 기반** | Notification 서비스와 비동기 통신 |
| **멱등성 보장** | 동일 예매 ID 중복 발행 방지 |

**티켓 상태 흐름:**
```
ISSUED → USED (공연 입장)
  ↓
CANCELED (예매 취소)
  ↓
EXPIRED (공연 종료)
```

**도메인 모델:**
| 모델 | 역할 |
|------|------|
| **Ticket** | Aggregate Root, 티켓 상태와 정책의 기준 소스 |
| **SeatInfo** | 좌석 정보 (seatId, grade, seatNumber, price) |
| **ReceiveMethod** | 수령 방식 (ON_SITE, EMAIL, MMS) |

**이벤트 연동:**
| 방향 | 이벤트 |
|------|--------|
| **발행** | 티켓 발행 → Notification Service (알림 발송) |
| **수신** | 상품 취소 → 관련 티켓 일괄 취소 처리 |
| **발행** | 모든 주요 행위 → Log Service |

**외부 서비스 연동:**
| 서비스 | 연동 시점 | 목적 |
|--------|----------|------|
| **Reservation** | 티켓 발행 시 | 예매 CONFIRMED 상태 검증 |
| **Product** | 티켓 발행 시 | 상품 정보 조회 |
| **User** | 티켓 발행 시 | 사용자 정보 조회 |

**동시성·정합성:**
- 티켓 발행 시 예매 상태(CONFIRMED) 필수 검증
- 동일 예매 ID 중복 발행 방지 (기존 티켓 자동 취소)
- 티켓 사용 처리 멱등성 보장 (중복 사용 방지)
- 예매 취소 시 티켓 상태 일관성 보장


---

#### Payment Service

<!-- TODO: 추후 작성 예정 -->

---

#### Notification Service

<!-- TODO: 추후 작성 예정 -->

---

#### Notification Sender Service

<!-- TODO: 추후 작성 예정 -->

---

#### Log Service
Tickatch 전 서비스의 **비즈니스 행위 이력(Audit Log)** 을 중앙에서 수집·저장하는 마이크로서비스입니다.  
생성/수정/삭제/상태변경 등 핵심 도메인 행위를 정형 데이터로 기록해 **운영·클레임 대응 근거**를 제공합니다.

---

### 핵심 책임

- 서비스별 주요 행위(생성/변경/삭제/상태변경) 로그 수집·저장
- 사용자/리소스/행위 단위 이력 조회 근거 데이터 제공
- 로그 저장 실패가 비즈니스 트랜잭션에 영향 없도록 비동기 수집

---

### 설계 핵심

| 특징 | 설명 |
|---|---|
| 3축 분리 | 운영 로그(ELK) / 비즈니스 행위 로그(DB) / 지표(Prometheus) |
| 행위 중심 | Read/디버깅 로그 제외, 핵심 도메인 행위만 저장 |
| 이벤트 기반 | RabbitMQ로 비동기 수집 → Log Service Consumer |
| 테이블 분리 | 서비스별 로그 테이블 + 공통 컬럼 규약 |
| Append Only | 로그 수정/삭제 금지, Audit 성격 유지 |

---

### 로그 수집 흐름

Business Service → Domain Action 발생 → RabbitMQ 발행 → Log Service 수신 → PostgreSQL 저장

---

### Frontend

#### Tickatch Web

티케팅 서비스 웹 프론트엔드 애플리케이션입니다.

**기술 스택:**

| 분류 | 기술 |
|------|------|
| Framework | Next.js 15 (App Router) |
| Language | TypeScript |
| Styling | Tailwind CSS v4 |
| State Management | React Context API, useSyncExternalStore |
| Authentication | HttpOnly Cookie 기반 토큰 관리 |

**사용자 유형:**

- Customer (고객) - 티켓 예매
- Seller (판매자) - 상품 등록/관리
- Admin (관리자) - 심사/운영

**주요 기능:**

- 사용자 타입별 독립 로그인/대시보드
- OAuth 팝업 방식 소셜 로그인 (카카오, 네이버, 구글)
- 반응형 UI + 다크모드 지원
- 대기열 시스템 연동

**API Routes 구성:**

| 영역 | 엔드포인트 수 |
|------|:------------:|
| Auth | 14개 |
| Products | 14개 |
| ArtHall/Stage/Seat | 11개 |
| Reservation | 5개 |
| ReservationSeat | 5개 |
| Queue | 2개 |

---

### Testing

#### Test Service

Gateway 및 마이크로서비스들의 **부하 내성을 검증**하기 위한 테스트 전용 서비스입니다.

**테스트 목적:**

| 목적 | 설명 |
|------|------|
| **Gateway 부하 테스트** | 동시 요청 처리 한계 검증 |
| **스레드 점유 테스트** | P99 레이턴시 기준 시스템 한계 측정 |
| **리소스 제한 테스트** | CPU/메모리 제한 환경에서 안정성 검증 |
| **Timeout 테스트** | Gateway/Client Timeout 동작 확인 |

**테스트 API:**

```bash
# 스레드 점유 시뮬레이션 (sleepMs 만큼 대기 후 응답)
GET  /api/v1/test/load?sleepMs=1000
POST /api/v1/test/load?sleepMs=1000
```

**JMeter 점진적 부하 테스트 결과:**

| 요청 수 | 결과 | 비고 |
|---------|:----:|------|
| 1,000 ~ 6,000 | ✅ | 모든 요청 성공 |
| 7,000 | ⚠️ | 스레드 고갈 징후 시작 |
| 8,000 | ⚠️ | 간헐적 에러 발생 |
| 9,000 ~ 10,000 | ❌ | OOM 에러 다수 발생 |

**결론:** 약 **7,000 요청** 수준부터 스레드 고갈 징후 발생 → **대기열 시스템 도입 필요성 확인**

**장애 분석:**

| 구분 | 에러 | 해결 방안 |
|------|------|----------|
| Test Service | `OutOfMemoryError` | 대기열 도입 |
| Gateway | `PrematureCloseException` | 대기열 도입 |

---

## 🚀 시작하기

### Prerequisites

- Docker & Docker Compose
- JDK 21+
- Node.js 18+ (Frontend)

### Quick Start

```bash
# 1. 인프라 실행
cd infrastructure
docker-compose up -d

# 2. 서비스 인프라 실행
cd service-infrastructure
docker-compose up -d

# 3. 개별 서비스 실행 (개발 환경)
cd {service-name}
./gradlew bootRun

# 4. 프론트엔드 실행
cd tickatch_web
npm install
npm run dev
```

---

© 2025 Tickatch Team
