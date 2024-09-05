

```jsx
npm init //Node.js 패키지 초기화
npm i @types/node //Node.js 내장 기능들의 타입 정보를 담고있는 패키지
sudo npm i -g typescript // TypeScript 패키지를 설치 (타입스크립트 컴파일러 설치)
(윈도우: npm i -g typescript)

tsc -v : 타입스크립트 컴파일러 버전 출력
tsc src/index.ts : 해당 경로 파일을 타입스크립트 컴파일러로 컴파일(js 파일 생김)
node src/index.js : 해당 경로의 있는 파일을 node를 이용해 실행시킴

//node 20버전 이후부터는 ts-node 적용 안됨
sudo npm i -g tsx : 전역(컴퓨터 어디서 쓸 수 있도록)으로 tsx를 설치(이전에는 ts-node)
tsx src/index.ts : 타입스크립트 코드를 한번에 실행할 수 있도록 만들어줌
```

## 타입스크립트 컴파일러 옵션 설정하기

tsconfig.json → 타입스크립트 컴파일러 옵션 파일

```jsx
{
  "compilerOptions": {
    "target": "ESNext", //버전 몇의 ESM으로 설정할건지(ES5에는 화살표 함수 X)
    "module": "ESNext", //어떤 모듈 시스템의 JS로 컴파일할건지(최신 ESM)
    "outDir": "dist", //컴파일된 파일을 어디에 저장할건지(dist 파일에 저장)
		"moduleDetection": "force", //각각의 파일을 개별 모듈로 판단할것인가(force하면 각각의 파일을 모듈로 인식)
		//이로 인해 export {} 가 컴파일 결과 생성됨
		"strict": true  //엄격한 검사 수행(이러면 파라미터의 타입도 정의해야함)
  },
	"ts-node": {
		"esm": true //이제부터 ts-node가 esm모드로 동작하게 됨
	},
  "include": ["src"] //src 파일 내부에 있는 파일들은 전부 컴파일 하겠다는 의미
  //그럼 이제 tsc (경로명) 이 아닌 tsc만 입력해도 알아서 컴파일이 됨 
}
```

- Legacy ! 이제는 tsx를 사용하기 때문에, 따로 설정해줄 필요는 없긴 하다.
    
    참고로, esm을 사용하려면 package.json에 "type": "module" 을 설정해주고, 기본적으로 ts-node는 esm모드를 인식하지 못하기 때문에 tsconfig.json에서 
    
    ```jsx
    	"ts-node": {
    		"esm": true //이제부터 ts-node가 esm모드로 동작하게 됨
    	},
    ```
    
    를 설정해주어야 한다. 
    

마지막으로 @types버전이 20버전 이상으로 업데이트 되면서 특정 라이브러리에서 타입 검사 오류가 발생하고 있으므로, tsconfig.json에 다음 옵션을 추가해주어야한다. 

```jsx
"skipLibCheck": true 
//타입 정의 파일(.d.ts 파일) 의 타입 검사를 생략하는 옵션
//타입
```

가끔 라이브러리의 타입 정의 파일에서 타입 오류가 발생하는 일이 발생할 수 있다. 따라서 해당 옵션을 true로 설정하셔서 불 필요한 타입 정의 파일의 타입 검사를 생략한다.

## 타입스크립트의 기본 타입 (내장 타입)

<aside>
💡

타입 주석(Type annotation) 
: 변수의 타입을 저장하는 가장 기본적인 방법( : 콜론 사용)

</aside>

### 원시타입(Primitive Type) - 하나의 값만 저장하는 타입

- number
- string
- boolean
- null
- undefined
- 리터럴 타입 
(ex. let numA: 10 = 10; let strA : “hi” = “hi, let boolA : true = true )  → 다른 리터럴(값)을 못 넣게 됨

<aside>
💡

컴파일러 옵션에 
”strictNullChedcks” : false 하면 number에 null을 임시로 넣을 수 있음 
(참고로 위의 옵션은 strict의 하위 타입임)

</aside>

### 배열과 튜플

> 배열
> 
- let 배열명 : **배열요소의타입[]** = {배열}
- let boolArr :  Array<boolean> = [true, false];
- 배열 요소의 타입이 다양하면 유니온 타입 이용! (number | string) []
- 다차원 배열의 타입 저장 ex) let doubleArr = number[][]

> 튜플 : 길이와 타입이 고정된 배열
> 

let tupl : [number, number] = [1, 2];

그러나, push, pop에서 오류를 안 일으킴 (동적으로 동작하기 때문) 

<aside>
💡

튜플을 사용하는 이유 → 배열의 순서가 정해져있어야 하는 경우 

```jsx
const users: [string, number][] = [
  ["이정환", 1],
  ["이아무개", 2],
  ["김아무개", 3],
  ["박아무개", 4],
  [5, "조아무개"], // 오류 발생
];

```

</aside>

### 객체 타입

- 아주 간단한 객체 타입 정의 (그러나, 객체라는 것만 알기 떄문에 쓸모 많지 않음)
    
    ```jsx
    let user: object = {
      id: 1,
      name: "이정환",
    };
    ```
    
- 객체 리터럴 타입 사용
    
    ```jsx
    let user: {
      id: number; 
      name: string;
      
    } = {
      id: 1,
      name: "이정환",
    };
    
    user.id;
    ```
    
    ```jsx
    let user: {
      id?: number; //optional property (선택적 프로퍼티) 
      readonly name: string; // name은 이제 Readonly 프로퍼티가 되었음(변경불가)
    } = {
      id: 1,
      name: "이정환",
    };
    
    user.name = "dskfd"; // 오류 발생
    ```
    
    <aside>
    💡
    
    타입스크립트는 **구조적 타입 시스템**을 사용한다. 
    - 구조적 타입 시스템 (Property type system) 
    
    - 명목적 타입 시스템(JAVA, C) : 이름을 기준으로 타입 정의
    
    </aside>
    

**타입 별칭, 인덱스 시그니처**

- **타입 별칭**
    
    ```jsx
    // 타입 별칭 - 아래는 객체 타입을 정의하는 예시 
    type User = {
      id: number;
      name: string;
      nickname: string;
      birth: string;
      bio: string;
      location: string;
    };
    ...
    ```
    
- **인덱스 시그니쳐 (규칙을 통해 유연한 타입을 정의해둠)** 
해당 규칙을 위반하는 프로퍼티가 아니라면 문제를 일으키지 x
    
    ```jsx
    type CountryNumberCodes = {
      [key: string]: number;
      Korea: string; // 오류! 
    };
    ```
    
    시그니쳐를 사용하면서 동시에 추가적인 프로퍼티를 또 정의할 때에는 인덱스 시그니쳐의 value 타입과 직접 추가한 프로퍼티의 value 타입이 호환되거나 일치해야함.
    
    ```jsx
    type CountryNumberCodes = {
      [key: string]: number;
      Korea: number; // 오류 해결
    };
    ```
    

### Enum 타입 (열거형 타입)

tsc로 컴파일해도 사라지지 않고 JS 객체(함수)로 남으므로 문제가 안생김!

```jsx
enum Role {
  ADMIN = 0,
  USER = 1,
  GUEST = 2,
}

enum Role {
  ADMIN, // 0 할당(자동)
  USER,  // 1 할당(자동)
  GUEST, // 2 할당(자동)
}

enum Role {
  ADMIN, // 10 할당 (자동)
  USER = 11,       // 11 할당
  GUEST,      // 12 할당(자동)
}

enum Language {
  korean = "ko",
  english = "en",
}
```

### Any, Unknown 타입

**any:** 특정 변수의 타입을 확실히 모를 때 

→ 타입 검사를 다 패스하기 떄문에, 에러 내용들은 런타임에 살펴볼 수 있다.

**unknown**: 어떤 타입의 값도 다 들어올 수 있으나, 어떤 타입의 변수에도 저장할 수 없다. 

```jsx
//가능
let unknownVar: unknown;

unknownVar = "";
unknownVar = 1;
unknownVar = () => {};

//불가능
let num: number = 10;
(...)

let unknownVar: unknown;
unknownVar = "";
unknownVar = 1;
unknownVar = () => {};

num = unknownVar; // 오류 !
```

그러나 타입 좁히기를 통해서는 가능하다.

```jsx
if (typeof unknownVar === "number") {
	// 이 조건이 참이된다면 unknownVar는 number 타입으로 볼 수 있음
  unknownVar * 2;
}
```

> 따라서, 타입을 제대로 모를 때는 any보다는 unknown 타입을 활용하는 것이 더욱 안전하다.
> 

### void 타입, Never 타입

**void 타입** : 아무런 값도 없다 

```jsx
function func2(): void {
  console.log("hello");
}
//어떤 값도 반환하지 않는다. 
```

<aside>
💡

참고로, 

```jsx
function func2(): void {
  console.log("hello");
  return;
}
```

이었다면 이는 undefined를 반환하는 함수다. 

가능한 이유: void는 undefined를 포함하는 타입이기 때문 !

</aside>

**never 타입** : 불가능, 즉 어떤 값도 올 수 없다(undefined, null 전부 불가능) 

→ 어떠한 값도 반환하지 않는 상태일 때(반환할 수 없는 상태일 때)는 void보다는 never가 더욱 적합하다.

```jsx
function func3(): never {
  while (true) {}
}

function func4(): never {
  throw new Error();
}
```

## 타입 스크립트 이해하기 (원리 이해)

제대로 배워서 좋은 코드를 만들고, 문제를 해결하자 !

어떤 기준으로

- 타입을 정의하는가
- 타입간의 관계를 정의하는가
- 타입의 오류를 검사하는가

<aside>
💡

타입이란, **집합**이다 !

- 동일한 속성과 특징들을 갖는 값들을 갖는 집합
- 따라서, 타입 계층도가 존재한다.
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/7105c227-dc91-4992-865f-85ac4bffca4b/image.png)
    ![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/7105c227-dc91-4992-865f-85ac4bffca4b/image.png)
  ![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/70b725b3-d977-46a7-a7b0-edb0aaf36477/Untitled.png)
    
- 업 캐스팅(가능) vs 다운 캐스팅(불가능)
    
    ![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/70b725b3-d977-46a7-a7b0-edb0aaf36477/Untitled.png)
    
</aside>

### 기본 타입의 호환성

이제, 아래의 타입 계층도를 통해 업캐스팅을 통해 값을 넣을 수 있는지를 파악할 수 있게 되었다. 

![타입계층도.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/1071e3ab-8672-4c0a-ba58-9ccf25c184ea/%ED%83%80%EC%9E%85%EA%B3%84%EC%B8%B5%EB%8F%84.png)

모든 집합의 슈퍼 타입(전체집합) : unknown 타입

모든 집합의 서브 집합(공집합) : never 타입

- void타입은 undefined 타입의 슈퍼 타입
- any 타입은 치트키 타입 (모든 타입의 슈퍼 타입이자 서브타입! 그래서 특이하게 never를 제외하고 다운캐스팅이 될 수 있음) 
→ never만 제외하고! 유일하게 any가 다운캐스팅이 안되는건 never 타입임)

### 객체 타입의 호환성

구조적 타입 시스템이기 때문에, 객체 타입은 프로퍼티를 기준으로 슈퍼 타입과 서브 타입이 결정된다. 

```jsx
type Book = {
  name: string;
  price: number;
};

type ProgrammingBook = {
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

book = programmingBook; // ✅ OK -> 업캐스팅
programmingBook = book; // ❌ NO -> 다운캐스팅 

```

그러나

```jsx
type Book = {
  name: string;
  price: number;
};

type ProgrammingBook = {
  name: string;
  price: number;
  skill: string;
};

(...)

let book2: Book = { // 오류 발생
  name: "한 입 크기로 잘라먹는 리액트",
  price: 33000,
  skill: "reactjs",
}; // -> ❌ 에러 발생 !
```

> 에러가 발생하는 이유? : **초과 프로퍼티 검사**
> 

<aside>
💡

**초과 프로퍼티 검사란** ?
: 변수를 객체 리터럴로 초기화 할 때 발동하는 타입스크립트의 특수한 기능

</aside>

따라서, 리터럴로 초기화하지말고 저장된 변수값으로 초기화해야 초과 프로퍼티 검사를 피할 수 있다.

## 대수 타입

- 여러개의 타입을 합성해서 새롭게 만들어낸 타입
- 합집합 타입 vs 교집합 타입

### 합집합(union) 타입

```jsx
(...)

let union1: Union1 = { // ✅
  name: "",
  color: "",
};

let union2: Union1 = { // ✅
  name: "",
  language: "",
};

let union3: Union1 = { // ✅
  name: "",
  color: "",
  language: "",
};

let union4: Union1 = { // ❌
  name: "",
};
```

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/71acef9f-b3b2-42cc-8f2e-855e5f50fb24/Untitled.png)

### **교집합(Intersection) 타입 (ex. string & number → never)**

```jsx
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
  name: "",
  color: "",
  language: "",
}; //3가지를 전부 가지고 있어야 함
```

![Untitled.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/c6bb2081-e489-40cb-bf7a-777665e2f3d1/Untitled.png)
