# TypeScript

[TOC]

## 1. 기본

### 변수 지정

```typescript
// 변수를 지정할 때는 타입을 지정
// 가능한 TypeScript가 추정하도록 내비두는 것이 좋음 
let a = "hello"
let b : string = "hello"

let c : number = 1
let d : boolean = true

let e : string[] = ['h','e','l','l','o']
let f : number[] = [1, 2, 3]
let g : boolean[] = [true, false]



// 선택적인 오브젝트 만들기
// name은 필수, age는 선택
const player : {
  name:string,
  age?:number
} = {
  name:"nico"
}

// 선택적인 요소에 대해(값이 undefined)인 요소에 대해 조건을 걸때
if(player.age && player.age <10){
  // do something
}

// 밑의 코드는 오류발생
if(player.age < 10){
  // do something
}

```



### Alias

```typescript
// Alias (코드 재사용)
// 동일한 형태를 가지는 여러 인스턴스들을 생성하고자 하는 경우(like class)
type Player = {
  name:string
  age?:number
}

const nico : Player = {
  name:"nico"
}
const lynn : Player = {
  name:"lynn",
  age: 12
}



// 인스턴스 생성
// 타입 스크립트에게 리턴하는 것이 Player라는 타입임을 알려줌
function playerMaker(name:string) : Player {
  return {
    name
  }
}
const nomad = playerMaker("nomad")
nomad.age = 12

// 화살표 함수로 function 만들기
// const playerMaker = (name:string) : Player => ({name})



// 타입 생성시에 수정 방지
type Gamer = {
  readonly name:string
  age?:number
}

// 수정이 가능한 경우
const numbers : number[] = [1, 2, 3]
numbers.push(1)
// 수정 방지(수정이 불가능)
const names : readonly string[] = ['a','b']
names.push('c')
```



### 그 외의 타입들

```typescript
// Tuple: 다양한 요소를 가지는 리스트
const students : [string, number, boolean] = ['nico', 1, true]
// 역시 readonly 적용 가능
// const students : readonly [string, number, boolean] = ['nico', 1, true]



// any: 타입스크립트의 보호장치에서 벗어나고 싶은 경우
// 최대한 사용하지 않는 것이 좋음
const numList : any[] = [1, 2, 3]
const str : any = "string"

numList + str



// unknown: 변수 타입을 미리 알지 못하는 경우 사용
let x : unknown

if(typeof x === 'number'){
  let y = x + 1
}
if(typeof x === 'string'){
  let y = x.toUpperCase()
}



// void: 아무것도 return하지 않는 함수를 대상으로 함
function hello():void {
  console.log('x')
}



// never: 절대로 return하지 않는 함수를 대상으로 함
function greeting():never{
  throw new Error('xxx')
}

// 타입이 두가지일 수 도 있는 경우에도 사용
// name이 string 또는 number여야 하는 경우 다른 것이 들어오면 아무것도 안하게 하기
function hi(name:string|number){
  if(typeof name === 'string'){
    // 이 때 name의 타입은 string
    name
  } else if (typeof name === 'number'){
    // 이 때 name의 타입은 string
    name
  } else {
    // 이 때 name의 타입은 never
    name
  }
}
```



---



## 2. 함수

### 함수 생성

```typescript
// 일반적인 함수 작성법(화살표 함수를 이용)
const add = (a:number, b:number) => a + b
```



### Call Signatures

```typescript
// 타입으로 만들어서 함수 생성
type Add = (a:number, b:Number) => number

const add:Add = (a, b) => a + b
```



### Overloading

```typescript
// Overloading
// 함수가 서로 다른 여러개의 Call Signatures를 가질 때 발생시킴
// 예시
type Add = {
  (a:number, b:number) : number
  (a:number, b:string) : number
}

// b가 number일수도 있고 string일수도 있는데 string이면 연산이 불가능하므로...
const add: Add = (a, b) => {
  if(typeof b === 'string') return a
  return a + b
}

// 실제로 볼 수 있는 오버로딩
type Config = {
  path: string,
  state: object
}

// Push는 파라미터로 그냥 경로를 보낼 수도 있고 Config처럼 경로를 포함한 오브젝트로 보낼수도 있음
type Push = {
  (path: string):void
  (config: Config):void
}

const push:Push = (config) => {
  if(typeof config === "string") { console.log(config) }
  else {
    console.log(config.path)
  }
}


// 서로 다른 여러개의 Call Signatures들이 다른 개수의 argument를 가지는 경우
type Add = {
  (a:number, b:number) :number
  (a:number, b:number, c:number) : number
}

// 공통되지 않은 인자는 선택인자로 지정해주기
const add:Add = (a, b, c?:nubmer) => {
  if(c) return a+b+c
  return a+b
}

add(1,2)
add(1,2,3)
```



### Polymorphism (다형성)

```typescript
// Generic: Concrete 타입에 대해서 사용하며 함수에 placeholder처럼 사용해서 그것이 뭔지 추론하여 typescript가 처리하게 함
type SuperPrint = {
  // 뭐 쓰던 상관없는데 이렇게 쓰면 Concrete 타입을 다 허용한다는 말
  <TypePlaceholder>(arr: TypePlaceholder[]):void
}

const superPrint:SuperPrint = (arr) => {
  arr.forEach(i => console.log(i))
}
// const superPrint: SuperPrint = (arr) => arr[0]

superPrint([1, 2, 3, 4])
superPrint([true, false, true])
superPrint(["a", "b", "c", "d"])
superPrint([1, 2, true, false, "hello"])


// 만약 SuperPrint의 argument가 두개라면?
type SuperPrint = <T, M>(a: T[], b:M) => T

const SuperPrint = ([1, 2, 3, 4], "x")


// 타입을 생성하는데 그 안에 요소가 다양한 타입을 가질 가능성을 두고 싶다면
type Player<E> = {
  name:string
  extraInfo: E
}

// Generic을 사용하는 또 다른 방식
type A = Array<number>

let a:A = [1, 2, 3, 4]

```



### React의 useState + TypeScript

```react
// React에서 useState를 사용할 때
// 어떤 타입인지 알려줘야함
useState<number>()
```



---



## 3. 클래스와 인터페이스

### Classes

#### JavaScript Code

```javascript
"use strict"
class Player {
  constructor(firstName, lastName, nickName) {
    this.firstName = firstName
    this.lastName = lastName
    this.nickName = nickName
  }
}

const nico = new Player('nico', 'las', '니꼬')
```

#### TypeScript Code

- private와 같은 보호 기능도 제공함

```typescript
class Player {
  constructor(
    private firstName:string,
    private lastName:string,
    public nickName:string
  ) {}
}

const nico = new Player('nico', 'las', '니꼬')
```



### Abstract Classes (상속이 가능한 클래스)

```typescript
// abstract class(추상클래스): 상속이 가능한 클래스
abstract class User {
  constructor(
    private firstName:string,
    private lastName:string,
    public nickName:string
  ) {}
}

// Player 클래스로 User 클래스를 상속
class Player extends User{
}

// 새로운 인스턴스 생성
const nico = new Player('nico', 'las', '니꼬')
// 단, 추상 클래스는 상속을 하지 않고 직접 사용은 불가능하다.
// const nico = new User() : 불가



// 추상함수 내의 method
abstract class User {
  constructor(
    private firstName:string,
    private lastName:string,
    public nickName:string
  ) {}

  getFullName(){
    return `${this.firstName} ${this.lastName}`
  }
  // property처럼 method에도 private사용 가능! 다만 상속 받아서 해당 메서드 사용 불가
  // private getFullName(){
  //   return `${this.firstName} ${this.lastName}`
  // }
}

// Player 클래스로 User 클래스를 상속후 인스턴스 생성
class Player extends User{
}
const nico = new Player('nico', 'las', '니꼬')

// 추상함수의 method 사용
nico.getFullName()
```



### Abstract Method (추상 메서드)

```typescript
// abstract method (추상메서드)
// 추상 클래스 내에서 구현되며, call signature만 적어야 함
// 추상 클래스를 상속 받는 모든 클래스에서 구현되어야만 하는 메서드
abstract class User {
  constructor(
    // protected: 외부 사용불가, 자식클래스 사용가능
    protected firstName:string,
    // private: 외부 사용불가, 자식클래스 사용불가
    private lastName:string,
    // publick: 외부 사용가능, 자식클래스 사용가능
    public nickName:string
  ) {}
  
  // 추상메서드
  abstract getFullName(): void
}

// Player 클래스로 User 클래스를 상속후 인스턴스 생성
  // 만약 상속받은 추상클래스 내에서 private로 지정된 property가 있다면,
  // 설령 그것을 상속 받아왔다 해도 해당 property에는 접근할 수 없다.

  // 추상 클래스에서 property를 지정할 때 외부에서는 사용이 불가하지만,
  // 자식 클래스에서는 사용이 가능하게 하고 싶다면 'protected'를 사용해야 한다.
class Player extends User{
  getFullName() {
    console.log(this.firstName)
  }
}
```



```typescript
type Words = {
  // Words 타입이 string 타입만을 객체로 가진다는 것을 지정
  [key:string]: string
}

class Dict {
    private words: Words
    constructor(){
      this.words={}
    }
    add(word:Word){
      if(this.words[word.term] === undefined) {
        this.words[word.term] = word.def
      }
    }
    def(term:string){
      return this.words[term]
    }
}

class Word {
  constructor(
    public term:string,
    public def :string
  ) {}
}

const kimchi = new Word('kimchi', '한국의 음식')

const dict = new Dict()

dict.add(kimchi)
dict.def("kimchi")
```



### Interfaces

```typescript
```



### Polymorphsim

```typescript
```

