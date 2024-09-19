# 3주차 (10 ~

## 조건부 타입

조건부 타입은  extends와 삼항 연산자를 이용해 조건에 따라 각각 다른 타입을 정의하도록 돕는 문법

```tsx
type A = number extends string ? number : string;
//number가 string을 extends 했는지 판단해보고 -> 참/거짓에 따라 다른 타입 할당
```

기본형만 쓰는 걸 얼핏 보면 쓸모 없어보이지만,  **제네릭과 함께 사용할 때 그 위력이 극대화된다.**

우선 text의 타입이 3가지가 가능하다고 가정하자. 그럼 이를 정상적으로 사용하려면 타입 가드를 사용해야한다. (replaceAll 이라는 함수는 string에서밖에 사용이 안되기 때문) 

```tsx
function removeSpaces(text: string | undefined | null) {
  if (typeof text === "string") {
    return text.replaceAll(" ", ""); 
  } else {
    return undefined;
  }
} 

let result = removeSpaces("hi im winterlood");
reslult.toUpperCase();
// string | undefined
```

그러나 타입 가드를 사용해도, 저 함수를 거쳐서 나온 결과물(result)는 string이나 undefined 로 추론된다는 문제점이 있다.

그래서 이렇게 조건부 타입을 사용한다. 

```tsx
function removeSpaces<T>(text: T): T extends string ? string : undefined {
  if (typeof text === "string") {
    return text.replaceAll(" ", ""); // ❌
  } else {
    return undefined; // ❌
  }
} 

let result = removeSpaces("hi im winterlood");
// string

let result2 = removeSpaces(undefined);
// undefined
```

단, 이렇게 되면 조건부 타입의 결과를 함수 내부에서 알 수 없게 되어 경고 문구가 뜬다.  이럴 땐 함수 오버로딩을 적용하여 해결한다.

```tsx
function removeSpaces<T>(text: T): T extends string ? string : undefined;
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

단순히 as any를 선언하는 것보다는 낫다.

## 분산적인 조건부 타입

조건부 타입을 유니온 타입과 같이 사용할 때 조건부 타입이 분산적으로 작동하게 업그레이드 되는 경우를 의미함! 

```tsx
type StringNumberSwitch<T> = T extends number ? string : number;

(...)

let c: StringNumberSwitch<number | string>;
// string | number
```

지금까지 배운 조건부 타입 문법에 따라 변수 c의 타입은 `number | string`은 `number`의 서브타입이 아니므로 조건식이 거짓이 되어 `number`가 될거라고 예상할 수 있다.

 변수 c는 `string | number` 타입으로 정의된다.

분산적인 조건부 타입은 다음과 같이 동작하기 때문이다.

타입 변수에 할당한 Union 타입 내부의 모든 타입이 분리된다. 즉, StringNuberSwitch<number | string> 타입은 다음과 같이 분산된다.

- StringNumberSwitch<number>
- StringNumberSwitch<string>

그리고 다음으로 분산된 각 타입의 결과를 모아 다시 Union 타입으로 묶인다.

- 결과 : number | string

이를 활용하면, 유니온 타입에서 특정 타입만 제거하는 Exclude 라는 타입도 정의할 수 있다.

```tsx
type Exclude<T, U> = T extends U ? never : T;

type A = Exclude<number | string | boolean, string>;
```

- 자세한 내용
    
    위 코드는 다음의 흐름으로 동작합니다.
    
    1. Union 타입이 분리된다.
        - `Exclude<number, string>`
        - `Exclude<string, string>`
        - `Exclude<boolean, string>`
    2. 각 분리된 타입을 모두 계산한다.
        - `T = number`, `U = string` 일 때 `number extends string` 은 거짓이므로 결과는 `number`
        - `T = string`, `U = string` 일 때 `string extends string` 은 참이므로 결과는 `never`
        - `T = boolean`, `U = string` 일 때 `boolean extends string` 은 거짓이므로 결과는 `boolean`
    3. 계산된 타입들을 모두 Union으로 묶는다
        - 결과 : `number | never | boolean`
    
    계산 결과 타입 A는 `number | never | boolean` 타입으로 정의됩니다. 그런데 여기서 공집합을 의미하는 `never` 타입은 Union으로 묶일 경우 사라집니다. 그 이유는 공집합과 어떤 집합의 합집합은 그냥 원본 집합이 되기 때문입니다.
    
    따라서 최종적으로 타입 A는 `number | boolean` 타입이 됩니다.
    

아, 참고로 조건부 타입이 분산되어 작동하지 않게 하고 싶다면

```tsx
type StringNumberSwitch<T> = [T] extends [number] ? string : number;
```

이렇게 대괄호를 씌워주면 된다

## **infer**

다음과 같이 사용하면 

```tsx
type ReturnType<T> = T extends () => string ? string : never;

type FuncA = () => string;

type FuncB = () => number;

type A = ReturnType<FuncA>;
// string

type B = ReturnType<FuncB>;
// never
```

함수의 반환 타입을 가져오길 원했으나, 딱 하나의 타입만(string) 고정되어 그 타입 여부만 검사한다. 따라서 목적에 맞는 기능을 하기 위해서, infer를 사용한다.

```tsx
type ReturnType<T> = T extends () => infer R ? R : never;

type FuncA = () => string;

type FuncB = () => number;

type A = ReturnType<FuncA>;
// string

type B = ReturnType<FuncB>;
// number
```

infer를 사용하면 참이 될 수 있게 하는 값인 R을 추론한다. 만약 타입을 추론할 수 없으면 false를 반환한다.

```tsx
type ReturnType<T> = T extends () => infer R ? R : never;

type FuncA = () => string;

type FuncB = () => number;

type A = ReturnType<FuncA>;
// string

type B = ReturnType<FuncB>;
// number

type C = ReturnType<number>;
// 조건식을 만족하는 R추론 불가능
// never
```

마지막으로 Promise의 예시를 들어보자 .

```tsx
type PromiseUnpack<T> = T extends Promise<infer R> ? R : never;
// 1. T는 프로미스 타입이어야 한다.
// 2. 프로미스 타입의 결과값 타입을 반환해야 한다.

type PromiseA = PromiseUnpack<Promise<number>>;
// number

type PromiseB = PromiseUnpack<Promise<string>>;
// string
```

## 유틸리티 타입

유틸리티 타입이란 타입스크립트가 자체적으로 제공하는 특수한 타입들이다. 우리가 지금까지 배웠던 제네릭, 맵드 타입, 조건부 타입 등의 타입 조작 기능을 이용해 실무에서 자주 사용되는 유용한 타입들을 모아 놓은 것을 의미한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/7e599313-0b74-4d2f-9f1f-b0364f7bbf30/image.png)

이 유틸리티 타입을 직접 정의해보는 시간을 가져보자~
(기존 객체 타입을 다른 타입으로 변환하는 타입인 맵드 타입을 이용) 

## Partial, Required, Readonly

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

const draft: Post = { // ❌ tags 프로퍼티가 없음
  title: "제목은 나중에 짓자...",
  content: "초안...",
};
```

### Partial : 모든 프로퍼티를 옵셔널로

```tsx
type Partial<T> = {
  [key in keyof T]?: T[key];
};
```

### Required : 모든 프로퍼티를 필수로

```tsx
type Required<T> = {
  [key in keyof T]-?: T[key]; //-? 를 통해 선택적인걸 제거한다
};
```

### Readonly : 모든 프로퍼티를 읽기 전용으로

```tsx
type Readonly<T> = {
  readonly [key in keyof T]: T[key];
};
```

### **Pick<T, K> → T에서 K타입만 뽑아오는 타입**

사용 예시

```tsx
interface Post {
  title: string;
  tags: string[];
  content: string;
  thumbnailURL?: string;
}

(...)

const legacyPost: Pick<Post, "title" | "content"> = {
  title: "",
  content: "",
};
// 추출된 타입 : { title : string; content : string }
```

**직접 구현해보기**

`K extends keyof T`  가 들어가야하는 이유는, K가 객체 T 타입에서 타입을 뽑아왔다는 것을 알리면서, 상속 받았다는 것을 알리기 위함이다. 즉, K가 T의 key로만 이루어진 String Literal Union 타입임을 보장하기 위해서이다. 

```tsx
type Pick<T, K extends keyof T> = {
  [key in K]: T[key];
};
```

### **Omit<T, K>→ 객체 타입으로부터 특정 프로퍼티를 제거하는 타입**

**직접 구현해보기** 

```tsx
type Omit<T, K extends keyof T> = Pick<T, Exclude<keyof T, K>>;
```

조금 어려워보여도 차근 차근 이해해보면, T의 유니온 타입에서 K를 제거한 유니온타입을 Pick 해오는 것이 Omit의 동작 방식이라는 것이다.

### **Record<K, V>**

언제 사용할까? 우선 다음과 같이 value 는 같은데 속성을 일일히 정의해줘야 하는 경우를 생각해보자.

```tsx
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
};
```

만약 같은 속성이 추가되면 중복된 코드 작성 뿐만 아니라, 수정이 발생한다면 전부 수정해야하는 문제가 발생한다. 

이때, Record 타입을 사용하면 간편하게 구현할 수 있다.

```tsx
type Thumbnail = Record<
  "large" | "medium" | "small",
  { url: string }
>;
```

저 유니온 타입 각각에 두번째 인자 값을 똑같이 넣어주어 타입을 생성한다. Record 타입을 직접 구현하면 다음과 같다.

K extends keyof any 라고 작성해준 이유도, 어떤 타입일지는 몰라도 객체 타입의 키 타입이라는 것을 알림으로써, in 연산자로 사용할 수 있도록(in 연산자 우측에 들어갈 수 있도록) 제약을 걸어두는 것이다.

```tsx
type Record<K extends keyof any, V> = {
  [key in K]: V;
};
```

(이제 3개는 이미 위에서 한번 했으니 간단하게)

### Exclude<T,U> → T로부터 U를 제거하는 타입

```tsx
type Exlcude<T, U> = T extends U ? never : T;
```

### Extract<T,U> → T에서 U를 뽑아오는 타입

```tsx
type Extract<T, U> = T extends U ? T : never;
```

### ReturnType<T> → 리턴값을 추론해주는 타입

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
