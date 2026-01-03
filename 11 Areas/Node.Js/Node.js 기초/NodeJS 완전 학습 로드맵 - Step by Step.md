# Node.js 패턴과 개념 정리

## 1. Node.js 플랫폼

### 1-1 Node.js 철학

- 단일 스레드, 논블로킹 I/O, 이벤트 기반 아키텍처로 높은 처리량과 확장성 제공

### 1-2 Node.js 작동 방식

- V8 JavaScript 엔진 기반, libuv를 통해 이벤트 루프와 비동기 I/O 처리
- 예시:
    
    ```js
    const fs = require('fs');fs.readFile('file.txt', 'utf8', (err, data) => {  if (err) throw err;  console.log(data);});
    ```
    

### 1-3 Node.js에서의 JavaScript

- ES6+ 기능 지원, 비동기 제어 흐름을 위한 콜백/Promise/Async-Await 활용

## 2. 모듈 시스템

### 2-1 모듈의 필요성

- 코드 재사용, 네임스페이스 충돌 방지, 유지보수 용이성 제공

### 2-2 JavaScript와 Node.js에서의 모듈 시스템

- Node.js는 CommonJS, 최신 JS는 ESM 사용

### 2-3 모듈 시스템과 패턴

- 다양한 패턴을 통해 모듈의 구조화 가능 (예: Singleton, Factory)

### 2-4 CommonJS 모듈

```js
// math.js
module.exports.add = (a, b) => a + b;

// main.js
const math = require('./math');
console.log(math.add(1, 2));
```

### 2-5 모듈 정의 패턴

- IIFE(즉시 실행 함수 표현식) 등을 통한 모듈화 구현

### 2-6 ESM: ECMAScript 모듈

```js
// math.mjs
export function add(a, b) { return a + b; }

// main.mjs
import { add } from './math.mjs';
console.log(add(1, 2));
```

### 2-7 ESM과 CommonJS의 차이점과 상호 운용

- 정적 vs 동적 로딩, `import`/`export` vs `require`/`module.exports` 방식의 차이

## 3. 콜백과 이벤트

### 3-1 콜백 패턴

```js
function asyncOperation(callback) {
  setTimeout(() => callback(null, 'Done!'), 1000);
}

asyncOperation((err, result) => {
  if (err) throw err;
  console.log(result);
});
```

### 3-2 관찰자 패턴

```js
const EventEmitter = require('events');
const emitter = new EventEmitter();

emitter.on('event', () => console.log('Event occurred'));
emitter.emit('event');
```

## 4. 콜백을 사용한 비동기 제어 흐름 패턴

### 4-1 비동기 프로그래밍의 어려움

- 콜백 지옥, 에러 처리 복잡성 등의 문제점

### 4-2 콜백 모범 사례와 제어 흐름 패턴

- 에러 우선 콜백, 순차 실행, 병렬 실행 패턴 등 활용

### 4-3 비동기 라이브러리

- `async`, `bluebird` 등의 라이브러리 활용

```js
const async = require('async');
async.series([
  cb => setTimeout(() => cb(null, 'one'), 1000),
  cb => setTimeout(() => cb(null, 'two'), 500)
], (err, results) => {
  console.log(results); // ['one', 'two']
});
```

## 5. Promise와 Async/Await를 이용한 비동기 제어 흐름 패턴

### 5-1 프라미스(Promise)

```js
function asyncOperation() {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve('Done!'), 1000);
  });
}

asyncOperation().then(console.log);
```

### 5-2 Async/await

```js
async function run() {
  try {
    const result = await asyncOperation();
    console.log(result);
  } catch (err) {
    console.error(err);
  }
}
run();
```

### 5-3 무한 재귀 프라미스 해결 체인의 문제

- Promise 체인이 깊어지면 스택 오버플로우나 메모리 누수 위험 발생

## 6. 스트림 코딩

### 6-1 스트림의 중요성

- 메모리 사용을 최소화하며 큰 데이터를 효율적으로 처리 가능

### 6-2 스트림 시작하기

```js
const fs = require('fs');
const stream = fs.createReadStream('large.txt');
stream.on('data', chunk => console.log(chunk));
```

### 6-3 스트림을 사용한 비동기 제어 흐름 패턴

- 데이터 이벤트 기반의 처리로 제어 흐름 관리

### 6-4 파이핑(Piping) 패턴

```js
const zlib = require('zlib');
const inp = fs.createReadStream('input.txt');
const out = fs.createWriteStream('output.txt.gz');

inp.pipe(zlib.createGzip()).pipe(out);
```

## 7. 생성자 디자인 패턴

### 7-1 팩토리

```js
function createUser(name) {
  return { name, role: 'user' };
}
```

### 7-2 빌더

```js
class UserBuilder {
  constructor() { this.user = {}; }
  setName(name) { this.user.name = name; return this; }
  setRole(role) { this.user.role = role; return this; }
  build() { return this.user; }
}
```

### 7-3 공개 생성자

```js
class User {
  constructor(name, role) {
    this.name = name;
    this.role = role;
  }
}
```

### 7-4 싱글톤

```js
const Singleton = (function () {
  let instance;
  function createInstance() {
    return { value: 'I am the instance' };
  }
  return {
    getInstance: function () {
      if (!instance) instance = createInstance();
      return instance;
    }
  };
})();
```

### 7-5 모듈 와이어링(Wiring)

- 의존성 주입(DI) 패턴을 통해 모듈을 동적으로 연결

## 8. 구조적 설계 패턴

### 8-1 프록시

- 실제 객체에 대한 접근을 제어하는 객체를 제공하는 패턴
- 리모트 객체나 권한 관리가 필요한 경우에 사용

```js
class RealSubject {
  request() {
    console.log("RealSubject: Handling request.");
  }
}

class Proxy {
  constructor(realSubject) {
    this.realSubject = realSubject;
  }
  request() {
    console.log("Proxy: Checking access before forwarding the request.");
    this.realSubject.request();
  }
}

const realSubject = new RealSubject();
const proxy = new Proxy(realSubject);
proxy.request();
```

### 8-2 데코레이터

- 객체에 새로운 기능을 동적으로 추가하는 패턴
- 원래 객체를 수정하지 않고 기능을 확장

```js
class Coffee {
  cost() {
    return 5;
  }
}

class MilkDecorator {
  constructor(coffee) {
    this.coffee = coffee;
  }
  cost() {
    return this.coffee.cost() + 2;
  }
}

const coffee = new Coffee();
console.log(coffee.cost()); // 5
const milkCoffee = new MilkDecorator(coffee);
console.log(milkCoffee.cost()); // 7
```

### 8-3 프록시와 데코레이터의 차이점

- 프록시는 접근 제어에, 데코레이터는 기능 확장에 중점

### 8-4 어댑터

- 호환되지 않는 인터페이스를 연결해주는 패턴

```js
class OldSystem {
  oldMethod() {
    console.log("Old system method");
  }
}

class NewSystemAdapter {
  constructor(oldSystem) {
    this.oldSystem = oldSystem;
  }
  newMethod() {
    this.oldSystem.oldMethod();
  }
}

const oldSystem = new OldSystem();
const adapter = new NewSystemAdapter(oldSystem);
adapter.newMethod(); // "Old system method"
```

## 9. 행위 디자인 패턴

### 9-1 전략 패턴

- 알고리즘을 캡슐화하여 독립적으로 선택할 수 있게 하는 패턴

```js
class Context {
  constructor(strategy) {
    this.strategy = strategy;
  }
  execute(a, b) {
    return this.strategy.calculate(a, b);
  }
}

class Addition {
  calculate(a, b) {
    return a + b;
  }
}

class Subtraction {
  calculate(a, b) {
    return a - b;
  }
}

const context = new Context(new Addition());
console.log(context.execute(5, 3)); // 8
context.strategy = new Subtraction();
console.log(context.execute(5, 3)); // 2
```

### 9-2 상태(State) 패턴

- 객체의 상태에 따라 행동을 다르게 하는 패턴

```js
class Context {
  constructor() {
    this.state = new ConcreteStateA();
  }
  setState(state) {
    this.state = state;
  }
  request() {
    this.state.handle();
  }
}

class State {
  handle() {}
}

class ConcreteStateA extends State {
  handle() {
    console.log("Handling state A");
  }
}

class ConcreteStateB extends State {
  handle() {
    console.log("Handling state B");
  }
}

const context = new Context();
context.request(); // "Handling state A"
context.setState(new ConcreteStateB());
context.request(); // "Handling state B"
```

### 9-3 템플릿 메소드 패턴

- 알고리즘의 구조는 정의하고 세부 단계는 하위 클래스에서 구현

```js
class AbstractClass {
  templateMethod() {
    this.step1();
    this.step2();
  }
  step1() {}
  step2() {}
}

class ConcreteClass extends AbstractClass {
  step1() {
    console.log("Step 1 implementation");
  }
  step2() {
    console.log("Step 2 implementation");
  }
}

const concrete = new ConcreteClass();
concrete.templateMethod();
```

### 9-4 반복자(Iterator) 패턴

- 컬렉션의 내부 구현을 노출하지 않고 요소에 접근하는 방법 제공

```js
class Iterator {
  constructor(items) {
    this.items = items;
    this.index = 0;
  }
  next() {
    return this.index < this.items.length
      ? this.items[this.index++]
      : null;
  }
}

const iterator = new Iterator([1, 2, 3]);
console.log(iterator.next()); // 1
console.log(iterator.next()); // 2
console.log(iterator.next()); // 3
```

### 9-5 미들웨어

- 요청-응답 주기에서 중간 처리 역할을 하는 패턴

```js
const express = require('express');
const app = express();

app.use((req, res, next) => {
  console.log('Middleware: Request received');
  next();
});

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(3000);
```

### 9-6 명령(Command) 패턴

- 요청을 객체로 캡슐화하여 요청자와 수신자를 분리

```js
class Command {
  execute() {}
}

class LightOnCommand extends Command {
  constructor(light) {
    super();
    this.light = light;
  }
  execute() {
    this.light.turnOn();
  }
}

class Light {
  turnOn() {
    console.log("Light is On");
  }
}

class RemoteControl {
  pressButton(command) {
    command.execute();
  }
}

const light = new Light();
const lightOnCommand = new LightOnCommand(light);
const remote = new RemoteControl();
remote.pressButton(lightOnCommand); // "Light is On"
```

## 10. 웹 애플리케이션을 위한 범용 JavaScript

### 10-1 브라우저와 코드 공유

- 브라우저와 서버에서 동일한 JavaScript 코드 공유 방법

### 10-2 크로스 플랫폼 개발의 기초

- 다양한 환경에서 동작하는 애플리케이션 개발 기법

### 10-3 React 개요

- UI를 구성하는 컴포넌트 기반 라이브러리

```js
class App extends React.Component {
  render() {
    return <h1>Hello, World!</h1>;
  }
}
```

### 10-4 범용 JavaScript 앱 만들기

- 서버와 클라이언트에서 동일 코드로 실행되는 앱 개발

```js
// 서버 측 Node.js 코드
app.get('/', (req, res) => {
  res.send('<div id="root"></div>');
});

// 클라이언트 측 React 코드
ReactDOM.hydrate(<App />, document.getElementById('root'));
```

## 11. 고급 레시피

### 11-1 비동기적으로 초기화되는 컴포넌트

- 비동기 초기화가 필요한 컴포넌트를 처리하는 방법

```js
class AsyncComponent extends React.Component {
  state = { data: null };
  
  componentDidMount() {
    fetchData().then(data => this.setState({ data }));
  }
  
  render() {
    const { data } = this.state;
    return data ? <div>{data}</div> : <div>Loading...</div>;
  }
}
```

### 11-2 비동기식 요청 일괄 처리 및 캐싱

- 여러 비동기 요청을 병렬 처리하고 결과를 캐시하는 방법

```js
let cache = {};

async function fetchData(url) {
  if (cache[url]) {
    return cache[url];
  }
  const response = await fetch(url);
  const data = await response.json();
  cache[url] = data;
  return data;
}

async function loadData() {
  const result1 = fetchData('/api/endpoint1');
  const result2 = fetchData('/api/endpoint2');
  const results = await Promise.all([result1, result2]);
  console.log(results);
}
```

### 11-3 비동기 작업 취소

- 진행 중인 비동기 작업을 취소하는 방법

```js
const controller = new AbortController();
const signal = controller.signal;

fetch('/api/data', { signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(err => console.error('Request was aborted', err));

// 요청 취소 방법
// controller.abort();
```

### 11-4 CPU 바운드 작업 실행

- CPU 집약적 작업을 메인 스레드와 분리해 비동기적으로 실행

```js
const { Worker, isMainThread, parentPort } = require('worker_threads');

if (isMainThread) {
  const worker = new Worker(__filename);
  worker.on('message', result => console.log('Result:', result));
  worker.postMessage(10);
} else {
  parentPort.on('message', num => {
    let result = num * 2; // CPU 집약적인 계산
    parentPort.postMessage(result);
  });
}
```

## 12. 확장성과 아키텍처 패턴

### 12-1 애플리케이션 확장 소개

- 애플리케이션을 확장 가능한 방식으로 설계하는 방법
- 분산 시스템, 마이크로서비스, 확장 가능한 DB 아키텍처 등

### 12-2 복제 및 로드 밸런싱

- 여러 서버에 작업을 분배해 시스템 부하 분산 방법
- 로드 밸런서를 통한 트래픽 분산 방식

### 12-3 복잡한 애플리케이션 분해

- 복잡한 앱을 여러 모듈이나 서비스로 분해하는 방법
- 모듈화 및 마이크로서비스 아키텍처 적용 방식

## 13. 메시징과 통합 패턴

### 13-1 메시징 시스템의 기초

- 분산 시스템에서의 메시지 큐나 이벤트 기반 통합 방식

```js
const amqp = require('amqplib/callback_api');
amqp.connect('amqp://localhost', (err, conn) => {
  conn.createChannel((err, ch) => {
    const queue = 'task_queue';
    ch.assertQueue(queue, { durable: true });
    ch.sendToQueue(queue, Buffer.from('Hello World!'), { persistent: true });
    console.log('Sent: Hello World!');
  });
});
```

### 13-2 발행/구독 패턴

- 메시지 발행자가 메시지를 발행하고 구독자가 수신하는 패턴

```js
const amqp = require('amqplib/callback_api');
amqp.connect('amqp://localhost', (err, conn) => {
  conn.createChannel((err, ch) => {
    const exchange = 'logs';
    ch.assertExchange(exchange, 'fanout', { durable: false });
    ch.assertQueue('', { exclusive: true }, (err, q) => {
      ch.bindQueue(q.queue, exchange, '');
      ch.consume(q.queue, (msg) => {
        console.log('Received:', msg.content.toString());
      }, { noAck: true });
    });
  });
});
```

### 13-3 작업 배포(Task distribution) 패턴

- 작업을 여러 소비자에게 분배하여 병렬 처리하는 패턴

```js
const amqp = require('amqplib/callback_api');
amqp.connect('amqp://localhost', (err, conn) => {
  conn.createChannel((err, ch) => {
    const queue = 'task_queue';
    ch.assertQueue(queue, { durable: true });
    ch.prefetch(1);  // 한 번에 하나의 작업만 처리
    ch.consume(queue, (msg) => {
      console.log('Received:', msg.content.toString());
      ch.ack(msg);  // 작업 완료 후 확인 메시지 전송
    });
  });
});
```

### 13-4 요청/응답(Request/Reply) 패턴

- 클라이언트가 요청을 보내고 서버가 응답을 보내는 패턴

```js
const amqp = require('amqplib/callback_api');
amqp.connect('amqp://localhost', (err, conn) => {
  conn.createChannel((err, ch) => {
    const corr = generateUuid();  // 고유 ID 생성
    ch.assertQueue('', { exclusive: true }, (err, q) => {
      ch.consume(q.queue, (msg) => {
        if (msg.properties.correlationId === corr) {
          console.log('Received:', msg.content.toString());
        }
      }, { noAck: true });
      ch.sendToQueue('rpc_queue', Buffer.from('Hello'), {
        correlationId: corr,  // 요청의 고유 ID
        replyTo: q.queue  // 응답을 받을 큐
      });
    });
  });
});

function generateUuid() {
  return Math.floor(Math.random() * 10000);
}
```