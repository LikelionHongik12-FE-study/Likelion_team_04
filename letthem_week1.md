# 📎 타입스크립트를 쓰는 이유

✔️ **JavaScript** : 웹브라우저에서만 사용하게 만들어진 언어
- 유연한 문법
- **버그 발생 가능성 높음** ➡️ 안정성 떨어트림 👿
- 자유로움

✔️ **node.js** : 자바스크립트를 **어디에서도 실행가능하게** 만들어주는 구동기(런타임)
- 웹 서버
- 모바일 앱
- 데스크탑 앱

등 JS를 모든 곳에서 사용할 수 있게 됨..!!

✔️ **TypeScript**
- 엄격한 문법
- 버그 발생 가능성 낮음 😇
- 안정적임

💡 **타입 시스템** : 언어의 타입 관련된 문법 체계
- 정적 타입 시스템과 동적 타입 시스템으로 나뉨
![image](https://github.com/user-attachments/assets/55c8bc6b-52b8-4d93-ac5f-074e70bd7417)

### 동적 타입 시스템
✔️ **자바스크립트(동적 타입 시스템)**에서는 변수의 타입들을 **코드가 실행되는 도중에 결정**함
➡️ 변수의 타입을 **우리가 직접 정의하지 않음**
```javascript
let a = "hello"; // 문자열
a = 20020506; // 숫자

a.toUpperCase(); // Type Error 코드 실행 후 발생
```
- 변수의 타입이 하나로만 고정되지 않음
- 아무 타입의 값이나 자유롭게 담을 수 있음
- 유연하다 😇
- but, 오류 발생 가능.. 코드 실행 전에 검사 거쳐서 실행하지 못하게 막는 게 좋은데 ㅠㅠ 갑자기 오류 발생해서 런타임에 오류가 발생하게 될 수도 있음 👿

### 정적 타입 시스템
✔️ **자바(정적 타입 시스템)**에서는 **코드 실행 이전에 모든 변수의 타입을 결정**함
➡️ 모든 변수의 타입을 **다 지정해 주어야 함**
```java
String a = "hello";
int b = 123;

int c = a * b; // Type Error 코드 작성 중 에디터에서 미리 발생
```
- 오류가 있다면 애초에 실행되지 않음 😇
- 코드의 양이 늘어난다 👿 일일이 타입 지정해줘야 해서 귀찮다ㅠ.ㅠ

### 점진적 타입 시스템
✔️ **타입스크립트**는 **동적 타입 시스템 + 정적 타입 시스템 = 점진적 타입 시스템(Gradual Type System)**
- 오류가 있다면 실행이 안 되게 막아줌 😇
- 모든 변수에 일일이 직접 타입 명시하지 않아도 됨 😇
- 변수에 담기는 초기값을 기준으로 자동으로 알아서 타입을 지정해줌
	- 점진적으로 타입이 정의된 변수 - 타입 미리 결정
	- 타입 정의되어 있지 않은 변수 - TS가 알아서 자동으로 타입 추론해줌
```typescript
let a = 1; // 숫자 값으로 초기화 되었으니 a는 숫자 타입이겠구나 !!

a.toUpperCase(); // Type Error 코드 작성 중 에디터에서 미리 발생
```

### **타입스크립트의 컴파일 과정**
>**코드를 AST로 먼저 변환**하고, **타입 검사**를 거쳐 **AST를 자바스크립트로 변환**하여 컴파일을 종료한다!
그 이후 Node.js나 웹브라우저 등으로 실행하면 **자바스크립트 컴파일 과정을 다시 거쳐서 실행!!**

### 😍 **TS를 쓰는 이유 ⬇️**
>➡️ 우리가 JavaScript를 **더욱 안전하게 사용할 수 있도록 미리 코드를 검사하는 용도**로 TypeScript를 쓴다!

# 📎 타입스크립트 컴파일러 옵션 설정하기
#### include
- 컴파일 범위 지정

#### target
- TS 코드를 컴파일해서 만들어지는 JS 코드의 버전을 설정
- 구형 브라우저 등 최신 JS 지원 안 해준다면 ES5처럼 버전을 낮출 필요가 있음

#### module
- JS 코드의 모듈 시스템 설정
- CommonJS 는 require 로 모듈 불러오고 module.exports 로 모듈 내보냄
- ESmodule system 은 import ~ from ~ 으로 모듈 불러오고 export default 로 모듈 내보냄

#### outDir
- 컴파일 결과 생성될 JS 파일들이 어디에 위치하면 좋겠는지 명시

#### strict
- 타입 검사 얼마나 엄격하게 할지

#### moduleDetection
- TS는 JS와 달리 모든 TS 파일을 전역 모듈로 본다
- 각각 독립된 이름 공간 쓸 필요가 있음

➡️ 해결 방법
1. export, import 등 모듈 관련 키워드 쓰기 -> 독립된 모듈로 바라봄
2. tsconfig.json에 **"moduleDetection": "force" 옵션 추가** -> 자동으로 JS 파일에 export {}; 추가해줌


#### skipLibCheck
- 불필요한 타입 정의 파일(.d.ts)의 타입 검사를 생략

### 😍 **최종 tsconfig.json ⬇️**
```json
{
  "compilerOptions": {
    "target": "ESNext",
    "module": "ESNext",
    "outDir": "dist",
    "strict": true,
    "moduleDetection": "force",
    "skipLibCheck": true
  },
  "include": ["src"]
}
```

# 📎 원시타입과 리터럴타입

### 원시 타입(Primitive Type)
- 하나의 값만 저장하는 타입

> number

```typescript
// number
let num1: number = 123;
let num2: number = -123;
let num3: number = 0.123;
let num4: number = -0.123;
let num5: number = Infinity;
let num6: number = -Infinity;
let num7: number = NaN;

num1.toFixed()
```

: number <- 타입 주석, 타입 annotation

문자열 전용 메서드 사용 불가
ex) toUpperCase()

number 타입 전용 메서드 사용 가능
ex) toFixed()

> string

```typescript
// string
let str1: string = "hello";
let str2: string = 'hello';
let str3: string = `hello`;
let str4: string = `hello ${num1}`; // 템플릿 리터럴 문자열

str1.toUpperCase();
```

> boolean

```typescript
// boolean
let bool1: boolean = true;
let bool2: boolean = false;
```

> null

```typescript
// null
let null1: null = null;
```

> undefined

```typescript
// undefined
let unde1: undefined = undefined;
```

❓ 이렇게 number에 null 넣고 싶다면
```typescript
let numA: number = null;
```

tsconfig.json에 추가 !!
```json
"strictNullChecks": false
```
 strict 하위에 strictNullChecks 옵션 있음. 영향 받는다.
 
> 리터럴 타입

- 값 자체가 타입이 되는 타입
```typescript
// 리터럴 타입
// 리터럴 -> 값
let numA: 10 = 10;

let strA: "hello" = "hello";

let boolA: true = true;
```

---
# 📎 배열과 튜플
> 배열

```typescript
// 배열
let numArr:number[] = [1, 2, 3];

let strArr: string[] = ["hello", "im", "winterlood"];

let boolArr: Array<boolean> = [true, false, true];
```
<> : 제네릭 문법

> 배열에 들어가는 요소들의 타입이 다양할 경우

( | ) : 유니온 타입

```typescript
// 배열에 들어가는 요소들의 타입이 다양할 경우
let multiArr: (string | number)[] = [1, "hello"];
```

> 다차원 배열

```typescript
// 다차원 배열의 타입을 정의하는 방법
let doubleArr: number[][] = [
  [1, 2, 3],
  [4, 5],
];
```

> 튜플

```typescript
// 튜플
// 길이와 타입이 고정된 배열
let tup1: [number, number] = [1, 2];

let tup2: [number, string, boolean] = [1, "2", true];

const users: [string, number][] = [
  ["이정환", 1],
  ["이아무개", 2],
  ["김아무개", 3],
  ["박아무개", 4],
  [5, "최아무개"], // TypeError
];
```

- push(), pop() 등 배열 메서드 사용할 땐 튜플의 길이 제한이 발동하지 않음. JS의 배열이라고 생각하므로 ➡️ _배열 메서드는 주의해서 사용_하자

- 배열을 사용할 때 인덱스의 위치에 따라서 **넣어야 하는 값들이 이미 정해져있고, 그 순서를 지키는 게 중요**할 때 **튜플**을 사용하면 좋다! 👍

---
# 📎 객체

object 만 띡 쓰면 객체라는 정보 외에는 모른다..!!
**객체 리터럴 타입**을 사용하자
```typescript
// object
let user: {
  id?: number;
  name: string;
} = {
  id: 1,
  name: "이정환",
};

user = {
  name: "홍길동",
};
```

TS 처럼 객체의 구조를 기준으로 Type을 정의한다 ➡️ **구조적 타입 시스템** ➡️ **Property Based Type System**

C, Java 등 이름을 기준으로 Type을 정의한다 ➡️ **명목적 타입 시스템**

- 있어도 되고 없어도 되는 속성
**?** 붙이면 된다! ➡️ **optional property**

```typescript
let config: {
  readonly apiKey: string;
} = {
  apiKey: "MY API KEY",
};

config.apiKey = "hacked"; // Error
```

- **readonly** 붙이면 읽기 전용 속성이므로 **수정 불가!** (API 키처럼 절대 값이 수정되면 안 되는 것에 유용)

---
# 📎 타입 별칭과 인덱스 시그니처

### 타입 별칭 (Type Alias)
- 중복으로 정의 방지 위해 사용
```typescript
// 타입 별칭

type User = {
  id: number;
  name: string;
  nickname: string;
  birth: string;
  bio: string;
  location: string;
};

function func() {
  type User = {}; // 여기에 선언된 type User는 func() 밖 User와 다름
};

let user: User = {
  id: 1,
  name: "이정환",
  nickname: "winterlood",
  birth: "1997.01.07",
  bio: "안녕하세요",
  location: "부천시",
};

let user2: User = {
  id: 2,
  name: "홍길동",
  nickname: "winterlood",
  birth: "1997.01.07",
  bio: "안녕하세요",
  location: "부천시",
};
```

### 인덱스 시그니처
- 객체 타입의 정의를 더 유연하게 하도록 도와줌
- **key, value 의 타입이 규칙**을 가지고 있을 때 유용
```typescript
// 인덱스 시그니처
type CountryCodes = {
  [key: string]: string;
};

let countryCodes: CountryCodes = {
  Korea : 'ko',
  UnitedState: "us",
  UnitedKingdom: "uk",
};

type CountryNumberCodes = {
  [key: string]: number;
};

let countryNumberCodes: CountryNumberCodes = {
  Korea : 410,
  UnitedState: 840,
  UnitedKingdom: 826,
};
```

주의할 점: 아무 값이 없어도 오류 안 남
```typescript
type CountryNumberCodes = {
  [key: string]: number;
};

let countryNumberCodes: CountryNumberCodes = {};
```

- 꼭 있어야 하는 key 값이 있다면 지정해서 넣어주기
```typescript

type CountryNumberCodes = {
  [key: string]: number;
  Korea: number; // 이럴 때 무조건 인덱스 시그니처의 value type과 같아야 함
};

let countryNumberCodes: CountryNumberCodes = {
  Korea : 410,
};
```

---
# 📎 Enum 타입

- 열거형 타입
- JS에는 없고 TS에만 있음
```typescript
// enum 타입
// 여러가지 값들에 각각 이름을 부여해 열거해두고 사용하는 타입

enum Role {
  ADMIN, // 자동으로 0번
  USER = 10, // 숫자형 enum
  GUEST, // 자동으로 11번
};

enum Language {
  korean = "ko",
  english = "en",
};

const user1 = {
  name : "이정환",
  role : Role.ADMIN, // 0 <- 관리자
  language: Language.korean,
};

const user2 = {
  name : "홍길동",
  role : Role.USER, // 1 <- 일반 유저
};

const user3 = {
  name : "아무개",
  role : Role.GUEST, // 2 <- 게스트
};

console.log(user1, user2, user3);
```
➡️ { name: '이정환', role: 0, language: 'ko' } { name: '홍길동', role: 10 } { name: '아무개', role: 11 }

- 아무 숫자 안 넣으면 0, 1, 2로 지정

> enum은 컴파일 결과 타입이 사라지지 않음 !!! 이넘이 JS의 객체로 변환된다

---
# 📎 Any 타입과 Unknown 타입
### any
- 어떤 타입이든지 해당 변수에 넣을 수 있다
- 메서드도 자유롭게 사용 가능
- 모든 타입의 변수에 any type의 값을 집어넣을 수도 있다..!
```typescript
// any
// 특정 변수의 타입을 우리가 확실히 모를 때

let anyVar: any = 10;
anyVar = "hello";

anyVar = true;
anyVar = {};
anyVar = () => {};

anyVar.toUpperCase();
anyVar.toFixed();

let num: number = 10;
num = anyVar;
```
but, 런타임에서 오류가 난다... 그냥 JS와 다를 게 없다ㅠ.ㅠ
TS 이점이 사라진다. 
> 쓰지 말자

### Unknown
- 어떤 타입이든지 해당 변수에 넣을 수 있다
- 메서드 자유롭게 사용 불가
- 모든 타입의 변수에 unknown type의 값을 집어넣을 수 없음
- 연산 불가
```typescript
// unknown

let unknownVar: unknown;

unknownVar = "";
unknownVar = 1;
unknownVar = ()=>{};

if (typeof unknownVar === "number") {
  num = unknownVar;
};
```
- 이처럼 조건문으로 typeof 사용 등 타입 정제를 한다면 num 타입으로 사용할 수 있긴 함

> type 불확실하다면 any 쓰지말고 unknown 쓰자

---
# 📎 Void 타입과 Never 타입
### Void
```typescript
// void
// void -> 공허 -> 아무 것도 없다.

function func1(): string {
  return "hello";
};

function func2(): void {
  console.log("hello");
};

```
- 반환값이 없는 함수는 void로 타입 지정
- undefined로 지정하면 undefined라도 반환하거나 return; 이런식으로 반환해줘야 함.
- return 없는 함수에는 함수 반환 타입으로 void 꼭 써주자

### Never
```typescript
// never
// never -> 존재하지 않는
// 불가능한 타입

function func3(): never {
  while (true) {}
};

function func4(): never {
  throw new Error();
};
```
- 애초에 정상종료가 안 돼서 반환한다는 것 자체가 모순일 때
- 아무 것도 못 담음
- any type도 못 담음

# 📎 타입은 집합이다

### 타입
- 동일한 속성과 특징들을 갖는 여러 개의 값들을 모아둔 집합

<img width="763" alt="image" src="https://github.com/user-attachments/assets/10f7474b-32d6-430f-9343-1d636f7a1498">


- 값의 집합인 타입들은 서로 포함하거나 포함된다

### 타입 호환성
어떤 타입을 다른 타입으로 취급해도 괜찮은지 판단하는 것

- **업 캐스팅: 가능 / 다운 캐스팅: 불가능**
<img width="725" alt="image" src="https://github.com/user-attachments/assets/5c235c52-6815-4ab8-bfc3-6142bcbd0032">


---
# 📎 타입 계층도와 함께 기본타입 살펴보기
<img width="765" alt="image" src="https://github.com/user-attachments/assets/b6a0a72d-cdcb-446d-aae9-b23b142e76a5">


```typescript
/**
 * Unknown 타입
 */

function unknownExam(){
  let a: unknown = 1;
  let b: unknown = "hello";
  let c: unknown = true;
  let d: unknown = null;
  let e: unknown = undefined;

  let unknownVar: unknown;

  // let num: number = unknownVar;
  // let str: string = unknownVar;
  // let bool: boolean = unknownVar;
}
```

```typescript
/**
 * Never 타입
 */

function neverExam(){
  function neverFunc(): never {
    while (true) {}
  }

  let num: number = neverFunc();
  let str: string = neverFunc();
  let bool: boolean = neverFunc();

  // let never1: never = 10;
  // let never2: never = "string";
  // let never3: never = true;
}
```

```typescript
/**
 * Void 타입
 */

function voidExam() {
  function voideFunc(): void {
    console.log("hi");
  }
  
  let voidVar: void = undefined;
}
```

```typescript
/**
 * any 타입 : 타입 계층도를 무시함
 */

function anyExam() {
  let unknownVar: unknown;
  let anyVar: any;
  let undefinedVar: undefined;
  let neverVar: never;

  anyVar = unknownVar;

  undefinedVar = anyVar;

  // neverVar = anyVar;
}
```

---
# 📎 객체 타입의 호환성
```typescript
/**
 * 기본 타입 간의 호환성
 */

let num1: number = 10;
let num2: 10 = 10;

num1 = num2;

/**
 * 객체 타입 간의 호환성
 * -> 어떤 객체타입을 다른 객체타입으로 취급해도 괜찮은가?
 */

type Animal = {
  name: string;
  color: string;
};

type Dog = {
  name: string;
  color: string;
  breed: string;
};

let animal: Animal = {
  name: "기린",
  color: "yellow",
};

let dog: Dog = {
  name: "돌돌이",
  color: "brown",
  breed: "진도",
};

animal = dog; // 업 캐스팅

// dog = animal; // Error: 다운 캐스팅
```

- Animal : name, color 갖고 있으면 됨.
- Dog : name, color, breed 갖고 있으면 됨.

- Dog 타입은 Animal에 포함!
- Animal 타입은 Dog 타입에 포함 X

예시)
```typescript

type Book = { // 슈퍼
  name: string;
  price: number;
};

type ProgrammingBook = { // 서브
  name: string;
  price: number;
  skill: string;
};

let book: Book;

let programmingBook: ProgrammingBook = {
  name: "한 입 크기로 잘라먹는 리액트",
  price: 33000,
  skill: "reactjs",
};

book = programmingBook;
// programmingBook = book;

```

#### 초과 프로퍼티 검사
skill처럼 실제 타입에 정의해놓지 않은 속성 작성 못 하게 막는 검사
```typescript

/**
 * 초과 프로퍼티 검사
 */

type Book = { // 슈퍼
  name: string;
  price: number;
};

let book2: Book = {
  name: "한 입 크기로 잘라먹는 리액트",
  price: 33000,
  // skill: "reactjs", // 초과 프로퍼티 검사로 Error
}

let book3: Book = programmingBook; // 초기화 시 객체 리터럴을 사용한 건 아니므로 이건 가능

function func(book: Book) {}
func({
  name: "한 입 크기로 잘라먹는 리액트",
  price: 33000,
  // skill: "reactjs", // 초과 프로퍼티 검사로 Error
});

func(programmingBook); // 인수로 변수를 전달하면 괜찮음
```

---
# 📎 대수 타입
### 합집합 타입
```typescript
/**
 * 대수 타입
 * -> 여러 개의 타입을 합성해서 새롭게 만들어낸 타입
 * -> 합집합 타입과 교집합 타입이 존재합니다
 */

/**
 * 1. 합집합 - Union 타입
 */

let a: string | number | boolean;
a = 1;
a = "hello";
a = true;

let arr: (number| string | boolean)[] = [1, "hello", true];

type Dog = {
  name: string;
  color: string;
};

type Person = {
  name: string;
  language: string;
};

type Union1 = Dog | Person;

let union1: Union1 = {
  name: "",
  color: "",
};

let union2: Union1 = {
  name: "",
  language: "",
};

let union3: Union1 = {
  name: "",
  color: "",
  language: "",
};

let union4: Union1 = {
  name: "", // Error
};
```

### 교집합 타입
교집합 타입 기본 타입들로는 웬만하면 never 타입으로 됨... -> 객체 타입에 많이 사용

```typescript
/**
 * 2. 교집합 타입 - Intersection 타입
 */

let variable: number & string;

type Dog = {
  name: string;
  color: string;
};

type Person = {
  name: string;
  language: string;
};

type Intersection = Dog & Person;

let intersection1: Intersection = {
  // 속성 하나라도 빠지면 X
  name: "",
  color: "",
  language: "",
}
```

---
# 📎 타입 추론

#### 초기값 있다면
기본적인 건 추론 아주 굳
```typescript
/**
 * 타입 추론
 */

let a = 10;
let b = "hello";
let c = {
  id: 1,
  name: "이정환",
  profile: {
    nickname: "winterlood",
  },
  urls: ["https://winterlood.com"],
};

let { id, name, profile } = c;

let [one, two, three] = [1, "hello", true];

function func(message = "hello") {
  return "hello"; // 반환값, 매개변수도 추론 굳
}
```

#### any 타입의 진화 👎
변수를 선언하고 초기값이 없다면
암묵적으로 any 타입으로 선언됨..
```typescript
let d; // any
d = 10; // any -> number
d.toFixed();

d = "hello"; // string -> string
```
> 이렇게는 쓰지 말자! type 자꾸 바뀌는 건 좋지 않다.

#### const로 선언하면 리터럴 타입으로 추론
```typescript
// const
const num = 10; // number가 아닌 number 리터럴 타입으로 추론됨
const str = "hello" // string 리터럴 타입으로 추론됨
```

#### union
```typescript
let arr = [1, "string"]; // union 타입으로 추론됨
```

#### 타입 넓히기
```typescript
let a = 10; // number 타입으로 추론됨
```
타입 넓히기: const로 선언된 것이 아니라면 number 리터럴 타입 말고 그냥 number로 추론해주는 등 조금 더 범용적으로 쓰일 수 있게 해주는 것

---
# 📎 타입 단언
**타입 단언 (Type Assertion)**
- typescript의 눈을 살짝 가려주는 용도
- 확실할 때만 쓰자!
- 초기화 값의 타입을 단언해주기! (as~)
```typescript
/**
 * 타입 단언
 */

type Person = {
  name: string;
  age: number;
};

let person = {} as Person;
person.name = "이정환";
person.age = 27;

type Dog = {
  name: string;
  color: string;
};

let dog = {
  name: "돌돌이",
  color: "brown",
  breed: "진도",
} as Dog;
```

> 타입 단언의 규칙
```typescript
/**
 * 타입 단언의 규칙
 * 값 as 단언 <- 단언식
 * A as B
 * A가 B의 슈퍼타입이거나
 * A가 B의 서브타입이어야 함
 */

let num1 = 10 as never; // A가 B의 슈퍼
let num2 = 10 as unknown; // A가 B의 서브

// let num3 = 10 as string; // A와 B는 교집합이 없음 -> 슈퍼도 서브도 아님

let num3 = 10 as unknown as string; // 다중 단언 -> 굳이 사용하지 말자..
```

> const 단언
```typescript
/**
 * const 단언
 */

let num4 = 10 as const;

let cat = {
  name: "야옹이",
  color: "yellow",
} as const; // readonly가 됨!

// cat.name = '' // 읽기 전용이라 Error
```

> Non Null 단언
```typescript
/**
 * Non Null 단언
 */

type Post = {
  title: string;
  author?: string;
};

let post: Post = {
  title: "게시글1",
  author: "이정환",
};

// const len: number = post.author?.length;
const len: number = post.author!.length;
```

> ? : 옵셔널 체이닝 -> post.author?.length 자체가 Undefined가 될 수도 있다
(이건 실제로도 많이 봤던 에러)
=> ?를 !로 바꿔주자

! : Non Null 단언. null이나 undefined 아니야! 라고 단언해줌

---
# 📎 타입 좁히기
활용 많이 하면 좋다 !

**타입 가드 ⬇️**
- typeof : null값에다가 typeof를 해도 object를 반환,,,🤔
- instance of 사용하면 해결 ! (왼쪽에 있는 값이 오른쪽의 instance냐(오른쪽이냐)? true), 근데 오른쪽에 오는 값이 Type이면 안됨 class 여야 함
- 위와 같은 문제가 있을 땐 in 사용하면 해결! (왼쪽에 있는 값이 오른쪽에 있냐? true)

```typescript
/**
 * 타입 좁히기
 * 조건문 등을 이용해 넓은타입에서 좁은타입으로
 * 타입을 상황에 따라 좁히는 방법을 말함
 */

type Person = {
  name: string;
  age: number;
}

// value => number: toFixed
// value => string: toUpperCase
// value => Date: getTime
// value => Person: name은 age살입니다.
function func(value: number | string | Date | null | Person) {
  if (typeof value === "number") {
    console.log(value.toFixed());
  } else if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else if (value instanceof Date) {
    console.log(value.getTime());
  } else if (value && "age" in value){
    console.log(`${value.name}은 ${value.age}살 입니다`);
  }
}
```

---
# 📎 서로소 유니온 타입
```typescript
/**
 * 서로소 유니온 타입
 * 교집합이 없는 타입들로만 만든 유니온 타입을 말함
 */

type Admin = {
  name: string;
  kickCount: number;
};

type Member = {
  name: string;
  point: number;
};

type Guest = {
  name: string;
  visitCount: number;
};

type User = Admin | Member | Guest;

// Admin -> {name}님 현재까지 {kickCount}명 강퇴했습니다.
// Member -> {name}님 현재까지 {point}모았습니다.
// Guest -> {name}님 현재까지 {visitCount}번 오셨습니다.
function login(user: User) {
  if ("kickCount" in user) {
    // admin 타입
    console.log(`${user.name}님 현재까지 ${user.kickCount}명 강퇴했습니다.`);
  } else if ("point" in user) {
    console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
  } else {
    console.log(`${user.name}님 현재까지 ${user.visitCount}번 방문하셨습니다`);
  }
};
```
=> 직관적이지 않다.

대신 서로소 유니온을 활용해보자

```typescript
/**
 * 서로소 유니온 타입
 * 교집합이 없는 타입들로만 만든 유니온 타입을 말함
 */

type Admin = {
  tag: "ADMIN"; // 태그드 유니온 타입
  name: string;
  kickCount: number;
};

type Member = {
  tag: "MEMBER";
  name: string;
  point: number;
};

type Guest = {
  tag: "GUEST";
  name: string;
  visitCount: number;
};

type User = Admin | Member | Guest;

function login(user: User) {
  switch(user.tag){
    case "ADMIN": {
      console.log(`${user.name}님 현재까지 ${user.kickCount}명 강퇴했습니다.`);
      break;
    }
    case "MEMBER": {
      console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
      break;
    }
    case "GUEST": {
      console.log(`${user.name}님 현재까지 ${user.visitCount}번 방문하셨습니다`);
      break;
    }
  }
}
```

> tag를 지정해주면 관계가 서로 아무런 교집합도 없는 서로소집합의 관계로 바뀌게 됨!

직관적으로 타입을 좁힐 수 있다!

```typescript

/**
 * 복습 겸 한 가지 더 사례
 */

// 비동기 작업의 결과를 처리하는 객체

type LoadingTask ={
  state: "LOADING"
};

type FailedTask = {
  state: "FAILED";
  error: {
    message: string;
  };
};

type SuccessTask = {
  state: "SUCCESS";
  response: {
    data: string;
  };
};

type AsyncTask = LoadingTask | FailedTask | SuccessTask;

function processResult(task:AsyncTask) {
  switch(task.state){
    case "LOADING": {
      console.log("로딩 중");
      break;
    }
    case "FAILED": {
      console.log(`에러 발생: ${task.error.message}`);
      break;
    }
    case "SUCCESS": {
      console.log(`성공: ${task.response.data}`);
      break;
    }
  }
}

const loading: AsyncTask = {
  state: "LOADING",
};

const failed: AsyncTask  = {
  state: "FAILED",
  error: {
    message: "오류 발생 원인은 ~~",
  },
};

const success: AsyncTask  = {
  state: "SUCCESS",
  response: {
    data: "데이터 ~~",
  }
}
```
