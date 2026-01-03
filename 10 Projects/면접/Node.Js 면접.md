## Node.js 면접 질문 및 답변

### 1. Node.js란 무엇이며 어디에서 사용할 수 있나요?

Node.js는 오픈 소스, 크로스 플랫폼 JavaScript 런타임 환경 및 라이브러리로, 웹 애플리케이션을 클라이언트 브라우저 외부에서 실행할 수 있도록 합니다. 주로 서버 사이드 웹 애플리케이션을 만들 때 사용됩니다.

Node.js는 비동기 및 이벤트 기반 모델을 사용하므로 데이터 집약적인 애플리케이션에 적합합니다. 예를 들어, 비디오 스트리밍 사이트와 같은 I/O 집약적인 웹 애플리케이션에 사용할 수 있습니다. 또한 다음과 같은 개발에 사용됩니다:

- 실시간 웹 애플리케이션
- 네트워크 애플리케이션
- 범용 애플리케이션
- 분산 시스템

---

### 2. 왜 Node.js를 사용하나요?

Node.js는 확장 가능한 네트워크 프로그램을 쉽게 구축할 수 있도록 합니다. 주요 장점은 다음과 같습니다:

- 일반적으로 빠름
- 차단(blocking)되지 않음
- 통합된 프로그래밍 언어와 데이터 타입 제공
- 모든 것이 비동기 방식으로 처리됨
- 높은 동시성 제공

---

### 3. Node.js는 어떻게 작동하나요?

Node.js 기반 웹 서버의 전형적인 동작 흐름은 다음과 같습니다:

1. 클라이언트가 웹 서버에 요청을 보냅니다. (데이터 조회, 삭제, 업데이트 등의 요청)
2. Node.js는 들어오는 요청을 이벤트 큐(Event Queue)에 추가합니다.
3. 요청은 이벤트 루프(Event Loop)를 통해 하나씩 처리됩니다. 이벤트 루프는 요청이 외부 리소스를 필요로 하는지 확인합니다.
4. 단순한 요청(비차단 작업)은 이벤트 루프에서 직접 처리됩니다. (예: I/O 작업)
5. 복잡한 요청(차단 작업)은 스레드 풀(Thread Pool)에서 하나의 스레드가 할당되어 처리합니다.
6. 처리가 완료되면 응답이 이벤트 루프로 반환되고, 이벤트 루프는 이를 클라이언트에게 응답합니다.

![[nodejs_event_loop.png]]
이벤트 루프는 크게 **6단계(Phases)** 로 동작합니다.

- **Timers** → `setTimeout`, `setInterval`
    
- **Pending Callbacks** → 일부 I/O 콜백
    
- **Idle, Prepare** → 내부 처리
    
- **Poll** → 새로운 I/O 이벤트 처리, 콜백 실행
    
- **Check** → `setImmediate` 실행
    
- **Close Callbacks** → socket close 같은 이벤트 처리
- [예시](https://youtube.com/shorts/cjbdAHjuK3w?si=WprW8LW7YHqzjy7v)

![[nodejs-infographic.png]]

---

### 4. Node.js는 왜 단일 스레드인가요?

Node.js는 비동기 처리를 위해 단일 스레드로 작동합니다. 기존의 멀티 스레드 기반 구현 방식보다 단일 스레드에서 비동기 처리를 하면 더 높은 성능과 확장성을 얻을 수 있기 때문입니다.

---

### 5. Node.js가 단일 스레드인데 동시성을 어떻게 처리하나요?

Node.js는 멀티스레드 요청/응답 모델을 따르지 않고, 단일 스레드 이벤트 루프 모델을 사용합니다. JavaScript의 이벤트 기반 모델과 콜백 시스템을 활용하여 여러 개의 동시 클라이언트 요청을 효율적으로 처리할 수 있습니다.

이벤트 루프(Event Loop)가 핵심적인 역할을 하며, 블로킹 작업이 필요한 경우 백그라운드에서 처리한 후 콜백을 실행하여 응답을 반환합니다.

---

### 6. Node.js에서 콜백(Callback)이란?

콜백 함수는 특정 작업이 완료된 후 호출되는 함수입니다. 이를 통해 다른 코드가 동시에 실행될 수 있으며, 차단(blocking)을 방지할 수 있습니다.

Node.js는 비동기 플랫폼이므로 콜백을 많이 사용합니다. 모든 Node.js API는 콜백을 지원하도록 작성되어 있습니다.

---

### 7. 콜백 대신 프로미스(Promise)를 사용하면 어떤 장점이 있나요?

- 비동기 로직의 흐름을 더 명확하고 구조적으로 관리 가능
- 낮은 결합도(Loose Coupling) 유지
- 내장된 오류 처리 기능 제공
- 코드 가독성이 향상됨

---

### 8. I/O(Input/Output)란?

I/O는 데이터를 한 매체에서 다른 매체로 전송하는 모든 작업을 의미합니다.

예를 들면:

- 네트워크 요청
- 파일 읽기/쓰기
- 데이터베이스 조회

---

### 9. Node.js는 주로 어디에 사용되나요?

Node.js는 다음과 같은 애플리케이션에서 널리 사용됩니다:

- 실시간 채팅 애플리케이션
- 사물인터넷(IoT) 애플리케이션
- 복잡한 단일 페이지 애플리케이션(SPA)
- 실시간 협업 도구 (예: Google Docs)
- 스트리밍 애플리케이션 (예: Netflix)
- 마이크로서비스 아키텍처

---

### 10. 프론트엔드와 백엔드 개발의 차이는 무엇인가요?

|프론트엔드|백엔드|
|---|---|
|클라이언트 측(Client-Side) 개발|서버 측(Server-Side) 개발|
|사용자가 직접 보고 상호작용하는 UI 관련 코드|데이터베이스, 서버 로직 등 내부 기능|
|HTML, CSS, JavaScript, React, Angular 사용|Node.js, Java, Python, PHP 등 사용|

---

### 11. NPM(Node Package Manager)이란?

NPM은 Node.js 패키지 및 모듈을 관리하는 도구입니다.

주요 기능:

1. 온라인 패키지 저장소 제공 ([search.nodejs.org](http://search.nodejs.org))
2. 명령줄을 통해 패키지를 설치, 관리, 삭제 가능

---

### 12. Node.js의 모듈이란?

모듈은 특정 기능을 캡슐화한 JavaScript 라이브러리입니다. `require()` 함수를 사용하여 모듈을 로드할 수 있습니다.

Node.js의 주요 기본 모듈:

- `http`: HTTP 서버 생성
- `fs`: 파일 시스템 관련 작업
- `url`: URL 파싱
- `querystring`: 쿼리 스트링 처리
- `stream`: 데이터 스트리밍
- `zlib`: 파일 압축/해제

---

### 13. `module.exports`의 목적은?

Node.js에서는 `module.exports`를 사용하여 함수나 객체를 외부 파일에서 사용할 수 있도록 내보냅니다.

```jsx
module.exports = function() {
  console.log("Hello, World!");
};
```

---

### 14. Node.js가 Java 및 PHP보다 선호되는 이유는?

- 빠른 실행 속도 (V8 엔진 기반)
- NPM을 통해 방대한 패키지 활용 가능
- 데이터 집약적인 실시간 애플리케이션에 적합
- 클라이언트와 서버에서 동일한 JavaScript 코드 사용 가능

---

### 15. Angular와 Node.js의 차이점

|Angular|Node.js|
|---|---|
|프론트엔드 프레임워크|백엔드 런타임 환경|
|TypeScript 기반|JavaScript 기반|
|SPA 개발용|서버 측 애플리케이션 개발용|

---

### 16. Node.js와 가장 많이 사용되는 데이터베이스는?

**MongoDB**가 가장 많이 사용됩니다.

- NoSQL 기반 문서 지향 데이터베이스
- 확장성 높음
- JSON과 유사한 데이터 구조

---

### 17. Node.js에서 많이 사용하는 라이브러리?

1. **Express.js**: 웹 서버 구축을 위한 프레임워크
2. **Mongoose**: MongoDB와의 데이터베이스 연동을 쉽게 해주는 라이브러리

---

### 18. Node.js의 장단점

|장점|단점|
|---|---|
|빠른 처리 속도|CPU 집약적인 작업에 부적합|
|JavaScript 기반|콜백 지옥(Callback Hell) 문제 발생 가능|
|이벤트 기반 비동기 처리|관계형 데이터베이스와의 호환성 부족|

---

### 19. 외부 라이브러리를 가져오는 데 사용하는 명령어는 무엇인가요?

`require` 명령어를 사용하여 외부 라이브러리를 가져올 수 있습니다.

예를 들어:

```jsx

var http = require("http");
```

위의 코드를 실행하면 `http` 라이브러리가 로드되고, `http` 변수에 단일 내보낸 객체가 저장됩니다.

---

## 중급 수준 Node.js 면접 질문 및 답변

### 20. 이벤트 기반 프로그래밍(Event-Driven Programming)이란 무엇인가요?

이벤트 기반 프로그래밍은 특정 이벤트(예: 키 입력, 마우스 클릭 등)가 발생할 때 미리 정의된 함수를 실행하는 방식입니다.

Node.js에서는 콜백 함수가 사전 등록되어 있으며, 이벤트가 발생하면 해당 함수가 실행됩니다.

---

### 21. Node.js의 이벤트 루프(Event Loop)란 무엇인가요?

이벤트 루프는 Node.js에서 비동기 콜백을 처리하는 핵심 요소입니다.

이벤트 루프 덕분에 Node.js는 비동기, 논블로킹 방식으로 동작할 수 있으며, 높은 성능을 제공합니다.

---

### 22. `process.nextTick()`과 `setImmediate()`의 차이점은 무엇인가요?

- `process.nextTick()`: 현재 이벤트 루프가 끝난 후 즉시 실행됨
- `setImmediate()`: 다음 이벤트 루프 사이클에서 실행됨

즉, `nextTick()`은 현재 실행 중인 이벤트 루프의 끝에서 콜백을 실행하고, `setImmediate()`는 다음 이벤트 루프 사이클에서 실행합니다.

---

### 23. Node.js의 EventEmitter란?

`EventEmitter`는 이벤트를 발생시키고 처리하는 기능을 제공하는 클래스입니다.

- `EventEmitter` 클래스를 상속받은 객체는 이벤트를 발생(`emit()`)하고, 해당 이벤트에 대한 리스너(`on()`)를 등록할 수 있습니다.

```jsx

const EventEmitter = require('events');
const myEmitter = new EventEmitter();
myEmitter.on('event', () => {
  console.log('이벤트 발생!');
});
myEmitter.emit('event');

```

---

### 24. Node.js의 API 함수 유형은 무엇인가요?

1. **비동기(Asynchronous), 논블로킹(Non-Blocking) 함수**
    - 이벤트 루프를 사용하여 블로킹 없이 실행됨
    - 예: `fs.readFile()`
2. **동기(Synchronous), 블로킹(Blocking) 함수**
    - 호출된 함수가 완료될 때까지 실행을 멈춤
    - 예: `fs.readFileSync()`

---

### 25. `package.json` 파일이란?

`package.json` 파일은 Node.js 프로젝트의 메타데이터를 포함하는 파일입니다.

이 파일은 프로젝트의 루트 디렉토리에 있으며, 종속성 목록, 스크립트, 버전 정보 등을 포함합니다.

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.17.1"
  }
}

```

---

### 26. Node.js에서 URL 모듈을 어떻게 사용하나요?

`url` 모듈을 사용하면 URL을 쉽게 파싱하고 조작할 수 있습니다.

```jsx

const url = require('url');
const parsedUrl = url.parse('<https://example.com/path?name=John>');
console.log(parsedUrl.query); // "name=John"

```

---

### 27. Express.js란?

Express.js는 Node.js 기반의 경량 웹 애플리케이션 프레임워크입니다.

- REST API 및 웹 애플리케이션 개발에 널리 사용됨
- 라우팅 및 미들웨어 기능 제공

---

### 28. 간단한 Express.js 애플리케이션을 만드는 방법은?

```jsx

const express = require('express');
const app = express();
app.get('/', (req, res) => {
  res.send('Hello, World!');
});
app.listen(3000, () => console.log('서버 실행 중...'));

```

---

### 29. Node.js에서 스트림(Stream)이란?

스트림은 데이터를 연속적으로 읽거나 쓸 수 있도록 하는 객체입니다.

- **Readable 스트림**: 읽기 전용 (`fs.createReadStream()`)
- **Writable 스트림**: 쓰기 전용 (`fs.createWriteStream()`)
- **Duplex 스트림**: 읽기 및 쓰기 가능 (`net.Socket`)
- **Transform 스트림**: 입력 데이터를 변환하여 출력 (`zlib.createGzip()`)

---

### 30. 의존성을 설치, 업데이트, 삭제하는 방법은?

- 설치: `npm install package-name`
- 업데이트: `npm update package-name`
- 삭제: `npm uninstall package-name`

---

### 31. 간단한 Node.js 서버를 만드는 방법은?

```jsx

const http = require('http');
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello, World!');
});
server.listen(8080, () => console.log('서버 실행 중...'));

```

---

### 32. Node.js의 비동기 및 논블로킹 API란?

Node.js의 API는 기본적으로 비동기이며, 서버가 API 호출을 기다리지 않고 다음 작업을 수행할 수 있도록 설계되었습니다.

```jsx

fs.readFile('file.txt', (err, data) => {
  if (err) throw err;
  console.log(data);
});
console.log('파일 읽는 중...');

```

---

### 33. Node.js에서 `async`를 구현하는 방법은?

```jsx

async function fetchData() {
  let data = await fetch('<https://api.example.com/data>');
  console.log(data);
}
fetchData();

```

---

### 34. 콜백 함수란?

콜백 함수는 특정 작업이 완료된 후 실행되는 함수입니다.

```jsx

function greet(name, callback) {
  console.log('Hello ' + name);
  callback();
}
greet('Alice', () => console.log('Welcome!'));

```

---

## 고급 수준 Node.js 면접 질문 및 답변

### 35. Node.js의 REPL이란?

REPL(Read-Eval-Print Loop)은 Node.js에서 인터랙티브하게 JavaScript 코드를 실행할 수 있는 환경입니다.

- `node` 명령어를 실행하면 REPL 환경이 시작됩니다.

---

### 36. 제어 흐름 함수(Control Flow Function)란?

비동기 함수 실행을 제어하는 함수입니다.

---

### 38. `fork()`와 `spawn()`의 차이점은?

|`fork()`|`spawn()`|
|---|---|
|새로운 V8 엔진 인스턴스 생성|새로운 프로세스를 생성하지만 V8 인스턴스를 공유|
|부모-자식 프로세스 간 메시지 교환 가능|부모 프로세스에서 데이터 스트림 관리|

---

### 39. Node.js의 `Buffer` 클래스란?

`Buffer` 클래스는 바이너리 데이터를 저장할 수 있도록 합니다.

```jsx

const buf = Buffer.from('Hello');
console.log(buf.toString()); // "Hello"

```

---

### 40. 파이핑(Piping)이란?

한 스트림의 출력을 다른 스트림의 입력으로 연결하는 방식입니다.

```jsx

const fs = require('fs');
fs.createReadStream('input.txt').pipe(fs.createWriteStream('output.txt'));

```

---

### 43. 콜백 헬(Callback Hell)이란?

중첩된 콜백이 많아 코드가 복잡해지는 문제를 말합니다.

```jsx

fs.readFile('file1.txt', (err, data) => {
  fs.readFile('file2.txt', (err, data) => {
    fs.readFile('file3.txt', (err, data) => {
      console.log('콜백 헬 발생!');
    });
  });
});

```

해결책: `Promise` 또는 `async/await` 사용

---

### 44. Node.js에서 리액터 패턴이란?

리액터 패턴은 논블로킹 I/O 작업 개념을 의미합니다. 이 패턴은 각 I/O 작업과 연결된 핸들러를 제공합니다. I/O 요청이 생성되면, 이를 디멀티플렉서(demultiplexer)에 제출합니다.

---

### 45. Node.js에서 테스트 피라미드란?

**테스트 피라미드**는 소프트웨어 테스트에서 이상적인 테스트 분포를 설명하는 개념입니다. 테스트 피라미드는 일반적으로 단위 테스트(Unit Test), 통합 테스트(Integration Test), 그리고 종단 간 테스트(End-to-End Test)의 3가지 계층으로 구성됩니다.

---

### 46. Google이 Node.js에서 V8 엔진을 사용하는 이유는?

V8 엔진은 Google이 개발한 오픈 소스이며, C++로 작성되었습니다. Google Chrome 브라우저에서도 사용되며, Node.js에서도 활용됩니다. V8 엔진은 자바스크립트 실행 속도를 높이기 위해 만들어졌으며, 해석기 대신 **JIT(Just-In-Time) 컴파일러**를 사용하여 실행 중 자바스크립트 코드를 기계 코드로 변환하여 성능을 최적화합니다.

---

### 47. Node.js의 종료 코드(exit codes)란?

- *종료 코드(exit code)**는 프로세스가 종료될 때 반환하는 숫자로, 실행 결과를 나타냅니다. 일반적인 종료 코드에는 다음이 포함됩니다:
- `0` : 정상 종료
- `1` : 일반적인 오류
- `5` : 파일이 존재하지 않음
- `8` : 메모리 부족

---

### 48. Node.js에서 미들웨어(Middleware)의 개념은?

**미들웨어**는 요청(request) 및 응답(response) 객체를 처리하는 함수입니다. 주요 기능은 다음과 같습니다:

- 코드 실행
- 요청 및 응답 객체 수정
- 요청-응답 주기 종료
- 다음 미들웨어 호출

---

### 49. HTTP 요청의 종류는?

HTTP 요청에는 여러 가지 메서드가 있습니다:

- `GET` : 데이터 조회
- `POST` : 서버에서 상태 변경
- `HEAD` : 응답 본문 없이 헤더만 요청
- `DELETE` : 특정 리소스 삭제

---

### 50. Node.js에서 MongoDB 데이터베이스를 연결하는 방법은?

MongoDB를 연결하려면 `MongoClient` 객체를 생성한 후 올바른 IP 주소 및 데이터베이스 이름을 지정하여 연결합니다.

```jsx
const { MongoClient } = require("mongodb");
const uri = "mongodb://localhost:27017/mydb";
const client = new MongoClient(uri);

async function connectDB() {
    try {
        await client.connect();
        console.log("MongoDB 연결 성공!");
    } catch (error) {
        console.error("연결 실패:", error);
    }
}
connectDB();

```

---

### 51. NODE_ENV의 목적은?

`NODE_ENV`는 환경 변수를 설정하여 **개발(development), 테스트(test), 프로덕션(production)** 환경을 구분하는 역할을 합니다.

```jsx

if (process.env.NODE_ENV === "production") {
    console.log("프로덕션 환경입니다.");
} else {
    console.log("개발 또는 테스트 환경입니다.");
}

```

---

### 52. Node.js에서 제공하는 타이머 기능은?

Node.js의 타이머 기능에는 다음과 같은 것들이 있습니다:

- `setTimeout(callback, delay)` : 일정 시간 후 실행
- `setInterval(callback, interval)` : 일정 간격마다 실행
- `setImmediate(callback)` : 이벤트 루프의 현재 사이클이 끝나면 실행

---

### 53. WASI란 무엇이며, 도입된 이유는?

WASI(WebAssembly System Interface)는 WebAssembly 애플리케이션이 OS에서 실행될 수 있도록 시스템 API를 제공하는 인터페이스입니다. 이를 통해 브라우저 외에서도 WebAssembly를 실행할 수 있습니다.

---

### 54. JavaScript에서 일급 함수(First-Class Function)란?

일급 함수는 **함수를 변수에 할당하거나, 매개변수로 전달하거나, 반환할 수 있는 함수**를 의미합니다. Node.js에서는 비동기 프로그래밍에서 콜백 함수와 함께 널리 사용됩니다.

```jsx
function greet(name) {
    return `Hello, ${name}`;
}
const sayHello = greet;
console.log(sayHello("Node.js")); // Hello, Node.js

```

---

### 55. Node.js 프로젝트에서 패키지를 관리하는 방법은?

패키지 관리는 **Node Package Manager(NPM)**을 사용하여 실행됩니다.

- 패키지 설치: `npm install package-name`
- 패키지 제거: `npm uninstall package-name`
- 프로젝트 의존성 설치: `npm install`

---

### 56. Node.js가 다른 프레임워크보다 나은 점은?

- **비동기 처리** : 논블로킹 I/O 모델을 사용
- **빠른 실행 속도** : Google V8 엔진을 사용
- **확장성** : 경량의 단일 스레드 아키텍처

---

### 57. Node.js에서 포크(Fork)란?

포크는 **자식 프로세스를 생성**하여 Node.js 프로세스를 분리하는 기능입니다.

```jsx
const { fork } = require("child_process");
const child = fork("child.js");
child.send("Hello from parent");

```

---

### 58. `async.queue`가 받는 두 가지 인자는?

1. **Worker 함수**: 큐의 각 작업을 처리하는 함수
2. **Concurrency limit**: 동시에 실행할 작업의 개수

---

### 59. `module.exports`의 목적은?

`module.exports`는 **모듈에서 함수를 내보내고(import)** 다른 파일에서 이를 사용할 수 있도록 합니다.

```jsx
javascript
복사편집
// utils.js
module.exports = function sum(a, b) {
    return a + b;
};

// app.js
const sum = require("./utils");
console.log(sum(2, 3)); // 5

```

---

### 60. 코드 스타일을 일관되게 유지하는 도구는?

- **ESLint** : 코드 품질 검사
- **Prettier** : 코드 포맷 자동화
- **Jest** : 테스트 자동화

---

### 61. JavaScript와 Node.js의 차이점은?

- **JavaScript** : 브라우저에서 실행되는 언어
- **Node.js** : JavaScript를 서버에서 실행할 수 있도록 만든 런타임 환경

---

### 62. 동기(synchronous)와 비동기(asynchronous) 함수의 차이점은?

- **동기 함수** : 한 번에 하나의 작업만 실행하며, 이전 작업이 끝날 때까지 기다림
- **비동기 함수** : 작업을 동시에 실행하며, 대기하지 않고 다음 작업으로 넘어감

---

### 63. 이벤트 루프에서 실행해야 하는 비동기 작업은?

- I/O 작업 (파일 읽기, 네트워크 요청)
- 타이머 (setTimeout, setInterval)
- 콜백 함수 실행

---

### 64. Node.js에서 제어 흐름의 실행 순서는?

이벤트 루프의 순서에 따라 실행됩니다:

1. **Timer 단계** (setTimeout, setInterval)
2. **I/O 콜백 단계**
3. **idle & prepare 단계**
4. **poll 단계** (대기 중인 I/O 작업 실행)
5. **check 단계** (setImmediate 실행)
6. **close 단계**

---

**65. 비동기 큐의 입력 인자는 무엇인가요?**

Node.js의 비동기 큐는 특정 순서로 함수를 실행할 수 있도록 하는 데이터 구조입니다. 함수가 큐에 추가되면 추가된 순서대로 실행됩니다. 비동기 큐는 특정 순서로 일련의 함수를 실행해야 할 때 유용합니다.

---

**66. Node.js의 단점이 있나요?**

- Node.js는 CPU 집약적인 작업에는 적합하지 않습니다. Node.js는 단일 스레드 환경에서 실행되므로 한 번에 하나의 작업만 수행할 수 있기 때문입니다.
- Node.js는 많은 메모리를 사용하는 애플리케이션에 적합하지 않습니다. 각 연결마다 많은 메모리를 사용하며, 연결 수가 많아지면 메모리 사용량이 빠르게 증가할 수 있습니다.

📌 **알고 계셨나요?**

풀스택 개발자에 대한 수요는 다른 기술 직군보다 35% 더 빠르게 증가하고 있습니다. 이는 프론트엔드와 백엔드 기술을 모두 다룰 수 있는 다재다능한 개발자를 찾는 기업이 많아지기 때문입니다.

---

**67. Node.js에서 이벤트 기반 모델을 사용하는 주요 이유는 무엇인가요?**

Node.js에서 이벤트 기반 모델을 사용하는 주요 이유는 **성능** 때문입니다. 이벤트 기반 모델을 사용하면 **비동기 I/O 작업**을 실행할 수 있어 적은 리소스로 많은 연결을 처리할 수 있습니다.

---

**68. Node.js와 Ajax의 차이점은 무엇인가요?**

- **Ajax**: 클라이언트 측 기술로, 클라이언트와 서버 간의 비동기 통신을 가능하게 합니다. 주로 웹 페이지의 특정 부분을 새로고침 없이 업데이트하는 데 사용됩니다.
- **Node.js**: 서버 측 기술로, 빠르고 확장성이 뛰어난 서버 애플리케이션을 구축하는 데 사용됩니다. 실시간 채팅, 온라인 게임, 스트리밍 서비스와 같은 애플리케이션에서 주로 사용됩니다.

---

**69. Node.js를 사용하는 장점은 무엇인가요?**

- 빠르고 확장성이 뛰어납니다.
- 배우고 사용하기 쉽습니다.
- 실시간 애플리케이션(예: 채팅 애플리케이션, 온라인 게임, 스트리밍 서비스)에 적합합니다.

---

**70. Node.js는 Windows에서 실행되나요?**

네, Node.js는 Windows에서 실행됩니다. Node.js는 **크로스 플랫폼 런타임 환경**이므로 Windows, macOS, Linux 등 다양한 운영 체제에서 실행할 수 있습니다.

---

**71. Node.js에서 DOM에 접근할 수 있나요?**

아니요, Node.js에서는 DOM에 접근할 수 없습니다. DOM은 브라우저 환경에서 HTML 및 XML 문서를 조작하기 위한 API이며, Node.js는 브라우저가 아니라 서버 환경에서 실행되기 때문입니다.

---

**72. Java 개발자들이 Node.js에 관심을 갖는 이유는 무엇인가요?**

Java는 강력한 서버 측 기술이지만, 실행 속도가 느리고 리소스를 많이 소비할 수 있습니다. 반면, **Node.js는 V8 JavaScript 엔진을 기반으로 하여 빠르고 효율적인 성능을 제공합니다**. 이 때문에 Java 개발자들이 Node.js를 점점 더 많이 사용하고 있습니다.

---

**73. Node.js의 주요 과제(단점)는 무엇인가요?**

- **단일 스레드**: 한 번에 하나의 작업만 실행할 수 있습니다.
- **비교적 새로운 기술**: Java나 PHP와 같은 기존 서버 기술보다 상대적으로 지원 리소스가 적습니다.
- **메모리 문제**: 많은 메모리를 필요로 하는 애플리케이션에는 적합하지 않습니다.

---

**74. Node.js에서 "논블로킹(Non-blocking)"이란 무엇인가요?**

논블로킹이란 **작업이 완료될 때까지 기다리지 않고 여러 작업을 동시에 실행할 수 있는 기능**을 의미합니다. 이는 비동기 I/O를 활용하여 여러 요청을 동시에 처리하는 방식입니다.

---

**75. Node.js는 어떻게 블로킹 I/O 문제를 해결하나요?**

Node.js는 **이벤트 기반, 논블로킹 I/O 모델**을 사용하여 I/O 작업을 더 효율적으로 처리합니다.

- **콜백(callback) 함수**를 사용하여 특정 작업이 완료될 때까지 기다리지 않고 다른 작업을 실행할 수 있습니다.
- **싱글 스레드 이벤트 루프**를 활용하여 많은 요청을 효과적으로 처리할 수 있습니다.

---

**76. Node.js에서 async/await를 사용하는 방법은?**

- `async` 키워드를 사용하여 비동기 함수를 선언할 수 있습니다.
- `await` 키워드를 사용하면 프로미스(Promise)가 해결될 때까지 기다린 후 다음 코드가 실행됩니다.

---

**77. Express 앱과 서버를 분리해야 하는 이유는 무엇인가요?**

- **테스트가 쉬워짐**: 애플리케이션 로직을 독립적으로 테스트할 수 있습니다.
- **확장성 증가**: 여러 서버 인스턴스를 실행하여 부하를 분산할 수 있습니다.
- **유연성 향상**: 서버를 변경하더라도 애플리케이션 코드에 영향을 주지 않습니다.

---

**78. Node.js에서 스텁(Stub)이란?**

스텁은 **더 복잡한 함수를 대신하는 간단한 함수**입니다. 주로 **단위 테스트**에서 사용되며, 실제 함수의 동작을 모방하여 테스트의 예측 가능성을 높입니다.

---

**79. 현재 Node.js에서 가장 많이 사용되는 프레임워크는 무엇인가요?**

- **Express.js**
- **Koa.js**

---

**80. Node.js에서 제공하는 주요 보안 기능은 무엇인가요?**

- **샌드박스 환경**: 악성 코드가 시스템에 접근하는 것을 방지합니다.
- **TLS/SSL 암호화**: 데이터 전송 중 보안을 강화합니다.

---

**81. Libuv란?**

Libuv는 **Node.js에서 논블로킹 I/O를 처리하는 핵심 라이브러리**입니다.

---

**82. Node.js의 글로벌 객체(Global Objects)란?**

Node.js에서 전역적으로 사용 가능한 객체로, `process`, `console`, `buffer` 등이 있습니다.

---

**83. Node.js에서 assert 모듈을 사용하는 이유는 무엇인가요?**

`assert` 모듈은 **테스트 작성 시 코드의 동작을 검증하는 데 사용됩니다**.

---

**84. ExpressJS를 사용하는 이유는 무엇인가요?**

Express는 **간결하고 강력한 웹 애플리케이션 프레임워크**로, 빠르고 확장성이 뛰어납니다.

---

**85. Node.js에서 Connect 모듈의 역할은?**

Connect 모듈은 **미들웨어를 관리하는 기능**을 제공합니다.

- **에러 핸들링 미들웨어**
- **쿠키 파싱 미들웨어**
- **세션 관리 미들웨어**

---

**86. 프론트엔드와 백엔드 개발의 차이점은 무엇인가요?**

- **프론트엔드**: 클라이언트 측(UI 및 사용자 인터페이스 개발)
- **백엔드**: 서버 측(데이터베이스, API, 서버 로직 개발)

---

**87. Node.js의 LTS(Long-Term Support) 릴리스란?**

LTS는 **장기 지원 버전**으로, 일반적으로 30개월 동안 안정적인 업데이트와 보안 패치를 제공합니다.

---

**88. ESLint란?**

ESLint는 **JavaScript 코드의 문법 오류와 잠재적 문제를 분석하는 정적 코드 분석 도구**입니다.

---

**89. 테스트 피라미드(Test Pyramid)란?**

소프트웨어 테스트 전략을 나타내는 개념으로, 다음과 같이 계층을 구성합니다.

1. **단위 테스트(Unit Test)** → 가장 많아야 함
2. **통합 테스트(Integration Test)**
3. **엔드 투 엔드 테스트(E2E Test)** → 가장 적어야 함

HTTP API 테스트에 적용할 때, 먼저 단위 테스트를 작성한 후, 통합 테스트, 마지막으로 엔드 투 엔드 테스트를 수행하는 것이 좋습니다.

---

### 90. Node.js는 자식 스레드를 어떻게 처리하나요?

Node.js는 별도의 Node.js 런타임 환경 인스턴스를 생성하여, 메인 프로세스와 병렬로 실행할 수 있도록 합니다.

---

### 91. Node.js의 이벤트 발생기(Event Emitter)란?

이벤트 발생기는 Node.js 애플리케이션 내에서 객체 간의 통신을 용이하게 하는 모듈입니다. EventEmitter 클래스의 인스턴스로, 이벤트를 감지하고 발생시키는 기능을 제공합니다. Node.js에서는 이벤트가 핵심 개념이며, 비동기 작업을 처리하는 데 사용됩니다.

---

### 92. 클러스터링을 통해 Node.js 성능을 어떻게 향상시킬 수 있나요?

클러스터링을 사용하면 HTTP 서버, 데이터베이스 연결 및 기타 I/O 작업의 성능을 향상시킬 수 있습니다. 하지만 클러스터링을 적용한다고 해서 성능이 선형적으로 증가하는 것은 아닙니다.

---

### 93. 스레드 풀(Thread Pool)이란 무엇이며, Node.js에서 이를 처리하는 라이브러리는 무엇인가요?

스레드 풀은 병렬로 실행되는 여러 개의 스레드 모음으로, 작업을 효율적으로 분배하는 데 사용됩니다. Node.js에서는 libuv 라이브러리가 스레드 풀을 관리하며, 비동기 I/O 작업을 수행합니다.

---

### 94. 워커 스레드(Worker Threads)와 클러스터(Cluster)의 차이점은?

- **클러스터(Cluster)**: 여러 개의 Node.js 프로세스를 생성하여 각 프로세스를 별도의 CPU 코어에서 실행하는 방식
- **워커 스레드(Worker Threads)**: 단일 프로세스 내에서 여러 개의 스레드를 생성하여 병렬 작업을 수행하는 방식

---

### 95. 비동기 작업의 실행 시간을 측정하는 방법은?

- `console.time()` 및 `console.timeEnd()`를 사용하여 코드 실행 시간을 측정할 수 있습니다.
- `performance.now()`를 사용하면 밀리초 단위로 보다 정밀한 시간 측정이 가능합니다.

---

### 96. 비동기 작업의 성능을 측정하는 방법은?

- `-prof` 플래그를 사용하여 Node.js 프로파일링을 수행
- `perf` 도구를 활용하여 성능 분석
- `benchmark.js` 같은 서드파티 라이브러리를 사용하여 벤치마킹

---

### 97. Node.js에서 사용 가능한 스트림 유형은?

1. **읽기 가능한 스트림(Readable Stream)**
2. **쓰기 가능한 스트림(Writable Stream)**
3. **듀플렉스 스트림(Duplex Stream)** (읽기 및 쓰기 가능)
4. **변환 스트림(Transform Stream)** (데이터 변환 가능)

---

### 98. Node.js에서 추적(Tracing)이란?

추적(Tracing)은 Node.js 애플리케이션의 성능을 프로파일링하는 기법입니다. 실행 중 발생하는 함수 호출 및 이벤트를 기록하여 성능 병목을 분석하는 데 사용됩니다.

---

### 99. `package.json` 파일은 Node.js에서 어디에 사용되나요?

`package.json` 파일은 애플리케이션의 루트 디렉토리에 위치하며, `npm` 패키지 관리자가 이를 사용하여 종속성 관리 및 프로젝트 설정을 수행합니다.

---

### 100. `readFile`과 `createReadStream`의 차이점은?

- `readFile()`: 파일의 전체 내용을 한 번에 읽어 메모리에 로드 (작은 파일에 적합)
- `createReadStream()`: 파일을 스트림 형태로 읽어 들여 메모리 사용 최소화 (대형 파일에 적합)

---

### 101. Node.js에서 `crypto` 모듈의 용도는?

`crypto` 모듈은 암호화 기능을 제공하며, 보안 랜덤 숫자 생성, 디지털 서명 생성 및 검증, AES, DES, RSA 등의 암호화 알고리즘을 지원합니다.

---

### 102. Node.js의 `passport`란?

`passport`는 Node.js에서 사용되는 인기 있는 인증 미들웨어로, 사용자명/비밀번호 인증, Facebook, Google 등의 소셜 로그인, JWT(JSON Web Token) 인증을 지원합니다.

---

### 103. Node.js에서 파일 정보를 가져오는 방법은?

`fs.stat()` 메서드를 사용하면 파일 크기, 생성 날짜, 수정 날짜 등의 정보를 포함하는 객체를 반환합니다.

---

### 104. Node.js에서 DNS 조회 함수가 작동하는 방식은?

Node.js의 `dns` 모듈을 사용하여 DNS 조회를 수행할 수 있습니다. `dns.lookup()` 메서드는 도메인 이름을 IP 주소로 변환하는 기능을 제공합니다.

---

### 105. `setImmediate()`와 `setTimeout()`의 차이점은?

- `setTimeout()`: 특정 시간이 지난 후 코드 실행을 예약
- `setImmediate()`: 현재 이벤트 루프가 완료된 직후 코드 실행을 예약 (우선순위가 `setTimeout()`보다 높음)

---

### 106. Node.js의 `Punycode` 개념은?

Punycode는 DNS에서 유니코드 문자를 ASCII 문자로 변환하는 인코딩 방식으로, 중국어나 아랍어 같은 비ASCII 문자를 포함하는 도메인 이름을 변환하는 데 사용됩니다.

---

### 107. Node.js에서 제공하는 디버거가 있나요?

네, Node.js는 기본적으로 내장된 디버거를 제공합니다.

---

### 108. Node.js에서 암호화 기능을 지원하나요?

네, `crypto` 모듈을 통해 암호화 기능을 기본적으로 제공합니다.

---

### 109. 왜 이 Node.js 역할에 적합하다고 생각하나요?

저는 Node.js를 활용하여 확장 가능하고 효율적인 서버 애플리케이션을 구축한 경험이 있으며, 협업과 커뮤니케이션 스킬도 갖추고 있습니다. 이러한 경험과 역량이 이 역할에 적합하다고 생각합니다.

---

### 110. 이전에 Node.js 관련 업무 경험이 있나요?

네, 저는 이전에 Node.js를 활용한 여러 프로젝트를 수행한 경험이 있습니다.

---

### 111. 해당 산업에서 근무한 경험이 있나요?

네, 저는 이전에도 Node.js를 사용하여 해당 산업과 관련된 프로젝트를 수행한 경험이 있습니다.

---

### 112. 이 Node.js 역할을 위한 자격증이 있나요?

네, 저는 **OpenJS Node.js Services Developer(JSNSD) 인증**을 보유하고 있습니다.

---

### 113. CallBack, Promise의 차이점
- **콜백 기반 비동기 처리** → 가독성이 낮고 콜백 지옥 발생 가능
    ```jsx
    CallBack기반
    
    const fs = require("fs");
    
    fs.readFile("file1.txt", "utf8", (err, data1) => {
      if (err) throw err;
      console.log("file1 읽기 완료");
    
      fs.readFile("file2.txt", "utf8", (err, data2) => {
        if (err) throw err;
        console.log("file2 읽기 완료");
    
        fs.readFile("file3.txt", "utf8", (err, data3) => {
          if (err) throw err;
          console.log("file3 읽기 완료");
        });
      });
    });
    
    ```

- **Promise 활용** → 더 구조적이고 오류 처리 용이
    ```jsx
    Promise기반
    const fs = require("fs").promises;
    
    fs.readFile("file1.txt", "utf8")
      .then((data1) => {
        console.log("file1 읽기 완료");
        return fs.readFile("file2.txt", "utf8");
      })
      .then((data2) => {
        console.log("file2 읽기 완료");
        return fs.readFile("file3.txt", "utf8");
      })
      .then((data3) => {
        console.log("file3 읽기 완료");
      })
      .catch((err) => {
        console.error("에러 발생:", err);
      });
    
    ```

---
### 114. 단일 스레드에서 비동기 처리 방법
- **이벤트 루프 활용**
    ```jsx
    
    const fs = require("fs");
    console.log("파일 읽기 시작");
    fs.readFile("example.txt", "utf8", (err, data) => {
      if (err) throw err;
      console.log("파일 읽기 완료:", data);
    });
    console.log("다른 작업 수행 중...");
    
    ```

- **Async/Await 활용**
    ```jsx
    const fs = require("fs/promises");
    async function readFile() {
      console.log("파일 읽기 시작");
      const data = await fs.readFile("example.txt", "utf8");
      console.log("파일 읽기 완료:", data);
    }
    readFile();
    console.log("다른 작업 수행 중...");
    
    ```

---
### 115. 기타 개념
- **Event-Driven Programming**
    - 이벤트(예: 클릭, 요청) 발생 시 특정 로직을 실행하는 방식
- **SQL Injection 방지 방법**
    - **Prepared Statement** 사용
    - **ORM** 활용 (Sequelize, TypeORM 등)
    - **입력값 검증**
- **ORM과 SQL의 관계**
    - ORM은 데이터베이스 조작을 쉽게 하지만, **SQL 지식이 있어야 최적화 가능**

- **RDBMS vs NoSQL 비교**0
    ![[Pasted image 20250919180808.png]]
    
- **세션 vs 쿠키 vs 토큰**
![[Pasted image 20250919180730.png]]

- **HTTP vs HTTPS**
   ![[Pasted image 20250919180557.png]]

- **[Google.com](http://Google.com) 입력 후 동작 과정**
    
    1. **DNS 조회** → IP 주소 획득
    2. **TCP 연결** (3-way handshake)
    3. **HTTPS 요청** (SSL/TLS 암호화)
    4. **서버 응답** → 브라우저가 렌더링

	![[web-request-flow.png]]
- **스택 vs 큐 비교**
  ![[Pasted image 20250919180503.png]]
- **리팩토링 (Refactoring)**
    - 중복 제거, 코드 간결화, 성능 최적화
- **객체지향 프로그래밍 (OOP) 목표**  
    - 코드 재사용 (상속)
    - 유지보수 용이 (캡슐화)
    - 확장성 (다형성)
- **SQL JOIN 종류**
    ![[Pasted image 20250919180429.png]]
    
- **ORM이란?**
    - SQL 없이 객체 지향 방식으로 데이터베이스 조작 가능
    - 예: Sequelize, TypeORM
- **JavaScript 개념**
    
    - **Hoisting**: 변수 및 함수 선언이 코드 최상단으로 이동
    
    ```jsx
    console.log(a); // undefined
    var a = 10;
    ```
    
    - **TDZ (Temporal Dead Zone)**: `let`과 `const`는 선언 전에 접근 불가
    
    ```jsx
    console.log(a); // ReferenceError
    let a = 10;
    ```
    
    - **Closure (클로저)**: 내부 함수가 외부 함수의 변수를 유지
    
    ```jsx
    function outer() {
      let count = 0;
      return function inner() {
        count++;
        return count;
      };
    }
    
    const counter = outer();
    console.log(counter()); // 1
    console.log(counter()); // 2
    
    ```

- **Node.js vs Express.js**
    
    - **Node.js**: JavaScript 런타임
    - **Express.js**: Node.js 기반 웹 프레임워크

- **DI (Dependency Injection, 의존성 주입)**
    - 객체 간 결합도를 낮추어 유지보수성을 높이는 패턴

- **OOP vs FP 비교**
	![[Pasted image 20250919180356.png]]

- ### CORS 설명

	- CORS(Cross-Origin Resource Sharing)는 브라우저 보안 정책인 **SOP(Same-Origin Policy)** 때문에 다른 출처(도메인/포트/프로토콜)의 자원 접근을 제한하는 것을 풀어주는 방법입니다.
    
	- 서버에서 **`Access-Control-Allow-Origin` 헤더**를 설정해 특정 출처에서 요청을 허용할 수 있습니다.
    
	- 단순 요청(Simple Request), 프리플라이트 요청(OPTIONS), Credential 요청(쿠키/인증 포함)이 있습니다.

- ### NoSQL Schema-less 설명

	- NoSQL은 **엄격한 스키마가 없음** → 같은 컬렉션에서도 다른 구조의 문서 저장 가능
    
	- 장점: 빠른 개발, 데이터 구조 변경이 자유로움
    
	- 단점: 구조 불일치로 인한 데이터 무결성 관리 어려움

- ### NoSQL 다루면서 난해했던 점

	- 데이터 중복 관리 (정규화가 약함)
    
	- 복잡한 조인(Join)이 어려움 → 애플리케이션 레벨에서 처리 필요
    
	- 인덱스 최적화 안 하면 쿼리 성능 급격히 저하 

- ### 기술 스택 접근 방식

	- “**문제 해결 관점**”으로 접근:
    
	    - 우선 요구사항 → 적합한 기술 → 성능/유지보수 고려 → 선택
        
	- 예: 실시간 채팅 → WebSocket, 높은 트래픽 → Redis 캐싱, 수평 확장 → Kubernetes


- ### 백엔드 담당 역할 및 설계 경험

	- API 설계, DB 모델링, 인증/인가(JWT, RBAC), 배포 자동화, 성능 최적화
    
	- 대규모 서비스 고려 시 → 로드밸런싱, 캐싱 전략, MSA 적용


- ### 최대 서비스 규모 경험
	- 예: **동시 접속자 1,000명 이상 처리 경험**
	- Redis 캐싱, DB 샤딩, 로드밸런서 활용으로 성능 최적화

- ### CI/CD 활용 여부

	- GitHub Actions, GitLab CI, Jenkins 경험
	- AWS (EC2, S3, RDS, Lambda), GCP, Azure 활용
	- 자동 빌드 → 테스트 → 배포 파이프라인 구축 경험

- ### 비개발 직군과 협업 방식
	- 비개발 직군(기획, 디자이너, QA)과는 **문서화 + 협업 툴(Jira, Confluence, Notion, Slack)** 로 해결
	- 개발자는 기술 용어 대신 **업무 언어로 변환해서 설명**하는 것 중요

- ### Express.js vs NestJS 차이점
	- **Express**: 최소한의 프레임워크, 자유도 높음, 빠른 프로토타입
	- **NestJS**: Angular 스타일 구조, 모듈화, DI(의존성 주입), 대규모 프로젝트에 적합
	- 장점 비교
	    - Express → 단순, 가볍고 빠름
	    - NestJS → 구조적, 유지보수/테스트 용이
	- 단점 비교
	    - Express → 규모 커질수록 구조 관리 어려움
	    - NestJS → 초기 러닝커브 있음

- ### MSA(Microservice Architecture) 설명 + 장단점
	- **MSA**: 서비스들을 독립적으로 분리(회원, 결제, 주문 등) → API/메시지로 통신
	    - 장점: 독립적 배포, 확장성, 장애 격리, 기술 스택 다양성
	    - 단점: 설계 복잡성, 배포/테스트 어려움, 분산 트랜잭션 관리 어려움

- 참고 사이트
	https://hazel-developer.tistory.com/145
	
	https://medium.com/@vdongbin/node-js-%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC-single-thread-event-driven-non-blocking-i-o-event-loop-ce97e58a8e21
	
	https://medium.com/@vdongbin/javascript-%EC%9E%91%EB%8F%99%EC%9B%90%EB%A6%AC-single-thread-event-loop-asynchronous-e47e07b24d1c
- 유튜브
	[클로저](https://youtu.be/6Ixyltr8_R0?si=yWEhsBPo-T9o3QF9)