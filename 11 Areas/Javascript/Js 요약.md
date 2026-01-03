# 📖- JavaScript Learning Guide

## 📜 목차
## Table of Contents

- [[#**JavaScript Syntax and Basic Operations**]]
    
    - [[Variables and Data Types]]
        
    - [[Operators]]
        
- [[DOM Manipulation]]
    
    - [[#**1. 요소 선택**]]
        
    - [[#**2. 콘텐츠 수정**]]
        
    - [[#**3. 이벤트 처리**]]
        
- [[#**Immediately Invoked Function Expressions (IIFE)즉시 실행 함수 표현식**]]
    
- [[#**Scopes in JavaScript**]]
    
    - [[Global Scope]]
        
    - [[Function Scope]]
        
    - [[Block Scope]]
        
- [[#**호이스팅(Hoisting)**]]
    
- [[#**클로저(Closures)**]]
    
- [[#**콜백(Callbacks)**]]
    
- [[#**프로미스(Promises)**]]
    
- [[#**Async/Await**]]

- [[#**BlackJack Game**| BlackJack]]
---

## **JavaScript Syntax and Basic Operations**
[코드팩토리 js 2025](https://youtu.be/ZOVG7_41kJE?si=tgb8t8SWusmsTyT9)

## **1. 문법 기초**
- JavaScript 문법은 코드가 작성되고 해석되는 방식을 정의합니다.
**예제:**
```run-javascript
console.log('Hello, world!');
```

## **2. 변수와 데이터 타입**
- `let`, `const`, 또는 `var`를 사용하여 변수를 선언합니다.
```
Var(함수레벨 스코프) : 중복 선언과 재할당이 가능합니다.
let(블록 스코프) : 중복선언은 불가하며 재할당은 가능합니다.
const(블록 스코프) : 중복선언과 재할당 둘 다 불가합니다.
```

- 일반적인 데이터 타입: 문자열(String), 숫자(Number), 불리언(Boolean), Null, Undefined, BigInt, Symbol.

**예제:**
```run-javascript title:'변수와 데이터타입'
//변수와 데이터타입
let name = 'Alice';
const PI = 3.14; 
let isActive = true;
console.log(name, PI, isActive)
```

## **3. 연산자**
- 산술 연산자: `+`, `-`, `*`, `/`, `%`
- 할당 연산자: `=`, `+=`, `-=`
- 비교 연산자: `==`, `===`, `<`, `>`

**예제:**
```run-javascript title:'연산자'
let x = 5; 
console.log(x + 3);
``` 

## **window 객체**


## **DOM Manipulation**

DOM(Document Object Model)은 JavaScript가 HTML 요소와 동적으로 상호작용할 수 있도록 합니다.
## **1. 요소 선택**
- 메서드: `getElementById`, `querySelector` 등.
**예제:**
```run-javascript title:'요소'
//html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>DOM Example</title>
</head>
<body>
  <h1 id="myElement">hello</h1>
   <script src="index.js"> </script>
</body>
</html>

//js
const element = document.getElementById('myElement');
console.log(element)
```

## **2. 콘텐츠 수정**
- 텍스트 또는 속성 변경:
**예제:**
```
element.textContent = "새로운 텍스트"; element.setAttribute('class', 'new-class');
```

## **3. 이벤트 처리**
- 상호작용 추가:
**예제:**
```
element.addEventListener('click', () => alert('클릭됨!'));
```

## **Immediately Invoked Function Expressions (IIFE)즉시 실행 함수 표현식**
IIFE는 정의되자마자 즉시 실행되는 함수입니다.
**예제:**
```
function() {   
console.log('IIFE 실행됨!'); 
};
```

**장점:**
- 전역 스코프 오염 방지.
- 비공개 변수 생성.

## **Scopes in JavaScript**
스코프는 변수의 가시성을 결정합니다.
## **스코프 유형**:
1. **전역 스코프(Global Scope)**: 어디서나 접근 가능.
2. **함수 스코프(Function Scope)**: 함수 내부에서 선언된 변수는 해당 함수에만 접근 가능.
3. **블록 스코프(Block Scope)**: `{}` 내에서 `let` 또는 `const`로 선언된 변수는 블록 내에서만 접근 가능.

**예제:**
`{   let blockScoped = '여기에서만 접근 가능'; } console.log(blockScoped); // 에러 발생`

## **호이스팅(Hoisting)**
호이스팅은 실행 중 선언을 해당 스코프의 최상단으로 이동시킵니다.
**예제:**
```run-javascript title:'호이스팅'
console.log(x); // 호이스팅으로 인해 
undefined 
출력 var x = 5;```
```
**참고:** `let` 또는 `const`로 선언된 변수는 초기화되지 않으므로 호이스팅 시 에러가 발생합니다.

## **클로저(Closures)**
클로저는 부모 함수가 종료된 후에도 부모 스코프에 대한 접근을 유지하는 함수입니다.

**예제:**
```run-javascript title:'클로저'
function outer() {   
let count = 0;  
return count;
function inner() {    
count++;    
return count;  }; 
} 
const increment = outer(); 
console.log(increment); 
// 1 console.log(increment()); // 2
```

**사용 사례:**
- 비공개 변수 생성.
- 호출 간 상태 유지.

## **콜백(Callbacks)**
콜백은 다른 함수에 인수로 전달되는 함수입니다.
**예제:**
```run-javascript title:'콜백'
function greet(name, callback) {   
console.log(`안녕하세요, ${name}`);  
callback(); 
} 
greet('Alice', () => console.log('콜백 실행됨!'));
```
**사용 사례:** 이벤트 처리 또는 API 호출과 같은 비동기 작업.

## **프로미스(Promises)**
프로미스는 값이 지금, 나중에, 혹은 절대 제공되지 않을 수도 있는 비동기 작업을 처리합니다.

**구문(Syntax):**
```run-javascript title:'프로미스'
let promise = new Promise((resolve, reject) => {     
  let success = true;    
  if (success) resolve("성공");    
  else reject("오류"); 
}); 

promise
  .then(value => console.log(value))
  .catch(err => console.error(err));

```

## **Async/Await**
Async/await는 프로미스를 사용한 비동기 코드를 동기 코드처럼 보이게 단순화합니다.
**예제:**
```run-javascript title:'비동기'
async function fetchData() {     
const response = await fetch('https://api.example.com/data');    
const data = await response.json();    
console.log(data); 
} 
fetchData();
```

**핵심 포인트:**
- 함수 선언 앞에 `async`를 사용합니다.
- 프로미스가 해결될 때까지 실행을 중단하려면 `await`를 사용합니다.
## **요약 테이블**

|개념|주요 특징|예제|
|---|---|---|
|문법|JavaScript 코드 작성 규칙|`console.log('Hello!');`|
|DOM 조작|HTML 요소와 상호작용|`document.getElementById('id').textContent = 'Hello';`|
|IIFE|즉시 실행|`(function(){})();`|
|스코프|변수 가시성 정의|`{ let x = 'block'; }`|
|호이스팅|선언을 최상단으로 이동|`console.log(a); var a = 5;`|
|클로저|부모 스코프에 대한 접근 유지|`function outer(){ return inner; }`|
|콜백|인수로 전달된 함수|`setTimeout(() => console.log('Done'), 1000);`|
|프로미스|비동기 작업 처리|`.then()` 및 `.catch()`|
|Async/Await|프로미스를 단순화|`async function(){ await promise; }`|


## **BlackJack Game**

**예제:**
``` run-javascript
// 카드 배열과 합계, 게임 상태를 나타내는 변수들 초기화

let cards = []; // 현재 손에 있는 카드들

let sum = 0; // 카드 합계

let hasBlackJack = false; // 블랙잭 여부

let isAlive = false; // 플레이어가 게임 중인지 여부

let message = ""; // 사용자에게 보여줄 메시지

  

// DOM 요소를 가져오는 변수들

const messageEl = document.getElementById("message-el"); // 메시지를 표시할 요소

const sumEl = document.getElementById("sum-el"); // 합계를 표시할 요소

const cardsEl = document.getElementById("cards-el"); // 카드 목록을 표시할 요소

const playerEl = document.getElementById("player-el"); // 플레이어 정보를 표시할 요소

  

// 플레이어 정보 객체

const player = {

  name: "Per", // 플레이어 이름

  chips: 145, // 플레이어가 가진 칩 수

  sayHello() {

    console.log("Heisann!"); // 플레이어가 인사하는 함수

  },

};

  

// 초기화 함수

function initializeGame() {

  playerEl.textContent = `${player.name}: $${player.chips}`; // 플레이어 이름과 칩 정보를 화면에 표시

  player.sayHello(); // 플레이어가 인사

}

  

// 랜덤 카드를 생성하는 함수

function getRandomCard() {

  const randomNumber = Math.floor(Math.random() * 13) + 1; // 1부터 13까지의 랜덤 숫자 생성

  if (randomNumber > 10) return 10; // J, Q, K는 10으로 처리

  if (randomNumber === 1) return 11; // Ace는 11로 처리

  return randomNumber; // 나머지는 그대로 반환

}

  

// 게임을 시작하는 함수

function startGame() {

  isAlive = true; // 게임 상태를 활성화

  hasBlackJack = false; // 블랙잭 상태 초기화

  cards = [getRandomCard(), getRandomCard()]; // 초기 카드 2장 생성

  sum = cards.reduce((acc, card) => acc + card, 0); // 카드 합계 계산

  renderGame(); // 게임 상태를 화면에 렌더링

}

  

// 게임 상태를 화면에 렌더링하는 함수

function renderGame() {

  cardsEl.textContent = `Cards: ${cards.join(" ")}`; // 카드 목록 표시

  sumEl.textContent = `Sum: ${sum}`; // 합계 표시

  

  if (sum < 21) {

    message = "Do you want to draw a new card?"; // 21 미만일 때 메시지

  } else if (sum === 21) {

    message = "Wohoo! You've got a Blackjack!"; // 블랙잭일 때 메시지

    hasBlackJack = true; // 블랙잭 상태 업데이트

  } else {

    message = "You're out of the game!"; // 21 초과일 때 메시지

    isAlive = false; // 게임 상태 비활성화

  }

  

  messageEl.textContent = message; // 메시지 화면에 표시

}

  

// 새로운 카드를 추가하는 함수

function newCard() {

  if (isAlive && !hasBlackJack) {

    const card = getRandomCard(); // 새로운 카드 생성

    cards.push(card); // 카드 배열에 추가

    sum += card; // 합계에 추가

    renderGame(); // 게임 상태를 다시 렌더링

  }

}

  

// 게임 상태를 초기화하는 함수

function resetGame() {

  cards = [];

  sum = 0;

  hasBlackJack = false;

  isAlive = false;

  message = "Game reset. Ready to play?";

  renderGame();

  messageEl.textContent = message;

}

  

// 초기화 함수 호출

initializeGame();
```

```
/* 전체 페이지 스타일 */

body {

    font-family: 'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif; /* 기본 글꼴 설정 */

    background-image: url("table.jpg"); /* 배경 이미지 설정 */

    background-size: cover; /* 배경 이미지를 화면에 맞게 조정 */

    text-align: center; /* 텍스트를 중앙 정렬 */

    color: white; /* 텍스트 색상을 흰색으로 설정 */

    font-weight: bold; /* 텍스트를 굵게 설정 */

}

  

/* 제목 스타일 */

h1 {

    color: goldenrod; /* 제목 색상을 황금색으로 설정 */

}

  

/* 메시지 표시 요소 스타일 */

#message-el {

    font-style: italic; /* 텍스트를 기울임꼴로 설정 */

}

  

/* 버튼 스타일 */

button {

    color: #016f32; /* 버튼 텍스트 색상을 녹색으로 설정 */

    width: 150px; /* 버튼 너비 설정 */

    background-color: goldenrod; /* 버튼 배경색을 황금색으로 설정 */

    padding-top: 5px; /* 버튼 상단 여백 */

    padding-bottom: 5px; /* 버튼 하단 여백 */

    font-weight: bold; /* 버튼 텍스트를 굵게 설정 */

    border: none; /* 버튼 테두리를 제거 */

    border-radius: 2px; /* 버튼 모서리를 약간 둥글게 설정 */

    margin-bottom: 4px; /* 버튼 하단 여백 */

    margin-top: 4px; /* 버튼 상단 여백 */

}
```

```<!DOCTYPE html>

<html lang="en">

<head>

   <!-- 외부 CSS 파일 연결 -->

   <link rel="stylesheet" href="index.css">

</head>

<body>

    <!-- 게임 제목 -->

    <h1>BlackJect</h1>

  

    <!-- 사용자에게 메시지를 표시할 요소 -->

    <p id="message-el">Want to play a round?</p>

  

    <!-- 현재 카드 목록을 표시할 요소 -->

    <p id="cards-el">Cards: </p>

  

    <!-- 합계를 표시할 요소 -->

    <!-- <p class="sum-el">Sum: </p> --> <!-- 주석 처리된 클래스 기반 요소 -->

    <p id="sum-el">Sum: </p>

  

    <!-- 게임 시작 버튼 -->

    <button onclick="startGame()">START GAME</button>

    <br>

  

    <!-- 새로운 카드 추가 버튼 -->

    <button onclick="newCard()">NEW CARD</button>

    <br>

  

     <!-- 게임 초기화 버튼 -->

     <button onclick="resetGame()">RESET GAME</button>

     <br>

    <!-- 플레이어 정보를 표시할 요소 -->

    <p id="player-el"></p>

  

    <!-- JavaScript 파일 연결 -->

    <script src="index.js"> </script>

</body>

</html>
```

- 출처
	- [클로저](https://youtu.be/6Ixyltr8_R0?si=q4V13hAtdHrgNwb-)
	- 