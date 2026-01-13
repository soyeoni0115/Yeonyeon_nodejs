# 타입스크립트
- 자바스크립트에 명시적으로 타입이 추가된 언어
- tsc라는 컴파일러를 통해 자바스크립트 코드로 변환 가능
- 디노: 타입스크립트 런타임 (아직 대중적이진 않음)

# tsconfig.json
- target: 결과문 문법을 어떤 버전 사용할지
- module: 결과물의 모듈 시스템을 어떤 종류로 할지
- esModuleInterop: CommonJS 모듈도 ECMAScript모듈로 인식하게 함
- forceConsistentCasingInFileNames: true이면 모듈 import할 때 파일 대포문자 구분
- strict: 엄격한 타입 검사 여부 결정
- skipLibCheck: true이면 모든 라이브러리의 타입을 검사하는 대신 내가 직접적으로 사용하는 라이브러리의 타입만 검사해 시간을 아낄 수 있음

# 타입 자리에 올 수 있는 타입
- 기본형
- 배열
- 상수
- 변수
- 클래스
- type 선언
- 인터페이스

