# Node.js 3장

## 내용이 많아서 중요한 것만 정리할게요!

## REPL 사용
- REPL: 입력한 코드를 읽고 해석하고 결과물을 반환한고 종료할 때까지 반복하는 것
    - 한두 줄 짜리 코드를 테스트해보는 용도로는 좋지만 여러 줄의 코드를 실행하기에는 불편함.
    - 긴 코드는 코드를 자바스크립트 파일로 만든 후 파일을 통째로 실행하는 것이 좋음

## 모듈
- 특정한 기능을 하는 함수나 변수들의 집합
- ex: 수학 관련된 코드들만 모여 있는 모듈
- 여러 프로그램에 모듈을 재사용할 수 있음(like 함수)
- 파일 하나->모듈 하나
- 모듈이 많아지고 모듈 간 관계가 얽히면 구조 파악에 어려움이 생김김

### CommonJS 모듈
- 표준 모듈은 아니지만 가장 널리 쓰임 (표준 나오기 전부터 사용했기에)
- require 함수 -> 불러올 모듈 경로 작성
    - const{odd, even}= require('./var'); <!--구조분해할당 기법-->
    - 반드시 파일의 최상단에 위치할 필요가 없음
    - 한번 require한 파일은 require.cache에 저장되므로 다음번에 다시 새로 불러오지 않음
    - 새로 require 원하면 require.cache속성을 제거하면 됌(권장은 안함)
- module.exports 또는 exports객체로 모듈 작성 가능
    - 반드시 파일의 최하단에 위치할 필요가 없음

### 순환 참조
- 서로 다른 두 파일에서 서로를 require하게 되면 그 파일의 module.exports가 함수가 아니라 빈 객체로 표시
- 에러 발생하지 않고 빈 객체로 변경되기에 예기치 못한 동작이 발생할 수 있음음

### ECMAScript 모듈
- 공식적인 자바스크립트 모듈 형식
- CommonJS에서의 require,export, module.exports가 각각 import, export, export default로 변경
- 파일 확장자 변경 (.js -> .mjs)


## 내장 객체

### global
- 전역 객체 = 모든 파일에서 접근 가능
- window.open에서 window를 생략하듯 global 생략가능
- 남용한다면 프로그램의 규모가 커질수록 어떤 파일에서 global객체에 값을 대입했는지 찾기 힘들어 유지 보수에 어려움 발생

### console
- 디버깅 목적, 변수에 값이 제대로 들어있는지 확인목적, 에러 내용 확인 목적, 코드 실행시간 확인 목적
- ex: console.log(내용) , console.time(레이블) , console.error(에러 내용) , console.dir(객체, 옵션)
    - log-> 평범한 내용 표시, 여러 내용 동시에 표시 가능
    - time -> 같은 레이블을 가진 time과 timeEnd사이의 시간 측정
    - table -> 배열 요소로 객체 리터럴 넣으면 객체 속성이 테이블 형식으로 표현
    - dir ->  객체를 콘솔에 표시

### 타이머
- setTimeout(콜백함수, 밀리초):  주어진 밀리초 이후 콜백 함수 실행
- setInterval(콜백함수, 밀리초): 주어진 밀리초마다 콜백 함수 "반복" 실행
- setImmediate(콜백함수) : 콜백함수 즉시 실행
- clearTimeout(아이디) : setTimeout 취소
- clearInterval(아이디) : setInterval 취소
- clearImmediate(아이디) : setImmediate 취소

setTimeout(콜백,0) vs setImmediate(콜백)
=> 서로 다름! 일반적으로 setImmediate가 먼저 실행
=> 그러나 항상은 아니니까 주의하기
=> 헷갈리지 않게 setTimeout(콜백,0)의 사용은 자제하자

### process
- 현재 실행되고 있는 노드 프로세스에 대한 정보를 담고 있음
- version, arch, platfor, pid, uptime, execPath, cwd, cpuUsage 등
- process.nextTick(콜백) : 다른 콜백 함수들보다 nextTick의 콜백함수를 우선으로 처리하게 함
    - setTimeout이나 setImmediate보다 먼저 실행
    - 코드 맨 밑에 promise 넣음 -> resolve된 promise도 nextTick처럼 다른 콜백들보다 우선시
    - => microtask

## 내장 모듈

### os
-  운영체제 정보
    - arch, platform, type, uptime, hostname, release
- 경로
    - homedir, tmpdir
- cpu 정보
    - cpus, cpus().length
- 메모리 정보
    - freemem, totalmem

### path
- 폴더와 파일의 경로를 쉽게 조작하도록 도와주는 모듈
- 운영체제별로 경로 구분자가 다르기에 path 필요
- 윈도우, posix로 타입 나뉘어짐
- ex: path.sep
- sep, delimiter, dirname(경로), extname(경로), basename(경로, 확장자), parse(경로), format(객체), isAbsolute(경로), relative(기준경로, 비교경로), join(경로1, 경로2,...), resolve(경로1, 경로2,...)
- join vs resolve
    - /를 만나면 resolve는 절대경로로 인식해 앞의 경로를 무시
    - join은 상대경로로 처리함
- //와 /의 차이
    - 기본적으로는 / 하나만 사용 but 자바스크립트 문자열에서는 /가 특수문자라 두개를 붙여서 경로 표시함

### crypto
- 다양한 방식의 암호화를 도와줌

#### 단방향 암호화
- 비밀번호가 이에 해당
- 복호화할 수 없는 암호화 방식 (복호화: 암호화된 문자열을 원래 문자열로 되돌려놓는 것을 의미)
- 한번 암호화하면 원래 문자열 찾을 수 없음 = 해시함수
- 해시 기법: 어떤 문자열을 고정된 길이의 다른 문자열로 바꿔버리는 방식
- createHash(알고리즘), update(문자열), digest(인코딩)

#### 양방향 암호화
- 암호화된 문자열을 복호화할 수 있으며 키(key)가 사용됨
- 대칭형에서는 암호를 복호화하려면 암호화할 때 사용한 키와 같은 키를 사용해야 함
- crypto.createCipherive(알고리즘, 키, iv) , cipher.update(문자열, 인코딩, 출력 인코딩), cipher.final(출력 인코딩), crypto.createDecipheriv(알고리즘, 키,iv), decipher.update(문자열, 인코딩, 출력인코딩), decipher.final(출력인코딩)

### util
- 각종 편의 기능 모아둔 모듈
- 계속해서 API가 추가되고 가끔 deprecated되어 사라지는 경우도 존재
- util.deprecate, util.promisify

### worker_threads
- 멀티 스레드 방식

### child_process
- 노드에서 다른 프로그램을 실행하고 싶거나 명령어 수행하고 싶을 때 사용하는 모듈
- 다른 언어의 코드를 실행하고 결괏값 받을 수 있음


## 파일 시스템 접근

### 동기 메서드와 비동기 메서드
- 노드는 대부분의 메서드를 비동기로 처리, 몇몇은 동기로도 할 수 있는데 그걸 가능하게 하는 모듈을 많이 갖고 있는 것이 fs모듈

## 이벤트 이해
- on(이벤트명, 콜백) : 이벤트 이름과 이벤트 발생시의 콜백을 연결(이벤트 리스너)
- addListener(이벤트명, 콜백) : on과 기능 동일
- emit(이벤트명): 이벤트를 호출하는 메서드
- once(이벤트명,콜백): 한 번만 실행되는 이벤트
- removeAllListeners(이벤트명): 이벤트에 연결된 모든 이벤트 리스너를 제거
- removeListener(이벤트명, 리스너): 이벤트에 연결된 리스너 하나씩 제거 
- off(이벤트명, 콜백): removeListener와 기능 동일
- listenerCount(이벤트명): 현재 리스너가 몇개 연결되어 있는지 확인


## 예외 처리하기
- try/catch문 사용하기

### 자주 발생하는 에러
- node: command not found  -> 환경변수가 제대로 설정되지 않아 발생
- ReferenceError : 모듈 si not defined  -> 모듈을 require했는지 확인
- Error: Cannot find module 모듈명  -> 해당 모듈을 require했지만 설치하지 않음
- Error [ERR_MODULE_NOT_FOUND]  -> 존재하지 않은 모듈을 불러오려 할 때 발생
- Error: Can't set headers after they are sent  -> 요청에 대한 응답을 보낼 때 응답을 두 번 이상 보냄
- FATAL ERROR: CALL_AND RETRY_LAST Allocation failed- JavaScript heap out of memory  -> 코드 실행할 때 메모리가 부족해서 스크립트가 정상적으로 작동하지 않는 경우
- UnhandledPromiseRejectionWarning: Unhandled promise rejection  -> 프로미스 사용 시 catch메서드 붙이지 않으면 발생 (항상 catch붙이기)
- EADDRINUSE 포트번호  -> 해당 포트 번호에 이미 다른 프로세스가 연결되어 있음. 
- EACCES 또는 EPERM   -> 작업 수행 시 권한이 불충분
- EJSONPARSE  -> json파일에 문법 오류가 존재
- ECONNREFUSED  -> 요청을 보냈으나 연결이 안됌
- ETARGET  -> package.json에 기록한 패키지 버전이 존재하지 않을 때 발생
- ETIMEOUT  -> 요청을 보냈으나 응답이 시간 내에 오지 않을 때 발생
- ENOENT: no such file or directory  -> 지정 폴더나 파일 존재 안함