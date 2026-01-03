# ✅ TypeScript 기초 문법 정리

## 📘 목차

1. [[#1. 문자열 (string)]]
2. [[#2. 숫자 (number)]]
3. [[#3. 불린 (boolean)]]
4. [[#4. null / undefined]]
5. [[#5. 배열 (Array)]]
6. [[#6. 객체 (Object & Interface)]]
7. [[#7. 함수 (Function)]]
8. [[#8. any / unknown]]
9. [[#9. 튜플 (Tuple)]]
10. [[#10. void / never]]
11. [[#11. 유니언 타입 (Union)]]
12. [[#12. 인터섹션 타입 (Intersection)]]

---

## 1. 문자열 (string)
```
let str: string; 
let red: string = "Red"; 
let green: string = "Green"; 
let myColor: string = `My Color is ${red}.`; 
let yourColor: string = 'Your color is ' + green;
```

---
## 2. 숫자 (number)
```
let num: number; 
let integer: number = 6; 
let float: number = 3.14; 
let infinity: number = Infinity; 
let nan: number = NaN;
```

---

## 3. 불린 (boolean)
```
let isBoolean: boolean; 
let isDone: boolean = false;
```
---
## 4. null / undefined
```
let nul: null = null;
let und: undefined; 
console.log(nul); 
console.log(und);
```
---
## 5. 배열 (Array)
```
const fruits: string[] = ['Apple', 'Banana', 'Cherry']; 
const numbers: number[] = [1, 2, 3, 4, 5, 6]; 
const union: (string | number)[] = ['Apple', 1, 2, 'Banana'];
```
---
## 6. 객체 (Object & Interface)
```
const obj: object = {}; 
const arr: object = []; 
const func: object = function () {};  
interface User {   name: string;   age: number;   isValid: boolean; }  
const userA: User = {   name: 'Heropy',   age: 85,   isValid: true };
```
---
## 7. 함수 (Function)
```
const add: (x: number, y: number) => number = function (x: number, y: number): number { 
return x + y; 
}

const add = function (x: number, y: number): number 
{   
return x + y; 
};  

const hello = function (): void {   
console.log('hello world~'); 
};  

const hello: () => void = function() {
console.log(`hello ${msg}`);
}

function hello(msg: string): void {   
console.log(`hello ${msg}`); 
}
```
---

## 8. any / unknown
```
let hello: any = 'Hello world';
hello = 123; hello = false; 
const u: unknown = 123; 
const a: any = u; 
```
---
## 9. 튜플 (Tuple)
```
const tuple: [string, number, boolean] = ['a', 1, false];  
const users: [number, string, boolean][] = [   [1, 'Neo', true],   [2, 'Evan', false],   [3, 'Lewis', true] ];
```
---
## 10. void / never
```
function hello(msg: string): void {   console.log(`hello ${msg}`); }  const nev: never[] = []; // nev.push(3); // ❌ never에는 값을 넣을 수 없음
```

---
## 11. 유니언 타입 (Union)
```
let union: string | number | boolean; 
union = 'hello type!'; 
union = 123; union = false;
```

---
## 12. 인터섹션 타입 (Intersection)
```
interface User {   name: string;   age: number; } 
interface Validation {   isValid: boolean; } 
const heropy: User & Validation = {   name: "Neo",   age: 85,   isValid: false };
```