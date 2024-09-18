
- S9. 조건부 타입
    
    ### 조건부 타입
    
    - 조건부 타입 : extends와 삼항 연산자를 이용해 조건에 따라 타입을 정의
    
    ```tsx
    type A = number extends string ? number : string; // 기본 문법 
    
    //number가 string의 확장이면 number 타입, 아니면 string 타입
    
    type ObjA = {
      a: number;
    };
    
    type ObjB = {
      a: number;
      b: number;
    };
    
    type B = ObjB extends ObjA ? number : string;
    
    // objB가 objA를 확장하면 (A의 서브타입이면) type B는 number 타입 
    => number 타입임
    ```
    
    - 제네릭과 조건부 타입
    
    ```tsx
    type StringNumberSwitch<T> = T extends number ? string : number;
    
    // T에 number가 들어오면 string 타입, 아니면 number 타입 할당
    
    let varA: StringNumberSwitch<number>;  
    // string
    
    let varB: StringNumberSwitch<string>;
    // number
    ```
    
    활용1
    
    ```tsx
    
    // 함수 오버로딩 이용
    function removeSpaces<T>(text: T): T extends string ? string : undefined; // 오버로드 시그니쳐
    
    function removeSpaces(text: any) {
      if (typeof text === "string") {
        return text.replaceAll(" ", "");
      } else {
        return undefined;
      }
    }
    
    let result = removeSpaces("hi im winterlood");
    // string
    
    let result2 = removeSpaces(undefined);
    // undefined
    ```
    
    활용2
    
    ```tsx
    type Exclude<T, U> = T extends U ? never : T;
    
    type A = Exclude<number | string | boolean, string>;
    
    // 1 단계
    // Exclude<number, string> |
    // Exclude<string, string> |
    // Exclude<boolean, string> 
    
    // 2 단계
    // number |
    // never |
    // boolean  
    
    // 결과 
    // number | boolean 
    ```
    
    참고 : 분산적 조건부 타입을 막는 법 - 대괄호를 씌워주면 됨.
    
    ```tsx
    type Exclude<T, U> = [T] extends [U] ? never : T;
    ```
    
    - infer : 조건부 타입 내에서 특정 타입을 추론하는 문법
    
    ```tsx
    type ReturnType<T> = T extends () => infer R ? R : never; 
    // 비교하는 타입을 () => R 모양으로 비교, R은 이 조건식이 참이 되는 타입을 추론함. 
    // inference의 약자
    
    type FuncA = () => string;
    
    type FuncB = () => number;
    
    type A = ReturnType<FuncA>;
    // string
    
    type B = ReturnType<FuncB>;
    // number
    
    type C = ReturnType<number>;
    // 조건식을 만족하는 R추론 불가능
    // never
    
    type PromiseUnpack<T> = T extends Promise<infer R> ? R : never;
    // 1. T는 프로미스 타입이어야 한다.
    // 2. 프로미스 타입의 결과값 타입을 반환해야 한다.
    
    type PromiseA = PromiseUnpack<Promise<number>>;
    // number
    
    type PromiseB = PromiseUnpack<Promise<string>>;
    // string
    ```
    
- S10. 유틸리티 타입
    
    
    - Partial<T> : 특정 객체 타입의 모든 프로퍼티를 선택적 프로퍼티로 바꿔주는 타입
    
    ```tsx
    interface Post {
      title: string;
      tags: string[];
      content: string;
      thumbnailURL?: string;
    }
    
    type Partial<T> = { // Partial<T> 구현
      [key in keyof T]?: T[key];
    };
    
    const draft: Partial<Post> = { // Partial<T> 타입으로 모든 프로퍼티를 선택적으로
      title: "제목은 나중에 짓자...",
      content: "초안...",
    };
    ```
    
    - Required<T> : 특정 객체 타입의 모든 프로퍼티를 필수 프로퍼티로 바꿔주는 타입
    
    ```tsx
    interface Post {
      title: string;
      tags: string[];
      content: string;
      thumbnailURL?: string;
    }
    
    type Required<T> = {
      [key in keyof T]-?: T[key];
    };
    
    const withThumbnailPost: Required<Post> = { 
      title: "한입 타스 후기",
      tags: ["ts"],
      content: "",
      thumbnailURL: "https://...",
    };
    ```
    
    - Readonly : 모든 프로퍼티를 읽기 전용 프로퍼티로 변환
    
    ```tsx
    interface Post {
      title: string;
      tags: string[];
      content: string;
      thumbnailURL?: string;
    }
    
    type Readonly<T> = {
      readonly [key in keyof T]: T[key];
    };
    
    const readonlyPost: Readonly<Post> = {
      title: "보호된 게시글입니다.",
      tags: [],
      content: "",
    };
    
    readonlyPost.content = '해킹당함'; // 오류
    ```
    
    - Pick<T, K> :  특정 객체 타입으로부터 특정 프로퍼티 만을 골라내는 타입
    
    ```tsx
    interface Post {
      title: string;
      tags: string[];
      content: string;
      thumbnailURL?: string;
    }
    
    const legacyPost: Pick<Post, "title" | "content"> = {
      title: "",
      content: "",
    };
    // 추출된 타입 : { title : string; content : string }
    ```
    
    - Omit<T, K> : 특정 프로퍼티를 제거하는 타입
    
    ```tsx
    const noTitlePost: Omit<Post, "title"> = {
      content: "",
      tags: [],
      thumbnailURL: "",
    };
    
    type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
    //Omit 구현
    ```
    
    - Record<K, V> :  동일한 패턴을 갖는 객체 타입 쉽게 정의
    
    ```tsx
    type Thumbnail = Record<
      "large" | "medium" | "small",
      { url: string }
    >;
    
    ->
    type Thumbnail = {
      large: {
        url: string;
      };
      medium: {
        url: string;
      };
      small: {
        url: string;
      };
    }; //와 같은 코드가 됨.
    
    type Record<K extends keyof any, V> = {
      [key in K]: V;
    };
    
    //구현
    ```
    
    - Exclude<T,K> : T로부터 K를 제거
    
    ```tsx
    type A = Exclude<string | boolean, string>;
    // boolean
    
    type Exlcude<T, U> = T extends U ? never : T;
    //구현
    ```
    
    - Extract<T, K> : T로부터 K를 추출
    
    ```tsx
    type B = Extract<string | boolean, boolean>;
    // boolean
    
    type Extract<T, U> = T extends U ? T : never;
    ```
    
    - ReturnType<T>
    
    ```tsx
    type ReturnType<T extends (...args: any) => any> = T extends (
      ...agrs: any
    ) => infer R
      ? R
      : never;
    
    function funcA() {
      return "hello";
    }
    
    function funcB() {
      return 10;
    }
    
    type ReturnA = ReturnType<typeof funcA>;
    // string
    
    type ReturnB = ReturnType<typeof funcB>;
    // number
    ```
