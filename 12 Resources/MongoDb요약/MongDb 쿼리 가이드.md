# MongoDB 쿼리 가이드

## 기본 쿼리 (Basic Queries)

sql       nosql
table -> collection
db -> db
raw -> document

### 1. 컬렉션 내 모든 문서 조회

javascript

복사

`db.collection.find()`

### 2. 특정 조건으로 문서 조회

javascript

복사

`// 정확한 일치 
db.users.find({ name: "John" }) 
// 비교 연산자 
db.products.find({ price: { $gt: 50 } })  // 50보다 큰 가격 
db.products.find({ price: { $lt: 100 } })  // 100보다 작은 가격`

### 3. 논리 연산자

javascript

복사

`// AND 연산 
db.users.find({      age: { $gte: 18 },    status: "active"  }) 

// OR 연산 
db.users.find({      $or: [        { status: "active" },        { age: { $lt: 30 } }    ] })`

### 4. 특정 필드 선택

javascript

복사

`// 특정 필드만 조회 
db.users.find(     { status: "active" },    { name: 1, email: 1, _id: 0 } )`

## 중급 쿼리 (Intermediate Queries)

### 1. 정렬

javascript

복사

`// 오름차순 정렬 
db.users.find().sort({ age: 1 }) 
// 내림차순 정렬 
db.users.find().sort({ age: -1 })`

### 2. 페이지네이션

javascript

복사

`// 10개씩 건너뛰고 5개 조회 
db.users.find()     .skip(10)    .limit(5)`

### 3. 배열 쿼리

javascript

복사

`// 배열에 특정 값 포함 
db.users.find({ tags: "developer" }) 
// 배열의 특정 요소 조회 
db.users.find({ "skills.0": "Python" })`

### 4. 정규 표현식

javascript

복사

`// 이름이 'J'로 시작하는 사용자 
db.users.find({ name: /^J/ }) 
// 대소문자 무시 
db.users.find({ name: /john/i })`

## 고급 쿼리 (Advanced Queries)

### 1. 집계 파이프라인

javascript

복사

`db.orders.aggregate([     

// 스테이지 1: 필터링   
{ $match: { status: "completed" } },        

// 스테이지 2: 그룹화 및 집계   
{ $group: {        _id: "$customerId",        totalSpent: { $sum: "$amount" },        averageOrder: { $avg: "$amount" }    }},         

// 스테이지 3: 정렬   
{ $sort: { totalSpent: -1 } },        

// 스테이지 4: 결과 제한    
{ $limit: 5 } ])`

### 2. 복합 인덱스

javascript

복사

`// 복합 인덱스 생성 
db.users.createIndex({      lastName: 1,    firstName: 1  }) 
// 인덱스를 활용한 쿼리 
db.users.find({      lastName: "Smith",    firstName: "John"  })`

### 3. 고급 업데이트 연산자

javascript

복사

`// 배열 업데이트 
db.users.updateOne(     
{ _id: userId },    
{        
$push: { skills: "Docker" },        
$inc: { skillCount: 1 }    
} ) 
// 조건부 업데이트 
db.products.updateMany(     
{ price: { $lt: 100 } },    
{ $mul: { price: 1.1 } }  // 10% 가격 인상 )`

### 4. 윈도우 함수

javascript

복사

`db.sales.aggregate([     
{ $setWindowFields: {        
sortBy: { date: 1 },        
output: {           
cumulativeSales: {                
$sum: "$amount",                
window: {                    
documents: ["unbounded", "current"]              
}           
}       
}    
}} 
])`

## 성능 최적화 팁

1. 인덱스 활용
2. 쿼리 실행 계획 분석 (`explain()` 메서드 사용)
3. 불필요한 필드 projection 제한
4. 대량 데이터 작업시 배치 처리

## 주의사항

- 대규모 컬렉션에서는 쿼리 성능에 주의
- 인덱스 설계에 신중
- 필요 이상의 복잡한 쿼리 지양


-출처-
https://youtu.be/yv6okH9Ig_M?si=5d-zSCcTuKrLEr7s