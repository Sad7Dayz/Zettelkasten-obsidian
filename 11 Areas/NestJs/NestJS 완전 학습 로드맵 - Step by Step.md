[NestJs FreeCodeCamp](https://youtu.be/sFnAHC9lLaw?si=OCXQ79SnMUMVEeRx)
## 🚀 Module 0: NestJS 기초 설정

**목표**: NestJS 기본 개념 이해 및 프로젝트 초기 설정

### Step 1: NestJS 개념 이해

- NestJS란 무엇인가?
- Express.js와의 차이점
- TypeScript 기반 Node.js 프레임워크의 장점

### Step 2: 프로젝트 생성

- NestJS CLI 설치
- 새 프로젝트 생성
- 개발 서버 실행

### Step 3: 디렉토리 구조 분석

- src 폴더 구조
- 각 파일의 역할 이해
- NestJS 아키텍처 패턴

---

## 🎯 Module 1: 핵심 구성 요소

**목표**: Controller, Service, Module의 기본 개념과 생성 방법 학습

### Step 1: Controller 생성 및 이해

- `@Controller()` 데코레이터 사용법
- HTTP 메서드 핸들러 (`@Get()`, `@Post()`, etc.)
- 라우트 매개변수 처리

### Step 2: Service 생성 및 의존성 주입

- `@Injectable()` 데코레이터
- 비즈니스 로직 분리
- 의존성 주입 기본 개념

### Step 3: Module 생성 및 구성

- `@Module()` 데코레이터
- providers, controllers, imports 설정
- 모듈 간 의존성 관리

---

## 🛠️ Module 2: 요청 처리 및 검증

**목표**: 미들웨어, 예외 처리, 데이터 변환 및 검증 구현

### Step 1: 미들웨어 구현

- 커스텀 미들웨어 생성
- 미들웨어 적용 방법
- 전역 미들웨어 설정

### Step 2: 예외 필터 생성

- `@Catch()` 데코레이터 사용
- 커스텀 예외 클래스
- 전역 예외 필터 적용

### Step 3: 파라미터 변환 (ParseIntPipe)

- 내장 파이프 사용법
- 데이터 타입 변환
- 파이프 체이닝

### Step 4: 요청 본문 검증 (class-validator)

- DTO 클래스 생성
- 검증 데코레이터 사용
- ValidationPipe 적용

---

## ⚙️ Module 3: 고급 의존성 주입

**목표**: 커스텀 프로바이더, 주입 스코프, 데이터베이스 관계 설정

### Step 1: 커스텀 프로바이더 생성

- useValue, useClass, useFactory 패턴
- 토큰 기반 주입
- 동적 프로바이더 생성

### Step 2: 주입 스코프 이해

- Singleton, Request, Transient 스코프
- 스코프별 생명주기
- 성능 고려사항

### Step 3: One To Many 관계 설정

- Entity 관계 정의
- 외래키 설정
- 관계 데이터 조회

---

## 💾 Module 4: 데이터베이스 기본 작업

**목표**: TypeORM을 활용한 데이터베이스 연결 및 기본 CRUD 작업

### Step 1: 데이터베이스 연결 설정

- TypeORM 설정
- 데이터베이스 연결 구성
- 환경별 설정 관리

### Step 2: Entity 생성

- `@Entity()` 데코레이터 사용
- 컬럼 정의 및 타입 설정
- 기본키 및 인덱스 설정

### Step 3: CRUD 작업 구현

- Repository 패턴 사용
- Create, Read, Update, Delete 구현
- 트랜잭션 처리

### Step 4: 페이지네이션 구현

- `take`와 `skip` 사용
- 페이지네이션 DTO 생성
- 메타데이터 반환

---

## 🔗 Module 5: 고급 데이터베이스 관계

**목표**: 복잡한 데이터베이스 관계 설정 및 관리

### Step 1: One to One 관계

- `@OneToOne()` 데코레이터
- `@JoinColumn()` 사용법
- 양방향 관계 설정

### Step 2: Many to Many 관계

- `@ManyToMany()` 데코레이터
- `@JoinTable()` 설정
- 중간 테이블 관리

---

## 🔐 Module 6: 인증 및 권한 관리

**목표**: 포괄적인 인증 시스템 구축

### Step 1: 사용자 회원가입 구현

- 패스워드 해싱 (bcrypt)
- 사용자 데이터 저장
- 입력 검증

### Step 2: 사용자 로그인 구현

- 로그인 로직 구현
- 세션 관리
- 에러 처리

### Step 3: Passport JWT 인증

- JWT 토큰 생성 및 검증
- Passport 전략 설정
- 인증 가드 구현

### Step 4: 역할 기반 인증 (RBAC)

- 역할 및 권한 정의
- 가드를 통한 접근 제어
- 데코레이터 기반 권한 확인

### Step 5: 2단계 인증 (2FA)

- TOTP 구현
- QR 코드 생성
- 인증 코드 검증

### Step 6: API 키 인증

- API 키 생성 및 관리
- 키 기반 인증 전략
- 레이트 리미팅

---

## 🐛 Module 7: 개발 도구 및 데이터베이스 관리

**목표**: 디버깅, 마이그레이션, 시딩 도구 활용

### Step 1: NestJS 애플리케이션 디버그

- VS Code 디버그 설정
- 브레이크포인트 활용
- 로깅 전략

### Step 2: 마이그레이션 관리

- 마이그레이션 파일 생성
- 스키마 변경 관리
- 롤백 전략

### Step 3: 데이터 시딩

- 시드 파일 작성
- 초기 데이터 설정
- 개발/테스트 환경 데이터

---

## ⚙️ Module 8: 설정 및 환경 관리

**목표**: 애플리케이션 설정 및 환경 변수 관리

### Step 1: 커스텀 설정

- ConfigModule 사용
- 설정 파일 구성
- 네임스페이스 설정

### Step 2: 환경 변수 검증

- Joi를 사용한 스키마 검증
- 환경별 설정 검증
- 타입 안전성 보장

### Step 3: 핫 모듈 리로딩

- Webpack HMR 설정
- 개발 효율성 향상
- 자동 리로드 구성

---

## 📚 Module 9: API 문서화

**목표**: Swagger를 활용한 API 문서 자동화

### Step 1: Swagger 기본 설정

- SwaggerModule 설정
- API 정보 구성
- UI 커스터마이징

### Step 2: 회원가입 라우트 문서화

- `@ApiOperation()` 사용
- 요청/응답 예시
- 상태 코드 문서화

### Step 3: ApiProperty를 활용한 스키마 생성

- DTO 스키마 정의
- 예시 값 설정
- 검증 규칙 문서화

### Step 4: JWT 인증 테스트

- Bearer 토큰 설정
- 인증이 필요한 엔드포인트 테스트
- 보안 스키마 구성

---

## 🗄️ Module 10: MongoDB 통합

**목표**: MongoDB를 활용한 NoSQL 데이터베이스 작업

### Step 1: Docker Compose로 MongoDB 설치

- docker-compose.yml 구성
- MongoDB 컨테이너 실행
- 개발 환경 설정

### Step 2: MongoDB 연결

- Mongoose 설정
- 연결 문자열 구성
- 연결 상태 모니터링

### Step 3: MongoDB 스키마 생성

- Mongoose 스키마 정의
- 타입 설정 및 검증
- 인덱스 구성

### Step 4: MongoDB 레코드 저장

- Document 생성
- 저장 로직 구현
- 에러 처리

### Step 5: 조회 및 삭제 작업

- 쿼리 메서드 사용
- 필터링 및 정렬
- 삭제 작업 구현

### Step 6: Populate 사용

- 참조 데이터 조회
- 중첩된 populate
- 성능 최적화

---

## 🚀 Module 11: 배포 및 환경 관리

**목표**: 실제 운영 환경으로 애플리케이션 배포

### Step 1: 개발/운영 환경 구성

- 환경별 설정 파일
- 환경 변수 관리
- 빌드 최적화

### Step 2: GitHub 저장소 관리

- Git 워크플로우
- 브랜치 전략
- CI/CD 파이프라인

### Step 3: Railway 배포

- Railway 플랫폼 설정
- 배포 설정 구성
- 도메인 연결

### Step 4: TypeORM 마이그레이션을 위한 Dotenv

- 환경 변수 로딩
- 마이그레이션 스크립트 설정
- 배포 환경 설정

### Step 5: 환경 버그 수정

- 일반적인 배포 이슈
- 로그 분석
- 트러블슈팅

---

## 🧪 Module 12: 테스트 전략

**목표**: 포괄적인 테스트 전략 수립 및 구현

### Step 1: Jest 시작하기

- Jest 설정
- 테스트 환경 구성
- 테스트 스크립트 작성

### Step 2: Auto Mocking

- 자동 모킹 설정
- 의존성 모킹
- 모킹 전략

### Step 3: SpyOn 함수 활용

- 함수 호출 감시
- 반환값 조작
- 호출 횟수 검증

### Step 4: Controller 단위 테스트

- 컨트롤러 테스트 작성
- 모킹된 서비스 사용
- HTTP 응답 검증

### Step 5: Service 단위 테스트

- 비즈니스 로직 테스트
- 의존성 모킹
- 예외 상황 테스트

### Step 6: E2E 테스트

- 전체 애플리케이션 테스트
- 실제 데이터베이스 사용
- API 엔드투엔드 테스트

---

## ⚡ Module 13: 실시간 통신 및 성능 최적화

**목표**: WebSocket과 최신 컴파일러 활용

### Step 1: NestJS v10 고속 웹 컴파일러

- SWC 컴파일러 사용
- 빌드 성능 향상
- 개발 경험 개선

### Step 2: WebSocket 서버 생성

- WebSocket 게이트웨이 구현
- 실시간 통신 설정
- 연결 관리

### Step 3: 프론트엔드와 메시지 통신

- 클라이언트 연결
- 메시지 송수신
- 이벤트 처리

---

## 🎯 Module 14: GraphQL 서버 구축

**목표**: GraphQL API 서버 구축 및 관리

### Step 1: GraphQL 서버 설정

- Apollo Server 통합
- 스키마 설정
- Playground 구성

### Step 2: 쿼리 및 뮤테이션 정의

- SDL 스키마 정의
- 타입 정의
- 입력 타입 설정

### Step 3: 쿼리 리졸버 구현

- 리졸버 함수 작성
- 데이터 페칭
- 필드 리졸버

### Step 4: 뮤테이션 리졸버 구현

- 데이터 변경 작업
- 입력 검증
- 응답 처리

### Step 5: 에러 처리

- GraphQL 에러 처리
- 커스텀 에러 타입
- 에러 포맷팅

---

## 🔐 Module 15: GraphQL 인증

**목표**: GraphQL에서의 인증 및 권한 관리

### Step 1: 인증 스키마 정의

- Auth 타입 정의
- 로그인/회원가입 뮤테이션
- JWT 토큰 타입

### Step 2: 인증 쿼리 및 뮤테이션 리졸버

- 인증 로직 구현
- 토큰 생성
- 사용자 검증

### Step 3: Auth Guard를 사용한 인증 적용

- GraphQL 가드 구현
- 보호된 리졸버
- 컨텍스트 기반 인증

---

## 📡 Module 16: 실시간 구독

**목표**: GraphQL Subscription을 통한 실시간 데이터 스트리밍

### Step 1: 실시간 구독 구현

- Subscription 타입 정의
- PubSub 시스템 구현
- 실시간 이벤트 발행

---

## 🧪 Module 17: GraphQL 테스트

**목표**: GraphQL API의 단위 테스트 및 통합 테스트

### Step 1: 리졸버 단위 테스트

- 리졸버 함수 테스트
- 모킹 전략
- 테스트 데이터 관리

### Step 2: GraphQL E2E 테스트

- 전체 GraphQL API 테스트
- 쿼리/뮤테이션 테스트
- 인증 테스트

---

## 🚀 Module 18: GraphQL 성능 최적화

**목표**: GraphQL 성능 최적화 및 외부 API 연동

### Step 1: Apollo 서버사이드 캐싱

- 쿼리 결과 캐싱
- Redis 통합
- 캐시 무효화

### Step 2: DataLoader를 사용한 쿼리 최적화

- N+1 문제 해결
- 배치 로딩
- 캐싱 전략

### Step 3: 외부 REST API 데이터 페칭

- 외부 API 통합
- 데이터 변환
- 에러 핸들링

---

## 🗄️ Module 19: Prisma ORM 통합

**목표**: 현대적인 ORM인 Prisma 활용

### Step 1: Prisma 설정

- Prisma 초기화
- 데이터베이스 연결
- Prisma Client 생성

### Step 2: 모델 및 마이그레이션

- Prisma 스키마 정의
- 마이그레이션 생성
- 데이터베이스 동기화

### Step 3: Prisma Client 생성

- 클라이언트 생성
- 타입 안전성
- 자동 완성

### Step 4: 기본 CRUD 작업

- Create, Find, FindOne 구현
- Prisma 쿼리 API 사용
- 타입 안전한 쿼리

### Step 5: 업데이트 및 삭제 작업

- Update, Delete 구현
- 조건부 업데이트
- 소프트 삭제

### Step 6: 관계 설정

- One to Many 관계
- One to One 관계
- Many to Many 관계

### Step 7: 벌크 작업

- 배치 생성/업데이트
- 성능 최적화
- 트랜잭션 활용

### Step 8: 중첩 쿼리 트랜잭션

- 중첩된 데이터 작업
- 트랜잭션 보장
- 롤백 처리

### Step 9: 인터랙티브 트랜잭션

- 복잡한 비즈니스 로직
- 조건부 트랜잭션
- 에러 처리

---

## 🔧 Module 20: 고급 기능 및 유틸리티

**목표**: 실무에서 필요한 고급 기능들 구현

### Step 1: 파일 업로드

- Multer 미들웨어 사용
- 파일 검증
- 클라우드 저장소 연동

### Step 2: 커스텀 데코레이터

- 메타데이터 기반 데코레이터
- 파라미터 데코레이터
- 재사용 가능한 로직

### Step 3: CRON 작업 스케줄링

- 스케줄 작업 설정
- 백그라운드 작업
- 작업 모니터링

### Step 4: 쿠키 관리

- 쿠키 설정/읽기
- 보안 쿠키
- 세션 관리

### Step 5: 큐 시스템

- Bull 큐 사용
- 작업 큐 관리
- 실패 처리

### Step 6: 이벤트 에미터

- 이벤트 기반 아키텍처
- 비동기 이벤트 처리
- 이벤트 리스너

### Step 7: 스트리밍

- 파일 스트리밍
- 실시간 데이터 스트림
- 메모리 효율성

### Step 8: 세션 관리

- 세션 저장소 설정
- 세션 보안
- 클러스터 환경 세션

---

## 🎯 학습 순서 권장사항

### 초급자 (1-2개월)

Module 0 → Module 1 → Module 2 → Module 4 → Module 9

### 중급자 (2-3개월)

Module 3 → Module 5 → Module 6 → Module 7 → Module 8 → Module 11 → Module 12

### 고급자 (3-4개월)

Module 10 → Module 13 → Module 14 → Module 15 → Module 16 → Module 17 → Module 18 → Module 19 → Module 20

### 💡 학습 팁

- 각 모듈을 순서대로 진행하되, 실제 프로젝트를 만들어보며 학습
- 코드를 직접 작성하고 테스트해보는 것이 중요
- 공식 문서와 함께 학습하여 최신 정보 확인
- 각 단계별로 간단한 프로젝트를 만들어 포트폴리오 구성