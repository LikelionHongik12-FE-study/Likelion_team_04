
- S5
    
    
    ### 함수 타입 정의
    
    ```tsx
    function func(a: number, b: number): number {
      return a + b;
    }
    ```
    
    어떤 타입의 매개변수를 받고, 어떤 타입의 결과값을 반환하는지 정의
    
    - 화살표 함수 타입 정의
    
    ```tsx
    const add = (a: number, b: number): number => a + b;
    ```
    
    - 함수 매개변수
    
    ```tsx
    function introduce(name = "이정환") {
    	console.log(`name : ${name}`);
    }
    ```
    
    - 오류 사례
        - 기본값의 다른 타입으로 매개변수 타입 정의
        - 기본값과 다른 타입의 값을 인수로 전달해도 오류
    
    - 선택적 매개변수
    
    ```tsx
    function introduce(name = "이정환", tall?: number, age: number) {
    	// 오류!
      console.log(`name : ${name}`);
      if (typeof tall === "number") {
        console.log(`tall : ${tall + 10}`);
      }
    }
    ```
    
     선택적 매개변수는 필수 매개변수 앞에 올 수 없다
    
    - 나머지 매개변수
    
    ```tsx
    function getSum(...rest: number[]) {
      let sum = 0;
      rest.forEach((it) => (sum += it));
      return sum;
    }
    
    ----
    
    function getSum(...rest: [number, number, number]) {
      let sum = 0;
      rest.forEach((it) => (sum += it));
      return sum;
    }
    
    getSum(1, 2, 3)    // 튜플 3개니까 가능
    getSum(1, 2, 3, 4) // ❌
    ```
    
    ---
    
    ### 함수 타입 표현식
    
    - 함수 타입 표현식 : 함수 타입을 별칭과 함께 별도로 정의
    
    ```tsx
    
    type Add = (a: number, b: number) => number; // 매개변수와 반환값 타입 정의
    
    const add: Add = (a, b) => a + b; // 자동으로 타입 넘어감
    ```
    
    사용 예
    
    ```tsx
    type Operation = (a: number, b: number) => number;
    
    const add: Operation = (a, b) => a + b;
    const sub: Operation = (a, b) => a - b;
    const multiply: Operation = (a, b) => a * b;
    const divide: Operation = (a, b) => a / b;
    ```
    
    - 호출 시그니쳐 : 함수 타입을 별도로 정의
    
    ```tsx
    type Operation2 = {
      (a: number, b: number): number;
    };
    
    const add2: Operation2 = (a, b) => a + b;
    const sub2: Operation2 = (a, b) => a - b;
    const multiply2: Operation2 = (a, b) => a * b;
    const divide2: Operation2 = (a, b) => a / b;
    ```
    
    ---
    
    - 함수 타입 호환성
        - 두 함수의 반환값 타입이 호환되는가?
        
        ```tsx
        type A = () => number;
        type B = () => 10;
        
        let a: A = () => 10;
        let b: B = () => 10;
        
        //A는 Number 반환, B는 Number Literal 
        
        a = b; // 가능 - 왼쪽이 더 큰 타입이면 호환(업캐스팅)
        b = a; // 불가능
        ```
        
        - 두 함수의 매개변수 타입이 호환되는가?
        
        매개변수 개수가 같을 때
        
        ```tsx
        type C = (value: number) => void;
        type D = (value: 10) => void;
        
        let c: C = (value) => {};
        let d: D = (value) => {};
        
        c = d; // 불가능
        d = c; // 가능  - 왼쪽이 더 작은 타입이면 호환(다운캐스팅)
        ```
        
        매개변수 개수가 다를 때 
        
        ```tsx
        type Func1 = (a: number, b: number) => void;
        type Func2 = (a: number) => void;
        
        let func1: Func1 = (a, b) => {};
        let func2: Func2 = (a) => {};
        
        func1 = func2; // 가능
        func2 = func1; // 불가능
        ```
        

---

### 함수 오버로딩

- 함수 오버로딩 : 하나의 함수를 매개변수의 개수나 타입에 따라 다르게 동작하도록 만듬

```tsx
// 버전들 -> 오버로드 시그니쳐
function func(a: number): void; //ver1 
function func(a: number, b: number, c: number): void; //ver2

// 실제 구현부 -> 구현 시그니쳐
function func(a: number, b?: number, c?: number) { // b c 선택적 프로퍼티로 만들어줘야 오류x 
  if (typeof b === "number" && typeof c === "number") {
    console.log(a + b + c);
  } else {
    console.log(a * 20);
  }
}
```

---

### 사용자 정의 타입 가드

- 사용자 정의 타입가드 : 참 거짓 함수를 이용해 타입 가드를 만듬

```tsx
// Dog 타입인지 확인하는 타입 가드
function isDog(animal: Animal): animal is Dog {
  return (animal as Dog).isBark !== undefined; // 사용자 정의 타입가드
}

// Cat 타입인지 확인하는 타입가드
function isCat(animal: Animal): animal is Cat {
  return (animal as Cat).isScratch !== undefined; // 사용자 정의 타입가드 
}

function warning(animal: Animal) {
  if (isDog(animal)) { //Dog 타입으로 추론 
    console.log(animal.isBark ? "짖습니다" : "안짖어요");
  } else {
    console.log(animal.isScratch ? "할큅니다" : "안할퀴어요");
  }
}
```

- S6
    
    ### 인터페이스
    
    - 인터페이스 : 타입에 이름을 지어주는 문법 = 타입 별칭
    
    ```tsx
    interface Person {
      name: string;
      age: number;
      sayHi: () => void; // 함수 타입 표현식으로 메서드 타입 정의
      sayHi(): void; // 호출 시그니쳐를 이용해 메서드 타입 정의
    }
    
    ---- //사용
    
    const person: Person = {
      name: "박규영",
      age : 24
    };
    ```
    
    - 메서드 오버로딩 : 호출 시그니처를 사용할 때만 오버로딩 구현 가능
    
    ```tsx
    interface Person {
      readonly name: string;
      age?: number;
      sayHi(): void;
      sayHi(a: number): void;
      sayHi(a: number, b: number): void;
    }
    ```
    
    ---
    
    - 인터페이스 확장 : 인터페이스를 다른 인터페이스가 상속받아 프로퍼티 중복 방지 (상속)
    
    → `interface 타입이름 extends 확장_할_타입이름`
    
    ```tsx
    interface Animal {
      name: string;
      color: string;
    }
    
    interface Dog extends Animal {  // Dog는 Animal의 확장 
      breed: string;
    }
    
    interface Cat extends Animal { // Cat는 Animal의 확장 
      isScratch: boolean;
    }
    
    interface Chicken extends Animal { // Chicken는 Animal의 확장 
      isFly: boolean;
    }
    
    interface DogCat extends Dog, Cat {} //다중 확장
    
    const dogCat: DogCat = {
      name: "",
      color: "",
      breed: "",
      isScratch: true,
    };
    ```
    
    ---
    
    - 선언 합침 : 타입 별칭과 달리 인터페이스는 동일한 이름으로 선언 가능
    
    ```tsx
    interface Person {
      name: string;
    }
    
    interface Person { 
      age: number;
    }
    
    //이렇게 쓰면 
    
    interface Person {
    	name: string;
    	age: number;
    }
    
    //이렇게 합쳐짐
    
    interface Person {
      name: string;
    }
    
    interface Person {
      name: number;
      age: number;
    }
    
    // 이런 충돌은 허용 x 
    
    ```
    

- S7
    
    ### 클래스
    
    객체 지향에서 배운거! 
    
    ```tsx
    class Student {
      // 필드
      name;
      age;
      grade;
    
      // 생성자
      constructor(name, grade, age) {
        this.name = name;
        this.grade = grade;
        this.age = age;
      }
      
      study() {
      console.log("열심히 공부 함");
      }  //메서드 
      
    }
    
    -----
    
    // 클래스로 객체 생성  -> 인스턴스 
    const studentB = new Student("홍길동", "A+", 27);
    
    studentB.study();
    ```
    
    - 클래스 상속하기
    
    ```tsx
    class Student {
      // 필드
      name;
      age;
      grade;
    
      // 생성자
      constructor(name, grade, age) {
        this.name = name;
        this.grade = grade;
        this.age = age;
      }
      
      study() {
      console.log("열심히 공부 함");
      }  //메서드 
      
    }
    
    class StudentDeveloper extends Student {
      // 필드
      favoriteSkill;
    
      // 생성자
      constructor(name, grade, age, favoriteSkill) {
        super(name, grade, age); // 부모 클래스의 값을 인수로 전달 해줘야함 
        this.favoriteSkill = favoriteSkill;
      }
    
      // 메서드
      programming() {
        console.log(`${this.favoriteSkill}로 프로그래밍 함`);
      }
    }
    
    ```
    
    ---
    
    ### 타입스크립트 클래스
    
    ```tsx
    class Employee { //클래스는 실제 타입으로도 사용할 수 있음
      // 필드
      name: string = "";
      age: number = 0;
      position: string = "";
    
      // 메서드
      work() {
        console.log("일함");
      }
    }
    
    const employeeC: Employee = { // Employee 타입임
      name: "",
      age: 0,
      position: "",
      work() {},
    };
    ```
    
    - 상속
    
    ```tsx
    class ExecutiveOfficer extends Employee {
      officeNumber: number;
    
      constructor(name: string, age: number, position: string, officeNumber: number) {
        super(name, age, position);
        this.officeNumber = officeNumber;
      }
    }
    ```
    
    ---
    
    ### 접근 제어자
    
    (객체에서 배운거) 
    
    - public : 모든 범위에서 접근 가능
    - private : 클래스 내부에서만 접근 가능. 외부x
    - proteced : 클래스 내부 또는 파생 클래스 내부에서만 접근 가능.
    
    ```tsx
    class Employee {
      // 필드
      private name: string; // private 접근 제어자 설정
      protected age: number;
      public position: string;
    }
    
    employee.name = "홍길동"; // 프라이빗이라 오류 
    
    class ExecutiveOfficer extends Employee {
     // 메서드
      func() {
        this.name; //  오류 private 
        this.age; //   가능 protected  
      }
    }
    
    // 생성자에서도 접근 제어자 설정 가능하지만, 필드에 선언 불가. 자동으로 함께 선언됨
    
    class Employee {
      // 생성자
      constructor(
        private name: string,
        protected age: number,
        public position: string
      ) {} // this.필드 = 매개변수 자동으로 실행
    
      // 메서드
      work() {
        console.log(`${this.name} 일함`);
      }
    }
    
    ```
    
    ---
    
    ### 인터페이스와 클래스
    
    클래스에 implements 키워드로 인터페이스의 타입을 만족하도록 정의 가능(설계도 역할)
    
    ```tsx
    /**
     * 인터페이스와 클래스
     */
    
    interface CharacterInterface {
      name: string;
      moveSpeed: number;
      move(): void;
    }
    
    class Character implements CharacterInterface { // implements(구현하다) 키워드 
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
    
- S8
    
    ### 제네릭
    
    - 제네릭 : 함수가 모든 다양한 타입에 두루두루 사용할 수 있게 해줌.
    
    ```tsx
    function func<T>(value: T): T { // T로 설정 
      return value;
    }
    
    let num = func(10);
    // number 타입
    ```
    
    - 타입변수  : 함수가 호출될 때 타입을 추론 ( T, U 등등 여러개 사용 가능)
    
    ```tsx
    function swap<T, U>(a: T, b: U) {
      return [b, a];
    }
    
    const [a, b] = swap("1", 2);
    ```
    
    - 다양한 배열 타입을 인수로 받는 제네릭 함수 만들기
    
    ```tsx
    function returnFirstValue<T>(data: T[]) {
      return data[0]; // number | string 유니온 타입으로 반환
    }
    
    let str = returnFirstValue([1, "hello", "mynameis"]);
    // number | string
    
    ---
    
    function returnFirstValue<T>(data: [T, ...unknown[]]) { // 첫번째 값의 타입을 받아옴
      return data[0]; // number 타입 반환
    }
    
    let str = returnFirstValue([1, "hello", "mynameis"]);
    // number
    ```
    
    - 타입 변수를 제한하는 방법
    
    ```tsx
    function getLength<T extends { length: number }>(data: T) { 
    // length 프로퍼티를 가지고 있어야 허용
      return data.length;
    }
    
    getLength("123");            // ✅
    
    getLength([1, 2, 3]);        // ✅
    
    getLength({ length: 1 });    // ✅
    
    getLength(undefined);        // ❌
    
    getLength(null);             // ❌
    ```
    
    ---
    
    ### Map, forEach 타입 메서드 타입
    
    - Map 메서드 : 배열의 각 요소에 콜백함수를 수행하고, 배열로 반환
    
    ```tsx
    const arr = [1, 2, 3];
    const newArr = arr.map((it) => it * 2);
    // [2, 4, 6]
    ```
    
    - 제네릭 함수로 map함수 만들기
    
    ```tsx
    const arr = [1, 2, 3];
    
    function map<T>(arr: T[], callback: (item: T) => T): T[] {
      let result = [];
      for (let i = 0; i < arr.length; i++) {
        result.push(callback(arr[i]));
      }
      return result;
    }
    
    map(arr, (it) => it * 2);
    
    ---- // 원본과 다른 반환값 타입으로 변환 가능하도록 하기
    
    function map<T, U>(arr: T[], callback: (item: T) => U): U[] {
      let result = [];
      for (let i = 0; i < arr.length; i++) {
        result.push(callback(arr[i]));
      }
      return result;
    }
    
    map(["hi", "hello"], (it) => parseInt(it)); // number값으로 반환
    
    ```
    
    - ForEach 메서드 : 배열의 모든 요소에 콜백함수를 수행
    
    ```tsx
    const arr2 = [1, 2, 3];
    
    arr2.forEach((it) => console.log(it));
    // 출력 : 1, 2, 3
    ```
    
    - 제네릭 함수로 정의
    
    ```tsx
    function forEach<T>(arr: T[], callback: (item: T) => void) {
      for (let i = 0; i < arr.length; i++) {
        callback(arr[i]);
      }
    }
    
    forEach(['123','456'], (it) => { 
    		it;
    }) 
    
    ```
    
    ---
    
    - 제네릭 인터페이스 : 제네릭 인터페이스는 타입으로 정의할 때 꺽쇠로 타입 변수에 할당할 타입을 명시해주어야 한다.
    
    ```tsx
    interface KeyPair<K, V> {
      key: K;
      value: V;
    }
    
    let keyPair: KeyPair<string, number> = {
      key: "key",
      value: 0,
    };
    ```
    
    - 인덱스 시그니쳐
    
    ```tsx
    interface Map<V> {
      [key: string]: V;  //키는 string 타입 , 값은 V타입 
    }
    
    let stringMap: Map<string> = {
      key: "value",
    };
    
    let booleanMap: Map<boolean> = {
      key: true,
    };
    ```
    
    - 제네릭 타입 별칭
    
    ```tsx
    type Map2<V> = {  //타입 별칭
      [key: string]: V;
    };
    
    let stringMap2: Map2<string> = { //할당할 타입 명시 
      key: "string",
    };
    ```
    
    ---
    
    - 제네릭 클래스
    
    ```tsx
    class List<T> {
      constructor(private list: T[]) {}  // 생성자 매개변수도 타입변수로 
    
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
    
    ---
    
    - promise 객체 : 비동기처리 객체
    
    https://reactjs.winterlood.com/45ee7daf-0c32-4f40-9b20-b7bba338d39f
    
    ```tsx
    const promise = new Promise<number>((resolve, reject) => {
      setTimeout(() => {
        resolve(20);
      }, 3000);
    });
    
    promise.then((response) => {
      console.log(response);
    });
    
    promise.catch((error) => {
      if (typeof error === "string") {
        console.log(error);
      }
    });
    
    --- // 반환값 타입을 직접 정의
    
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
    
- S9
    
    
    - 타입 조작하기 : 상황에 따라 유동적으로 다른  타입으로 변환
        - 제네릭
        - 인덱스드 엑세스 타입
        - keyof 연산자
        - Mapped 타입
        - 템플릿 리터럴 타입
        - 조건부 타입
    
    - 인덱스드 엑세스 타입 : 인덱스를 이용해 다른 타입 내 특정 프로퍼티 타입을 추출
    
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
    
    function printAuthorInfo(author: Post["author"]) { // author 타입 프로퍼티 타입만 떼어줌
      console.log(`${author.id} - ${author.name}`);
    }
    ```
    
    배열 요소 타입 추출
    
    ```tsx
    type PostList = {
      title: string;
      content: string;
      author: {
        id: number;
        name: string;
        age: number;
      };
    }[];
    
    const post: PostList[number] = {  // 배열타입 뒤에 number 나 number literal 타입 넣으면 
                                      // 숫자 관계 없이 동일하게 뽑아온다.  
      title: "게시글 제목",
      content: "게시글 본문",
      author: {
        id: 1,
        name: "이정환",
        age: 27,
      },
    };
    ```
    
    튜플 요소 타입 추출
    
    ```tsx
    type Tup = [number, string, boolean];
    
    type Tup0 = Tup[0];
    // number
    
    type Tup1 = Tup[1];
    // string
    
    type Tup2 = Tup[2];
    // boolean
    
    type Tup3 = Tup[number] // 공통타입으로 추출 = 유니온타입 
    // number | string | boolean
    ```
    
    ---
    
    - Keyof 연산자 : 프로퍼티의 모든 key들을 String Literal Union 타입으로 추출하는 연산자
    
    ```tsx
    interface Person {
      name: string;
      age: number;
      location: string; // 추가
    }
    
    function getPropertyKey(person: Person, key: keyof Person) { 
                                    //Person 객체의 모든 프로퍼티 키를 유니온 타입으로 추출 
      return person[key];
    }
    
    const person: Person = {
      name: "이정환",
      age: 27,
    };
    ```
    
    - typeof 연산자 : 특정 값의 타입을 문자열로 반환하는 연산자
    → 특정 변수의 타입을 추론하는 기능도 포함!
    
    ```tsx
    type Person = typeof person; // {name: string, age: number, location:string} 반환
    
    function getPropertyKey(person: Person, key: keyof typeof person) {  
                                       // 위와 같은 결과값
      return person[key];
    }
    
    const person: Person = {
      name: "",
      age: 27,
    };
    ```
    
    ---
    
    - 맵드 타입 : 새로운 객체 타입을 만드는 타입 조작
    
    ```tsx
    interface User {
      id: number;
      name: string;
      age: number;
    }
    
    type PartialUser = {
      [key in "id" | "name" | "age"]?: User[key];  //맵드 타입 
    };
    ```
    
    > `key in “id” | “name” | “age”]` 는 이 객체 타입은 key가 한번은 id, 한번은 name, 한번은 age가 된다는 뜻 입니다. 따라서 다음과 같이 3개의 프로퍼티를 갖는 객체 타입으로 정의됩니다.
    > 
    > - key가 “id” 일 때 → `id : User[id]` → `id : number`
    > - key가 “name”일 때 → `name : User[user]` → `name : string`
    > - key가 “age”일 때 → `age : User[age]` → `age : number`
    > 
    > 여기에 대 괄호 뒤에 선택적 프로퍼티를 의미하는 물음표(?) 키워드가 붙어있으므로
    > 
    > ```tsx
    > {
    >   id?: number;
    >   name?: string;
    >   age?: number;
    > }
    > ```
    > 
    > 와 같은 타입이 됩니다.
    > 
    
    ```tsx
    type PartialUser = {
      [key in keyof User]?: User[key]; 
    };
    
    type ReadonlyUser = {
      readonly [key in keyof User]: User[key];  //모든 프로퍼티가 읽기 전용
    };
    
    //이렇게 활용 가능
    ```
    
    주의 : 맵드타입은 interface로 쓸 수 없음. type 별칭으로만 사용
    
    ---
    
    - 템플릿 리터럴 타입 : 템플릿 리터럴(` `) 을 이용해 조합한 문자열 타입을 만듬
    
    ```tsx
    type Color = "red" | "black" | "green";
    type Animal = "dog" | "cat" | "chicken";
    
    type ColoredAnimal = `${Color}-${Animal}`;
    
    // type ColoredAnimal = `red-dog` | 'red-cat' | 'red-chicken' | 'black-dog' | ...;
    ```
