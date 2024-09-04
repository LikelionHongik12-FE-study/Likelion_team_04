# 한 입 크기로 잘라먹는 타입 스크립트

- S2. 타입스크립트 개론
    
    
    > 타입스크립트는 자바스크립트를 더 안전하게 사용할 수 있도록 하는 확장판!
    > 
    
    자바스크립트를 아는 상태에서는 “타입 관련 기능”만 추가적으로 학습하면 된다. 
    
    굳이 타입스크립트 배워야해요??
    
    ⇒자바스크립트는 유연한 문법을 가지고, 버그 발생 가능성이 높다. 
    
    ⇒ 노드js의 등장으로 무엇이든 자바스크립트로 만들 수 있게 되었다. 
    
    ⇒ 프로그램 안정성이 떨어지기 때문에 안정성을 추구하는 타입스크립트를 배워야한다!
    
    ### 타입 시스템이란?
    
    언어의 타입 관련된 문법 체계. 
    
![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9f7779ce-1ec5-4e28-a76f-f72333846462/b25e8c53-a5e1-495e-bd33-466f40ea652d/image.png)
    
    자바스크립트는 동적 타입 시스템! - 문자열 넣으면 문자열 변수, 숫자 넣으면 숫자 변수가 됨.
    
    —> 유연함
    
    but, 런타임 오류에 취약함. 
    
    정적 타입 시스템은 C, Java 처럼 모든 변수 타입을 고정적으로 결정
    
    코드 실행 전에 타입 오류를 알 수 있음! 
    
    TS는 동적 타입 시스템 + 정적 타입 시스템
    
    = 점진적 타입 시스템
    
    ![image.png](%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%8F%E1%85%B3%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%20%E1%84%8C%E1%85%A1%E1%86%AF%E1%84%85%E1%85%A1%E1%84%86%E1%85%A5%E1%86%A8%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20e4e8f0c5453347888501ae82663900df/image%201.png)
    
    보통 컴파일러의 컴파일 과정 
    
    1. 코드 → AST(추상 문법 트리) 로 변환
    2. AST → 바이트 코드
    
    타입스크립의 동작 과정 
    
    1. 코드 → AST 
    2. 타입 검사 → 검사 실패하면 컴파일 종료 
    3. 검사성공 → 자바스크립트 코드로 변환
    4. 코드 →AST
    5. AST → 바이트 코드 → 실행
    
    ---
    
    타입 스크립트 사용하기! 
    
    1.  npm init  // Node.js 패키지 초기화하기 
    2. npm i @types/node //  타입스크립트 패키지 설치
    3. npm i typescript -g // 타입스크립트 컴파일러 설치
    
    ```jsx
    > tsc src/index.ts // tsc로 컴파일하기. 해당 파일명 -> 자바스크립트 코드로 컴파일 됨
    ```
    
    ```jsx
    > node src/index.js
    Hello Typescript    // node를 이용해 실행 
    ```
    
    tsx = 명령어 한번으로 타입 스크립트 코드를 바로  실행시켜줌 
    
    ```jsx
    npm i -g tsx // 설치
    ```
    
    ```jsx
    > tsx src/index.ts
    Hello TypeScript  // tsx로 실행
    ```
    
    ### 타입스크립트 컴파일러 옵션 설정하기
    
    ```jsx
    tsc --init //옵션 파일 생성
    ```
    
    include 옵션 
    
    ```jsx
    {
      "include": ["src"]
    }
    
    //src 경로 파일을 모두 컴파일 해! 
    ```
    
    target 옵션 
    
    ```jsx
    {
      "compilerOptions": {
        "target": "ES5" //원하는 자바스크립트 버전
      },
      "include": ["src"]
    }
    
    // 생성되는 자바스크립트 파일 버전 관리
    ```
    
     module 옵션
    
    ```jsx
    {
      "compilerOptions": {
        "target": "ESNext",
    		"module": "CommonJS"  // 모듈을 CommonJS로 
      },
      "include": ["src"]
    }
    ```
    
    outDir 옵션
    
    : 컴파일 결과 생성할 자바스크립트 코드 위치를 결정
    
    ```jsx
    {
      "compilerOptions": {
        "target": "ESNext",
        "module": "ESNext",
        "outDir": "dist"
      },
      "include": ["src"]
    }
    
    // dist 폴더에 컴파일 결과 생성
    ```
    
    strict
    
    : 컴파일러 타입 검사의 엄격함 수준을 검사하는 옵션
    
    ```jsx
    {
      "compilerOptions": {
        ...
        "strict": true  // 엄격한 검사모드 
      },
      "include": ["src"]
    }
    
    ```
    
    타입스크립트는 모든 파일을 전역 모듈로 본다 → 각각 파일이 모두 각각 독립된 모듈이 아니다.
    
    ModuleDetection 옵션
    
    ```jsx
    {
      "compilerOptions": {
        "target": "ESNext",
        "module": "ESNext",
        "outDir": "dist",
    		"moduleDetection": "force" // 모든 TS 파일이 로컬 모듈로 취급
      },
      "include": ["src"]
    }
    ```
    
    ❗ 모든 tsconfig.json에 아래 옵션 추가해야 됨! 
    
    ![image.png](%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%8F%E1%85%B3%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%20%E1%84%8C%E1%85%A1%E1%86%AF%E1%84%85%E1%85%A1%E1%84%86%E1%85%A5%E1%86%A8%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20e4e8f0c5453347888501ae82663900df/image%202.png)
    
- S3. 타입스크립트 기본
    
    
    원시타입 : 동시에 한개의 값만 저장할 수 있는 타입
    
    - number 타입
    
    ```jsx
    // number
    let num1: number = 123;
    let num2: number = -123;
    let num3: number = 0.123;
    let num4: number = -0.123;
    let num5: number = Infinity;
    let num6: number = -Infinity;
    let num7: number = NaN;
    ```
    
    - string 타입
    
    ```jsx
    // string
    let str1: string = "hello";
    let str2: string = 'hello';
    let str3: string = `hello`;
    let str4: string = `hello ${str1}`;
    ```
    
    - boolean 타입
    
    ```jsx
    // boolean
    let bool1 : boolean = true;
    let bool2 : boolean = false;
    ```
    
    - null 타입
    
    ```jsx
    // null
    let null1: null = null;
    ```
    
    - undefined 타입
    
    ```jsx
    // undefined 타입
    let unde1: undefined = undefined;
    ```
    
    ```jsx
      "compilerOptions": {
        ...
        "strictNullChecks": false,
    		...
      }
      
      // 하면 number 타입 등에도 null 값 할당 가능
    ```
    
    - 리터럴 타입
    
    ```jsx
    let numA: 10 = 10; // 10이라는 값만 허용하는 변수 
    
    let strA: "hello" = "hello";
    let boolA: true = true;
    let boolB: false = false;
    
    // 리터럴 타입에는 다른 값을 넣을 수 없음 
    ```
    
    ---
    
    - 배열 타입
    
    ```tsx
    let numArr: number[] = [1, 2, 3];
    let strArr: string[] = ["hello", "im", "winterlood"];
    
    let boolArr: Array<boolean> = [true, false, true];
    
    let multiArr = [1, "hello"]; // 다양한 타입 요소를 갖는 타입
    
    let multiArr: (number | string)[] = [1, "hello"]; // 배열 요소의 타입을 정의 
    ```
    
    - 다차원 배열 타입
    
    ```tsx
    let doubleArr : number[][] = [
      [1, 2, 3], 
      [4, 5],
    ];
    ```
    
    - 튜플 : 길이와 타입이 고정된 배열
    
    ```tsx
    let tup1: [number, number] = [1, 2]; // 길이가 2인 number만 들어가는 튜플 
    
    let tup2: [number, string, boolean] = [1, "hello", true]; // 길이가 3인 튜플
    ```
    
    ```tsx
    const users: [string, number][] = [
      ["이정환", 1],
      ["이아무개", 2],
      ["김아무개", 3],
      ["박아무개", 4],
      [5, "조아무개"], // 오류 발생
    ];
    
    // 이런식으로 타입을 정해둘 수 있음
    ```
    
    ---
    
    - 객체 타입
    
    ```tsx
    let user: object = {
      id: 1,
      name: "이정환",
    };
    
    // object 타입으로 정의 -> 객체 프로퍼티에 접근 못함 = 잘 안씀
    
    user.id; // 오류
    ```
    
    - 객체 리터럴 타입
    
    ```tsx
    let user: {
      id: number;
      name: string;
    } = {
      id: 1,
      name: "이정환",
    };
    
    user.id;
    
    // 구조적 타입 시스템. 
    ```
    
    - 선택적 프로퍼티
    
    ```tsx
    let user: {
      id?: number; // 선택적 프로퍼티가 된 id. ? 붙여주면 있어도 되고 없어도 됨.
      name: string;
    } = {
      id: 1,
      name: "이정환",
    };
    
    user = {
      name: "홍길동",
    };
    ```
    
    - 읽기 전용 프로퍼티
    
    ```tsx
    let user: {
      id?: number;
      readonly name: string; // name은 이제 Readonly 프로퍼티가 되었음. 값을 수정할 수 없다.
    } = {
      id: 1,
      name: "이정환",
    };
    
    user.name = "dskfd"; // 오류 발생
    ```
    
    ---
    
    - 타입 별칭
    
    ```tsx
    type User = {
      id: number;
      name: string;
      nickname: string;
      birth: string;
      bio: string;
      location: string;
    };
    
    let user: User = { // 타입을 변수처럼 정의 
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
    
    - 인덱스 시그니처
    
    ```tsx
    type CountryCodes = {
      Korea: string;
      UnitedState: string;
      UnitedKingdom: string;
      // (... 약 100개의 국가)
      Brazil : string
    };
    
    let countryCodes: CountryCodes = {
      Korea: "ko",
      UnitedState: "us",
      UnitedKingdom: "uk",
      // (... 약 100개의 국가)
      Brazil : 'bz'
    };
    
    ------- // 위의 타입 별칭을 인덱스 시그니처로 정의 
    
    type CountryCodes = {
      [key: string]: string;  
    };
    
    let countryCodes: CountryCodes = {
      Korea: "ko",
      UnitedState: "us",
      UnitedKingdom: "uk",
      // (... 약 100개의 국가)
      Brazil : 'bz'
    };
    ```
    
    ```tsx
    type CountryNumberCodes = {
      [key: string]: number;
      Korea: string; // 오류!
    };
    
    // 이렇게 되면 오류 발생 
    ```
    
    ---
    
    - Enum 타입 : 열거형
    
    ```tsx
    enum Role {
      ADMIN = 0, // 작성하지 않아도 자동으로 0 1 2 로 들어감, 맨 위 10 넣으면 자동으로 10 11 12
      USER = 1,
      GUEST = 2,
    }
    
    //각 멤버에 숫자 할당
    
    const user1 = {
      name: "이정환",
      role: Role.ADMIN, //관리자
    };
    
    const user2 = {
      name: "홍길동",
      role: Role.USER, // 회원
    };
    
    const user3 = {
      name: "아무개",
      role: Role.GUEST, // 게스트
    };
     
     // 역할 분류 가능!
     
    -----------
    
     
     enum Language {
      korean = "ko",
      english = "en",
    }
    
    const user1 = {
      name: "이정환",
      role: Role.ADMIN, // 0
      language: Language.korean,// "ko"
    };
    
    // 이렇게 문자열도 할당 가능
    ```
    
    ---
    
    - any타입 : 타입 검사를 받지 않는 특수한 치트키 타입 / 최대한 사용하지 않는걸 추천
    
    ```tsx
    let anyVar: any = 10;
    anyVar = "hello";
    
    anyVar = true;
    anyVar = {};
    
    anyVar.toUpperCase();
    anyVar.toFixed();
    anyVar.a;
    
    let num : anyVar;
    ```
    
    - unknown타입 : 모든 값을 저장할 수는 있지만, 변수에 저장할 수 없음, 연산, 메서드도 x
    
    ```tsx
    let unknownVar: unknown;
    
    unknownVar = "";
    unknownVar = 1;
    unknownVar = () => {};
    
    let num = unknownVar; // 오류! 
    ```
    
    any 보단 unknown 사용 추천 
    
    ---
    
    - void 타입 : 아무것도 없음, 보통은 함수의 반환값 타입으로 사용
    
    ```tsx
    function func2(): void {
      console.log("hello");
    }
    
    let a: void;
    a = undefined; // undefined 값 담을 수 있음
    ```
    
    - never 타입 : 불가능한, 어떤 값도 반환할 수 없는 함수의 반환값에 사용
    
    ```tsx
    function func3(): never {
      while (true) {}
    }
    ```
    
- S4. 타입스크립트 이해하기
    
    
    타입스크립트 주요 문법
    
    [https://www.typescriptlang.org/cheatsheets/](https://www.typescriptlang.org/cheatsheets/)
    
    타입은 집합이다
    
    ![image.png](%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%8F%E1%85%B3%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%20%E1%84%8C%E1%85%A1%E1%86%AF%E1%84%85%E1%85%A1%E1%84%86%E1%85%A5%E1%86%A8%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20e4e8f0c5453347888501ae82663900df/image%203.png)
    
    ![image.png](%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%8F%E1%85%B3%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%20%E1%84%8C%E1%85%A1%E1%86%AF%E1%84%85%E1%85%A1%E1%84%86%E1%85%A5%E1%86%A8%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20e4e8f0c5453347888501ae82663900df/image%204.png)
    
    ![image.png](%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%8F%E1%85%B3%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%20%E1%84%8C%E1%85%A1%E1%86%AF%E1%84%85%E1%85%A1%E1%84%86%E1%85%A5%E1%86%A8%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20e4e8f0c5453347888501ae82663900df/image%205.png)
    
    업 캐스팅 / 다운 캐스팅 ( 객체에서 배웠던거..!)
    
    타입 계층도
    
    ![image.png](%E1%84%92%E1%85%A1%E1%86%AB%20%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%8F%E1%85%B3%E1%84%80%E1%85%B5%E1%84%85%E1%85%A9%20%E1%84%8C%E1%85%A1%E1%86%AF%E1%84%85%E1%85%A1%E1%84%86%E1%85%A5%E1%86%A8%E1%84%82%E1%85%B3%E1%86%AB%20%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%B8%E1%84%90%E1%85%B3%20e4e8f0c5453347888501ae82663900df/image%206.png)
    
    - unknown 타입 : 모든 타입의 super 타입. 모든 타입의 값을 할당할 수 있음.
    
    ```tsx
    let a: unknown = 1;                 // number -> unknown
    let b: unknown = "hello";           // string -> unknown
    let c: unknown = true;              // boolean -> unknown
    let d: unknown = null;              // null -> unknown
    let e: unknown = undefined;         // undefined -> unknown
    let f: unknown = [];                // Array -> unknown
    let g: unknown = {};                // Object -> unknown
    let h: unknown = () => {};          // Function -> unknown
     // 모든 타입을 다운 캐스팅 가능. 업 캐스팅 불가
    ```
    
    - never 타입 : 타입 계층도에서 가장 아래에 위치.
    
    ```tsx
    let neverVar: never;
    
    let a: number = neverVar;            // never -> number
    let b: string = neverVar;            // never -> string
    let c: boolean = neverVar;           // never -> boolean
    let d: null = neverVar;              // never -> null
    let e: undefined = neverVar;         // never -> undefined
    let f: [] = neverVar;                // never -> Array
    let g: {} = neverVar;                // never -> Object
    
    // 모든 타입을 업캐스팅 가능. 다운 캐스팅 불가.
    ```
    
    - any 타입(치트키)
    
    ```tsx
    let anyValue: any;
    
    let num: number = anyValue;   // any -> number (다운 캐스트)
    let str: string = anyValue;   // any -> string (다운 캐스트)
    let bool: boolean = anyValue; // any -> boolean (다운 캐스트)
    
    anyValue = num;  // number -> any (업 캐스트)
    anyValue = str;  // string -> any (업 캐스트)
    anyValue = bool; // boolean -> any (업 캐스트)
    
    // never타입 빼고 가능 
    ```
    
    ---
    
    - 객체 타입 호환
    
    ```tsx
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
    
    animal = dog; // ✅ OK
    dog = animal; // ❌ NO
    
    // 추가 프로퍼티가 없는 , 더 적을수록 상위 타입!  
    ```
    
    ---
    
    - 대수타입 
    → 여러개 타입을 합성해서 새롭게 만들어낸 타입
    → 합집합 타입과 교집합 타입이 존재함
    
    - 합집합 타입
    
    ```tsx
    let a: string | number | boolean; 
    a = 1;
    a = "hello";
    a = true; 
    
    // | (바) 로 타입을 추가  
    ```
    
    ```tsx
    type Dog = {
      name: string;
      color: string;
    };
    
    type Person = {
      name: string;
      language: string;
    };
    
    type Union1 = Dog | Person;
    
    let union1 = Union1 = {
     name : "",
     color : "",
     language : "",
    }; 
    
    // 두 객체의 프로퍼티를 모두 가지는 객체 만들 수 있음.
    
     
    let union4: Union1 = { // ❌
      name: "",
    };
    
    // 공통된 하나만 가지는 객체는 오류 (합집합 밖에 있음) 
    ```
    
    - 교집합 타입
    
    ```tsx
    let variable: number & string; 
    // never 타입으로 추론된다
    
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
    };
    
    // 두 타입의 모든 프로퍼티를 가진 객체만 생성 가능하다. 
    ```
    
    ---
    
    - 타입 추론 :  정의되어 있지 않는 변수의 타입을 자동으로 추론함. 일일이 타입을 정의하지 않아도 됨.
    
    - 주의해야 할 상황들
    
    ```tsx
    let d;
    // 암시적인 any 타입으로 추론
    
    //변수에 값을 할당하면 그 다음 라인부터 해당 값 타입으로 변화함.
    d = 10; // number 타입으로 추론 
    
    ------
    
    //상수 
    
    const num = 10;
    // 10 Number Literal 타입으로 추론
    
    const str = "hello";
    // "hello" String Literal 타입으로 추론
    
    let arr = [1, "string"];
    // (string | number)[] 타입으로 추론. 합집합 
    ```
    
    ---
    
    - 타입 단언 : `값  as 타입`으로 원하는 타입으로 단언
    
    ```tsx
    type Person = {
      name: string;
      age: number;
    };
    
    let person = {} as Person;
    person.name = "";
    person.age = 23; 
    ```
    
    타입 단언으로 초과 프로퍼티 검사 피하기
    
    ```tsx
    type Dog = {
      name: string;
      color: string;
    };
    
    let dog: Dog = {
      name: "돌돌이",
      color: "brown",
      breed: "진도", // 원래라면 초과 프로퍼티 검사에 걸림
    } as Dog
    ```
    
    타입 단언 조건  `A as B`
    
    - A가 B의 슈퍼타입이다.
    - A가 B의 서브타입이다.
    
    ```tsx
    let num1 = 10 as never;   // ✅
    let num2 = 10 as unknown; // ✅
    
    let num3 = 10 as string;  // ❌
    ```
    
    - const 단언
    
    ```tsx
    let num4 = 10 as const;
    // 10 Number Literal 타입으로 단언됨
    
    let cat = {
      name: "야옹이", //readonly 프로퍼티가 됨.
      color: "yellow",
    } as const;
    // 모든 프로퍼티가 readonly를 갖도록 단언됨
    ```
    
    - Non Null 단언:  값 뒤에 !를 붙여서 undefined이거나 null이 아니라고 단언
    
    ```tsx
    type Post = {
      title: string;
      author?: string;
    };
    
    let post: Post = {
      title: "게시글1",
    };
    
    const len: number = post.author!.length; //값이 있을거라고 믿어!!
    ```
    
    ---
    
    - 타입 좁히기 : 조건문 등을 이용해 넓은 타입에서 좁은타입으로 좁힘
    
    ```tsx
    function func(value: number | string) {
    //여기서는 value가 union 타입 
      if (typeof value === "number") {  // 조건문 내부에서 value는 number타입
        console.log(value.toFixed()); 
      } else if (typeof value === "string") { // 조건문 내부에서 value는 string타입 
        console.log(value.toUpperCase());
      }
    }
    ```
    
     `if (typeof === …)` —> 타입 가드
    
    instanceof 타입 가드 : 내장 클래스 또는 직접 만든 클래스 
    
    in 타입 가드 : 직접 만든 타입 
    
    ```tsx
    type Person = {
      name: string;
      age: number;
    };
    
    function func(value: number | string | Date | null) {
      if (typeof value === "number") {
        console.log(value.toFixed());
      } else if (typeof value === "string") {
        console.log(value.toUpperCase());
      } else if (value instanceof Date) { // Date 객체일 것임을 보장 
        console.log(value.getTime());
    		else if ("age" in value) { // 내가 만든 타입인 age 
        console.log(`${value.name}은 ${value.age}살 입니다`)
      }
    }
    ```
    
    ---
    
    - 서로소 유니온 타입 : 교집합이 없는 타입들로만 만든 유니온 타입
    
    ```tsx
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
    
    function login(user: User) {
      if ("kickCount" in user) {
    		// Admin
        console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
      } else if ("point" in user) {
    		// Member
        console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
      } else {
    		// Guest
        console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
      }
    }
    
    ------------------- // 아래가 훨씬 직관적임. 
    type Admin = {
      tag: "ADMIN";
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
    
    function login(user: User) {
      if (user.tag === "ADMIN") {
        console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
      } else if (user.tag === "MEMBER") {
        console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
      } else {
        console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
      }
    }
    
    or 
    
    switch (user.tag) {
        case "ADMIN": {
          console.log(`${user.name}님 현재까지 ${user.kickCount}명 추방했습니다`);
          break;
        }
        case "MEMBER": {
          console.log(`${user.name}님 현재까지 ${user.point}모았습니다`);
          break;
        }
        case "GUEST": {
          console.log(`${user.name}님 현재까지 ${user.visitCount}번 오셨습니다`);
          break;
        }
      }
    
    ```
