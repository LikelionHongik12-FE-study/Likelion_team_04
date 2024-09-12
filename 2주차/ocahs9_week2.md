# 2주차 (5 ~ 9)

### 함수 타입을 정의하는 방법!

<aside>
💡

매개변수, 리턴값의 타입을 정의해주면 된다!

</aside>

```tsx
function func(a: number, b: number): number {
  return a + b; //이 경우 반환값은 number로 추론되긴 한다.
}
```

```tsx
const add = (a: number, b: number): number => a + b;
```

### 매개변수 기본값 설정 - 자동 타입 추론

```tsx
function introduce(name = "이정환") {
	console.log(`name : ${name}`);
} 

introduce(1); // 오류
```

이 경우엔 name이 string으로 추론된다.  따라서 
`introduce(1); // 오류` 은 오류일 수 밖에 없다.

- 참고
    
    ```tsx
    function introduce(name:number = "이정환") {
    	console.log(`name : ${name}`);
    } 
    
    introduce(1); // 오류
    ```
    
    name: number의 경우에도 기본값을 바탕으로 먼저 타입이 추론되기 때문에 에러가 발생한다 !
    

### 선택적 매개변수 설정 - 반드시 맨 뒤에!

선택적 매개변수는 반드시 필수 매개변수들 뒤에 와야한다.

또한, 선택적 매개변수는 undefined로 추론될 수 있기 때문에 타입가드(타입 좁히기)를 사용해서 적절한 타입으로 추론하여 계산해야 한다. (무지성 as 는 x)

```tsx
function introduce(name = "이정환", tall?: number) {
  console.log(`name : ${name}`);
  if (typeof tall === "number") {
    console.log(`tall : ${tall + 10}`);
  }
}
```

### rest 파라미터의 경우

rest 파라미터는 가변적인 길이의 파라미터들을 배열로 받아오는 방식이었다. 따라서 타입도 배열 형식으로 지정한다.

```tsx
function getSum(...rest: number[]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));
  return sum;
}
```

```tsx
function getSum(...rest: [number, number, number]) {
  let sum = 0;
  rest.forEach((it) => (sum += it));
  return sum;
}

getSum(1, 2, 3)    // ✅
getSum(1, 2, 3, 4) // ❌
```

### 함수 타입 표현식

함수 타입을 타입 별칭과 함께 별도로 정의하는 방식

```tsx
type Operation = (a: number, b: number) => number;

const add: (a: number, b: number) => number = (a, b) => a + b;
const sub: Operation = (a, b) => a - b;
const multiply: Operation = (a, b) => a * b;
const divide: Operation = (a, b) => a / b;
```

### 호출 시그니쳐

함수 타입 표현식과 동일하게 함수의 타입을 별도로 정의하는 방식

```tsx
type Operation2 = {
  (a: number, b: number): number;
};

const add2: Operation2 = (a, b) => a + b;
const sub2: Operation2 = (a, b) => a - b;
const multiply2: Operation2 = (a, b) => a * b;
const divide2: Operation2 = (a, b) => a / b;
```

객체 형태로 타입을 정의하는 이유는, 함수도 객체이기 때문!

https://reactjs.winterlood.com/0f33b159-6b19-433b-8db4-68d6b4a122e0

따라서,  호출 시그니쳐 아래에 프로퍼티를 추가 정의하는 것도 가능하다.

```tsx
type Operation2 = {
  (a: number, b: number): number;
  name: string;
};

const add2: Operation2 = (a, b) => a + b;
(...)

add2(1, 2);
add2.name;
```

함수이자 일반 객체를 의미하는 타입으로 정의되며 이를 하이브리드 타입이라고 부른다.

## 함수 타입의 호환성 (중요, 어려움)

함수 타입의 호환성이란 특정 함수 타입을 다른 함수 타입으로 괜찮은지 판단하는 것을 의미

다음 2가지 기준으로 함수 타입의 호환성을 판단!

- 두 함수의 반환값 타입이 호환되는가?
- 두 함수의 매개변수의 타입이 호환되는가?

> **두 함수의 반환값 타입이 호환되는가?**
> 

```tsx
type A = () => number;
type B = () => 10;

let a: A = () => 10;
let b: B = () => 10;

a = b; // ✅ (업캐스팅이므로 가능)
b = a; // ❌ (다운캐스팅이므로 불가능) 
```

> **두 함수의 매개변수의 타입이 호환되는가?**
> 

### 2-1) **매개변수의 개수가 같을 때**

얼핏 보면 다운캐스팅을 허용하는 것처럼 보임

```tsx
type C = (value: number) => void;
type D = (value: 10) => void;

let c: C = (value) => {};
let d: D = (value) => {};

c = d; // ❌
d = c; // ✅ (다운캐스팅인데.. 허용된다고? 이유는?)
```

근데 이를 매개변수 타입이 객체일 때를 상상해보면 그 이유가 명확해진다. 

```tsx
type Animal = {
  name: string;
};

type Dog = {
  name: string;
  color: string;
};

let animalFunc = (animal: Animal) => {
  console.log(animal.name);
};

let dogFunc = (dog: Dog) => {
  console.log(dog.name);
  console.log(dog.color);
};

animalFunc = dogFunc; // ❌ (업캐스팅되는건데도 안됨)
dogFunc = animalFunc; // ✅ (다운캐스팅인데 됨)
```

이 경우 Animal이 슈퍼타입이다. (조건이 더 적으니까) 

그러나 잘 생각해보면 오히려 서브타입이면 조건이 더 많다는 뜻이고, 이 서브타입이 업캐스팅되어 슈퍼타입에 할당된다면 에러가 발생한다.

```tsx
let testFunc = (animal: Animal) => {
	console.log(animal.name); 
	console.log(animal.color); //이건 불가능
}
```

따라서 저렇게 할당이 되는 것이다. 

### 2-2) **매개변수의 개수가 다를 때**

**매개변수의 개수가 더 적으면 할당이 가능하다.**

```tsx
type Func1 = (a: number, b: number) => void;
type Func2 = (a: number) => void;

let func1: Func1 = (a, b) => {};
let func2: Func2 = (a) => {};

func1 = func2; // ✅  
func2 = func1; // ❌
```

아, 언급은 안했는데 매개변수의 타입 자체가 다르면 당연히 타입이 호환되지 않는다.

## 함수 오버로딩

하나의 함수를 매개변수의 개수나 타입에 따라 다르게 동작하도록 만드는 문법 (JS에서는 지원 안함. TS에서만 지원함)

### 오버로드 시그니쳐, 구현 시그니쳐

오버로드 시그니쳐 : 함수의 버전들 정의 → 이걸로 타입 판단

구현 시그니쳐 : 실제 함수 구현부 

```tsx
// 버전들 -> 오버로드 시그니쳐
function func(a: number): void;
function func(a: number, b: number, c: number): void;

// 실제 구현부 -> 구현 시그니쳐
// 실제 동작하는 부분이기 때문에, 첫번째 버전과 맞으려면 B,C는 옵셔널로 정의해주어야 한다
function func(a: number, b?: number, c?: number) {
  if (typeof b === "number" && typeof c === "number") {
    console.log(a + b + c);
  } else {
    console.log(a * 20);
  }
}

func(1);        // ✅ 버전 1 - 오버로드 시그니쳐
func(1, 2);     // ❌ (오버로드 시그니처에 없음)
func(1, 2, 3);  // ✅ 버전 3 - 오버로드 시그니쳐
```

대부분의 타입스크립트 라이브러리들은 함수 오버로딩을 통해 정의되어 있으므로 잘 알아둘 것 !

## 사용자 정의 타입 가드

안좋은 케이스

```tsx
type Dog = {
  name: string;
  isBark: boolean;
};

type Cat = {
  name: string;
  isScratch: boolean;
};

type Animal = Dog | Cat;

function warning(animal: Animal) {
  if ("isBark" in animal) {
    console.log(animal.isBark ? "짖습니다" : "안짖어요");
  } else if ("isScratch" in animal) {
    console.log(animal.isScratch ? "할큅니다" : "안할퀴어요");
  }
}
```

이 경우엔,  in 연산자를 이용해 타입을 좁히므로  타입의 프로퍼티가 중간에 이름이 수정되거나 추가 또는 삭제될 경우에는 타입 가드가 제대로 동작하지 않을 수 있다. 

좋은 케이스 

```tsx
(...)- Dog, Cat, Animal 타입 정의

// Dog 타입인지 확인하는 타입 가드
function isDog(animal: Animal): animal is Dog {
  return (animal as Dog).isBark !== undefined;
}

// Cat 타입인지 확인하는 타입가드
function isCat(animal: Animal): animal is Cat {
  return (animal as Cat).isScratch !== undefined;
}

function warning(animal: Animal) {
  if (isDog(animal)) {
    console.log(animal.isBark ? "짖습니다" : "안짖어요");
  } else {
    console.log(animal.isScratch ? "할큅니다" : "안할퀴어요");
  }
}
```

- 타입 가드용 함수를 하나 작성한다.
    - 리턴값은 is 문법을 사용해서 타입을 명확히 알려준다. 그럼 isDog을 거쳐서 나온 animal은 이제 Dong타입임을 알수 있게 된다.

## 인터페이스

타입에 이름을 지어주는 또 다른 문법 (상호간에 약속된 규칙) → 객체 타입 정의에 특화되어 있다. (추가적인 기능들도 존재함) 

```tsx
interface Person {
  readonly name: string; //읽기 전용 프로퍼티
  age?: number; //옵셔널 프로퍼티 
}

const person: Person = {
  name: "이정환",
  // age: 27,
};

person.name = '홍길동' // ❌
```

### 메서드 타입 정의하기

```tsx
interface Person {
  readonly name: string;
  age?: number;
  sayHi: () => void; //함수 타입 표현식
}
```

```tsx
interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void; //호출 시그니쳐
}
```

**함수 타입 표현식 사용하지 말고,  호출 시그니쳐 사용하기 !**

→ 함수 타입 표현식은 메서드 오버로딩을 인식 못하기 때문

### **메서드 오버로딩**

```tsx
interface Person {
  readonly name: string;
  age?: number;
  sayHi: () => void; 
  sayHi: (a: number, b: number) => void; // ❌
}
```

```tsx
interface Person {
  readonly name: string;
  age?: number;
  sayHi(): void; 
  sayHi(a: number): void; //정상적으로 인식!
  sayHi(a: number, b: number): void;
}
```

### 하이브리드 타입

인터페이스 또한 함수이자 일반 객체인 하이브리드 타입을 정의 가능

```tsx
interface Func2 {
  (a: number): string;
  b: boolean;
}

const func: Func2 = (a) => "hello";
func.b = true;
```

### 인터페이스에서 주의해야할 점

인터페이스로 만든 타입을 Union 이나 Intersection으로 사용하고 싶다면 타입 별칭과 함께 사용하거나 타입 주석에서 직접 사용해야 한다

```tsx
type Type1 = number | string;
type Type2 = number & string;

interface Person {
  name: string;
  age: number;
} | number // ❌ 타입별칭 방식은 불가능 
```

```tsx
type Type1 = number | string | Person;
type Type2 = number & string & Person;

//이런 식으로 사용하는 것은 가능
const person: Person & string = {
  name: "이정환",
  age: 27,
};
```

## 인터페이스의 확장(extends)

```tsx
interface Animal {
  name: string;
  age: number;
}

interface Dog {
  name: string;
  age: number;
  isBark: boolean;
}

interface Cat {
  name: string;
  age: number;
  isScratch: boolean;
}

interface Chicken {
  name: string;
  age: number;
  isFly: boolean;
}
```

속성의 중복이 너무 많다. 하나가 수정되면 다른 것들도 일일히 전부 수정해주어야 하는 번거로움이 늘어난다.

```tsx
interface Animal {
  name: string;
  color: string;
}

interface Dog extends Animal {
  breed: string;
}

interface Cat extends Animal {
  isScratch: boolean;
}

interface Chicken extends Animal {
  isFly: boolean;
}
```

따라서, extends 문법을 사용해서 슈퍼타입의 모든 프로퍼티를 가져온다. 이게 인터페이스의 확장이다!

### **프로퍼티 재 정의하기**

또한, 확장했을 때 서브타입에서 프로퍼티를 재정의할 수 있다. 단, 재정의되는 프로퍼티는 반드시 원본의 서브타입이어야 한다.

```tsx
interface Animal {
  name: string;
  color: string;
}

interface Dog extends Animal {
  name: "doldol"; // 타입 재 정의 
  //name: number ; 와 같은 건 불가능! (서브타입 아니니까) 
  breed: string;
}
```

### **타입 별칭을 확장하기**

뿐만 아니라 인터페이스만 확장해서 가져오는 것이 아니라, type도 확장해서 가져올 수 있다.

```tsx
type Animal = {
  name: string;
  color: string;
};

interface Dog extends Animal {
  breed: string;
}
```

### 다중 확장

여러개의 인터페이스를 확장하는 것도 ㅏ능하다. 

```tsx
interface DogCat extends Dog, Cat {}

const dogCat: DogCat = {
  name: "",
  color: "",
  breed: "",
  isScratch: true,
};
```

## 인터페이스 합치기

**인터페이스는 같은 이름으로 중복 선언되었을 때, 자동으로 합쳐진다. 즉, 선언 합침이 이루어진다.**

```tsx
interface Person {
  name: string;
}

interface Person {
  age: number;
}

const person: Person = {
  name: "이정환",
  age: 27,
};
```

이렇게 2개의 프로퍼티를 갖는 Person 인터페이스가 탄생한다. 이를 **선언 합침(Declaration Merging)**이라 한다.

cf) 참고로 , 타입은 선언 합침이 존재하지 않는다

```tsx
type Person = {
  name: string;
};

type Person = { ❌
  age: number;
}; 
```

타입은 동일한 스코프 내에 중복된 이름으로 선언할 수 없다.

### 선언 합침 시 주의해야 할 점

**선언 합침 시에**는 동일한 프로퍼티를 정의하게 되더라도, **아예 같은 타입**이어야 한다. (원본의 서브타입이어도 안된다.)

```tsx
interface Person {
  name: string;
}

interface Person {
  name: number; // 불가능
  //name: "12323" -> 얘도 불가능 (타입 재정의)
  age: number;
}
```

 프로퍼티의 타입을 다르게 정의한 상황을 ‘충돌’ 이라고 표현 → 즉, 충돌은 허용되지 않는다. 

### 그럼 선언 합침은 언제 쓰는가?

보통 모듈 보강을 할 때 주로 쓴다.

라이브러리 모듈에서 추가적으로 필요한 타입이 있을 때, 직접 선언 합침의 개념을 사용해서 타입을 정의한다.

## 클래스

### 자바스크립트의 클래스

src 파일내부어 .js 파일을 만들었더니 tsconfig.json 에서 에러가 남. → “include: [”src”]” 를 통해 해당 폴더를 전부 관제하고 있기 때문에, 잘못된 거 아니냐는 메세지임.

따라서 compilerOptions 객체 안에 “allowJs” : true 를 추가해주어 해결하면 된다.

```tsx
class Student {
  // 필드
  name;
  grade;
  age;

  // 생성자 
  constructor(name, grade, age) {
    this.name = name;
    this.grade = grade;
    this.age = age;
  }

  // 메서드
  study() {
    console.log("열심히 공부 함");
  }

  introduce() { 
    console.log(`안녕하세요! ${this.name} 입니다!`);
  }
}

//new를 통해 student 객체(인스턴스)를 생성
//반드시 new가 있어야하며, Student()는 사실상 생성자 호출이나 마찬가지이다.
let studentB = new Student("홍길동", "A+", 27);

studentB.study(); // 열심히 공부 함
studentB.introduce(); // 안녕하세요!
```

 this는 현재 만들고 있는 객체를 의미한다. 

클래스 정의에는 필드, 생성자, 메서드가 존재한다. 

### 클래스 상속 (혹은 확장)  - extends

주석 참고 !

```tsx
class StudentDeveloper extends Student {
  // 필드
  favoriteSkill;

  // 생성자 (여기서는 name, grade, age가 생략되어서는 안됨)
  constructor(name, grade, age, favoriteSkill) {
  //Student의 생성자를 호출하여 기본 작업을 하고, 추가적인 필드에 대한 작업을 한다.
    super(name, grade, age); //부모의 생성자를 호출함
    this.favoriteSkill = favoriteSkill;
  }

  // 메서드
  programming() {
    console.log(`${this.favoriteSkill}로 프로그래밍 함`);
  }
}
```

### 타입스크립트 클래스

```tsx
class Employee {
  // 필드
  name: string //에러 뜸.
  age: number 
  position: string 

  // 메서드
  work() {
    console.log("일함");
  }
}
```

속성 “~~”는 이니셜라이저가 없고 생성자에 할당되어 있지 않습니다.  

라는 문구가 뜬다. 이는 초기값이 없으면 어차피 undefined 으로 추론되고, 필드에 선언한 타입과 맞지 않기 때문이다. 

이를 해결하기 위해 필드 모두에 ? (선택적 프로퍼티)로 만들어주는 방법이 있다. → 별로 좋은 방법은 아님

따라서 만약 할당할만한 기본 값이 있다면,  기본값을 설정해주는 것도 방법이긴 하다.

```tsx
class Employee {
  // 필드
  name: string = "";
  age: number = 0;
  position: string = "";

  // 메서드
  work() {
    console.log("일함");
  }
}
```

이것보다 더더욱 좋은 방법은, 그냥 생성자에서 값을 할당하도록 하는 것이다.

```tsx
class Employee {
  // 필드
  name: string = "";
  age: number = 0;
  position: string = "";

  // 생성자
  constructor(name: string, age: number, position: string) {
    this.name = name;
    this.age = age;
    this.position = position;
  }

  // 메서드
  work() {
    console.log("일함");
  }
}
```

### 상속

js와 마찬가지이다. 다만, super를 만들어달라는 경고문구가 뜬다.

```tsx
class ExecutiveOfficer extends Employee {
  officeNumber: number;

  constructor(
    name: string,
    age: number,
    position: string,
    officeNumber: number
  ) {
    super(name, age, position);
    this.officeNumber = officeNumber;
  }
}
```

### 클래스는 타입

```tsx
class Employee {
  (...)
}

const employeeC: Employee = {
  name: "",
  age: 0,
  position: "",
  work() {},
};
```

클래스를 타입으로 사용하면 해당 클래스가 생성하는 객체의 타입과 동일한 타입이 생성된다.

**구조적 타입 형식이기 때문에 가능**

### 접근 제어자(타입스크립트에서만 가능함)

public, private등을 사용 가능.

타입스크립트에서는 다음과 같은 3개의 접근 제어자를 사용가능!

- public : 모든 범위에서 접근 가능
- private : 클래스 내부에서만 접근 가능
- proteced : 클래스 내부 또는 파생 클래스 내부에서만 접근 가능

```tsx
class Employee {
  // 필드
  private name: string; // private 접근 제어자 설정
  protected age: number;
  public position: string;

  ...

  // 메서드
  work() {
    console.log(`${this.name}이 일함`); // 여기서는 접근 가능
  }
}

class ExecutiveOfficer extends Employee {
 // 메서드
  func() {
    this.name; // ❌ 오류 
    this.age; // ✅ 가능
  }
}

const employee = new Employee("이정환", 27, "devloper");

employee.name = "홍길동"; // ❌ 오류
employee.age = 30; // ❌ 오류
employee.position = "디자이너";
```

생성자에 접근제어자를 달아주게 된다면 필드를 생성하게 된다. 따라서 생성자에 접근제어자를 사용할거라면 필드는 생략해야만 한다. 

```tsx
class Employee {
  // 필드
  private name: string;    // ❌
  protected age: number;   // ❌
  public position: string; // ❌

  // 생성자
  constructor(
    private name: string,
    protected age: number,
    public position: string
  ) {
    this.name = name;
    this.age = age;
    this.position = position;
  }

  // 메서드
  work() {
    console.log(`${this.name} 일함`);
  }
}
```

생략하면 다음과 같다.

```tsx
class Employee {
  // 생성자
  constructor(
    private name: string,
    protected age: number,
    public position: string
  ) {
    this.name = name;
    this.age = age;
    this.position = position;
  }

  // 메서드
  work() {
    console.log(`${this.name} 일함`);
  }
}
```

또 다음과 접근 제어자가 설정된 매개변수들은 `this.필드 = 매개변수가` 자동으로 수행되므로 생략 가능하다.

```tsx
class Employee {
  // 생성자
  constructor(
    private name: string,
    protected age: number,
    public position: string
  ) {}

  // 메서드
  work() {
    console.log(`${this.name} 일함`);
  }
}
```

### **인터페이스를 구현하는 클래스**

타입스크립트의 인터페이스는 클래스의 설계도 역할을 할 수 있다.

```tsx
/**
 * 인터페이스와 클래스
 */

interface CharacterInterface {
  name: string;
  moveSpeed: number;
  move(): void;
}

//참고로 interface에 정의된 필드들은 모두 public이다.
//protected나 private를 사용하고 싶으면 추가 필드를 사용해야한다.
class Character implements CharacterInterface {
  constructor(
    public name: string,
    public moveSpeed: number,
    private extra: string
  ) {}

  move(): void {
    console.log(`${this.moveSpeed} 속도로 이동!`);
  }
}
```

## 제네릭

### 제네릭 소개( 타입스크립트의 강력한 기능)

일반적인, 포괄적인 

<aside>
💡

제네릭이란 함수나 인터페이스, 타입 별칭, 클래스 등을 다양한 타입과 함께 동작하도록 만들어 주는 타입스크립트의 놀라운 기능 중 하나!

</aside>

**제네릭(Generic) 함수**

사용방법은 간단하다.

<> 안에 제네릭 타입을 쓴다. 그리고 그걸 매개변수나 리턴값 타입 정의에 사용하면 된다.

```tsx
function func<T>(value: T): T {
  return value;
}
//알아서 타입 추론한 결과를 제네릭 타입 T로 넘겨줌
let arr = func([1, 2, 3]);
//이렇게 하면 제네릭 타입 T가 튜플 타입임을 알려주면서 넘겨줌
let arr = func<[number, number, number]>([1, 2, 3]);
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/50c230df-a714-40a7-8412-3063c96fa504/fb4a251a-1c42-494c-935a-0d327007e494/image.png)

그러나, 변수의 타입 2개가 다를 수 있다. 그럴 땐 첫번째 인자값으로 넣은 값을 기반으로 타입을 추론하기 때문에 타입의 불일치가 존재할 수 있다.

### 타입변수 응용하기

그럴 땐 제네릭 타입을 2가지 사용하면 된다.

```tsx
function swap<T, U>(a: T, b: U) {
  return [b, a];
}

const [a, b] = swap("1", 2);
```

### 다양한 배열 타입을 인수로 받는 제네릭 함수

```tsx
function returnFirstValue<T>(data: T[]) {
  return data[0];
}

let num = returnFirstValue([0, 1, 2]);
// number

let str = returnFirstValue([1, "hello", "mynameis"]);
// number | string
```

### 위의 사례에서  반환값의 타입을 배열의 첫번째 요소의 타입이 되도록 하려면

```tsx
function returnFirstValue<T>(data: [T, ...unknown[]]) {
  return data[0];
}

let str = returnFirstValue([1, "hello", "mynameis"]);
// number 로 추론
```

### 타입 변수를 제한하는 사례

타입 변수를 제한한다는 것은 함수를 호출하고 인수로 전달할 수 있는 값의 범위에 제한을 두는 것을 의미한다.

타입 변수를 적어도 length 프로퍼티를 갖는 객체 타입으로 제한한 예시 

```tsx
function getLength<T extends { length: number }>(data: T) {
  return data.length;
}

getLength("123");            // ✅

getLength([1, 2, 3]);        // ✅

getLength({ length: 1 });    // ✅

getLength(undefined);        // ❌

getLength(null);             // ❌
```

- 자세한 설명
    
    타입 변수를 제한할 때에는 확장(extends)을 이용합니다.
    
    위와 같이 T extends { length : number } 라고 정의하면 T는 이제 { length : number } 객체 타입의 서브 타입이 됩니다. 바꿔말하면 이제 T는 무조건 Number 타입의 프로퍼티 length 를 가지고 있는 타입이 되어야 한다는 것 입니다. 따라서 이렇게 extends를 이용해 타입 변수를 제한하면 아래와 같은 결과가 나타납니다.
    
    - 1번 호출은 인수로 length 프로퍼티가 존재하는 String 타입의 값을 전달 했으므로 허용됩니다.
    - 2번 호출은 인수로 length 프로퍼티가 존재하는 Number[] 타입의 값을 전달 했으므로 허용됩니다.
    - 3번 호출은 인수로 length 프로퍼티가 존재하는 객체 타입의 값을 전달 했으므로 허용됩니다.
    - 4번 호출은 인수로 undefined을 전달했으므로 오류가 발생합니다.
    - 5번 호출은 인수로 null을 전달했으므로 오류가 발생합니다.

## map, forEach 메서드 타입 정의하기

- MAP 타입

```tsx
const arr = [1, 2, 3];

function map<T, U>(arr: T[], callback: (item: T) => U): U[] {
  (...)
}

map(arr, (it) => it.toString());
// string[] 타입의 배열을 반환
// 결과 : ["1", "2", "3"]
```

- forEach 타입

사용예시

```tsx
const arr2 = [1, 2, 3];

arr2.forEach((it) => console.log(it));
// 출력 : 1, 2, 3
```

```tsx
function forEach<T>(arr: T[], callback: (item: T) => void) {
  for (let i = 0; i < arr.length; i++) {
    callback(arr[i]);
  }
}
```

## 제네릭 인터페이스, 제네릭 타입 별칭

```tsx
interface KeyPair<K, V> {
  key: K;
  value: V;
}

let keyPair: KeyPair<string, number> = {
  key: "key",
  value: 0,
};

let keyPair2: KeyPair<boolean, string[]> = {
  key: true,
  value: ["1"],
};
```

### 인덱스 시그니쳐와 함께 사용하기

훨씬 유연한 객체 타입!

```tsx
interface Map<V> {
  [key: string]: V;
}

let stringMap: Map<string> = {
  key: "value",
};

let booleanMap: Map<boolean> = {
  key: true,
};
```

제네릭 타입 별칭

```tsx
type Map2<V> = {
  [key: string]: V;
};

let stringMap2: Map2<string> = {
  key: "string",
};
```

## 제네릭 클래스

```tsx
class List<T> {
  constructor(private list: T[]) {}

  push(data: T) {
    this.list.push(data);
  }

  pop() {
    return this.list.pop();
  }

  print() {
    console.log(this.list);
  }
}

const numberList = new List([1, 2, 3]);
const stringList = new List(["1", "2"]);

```

```tsx
class List<T> {
  constructor(private list: T[]) {}

  (...)
}

const numberList = new List<number>([1, 2, 3]);
const stringList = new List<string>(["1", "2"]);
```

## **프로미스와 제네릭**

### **Promise 사용하기**

Promise는 제네릭 클래스로 구현되어 있다.

다음과 같이 타입 변수에 할당할 타입을 직접 설정해 주면 해당 타입이 바로 resolve 결과값의 타입이 된다.

```tsx
const promise = new Promise<number>((resolve, reject) => {
  setTimeout(() => {
    // 결과값 : 20
    resolve(20);
  }, 3000);
});

promise.then((response) => {
  // response는 number 타입
  console.log(response);
});

promise.catch((error) => {
  if (typeof error === "string") {
    console.log(error);
  }
});
```

떤 함수가 Promise 객체를 반환한다면 함수의 반환값 타입을 위해 다음과 같이 할 수 있다.

```tsx
function fetchPost() {
  return new Promise<Post>((resolve, reject) => {
    setTimeout(() => {
      resolve({
        id: 1,
        title: "게시글 제목",
        content: "게시글 본문",
      });
    }, 3000);
  });
}
```

```tsx
function fetchPost(): Promise<Post> {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({
        id: 1,
        title: "게시글 제목",
        content: "게시글 본문",
      });
    }, 3000);
  });
}
```

# **타입 조작**

먼저 타입을 조작한다는 것은 **기본 타입이나 별칭 또는 인터페이스로 만든 원래 존재하던 타입들을 상황에 따라 유동적으로 다른 타입으로 변환**하는 타입스크립트의 강력하고도 독특한 기능

## 인덱스드 엑세스 타입

 인덱스를 이용해 다른 타입내의 특정 프로퍼티의 타입을 추출하는 타입

```tsx
interface Post {
  title: string;
  content: string;
  author: {
    id: number;
    name: string;
    age: number; // 추가
  };
}

function printAuthorInfo(author: Post["author"]) {
  console.log(`${author.id} - ${author.name}`);
}

(...)
```

Post["author"] 

**배열 요소의 타입 추출하기**

**튜플의 요소 타입 추출하기**

## **Keyof 연산자**

객체 타입으로부터 프로퍼티의 모든 key들을 String Literal Union 타입으로 추출하는 연산자

```tsx
interface Person {
  name: string;
  age: number;
  location: string; // 추가
}

function getPropertyKey(person: Person, key: keyof Person) {
  return person[key];
}

const person: Person = {
  name: "이정환",
  age: 27,
};
```

### **Typeof와 Keyof 함께 사용하기**

typeof 연산자는 자바스크립트에서 특정 값의 타입을 문자열로 반환하는 연산자 였다. 그러나 다음과 같이 타입을 정의할 때 사용하면 특정 변수의 타입을 추론하는 기능도 가지고 있다.

### **맵드 타입**

 기존의 객체 타입을 기반으로 새로운 객체 타입을 만드는 마법같은 타입 조작 기능

```tsx
interface User {
  id: number;
  name: string;
  age: number;
}

type PartialUser = {
  [key in "id" | "name" | "age"]?: User[key];
};

(...)
```

`key in “id” | “name” | “age”]` 는 이 객체 타입은 key가 한번은 id, 한번은 name, 한번은 age가 된다는 뜻 이다. 따라서 다음과 같이 3개의 프로퍼티를 갖는 객체 타입으로 정의된다.

- key가 “id” 일 때 → `id : User[id]` → `id : number`
- key가 “name”일 때 → `name : User[user]` → `name : string`
- key가 “age”일 때 → `age : User[age]` → `age : number`

### 템플릿 리터럴 타입

```tsx
type Color = "red" | "black" | "green";
type Animal = "dog" | "cat" | "chicken";

type ColoredAnimal = `red-dog` | 'red-cat' | 'red-chicken' | 'black-dog' ... ;
```

템플릿 리터럴 타입 적용시

```tsx
type ColoredAnimal = `${Color}-${Animal}`;
```
