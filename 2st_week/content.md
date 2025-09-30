1. 변수

   1) +로 변수와 문자열을 연결하는 방법

    ```javascript
    let myPet = 'armadillo';
    console.log('I own a pet ' + myPet + '.'); 
    // Output: 'I own a pet armadillo.'
    ```
   2) console.log에 변수를 문자열에 넣을 때 ${}로 {}안에 넣는 방법
    ```javascript
    let myName="yeoni"
    consolog.log("My Name is ${myName}");
    ```





2. 비교 연산자

   1) 둘 다 모두 일때 (&&)
    ```javascript
    let mood = 'sleepy';
    let tirednessLevel = 6;
    if(mood==='sleepy'&& tirednessLevel>8){
    console.log('time to sleep')
    }
    else{
    console.log('not bedtime yet')
    }
    ```

   2) 둘 중에 하나 일때 (||)

    ```javascript
    if (day === 'Saturday' || day === 'Sunday') {
    console.log('Enjoy the weekend!');
    } else {
    console.log('Do some work.');
    }
    ```

   3) 주어진 문장의 반대를 말할 때 (!)
    ```javascript
    let excited = true;
    console.log(!excited); // Prints false

    let sleepy = false;
    console.log(!sleepy); // Prints true
    ```



3. switch case 문

    else if문은 여러 조건을 확인해야 하는 경우 문장은 훌륭한 도구. 프로그래밍에서 우리는 종종 여러 값을 확인하고 각각에 대해 다르게 처리해야 하는 경우가 많음. 

```javascript
    let groceryItem = 'papaya';

    if (groceryItem === 'tomato') {
    console.log('Tomatoes are $0.49');
    } else if (groceryItem === 'papaya'){
    console.log('Papayas are $1.29');
    } else if (groceryItem === 'lime'){
    console.log('limes are $1.49');
    } else {
    console.log('Invalid item');
}
```

만약 else if에 해당하는 조건이 엄청 많아진다면 어떨까?

코드가 상당히 길어질 것임 이를 방지하기 위한 문법이 바로 switch문  

위의 코드블럭이 else if문을 사용한 코드이고 아래의 코드블럭은 switch문을 사용한 코드

간단하게만 보더라도 아래 코드가 더 읽기에도 보기에도 편하다는 것이 느껴질 것!

```javascript
let groceryItem = 'papaya';

switch (groceryItem) {
  case 'tomato':
    console.log('Tomatoes are $0.49');
    break;
  case 'lime':
    console.log('Limes are $1.49');
    break;
  case 'papaya':
    console.log('Papayas are $1.29');
    break;
  default:
    console.log('Invalid item');
    break;
}

// Prints 'Papayas are $1.29'
```



4. 화살표 함수의 간결화

    1) 화살표 함수

    ```javascript
    const squareNum = (num) => {
    return num * num;
    };
    ```

    2) 화살표 함수 간단 버전
    ```javascript
    const squareNum = num => num * num;
    ```





5. 배열 객체 함수

- push: 배열의 끝에 요소를 추가

```javascript
const chores = ['wash dishes', 'do laundry', 'take out trash'];
chores.push('washingdish','cleanRoom');
```

- pop: 배열의 마지막 요소를 삭제하고 반환
```javascript
const newItemTracker = ['item 0', 'item 1', 'item 2'];
const removed = newItemTracker.pop();
console.log(newItemTracker); 
// Output: [ 'item 0', 'item 1' ]
console.log(removed);
// Output: item 2
```


- unshift: 배열의 시작에 요소를 추가
- shift: 배열의 시작 요소를 삭제하고 반환
- slice: 시작과 끝 범위 요소를 잘라 새로운 배열 생성
- indexOf: 찾고자 하는 요소의 인덱스번호 찾기

```javascript
const groceryList = ['orange juice', 'bananas', 'coffee beans', 'brown rice', 'pasta', 'coconut oil', 'plantains'];

groceryList.shift(); // 배열의 0번째 요소 삭제

console.log(groceryList);

groceryList.unshift('popcorn'); //배열의 0번째에 요소 추가

console.log(groceryList);

console.log(groceryList.slice(1, 4)); //배열 나누기

console.log(groceryList);

const pastaIndex = groceryList.indexOf('pasta'); //찾고자 하는 요소의 인덱스번호 찾기

console.log(pastaIndex);
```

- splice: 특정 위치에 요소를 추가하거나 삭제
- sort: 배열의 요소를 정렬
- map: 배열을 순회하며 각 요소에 함수를 적용, 새로운 배열 반환
- filter: 배열에서 조건을 만족하는 요소만 새로운 배열로 반환
- reduce: 배열의 모든 요소를 하나의 값으로 축소(합산 등)
- some: 배열 중 하나라도 조건을 만족하면 true 반환
- every: 모든 요소가 조건을 만족해야 true 반환


6. 반복자(Iterators)

    1) forEach : 배열의 각 요소를 반복해서 어떤 동작을 수행할 때 사용

    ```javascript
    const fruits = ['mango', 'papaya', 'pineapple', 'apple'];

    fruits.forEach(fruitItem => console.log('I want to eat a '+fruitItem));

    /*
    I want to eat a mango
    I want to eat a papaya
    I want to eat a pineapple
    I want to eat a apple
    */
    ```

    2) filter :  조건을 만족하는 요소만 걸러내서 새로운 배열을 반환

    ```javascript
    const words = ['chair', 'music', 'pillow', 'brick', 'pen', 'door']; 

    const shortWords = words.filter(word => {
    return word.length < 6;
    });

    console.log(words); // Output: ['chair', 'music', 'pillow', 'brick', 'pen', 'door']; 
    console.log(shortWords); // Output: ['chair', 'music', 'brick', 'pen', 'door']
    ```


    3) map : 배열의 각 요소를 변형해서 새로운 배열을 반환

    ```javascript
    const numbers = [1, 2, 3, 4, 5]; 

    const bigNumbers = numbers.map(number => {
    return number * 10;
    });

    console.log(numbers); // Output: [1, 2, 3, 4, 5]
    console.log(bigNumbers); // Output: [10, 20, 30, 40, 50]
    ```

    4) findIndex : 조건에 맞는 첫 번째 요소의 인덱스를 반환 ( 만약 조건에 맞는 요소가 없으면 -1 반환 )

    ```javascript
    const jumbledNums = [123, 25, 78, 5, 9]; 
    const lessThanTen = jumbledNums.findIndex(num => {
    return num < 10;
    });

    console.log(lessThanTen); // Output: 3 
    console.log(jumbledNums[3]); // Output: 5


    const greaterThan1000 = jumbledNums.findIndex(num => {
    return num > 1000;
    });

    console.log(greaterThan1000); // Output: -1
    ```

    5) reduce : 배열을 하나의 값으로 줄일 때(누적) 사용  

        여기서는 accumulator와 currentValue가 매개변수로 존재

        accumulator: 지금까지 누적된 값
        currentValue: 현재 배열에서 꺼낸 요소

        ```javascript
        const numbers = [1, 2, 4, 10];

        const summedNums = numbers.reduce((accumulator, currentValue) => {
        return accumulator + currentValue
        })

        console.log(summedNums) // Output: 17
        ```

       여기에서는 accumulator+currentValue를 반환해주는 형태

       summedNums는 0(acc)+1(cur)->1(acc)+2(cur)-> 3(acc) +4(cur)->7(acc)+10(cur)->17의 순서로 작동



    6) some : 배열 안에 하나라도 조건을 만족하는 요소가 있는지 확인해 만족하는 게 하나라도 있으면 true를, 없다면 false를 반환

        ```javascript
        const words = ['unique', 'uncanny', 'pique', 'oxymoron', 'guise'];

        console.log(words.some(word => {
        return word.length < 6;
        }));

        //result: true
        ```


    7) every : 배열의 모든 요소가 조건을 만족하는지 확인하여 모두 만족하면 true, 아니라면 false를 반환

        ```javascript
        const words = ['unique', 'uncanny', 'pique', 'oxymoron', 'guise'];

        const interestingWords = words.filter((word) => {return word.length > 5});

        console.log(interestingWords.every((word) => {return word.length > 5}));
        //result: true
        ```




8. 객체 (Object )

    1) 객체의 기본 구조

        객체는 주로 key와 value로 구성되어있는 구조입니다. key와 value세트는 ,로 연결합니다.

        ```javascript
        let 구조명 ={
            key(키): value(값),
            key2 : value2 
        };
        ```


    2) 객체 안의 키에 접근하는 법

        객체 안의 키값을 가져오거나 변경하려면 해당 키값을 부를 때 특정한 방법이 필요합니다.

        ① dot notation (점 연산자)

            ● 객체. 키(프로퍼티); 

            ●  값 고칠 시 -> 객체.키='바꿀 값'; 

            ●  프로퍼티 및 값을 추가할 시에도 값 고칠 때와 동일합니다.

            ●  값 삭제 시 delete 객체.프로퍼티; 

            ```javascript
            let spaceship = {
            homePlanet: 'Earth',
            color: 'silver'
            };
            spaceship.homePlanet; // Returns 'Earth'
            spaceship.color = 'gold'; // Returns 'silver'
            ```

        ② bracket notation (브라켓 표기법)

            ●  객체 [프로퍼티]; 

            ●  값 고칠 시 -> 객체 [프로퍼티] = '바꿀 값';

            ●  프로퍼티 및 값을 추가할 시에도 값 고칠 때와 동일합니다.

            ●  값 삭제 시 delete 객체 [프로퍼티]; 

            ```javascript
            let spaceship = {
            'Fuel Type': 'Turbo Fuel',
            'Active Duty': true,
            homePlanet: 'Earth',
            numCrew: 5
            };
            spaceship['Active Duty'];   // Returns true
            spaceship['Fuel Type'];   // Returns 'Turbo Fuel'
            spaceship['numCrew'];   // Returns 5
            spaceship['!!!!!!!!!!!!!!!'];   // Returns undefined
            spaceship['homePlanet']= 'moon';
            ```

