# 2-1
## 기본 문법

### const, let
- var 대신 const,let이 대체
- const, let은 블록스코프
- const: 한 번 값 할당 후 변경 불가 (aka 상수)

### 화살표 함수
```javascript
function add1(x,y) {
    return x+y;
}

const add2=(x,y)=>{
    return x+y;
}

const add3 = (x,y) => x+y;
//화살표함수에서 return만 있으면 줄일 수 있음음
```
```javascript
var relationship1={
    name:'zero',
    friends: ['nero','hero','xero'],
    logFriends: function(){
        var that= this;
        this.friends.forEach( function(friend){
            console.log(that.name,friend);
        });
    };
};

relationship1.logFriends();

var relationship1={
    name:'zero',
    friends: ['nero','hero','xero'],
    logFriends(){
        this.friends.forEach( function(friend){
            console.log(this.name,friend);
        });
    };
};

relationship2.logFriends();

//첫번째에서는 function선언문을 사용했음. 각자 다른 함수 스코프의 this를 가지므로 that을 사용해서 relationship1에 접근

//하지만 두번째에서는 forEach문에서 화살표 함수 사용함. 그래서 logFriend의 this를 그대로 사용 가능능
```
### 구조분해할당
```javascript
var candyMachine = {
    status: {
        name:'node',
        count: 5,
    },
    getCandy: function(){
        this.status.count--;
        return this.status.count;
    },
};

var getCandy= cnadyMachine.getCandy;
var count = candyMachine.status.count;

//아래는 바뀐 버전
var candyMachine = {
    status: {
        name:'node',
        count: 5,
    },
    getCandy(){
        this.status.count--;
        return this.status.count;
    },
};
const { getCandy, status {count}}=candyMachine;
// candyMachine 객체 안의 속성을 찾아서 변수와 매칭칭
```
### 클래스
- 클래스 문법이 javascript에도 생겼으나 클래스 기반이 아닌 프로토기반으로 동작
- 프로토타입 기반 문법을 보기 좋게 클래스로 바꾼 거라 생각하기

```javascript
var Human = function(type) {
  this.type = type || 'human';
};

Human.isHuman = function(human) {
  return human instanceof Human;
}

Human.prototype.breathe = function() {
  alert('h-a-a-a-m');
};

var Zero = function(type, firstName, lastName) {
  Human.apply(this, arguments);
  this.firstName = firstName;
  this.lastName = lastName;
};

Zero.prototype = Object.create(Human.prototype);
Zero.prototype.constructor = Zero; // 상속하는 부분
Zero.prototype.sayName = function() {
  alert(this.firstName + ' ' + this.lastName);
};
var oldZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(oldZero); // true
```
이를 클래스 기반으로 바꿔보자!

```javascript
class Human {
  constructor(type = 'human') {
    this.type = type;
  }

  static isHuman(human) {
    return human instanceof Human;
  }

  breathe() {
    alert('h-a-a-a-m');
  }
}

class Zero extends Human {
  constructor(type, firstName, lastName) {
    super(type);
    this.firstName = firstName;
    this.lastName = lastName;
  }

  sayName() {
    super.breathe();
    alert(`${this.firstName} ${this.lastName}`);
  }
}

const newZero = new Zero('human', 'Zero', 'Cho');
Human.isHuman(newZero); // true
```
### 프로미스
- 이벤트 리스너를 사용할 때 콜백 함수 자주 사용
- API들이 콜백 대신 프로미스 기반으로 재구성되고 콜백 지옥 현상 끝남
- new Promise로 프로미스 생성하고 안에 resolve와 reject를 ㅐㅁ개변수로 갖는 콜백 함수 넣음
- promise변수에 then과 catch메서드 넣을 수 있음
- resolve가 호출되면 then이, refect가 호출되면 catch가 실행.. finally는 성공, 실패 상관없이 실행
(시험 끝나고 다시 더 정리 예정)

```javascript
const condition = true; // true면 resolve, false면 reject
const promise = new Promise((resolve, reject) => {
  if (condition) {
    resolve('성공');
  } else {
    reject('실패');
  }
});
// 다른 코드가 들어갈 수 있음
promise
  .then((message) => {
    console.log(message); // 성공(resolve)한 경우 실행
  })
  .catch((error) => {
    console.error(error); // 실패(reject)한 경우 실행
  })
  .finally(() => { // 끝나고 무조건 실행
    console.log('무조건');
  });
```

### async/await
- 프로미스를 사용한 코드를 한 번 더 깔끔하게 줄임
```javascript
function findAndSaveUser(Users) {
  Users.findOne({})
    .then((user) => {
      user.name = 'zero';
      return user.save();
    })
    .then((user) => {
      return Users.findOne({ gender: 'm' });
    })
    .then((user) => {
      // 생략
    })
    .catch(err => {
      console.error(err);
    });
}
```
위의 코드를 async/await문법을 사용해 바꾼 버전

```javascript
async function findAndSaveUser(Users) {
  let user = await Users.findOne({});
  user.name = 'zero';
  user = await user.save();
  user = await Users.findOne({ gender: 'm' });
  // 생략
}
```
- 코드가 매우 짧아지고 함수선언부를 async function으로 교체한 후 프로미스 앞에 await 붙임.
- 해당 프로미스가 resolve될 때까지 기다린 뒤 다음 로직으로 넘어감
- 에러처리를 위해 try catch문으로 에러 감쌀 수 있음

```javascript
const findAndSaveUser = async (Users) => {
  try {
    let user = await Users.findOne({});
    user.name = 'zero';
    user = await user.save();
    user = await Users.findOne({ gender: 'm' });
    // 생략
  } catch (error) {
    console.error(error);
  }
};
//화살표 함수도 같이 사용
```

- for await of문을 사용해 프로미스 배열을 순회할 수도 있음
- async 함수의 반환값은 항상 Promise로 감싸짐.
- 중첩되는 콜백 함수가 있을 시 프로미스를 거쳐 async/ await문법으로 바꾸는 것이 좋음
### Map/Set
#### Map
- 객체와 유사
- 속성들 간의 순서를 보장하고 반복문 사용 가능
- 속성명으로 문자열이 아닌 값도 사용가능
- size 메서드를 통해 속성의 수를 쉽게 알 수 있음

#### Set
- 중복을 허용하지 않음
- 배열 자료구조를 사용하고 싶은데 중복 허용 안하고 싶으면 Set사용 하면 됨
- 기존 배열에서 중복을 제거하고 싶을 때도 사용
- 만약 set으로 원하는 동작을 한 후 배열로 되돌리고 싶으면 Array.from(Set)하기
### 널 병합/ 옵셔널 체이닝
- ??(널 병합), ?.(옵셔널 체이닝)
    - 널병합-> ||대용으로 사용. falsy값(0,'',false, NaN, null, undefined) 중 null과 undefined만 따로 구분
    - 옵셔널 체이닝 -> null이나 undefined의 속성을 조회하는 경우 에러가 발생하는 걸 막음

# 2-2

## AJAX(Asynchronous Javascript And Xml)
- 비동기적 웹서비스를 개발할 때 사용하는 기법
- 요즘에는 JSON 많이 사용(페이지 이동 없이 서버에 요청을 보내고 응답 받는 기술)
- AJAX 요청은 jQuery나 axios같은 라이브러리로 보냄

### FormData
```html
<script>
  const formData = new FormData();
  formData.append('name', 'zerocho');
  formData.append('item', 'orange');
  formData.append('item', 'melon');
  formData.has('item'); // true
  formData.has('money'); // false;
  formData.get('item');// orange
  formData.getAll('item'); // ['orange', 'melon'];
  formData.append('test', ['hi', 'zero']);
  formData.get('test'); // hi, zero
  formData.delete('test');
  formData.get('test'); // null
  formData.set('item', 'apple');
  formData.getAll('item'); // ['apple'];
</script>
```
- Html form 태그의 데이터를 동적으로 제어가능.. AJAX와 함께 주로 사용
- FormData생성자로 formData객체 만드면 생성된 객체의 append메서드로 키-값 형식의 데이터를 저장 가능함
- append 메서드를 여러 번 사용해 키 하나에 여러 개의 값 추가 가능
- has메서드는 주어진 키에 해당하는 값이 있는지 여부를 알려줌
- get 메서드는 주어진 키에 해당하는 값 하나를 가져옴
- get All메서드는 해당하는 모든 값을 가져옴
- delete 메서드는 현재 키를 제거하는 메서드
- set 메서드는 현재 키를 수정하는 메서드

### encodeURIComponent, decodeURIComponent
- AJAX 요청을 보낼 때 한글이 주소에 포함되는 경우 존재
- 한글 인식 못할 수도 있는데 이때는 encodeURIComponent메서드 사용하면 됌.
```javascript
const result = await axios.get('https://www.zerocho.com/api/search/${encodeURIComponent('노드')}')
```
- encodeURIComponent는 한글을 문자열로 변환시켜 줌. 그 반대로 하는 건 decodeURIComponent
