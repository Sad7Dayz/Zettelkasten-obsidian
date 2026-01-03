
---

## 타입 유효성 검사기 (Type Validators)

이 유효성 검사기들은 값의 근본적인 타입을 확인합니다.

- **`@IsDefined()`**: 값이 **정의되었는지** (`undefined` 또는 `null`이 아닌지) 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsDefined()
        propertyName: any; // propertyName이 값을 가지고 있는지 확인
        ```
        
- **`@IsOptional()`**: 속성을 **선택 사항**으로 표시합니다. 이 속성이 제공되지 않으면, 해당 속성에 대한 다른 유효성 검사기는 확인되지 않습니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsOptional()
        @IsString()
        optionalString?: string; // 이 속성은 생략될 수 있음
        ```
        
- **`@Equals(value: any)`**: 값이 제공된 `value`와 **엄격하게 동일한지** (`===`) 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @Equals('code factory')
        companyName: string; // 반드시 'code factory'여야 함
        ```
        
- **`@NotEquals(value: any)`**: 값이 제공된 `value`와 **엄격하게 동일하지 않은지** (`!==`) 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @NotEquals('admin')
        username: string; // 'admin'이 될 수 없음
        ```
        
- **`@IsEmpty()`**: 값이 **비어 있는지** 확인합니다. 문자열의 경우 빈 문자열 `''`, 배열의 경우 빈 배열 `[]`, 객체의 경우 빈 객체 `{}`를 의미합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsEmpty()
        emptyField: string; // 반드시 ''여야 함
        ```
        
- **`@IsNotEmpty()`**: 값이 **비어 있지 않은지** 확인합니다. `@IsEmpty()`의 반대입니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsNotEmpty()
        requiredField: string; // 반드시 ''가 아니어야 함
        ```
        
- **`@IsIn(values: any[])`**: 값이 제공된 배열의 값 중 **하나인지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsIn(['action', 'fantasy'])
        genre: string; // 'action' 또는 'fantasy'여야 함
        ```
        
- **`@IsNotIn(values: any[])`**: 값이 제공된 배열의 값 중 **하나가 아닌지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsNotIn(['horror', 'thriller'])
        excludedGenre: string; // 'horror' 또는 'thriller'가 될 수 없음
        ```
        
- **`@IsBoolean()`**: 값이 **불리언** (`true` 또는 `false`)인지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsBoolean()
        isActive: boolean;
        ```
        
- **`@IsString()`**: 값이 **문자열**인지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsString()
        name: string;
        ```
        
- **`@IsNumber()`**: 값이 **숫자**인지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsNumber()
        price: number;
        ```
        
- **`@IsInt()`**: 값이 **정수**인지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsInt()
        age: number;
        ```
        
- **`@IsArray()`**: 값이 **배열**인지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsArray()
        items: string[];
        ```
        
- **`@IsDateString()`**: 값이 **유효한 날짜를 나타내는 문자열**인지 (예: '2023-01-01') 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsDateString()
        eventDate: string;
        ```
        
- **`@IsEnum(entity: object)`**: 값이 제공된 TypeScript enum에 정의된 값 중 **하나인지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        enum MovieGenre {
            ACTION = 'action',
            FANTASY = 'fantasy',
            COMEDY = 'comedy',
        }
        
        @IsEnum(MovieGenre)
        selectedGenre: MovieGenre; // MovieGenre.ACTION, MovieGenre.FANTASY, MovieGenre.COMEDY 중 하나여야 함
        ```
        

---

## 숫자 유효성 검사기 (Number Validators)

이 유효성 검사기들은 숫자 타입에 특화되어 있습니다.

- **`@IsDivisibleBy(number: number)`**: 숫자가 주어진 `number`로 **나누어 떨어지는지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsDivisibleBy(5)
        score: number; // 5의 배수여야 함
        ```
        
- **`@IsPositive()`**: 숫자가 **양수인지** (0보다 큰지) 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsPositive()
        amount: number; // 0보다 커야 함
        ```
        
- **`@IsNegative()`**: 숫자가 **음수인지** (0보다 작은지) 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsNegative()
        temperature: number; // 0보다 작아야 함
        ```
        
- **`@Max(maxValue: number)`**: 숫자가 `maxValue`보다 **작거나 같은지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @Max(100)
        percentage: number; // 100 이하여야 함
        ```
        
- **`@Min(minValue: number)`**: 숫자가 `minValue`보다 **크거나 같은지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @Min(1)
        quantity: number; // 1 이상이어야 함
        ```
        

---

## 문자열 유효성 검사기 (String Validators)

이 유효성 검사기들은 문자열 타입에 특화되어 있습니다.

- **`@Contains(seed: string)`**: 문자열에 주어진 `seed` (부분 문자열)가 **포함되어 있는지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @Contains('code factory')
        description: string; // 'code factory'를 포함해야 함
        ```
        
- **`@NotContains(seed: string)`**: 문자열에 주어진 `seed` (부분 문자열)가 **포함되어 있지 않은지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @NotContains('secret')
        message: string; // 'secret'을 포함하면 안 됨
        ```
        
- **`@IsAlphanumeric()`**: 문자열이 **글자와 숫자만** 포함하는지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsAlphanumeric()
        productCode: string; // 예: 'ABC123'
        ```
        
- **`@IsCreditCard()`**: 문자열이 **유효한 신용카드 번호**인지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsCreditCard()
        cardNumber: string;
        ```
        
- **`@IsHexColor()`**: 문자열이 **유효한 16진수 색상 문자열**인지 (예: '#RRGGBB' 또는 '#RGB') 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsHexColor()
        color: string; // 예: '#FF0000'
        ```
        
- **`@MaxLength(maxLength: number)`**: 문자열의 길이가 `maxLength`보다 **작거나 같은지** 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @MaxLength(255)
        title: string; // 최대 255자
        ```
        
- **`@IsUUID(version?: 'all' | '3' | '4' | '5')`**: 문자열이 **유효한 UUID** (Universally Unique Identifier)인지 확인합니다. UUID 버전을 지정할 수 있습니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsUUID('4') // 유효한 UUID v4인지 확인
        id: string;
        ```
        
- **`@IsLatLong()`**: 문자열이 **"위도,경도" 형식** (예: "34.052235, -118.243683")의 유효한 위도 및 경도인지 확인합니다.
    
    - **사용법:**
        
        TypeScript
        
        ```
        @IsLatLong()
        coordinates: string;
        ```