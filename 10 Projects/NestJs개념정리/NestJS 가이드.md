# 📖 필독! NestJS 가이드

## 📜 목차
- [[#여정을 시작하며|여정을 시작하며]]
- [[#제2장 NestJS 핵심 개념|개념]]
- [[#제3장 코드와 함께 춤을|코드]]

---

## 여정을 시작하며
이 책의 서문 내용...

## 제2장: NestJS 핵심 개념
- 🔹 모듈(Module)
	- **정의:** **@Module()** 데코레이터로 정의하는 NestJS의 기본 구성 단위입니다. 관련 있는 컴포넌트(컨트롤러, 프로바이더, 모듈 등)를 그룹화하여 애플리케이션의 구조를 체계적으로 관리합니다. 모든 애플리케이션은 최소한 하나의 루트 모듈(AppModule)을 가집니다.

**예시:** `CatsModule`은 고양이 관련 기능(컨트롤러, 서비스 등)을 묶어 관리하는 모듈입니다.
```
// cats.module.ts
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  imports: [], // 다른 모듈을 가져와 사용
  controllers: [CatsController], // 이 모듈의 컨트롤러
  providers: [CatsService], // 이 모듈의 프로바이더
  exports: [CatsService], // 다른 모듈에서 사용 가능하도록 내보냄
})
export class CatsModule {}		
```

- 🔹 프로바이더(Provider)
	- **정의:** **@Injectable()** 데코레이터로 정의하며, NestJS의 **의존성 주입(Dependency Injection, DI)** 시스템의 핵심 요소입니다. 클래스, 값, 팩토리 등 다양한 형태로 존재하며, 주로 **Service**가 이 역할을 담당합니다.

**예시:** `CatsService`는 `CatsModule`의 프로바이더로, 컨트롤러 등 다른 컴포넌트에 주입되어 데이터 처리 로직을 담당합니다.
```
// cats.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```
- 🔹 데코레이터(Decorator)
- 🔹 서비스(Service)

## 제3장: 코드와 함께 춤을
- 🔹 소프트웨어 엔트로피
- 🔹 

## 제4장: 장 제목
제3장의 이론 및 사례...

## 결론 및 후기
책을 읽고 난 뒤의 감상...
