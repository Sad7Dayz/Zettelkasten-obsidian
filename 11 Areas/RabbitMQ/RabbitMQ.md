![[Pasted image 20251007231243.png]]

# RabbitMQ Exchange 타입 완벽 정리

## Exchange란?

Exchange는 Producer로부터 메시지를 받아 어떤 Queue로 메시지를 라우팅할지 결정하는 **메시지 라우터**입니다. Exchange는 Routing Key와 Binding 규칙을 기반으로 메시지를 적절한 큐로 전달합니다.

---

## 1. Direct Exchange

### 개념

**정확한 Routing Key 매칭**을 통해 메시지를 라우팅합니다. Routing Key가 완전히 일치하는 큐로만 메시지를 전달합니다.

### 동작 방식

- Producer가 메시지와 함께 Routing Key를 지정
- Exchange가 해당 Routing Key와 정확히 일치하는 Binding Key를 가진 큐로 전달
- 1:1 매칭 방식

### 사용 사례

```
Routing Key: "error"     → error_queue
Routing Key: "info"      → info_queue
Routing Key: "warning"   → warning_queue
```

### 예시 코드 (Node.js)

```javascript
// Producer
channel.publish(
  'direct_logs',           // exchange name
  'error',                 // routing key
  Buffer.from('Error occurred!')
);

// Consumer
channel.bindQueue(queue, 'direct_logs', 'error');
```

### 장점

- 간단하고 명확한 라우팅
- 로그 레벨별 처리에 적합
- 빠른 성능

---

## 2. Fanout Exchange

### 개념

**브로드캐스트 방식**으로 메시지를 전달합니다. Routing Key를 무시하고 연결된 모든 큐에 메시지를 복사하여 전송합니다.

### 동작 방식

- Routing Key는 무시됨
- Exchange에 바인딩된 모든 큐에 메시지 전송
- 1:N 브로드캐스팅

### 사용 사례

```
하나의 이벤트 → 모든 구독자에게 전달
- 실시간 채팅방 (모든 참가자에게)
- 주식 시세 업데이트 (모든 구독자에게)
- 알림 서비스 (다중 채널 동시 발송)
```

### 예시 코드 (Node.js)

```javascript
// Producer
channel.publish(
  'fanout_exchange',       // exchange name
  '',                      // routing key (무시됨)
  Buffer.from('Broadcast message!')
);

// Consumer 1
channel.bindQueue(emailQueue, 'fanout_exchange', '');

// Consumer 2
channel.bindQueue(smsQueue, 'fanout_exchange', '');

// Consumer 3
channel.bindQueue(pushQueue, 'fanout_exchange', '');
```

### 장점

- 가장 빠른 성능 (라우팅 로직 없음)
- 동일 메시지를 여러 서비스에 전달
- 구현이 간단함

---

## 3. Topic Exchange

### 개념

**패턴 매칭**을 통해 메시지를 라우팅합니다. Routing Key를 점(.)으로 구분된 단어들로 구성하고, 와일드카드를 사용하여 유연한 라우팅이 가능합니다.

### 동작 방식

- Routing Key: 점(.)으로 구분된 단어들 (예: `order.created.payment`)
- 와일드카드 사용:
    - `*` : 정확히 하나의 단어 매칭
    - `#` : 0개 이상의 단어 매칭

### 사용 사례

```
Routing Key: "order.created"
- Pattern: "order.*"        → 매칭 O
- Pattern: "order.#"        → 매칭 O
- Pattern: "*.created"      → 매칭 O
- Pattern: "#.created"      → 매칭 O
- Pattern: "payment.*"      → 매칭 X

Routing Key: "order.korea.seoul.created"
- Pattern: "order.#"        → 매칭 O
- Pattern: "order.*.*.created" → 매칭 O
- Pattern: "order.*"        → 매칭 X
```

### 예시 코드 (Node.js)

```javascript
// Producer
channel.publish(
  'topic_exchange',
  'order.created.payment',  // routing key
  Buffer.from('Order created!')
);

// Consumer 1 - 모든 주문 이벤트
channel.bindQueue(allOrderQueue, 'topic_exchange', 'order.#');

// Consumer 2 - 생성 이벤트만
channel.bindQueue(createdQueue, 'topic_exchange', '*.created.*');

// Consumer 3 - 주문 생성만
channel.bindQueue(orderCreatedQueue, 'topic_exchange', 'order.created.*');
```

### 장점

- 매우 유연한 라우팅
- 복잡한 이벤트 구조에 적합
- 마이크로서비스 아키텍처에 최적

---

## 4. Headers Exchange

### 개념

**메시지 헤더**를 기반으로 라우팅합니다. Routing Key 대신 메시지 헤더의 속성(key-value)을 사용하여 매칭합니다.

### 동작 방식

- 메시지 헤더의 속성들을 검사
- 바인딩 시 지정한 헤더 조건과 비교
- `x-match` 옵션:
    - `all`: 모든 헤더가 일치해야 함 (AND 조건)
    - `any`: 하나 이상의 헤더가 일치하면 됨 (OR 조건)

### 사용 사례

```
메시지 헤더: {format: 'pdf', type: 'report', priority: 'high'}

Binding 1: {format: 'pdf', x-match: 'any'}        → 매칭 O
Binding 2: {type: 'report', priority: 'high', x-match: 'all'} → 매칭 O
Binding 3: {format: 'excel', x-match: 'any'}      → 매칭 X
```

### 예시 코드 (Node.js)

```javascript
// Producer
channel.publish(
  'headers_exchange',
  '',                      // routing key (무시됨)
  Buffer.from('Message'),
  {
    headers: {
      format: 'pdf',
      type: 'report',
      priority: 'high'
    }
  }
);

// Consumer 1 - 모든 조건 일치 (AND)
channel.bindQueue(queue1, 'headers_exchange', '', {
  'x-match': 'all',
  'format': 'pdf',
  'type': 'report'
});

// Consumer 2 - 하나 이상 일치 (OR)
channel.bindQueue(queue2, 'headers_exchange', '', {
  'x-match': 'any',
  'priority': 'high',
  'urgent': true
});
```

### 장점

- 복잡한 조건부 라우팅 가능
- Routing Key보다 더 많은 메타데이터 활용
- 다중 속성 기반 필터링

### 단점

- 성능이 상대적으로 느림
- 복잡도가 높음

---

## Exchange 타입 비교표

|Exchange 타입|Routing Key 사용|매칭 방식|성능|복잡도|주요 용도|
|---|---|---|---|---|---|
|**Direct**|O (정확히 일치)|1:1 완전 매칭|빠름|낮음|로그 레벨별 처리|
|**Fanout**|X (무시)|브로드캐스트|가장 빠름|가장 낮음|전체 알림, 채팅|
|**Topic**|O (패턴 매칭)|와일드카드|보통|중간|이벤트 기반 시스템|
|**Headers**|X (헤더 사용)|헤더 속성 매칭|느림|높음|복잡한 필터링|

---

## 선택 가이드

### Direct Exchange를 사용할 때

- 명확한 1:1 라우팅이 필요할 때
- 로그 레벨별 처리 (error, info, warning)
- 작업 큐 (특정 작업자에게 할당)

### Fanout Exchange를 사용할 때

- 모든 구독자에게 동일 메시지 전달
- 실시간 브로드캐스트 (채팅, 알림)
- 캐시 무효화 (모든 서버에 알림)

### Topic Exchange를 사용할 때

- 유연한 라우팅이 필요할 때
- 이벤트 기반 마이크로서비스
- 계층적 카테고리 시스템 (지역.도시.구 등)

### Headers Exchange를 사용할 때

- 복잡한 조건부 라우팅 필요
- 다중 속성 기반 필터링
- Routing Key로 표현하기 어려운 복잡한 로직

---

## 실전 예시: E-commerce 주문 시스템

```javascript
// Topic Exchange 사용 예시
const routingKeys = {
  orderCreated: 'order.created',
  orderPaid: 'order.paid',
  orderShipped: 'order.shipped',
  orderDelivered: 'order.delivered',
  orderCancelled: 'order.cancelled'
};

// 서비스별 구독 패턴
// 1. 알림 서비스: 모든 주문 이벤트
channel.bindQueue(notificationQueue, 'order_exchange', 'order.#');

// 2. 재고 서비스: 생성, 취소 이벤트만
channel.bindQueue(inventoryQueue, 'order_exchange', 'order.created');
channel.bindQueue(inventoryQueue, 'order_exchange', 'order.cancelled');

// 3. 배송 서비스: 결제, 배송 관련 이벤트만
channel.bindQueue(shippingQueue, 'order_exchange', 'order.paid');
channel.bindQueue(shippingQueue, 'order_exchange', 'order.shipped');

// 4. 분석 서비스: 모든 이벤트 수집
channel.bindQueue(analyticsQueue, 'order_exchange', '#');
```

이렇게 Topic Exchange를 사용하면 각 서비스가 필요한 이벤트만 선택적으로 구독할 수 있어 효율적인 마이크로서비스 아키텍처를 구성할 수 있습니다.


---

## 5. Priority Queue

### 개념

Priority Queue(우선순위 큐)는 메시지에 **우선순위**를 부여하여, 높은 우선순위를 가진 메시지가 먼저 처리되도록 하는 큐입니다. 일반적인 FIFO(First In First Out) 방식이 아닌, **우선순위 기반**으로 메시지를 처리합니다.

---

## 동작 원리

### 일반 큐 vs Priority 큐

**일반 큐 (FIFO):**

```
메시지 순서: A → B → C → D
처리 순서:   A → B → C → D
```

**Priority 큐:**

```
메시지 순서: A(우선순위 5) → B(우선순위 10) → C(우선순위 3) → D(우선순위 10)
처리 순서:   B(10) → D(10) → A(5) → C(3)
```

### 핵심 개념

- 우선순위는 **0 ~ 255** 범위의 정수값 (숫자가 클수록 높은 우선순위)
- 같은 우선순위를 가진 메시지는 FIFO 순서로 처리
- 큐 생성 시 최대 우선순위 값을 지정해야 함

---

## Priority Queue 설정

### 1. 큐 선언 시 우선순위 설정

javascript

```javascript
// Node.js (amqplib)
const channel = await connection.createChannel();

// Priority Queue 생성 (최대 우선순위: 10)
await channel.assertQueue('priority_queue', {
  durable: true,
  maxPriority: 10  // 0~10 범위의 우선순위 사용 가능
});
```

### 2. 메시지 발행 시 우선순위 지정

javascript

```javascript
// 높은 우선순위 메시지 (긴급)
channel.sendToQueue(
  'priority_queue',
  Buffer.from('긴급 주문 처리 요청'),
  {
    persistent: true,
    priority: 10  // 최고 우선순위
  }
);

// 중간 우선순위 메시지 (일반)
channel.sendToQueue(
  'priority_queue',
  Buffer.from('일반 주문 처리 요청'),
  {
    persistent: true,
    priority: 5  // 중간 우선순위
  }
);

// 낮은 우선순위 메시지 (배치)
channel.sendToQueue(
  'priority_queue',
  Buffer.from('배치 작업 요청'),
  {
    persistent: true,
    priority: 1  // 낮은 우선순위
  }
);
```

### 3. Consumer 설정

javascript

```javascript
// Consumer는 일반 큐와 동일하게 설정
channel.consume('priority_queue', (msg) => {
  if (msg !== null) {
    console.log('받은 메시지:', msg.content.toString());
    console.log('우선순위:', msg.properties.priority);
    channel.ack(msg);
  }
}, {
  noAck: false
});
```

---

## 실전 예시

### 예시 1: E-commerce 주문 시스템

javascript

```javascript
// Express.js + RabbitMQ
const amqp = require('amqplib');

class OrderQueue {
  constructor() {
    this.connection = null;
    this.channel = null;
  }

  async connect() {
    this.connection = await amqp.connect('amqp://localhost');
    this.channel = await this.connection.createChannel();
    
    // Priority Queue 생성
    await this.channel.assertQueue('order_queue', {
      durable: true,
      maxPriority: 10
    });
  }

  // VIP 고객 주문 (최고 우선순위)
  async publishVIPOrder(orderData) {
    this.channel.sendToQueue(
      'order_queue',
      Buffer.from(JSON.stringify(orderData)),
      {
        persistent: true,
        priority: 10,  // VIP 우선 처리
        headers: {
          orderType: 'VIP',
          customerId: orderData.customerId
        }
      }
    );
    console.log('VIP 주문 발행 (우선순위: 10)');
  }

  // 일반 주문 (중간 우선순위)
  async publishNormalOrder(orderData) {
    this.channel.sendToQueue(
      'order_queue',
      Buffer.from(JSON.stringify(orderData)),
      {
        persistent: true,
        priority: 5,  // 일반 처리
        headers: {
          orderType: 'NORMAL',
          customerId: orderData.customerId
        }
      }
    );
    console.log('일반 주문 발행 (우선순위: 5)');
  }

  // 배치 작업 (낮은 우선순위)
  async publishBatchOrder(orderData) {
    this.channel.sendToQueue(
      'order_queue',
      Buffer.from(JSON.stringify(orderData)),
      {
        persistent: true,
        priority: 1,  // 나중에 처리
        headers: {
          orderType: 'BATCH',
          customerId: orderData.customerId
        }
      }
    );
    console.log('배치 주문 발행 (우선순위: 1)');
  }

  // Consumer
  async consumeOrders() {
    this.channel.consume('order_queue', async (msg) => {
      if (msg) {
        const order = JSON.parse(msg.content.toString());
        const priority = msg.properties.priority;
        
        console.log(`\n=== 주문 처리 중 ===`);
        console.log(`우선순위: ${priority}`);
        console.log(`주문 타입: ${msg.properties.headers.orderType}`);
        console.log(`주문 데이터:`, order);
        
        // 주문 처리 로직
        await this.processOrder(order);
        
        this.channel.ack(msg);
      }
    }, {
      noAck: false
    });
  }

  async processOrder(order) {
    // 실제 주문 처리 로직
    return new Promise(resolve => {
      setTimeout(() => {
        console.log('주문 처리 완료!');
        resolve();
      }, 1000);
    });
  }
}

// 사용 예시
(async () => {
  const orderQueue = new OrderQueue();
  await orderQueue.connect();

  // Consumer 시작
  await orderQueue.consumeOrders();

  // 다양한 우선순위로 주문 발행
  await orderQueue.publishBatchOrder({ id: 1, item: 'A' });
  await orderQueue.publishNormalOrder({ id: 2, item: 'B' });
  await orderQueue.publishVIPOrder({ id: 3, item: 'C' });
  await orderQueue.publishNormalOrder({ id: 4, item: 'D' });
  await orderQueue.publishVIPOrder({ id: 5, item: 'E' });

  // 처리 순서: C(10) → E(10) → B(5) → D(5) → A(1)
})();
```

### 예시 2: 고객 지원 티켓 시스템

javascript

```javascript
const PRIORITY = {
  CRITICAL: 10,   // 시스템 다운, 보안 문제
  HIGH: 8,        // 결제 오류, 중요 기능 장애
  MEDIUM: 5,      // 일반 문의
  LOW: 2          // 개선 요청, 피드백
};

class SupportTicketQueue {
  async publishTicket(ticket) {
    const priority = this.getPriority(ticket.severity);
    
    this.channel.sendToQueue(
      'support_queue',
      Buffer.from(JSON.stringify(ticket)),
      {
        persistent: true,
        priority: priority,
        headers: {
          severity: ticket.severity,
          category: ticket.category,
          createdAt: new Date().toISOString()
        }
      }
    );
    
    console.log(`티켓 발행 - ${ticket.severity} (우선순위: ${priority})`);
  }

  getPriority(severity) {
    switch(severity) {
      case 'CRITICAL': return PRIORITY.CRITICAL;
      case 'HIGH': return PRIORITY.HIGH;
      case 'MEDIUM': return PRIORITY.MEDIUM;
      case 'LOW': return PRIORITY.LOW;
      default: return PRIORITY.MEDIUM;
    }
  }
}

// 사용
const ticketQueue = new SupportTicketQueue();

// 다양한 심각도의 티켓 발행
await ticketQueue.publishTicket({
  id: 101,
  severity: 'LOW',
  category: 'Feature Request',
  description: '다크모드 추가 요청'
});

await ticketQueue.publishTicket({
  id: 102,
  severity: 'CRITICAL',
  category: 'System Down',
  description: '서버 다운됨'
});

await ticketQueue.publishTicket({
  id: 103,
  severity: 'HIGH',
  category: 'Payment Error',
  description: '결제 실패'
});

// 처리 순서: 102(CRITICAL) → 103(HIGH) → 101(LOW)
```

### 예시 3: 이메일 발송 시스템

javascript

```javascript
const EMAIL_PRIORITY = {
  TRANSACTIONAL: 10,  // 비밀번호 재설정, OTP 등
  NOTIFICATION: 7,    // 주문 확인, 배송 알림
  MARKETING: 3,       // 프로모션, 뉴스레터
  BULK: 1            // 대량 마케팅 메일
};

class EmailQueue {
  // 트랜잭션 이메일 (즉시 발송)
  async sendTransactionalEmail(emailData) {
    await this.channel.sendToQueue(
      'email_queue',
      Buffer.from(JSON.stringify(emailData)),
      {
        persistent: true,
        priority: EMAIL_PRIORITY.TRANSACTIONAL,
        headers: {
          type: 'TRANSACTIONAL',
          urgent: true
        }
      }
    );
  }

  // 알림 이메일
  async sendNotificationEmail(emailData) {
    await this.channel.sendToQueue(
      'email_queue',
      Buffer.from(JSON.stringify(emailData)),
      {
        persistent: true,
        priority: EMAIL_PRIORITY.NOTIFICATION,
        headers: {
          type: 'NOTIFICATION'
        }
      }
    );
  }

  // 마케팅 이메일 (나중에 발송)
  async sendMarketingEmail(emailData) {
    await this.channel.sendToQueue(
      'email_queue',
      Buffer.from(JSON.stringify(emailData)),
      {
        persistent: true,
        priority: EMAIL_PRIORITY.MARKETING,
        headers: {
          type: 'MARKETING'
        }
      }
    );
  }

  // 대량 이메일 (여유 있을 때 발송)
  async sendBulkEmail(emailData) {
    await this.channel.sendToQueue(
      'email_queue',
      Buffer.from(JSON.stringify(emailData)),
      {
        persistent: true,
        priority: EMAIL_PRIORITY.BULK,
        headers: {
          type: 'BULK'
        }
      }
    );
  }
}
```

## Lazy Queue란?

Lazy Queue는 메시지를 **메모리가 아닌 디스크에 우선적으로 저장**하는 큐입니다. 일반 큐는 메시지를 메모리에 저장하다가 메모리가 부족하면 디스크로 이동시키지만, Lazy Queue는 처음부터 디스크에 저장합니다.

---

## 일반 큐 vs Lazy 큐

### 일반 큐 (Normal Queue)

```
메시지 도착 → 메모리 저장 → 메모리 부족 시 디스크로 이동
```

- **장점**: 빠른 성능 (메모리 접근)
- **단점**: 많은 메시지 축적 시 메모리 부족, OOM(Out of Memory) 위험

### Lazy 큐 (Lazy Queue)

```
메시지 도착 → 즉시 디스크 저장 → 필요 시 메모리로 로드
```

- **장점**: 메모리 사용량 최소화, 수백만 개 메시지 처리 가능
- **단점**: 디스크 I/O로 인한 처리 속도 저하

---

## 동작 원리

### 일반 큐의 문제점

javascript

```javascript
// 시나리오: 1000만 개의 메시지가 큐에 쌓임
// 일반 큐의 경우
메모리: 8GB 사용 (메시지가 메모리에 저장)
디스크: 100MB 사용 (일부만 페이징)
결과: 메모리 부족으로 RabbitMQ 다운 위험 ⚠️
```

### Lazy 큐의 해결책

javascript

```javascript
// 같은 시나리오: 1000만 개의 메시지
// Lazy 큐의 경우
메모리: 100MB 사용 (메타데이터만)
디스크: 8GB 사용 (모든 메시지)
결과: 안정적으로 처리 가능 ✅
```

---

## Lazy Queue 설정

### 1. 큐 생성 시 설정 (Node.js)

javascript

```javascript
const amqp = require('amqplib');

async function createLazyQueue() {
  const connection = await amqp.connect('amqp://localhost:5672');
  const channel = await connection.createChannel();

  // Lazy Queue 생성
  await channel.assertQueue('lazy_queue', {
    durable: true,
    arguments: {
      'x-queue-mode': 'lazy'  // ← 이것이 핵심!
    }
  });

  console.log('✅ Lazy Queue 생성 완료');
}

createLazyQueue();
```

### 2. 기존 큐를 Lazy로 변경

javascript

```javascript
// 주의: 기존 큐는 삭제 후 재생성해야 함
async function recreateAsLazy() {
  const connection = await amqp.connect('amqp://localhost:5672');
  const channel = await connection.createChannel();

  // 1. 기존 큐 삭제
  await channel.deleteQueue('existing_queue');

  // 2. Lazy Queue로 재생성
  await channel.assertQueue('existing_queue', {
    durable: true,
    arguments: {
      'x-queue-mode': 'lazy'
    }
  });

  console.log('✅ Lazy Queue로 변경 완료');
}
```

### 3. Management UI에서 설정

1. **Admin** → **Queues** → **Add a new queue**
2. **Arguments** 섹션에서:
    - Key: `x-queue-mode`
    - Value: `lazy`
3. **Add queue** 클릭

---

## 실전 예시

### 예시 1: 대량 로그 처리 시스템

javascript

```javascript
const amqp = require('amqplib');

class LogProcessingSystem {
  constructor() {
    this.connection = null;
    this.channel = null;
  }

  async connect() {
    this.connection = await amqp.connect('amqp://localhost:5672');
    this.channel = await this.connection.createChannel();

    // Lazy Queue 생성 (수백만 개의 로그 처리 가능)
    await this.channel.assertQueue('log_queue', {
      durable: true,
      arguments: {
        'x-queue-mode': 'lazy'
      }
    });

    console.log('✅ Log processing queue created (Lazy mode)');
  }

  // 대량 로그 발행
  async publishLogs(logCount = 1000000) {
    console.log(`📤 Publishing ${logCount} log messages...`);
    
    const startTime = Date.now();

    for (let i = 0; i < logCount; i++) {
      const logMessage = {
        id: i,
        timestamp: new Date().toISOString(),
        level: 'INFO',
        message: `Log message ${i}`,
        service: 'api-server',
        userId: Math.floor(Math.random() * 10000)
      };

      this.channel.sendToQueue(
        'log_queue',
        Buffer.from(JSON.stringify(logMessage)),
        { persistent: true }
      );

      // 진행 상황 출력
      if ((i + 1) % 100000 === 0) {
        console.log(`  → ${i + 1} messages published`);
      }
    }

    const duration = Date.now() - startTime;
    console.log(`✅ Published ${logCount} messages in ${duration}ms`);
    console.log(`💾 All messages stored on disk (Lazy mode)`);
  }

  // 로그 소비 (처리)
  async consumeLogs() {
    console.log('👂 Waiting for log messages...\n');

    let processedCount = 0;

    this.channel.consume('log_queue', async (msg) => {
      if (msg !== null) {
        const log = JSON.parse(msg.content.toString());
        
        // 로그 처리 (예: 데이터베이스 저장, 분석 등)
        await this.processLog(log);
        
        this.channel.ack(msg);
        processedCount++;

        if (processedCount % 10000 === 0) {
          console.log(`📊 Processed ${processedCount} logs`);
        }
      }
    }, {
      noAck: false,
      prefetch: 100  // 한 번에 100개씩 가져옴
    });
  }

  async processLog(log) {
    // 실제 로그 처리 로직
    // 예: 데이터베이스 저장, 에러 필터링, 알림 발송 등
    return new Promise(resolve => {
      // 비동기 처리 시뮬레이션
      setImmediate(() => resolve());
    });
  }
}

// 사용 예시
(async () => {
  const logSystem = new LogProcessingSystem();
  await logSystem.connect();

  // Producer: 100만 개의 로그 발행
  await logSystem.publishLogs(1000000);

  // Consumer: 로그 처리 시작
  await logSystem.consumeLogs();
})();
```