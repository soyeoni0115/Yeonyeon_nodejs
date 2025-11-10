# 데이터베이스란?
- 관련성을 가지며 중복이 없는 데이터들의 집합
- 데이터베이스를 관리하는 시스템은 DBMS
- 저장매체가 고장 나거나 사용자가 직접 데이터를 지우지 않는 이상 계속 데이터가 보존되므로 서버 종료 여부와 상관없이 데이터를 계속 사용 가능
- 서버에 데이터베이스를 올리면 여러 사람이 동시에 사용 가능
- 사람들에게 각각 다른 권한을 줘서 읽기만 가능할 수도, 모든 작업을 가능하게 할 수도 있음
- RDBMS라고 불리는 관계형 DBMS가 많이 사용됨.
    - Oracle, MySQL, MSSQL

# MySQL에서 데이터베이스 생성하기
- CREATE SCHEMA [데이터베이스명] : 데이터베이스 생성 명령어
- use nodejs; 명령어 추가 기입
- 데이터베이스명 뒤에 DEFAULT CHARACTER SET utf8mb4 DEFAULT COLLATE utf8mb4_general_ci 붙이면 한글과 이모티콘 사용 가능
- 명령어가 대문자면 지켜서 써주는게 좋음 

# 테이블 생성하기
- CREATE TABLE [데이터베이스명. 테이블명] : 테이블을 생성하는 명령어
- 명령어 아래에는 한 줄에 하나씩 콤마로 구분해 칼럼 생성 (만드는 사람이 알아서 결정. 단, 컬럼으로 만들어두지 않은 정보들은 저장 불가)

# CRUD 작업
- CRUD: Create, Read, Update, Delete의 첫글자만 딴 것으로 데이터베이스에서 가장 많이 수행하는 네 가지 작업
- 4가지 작업 방법만 익혀도 웬만한 프로그램은 다 만들 수 있음
- Create: 데이터를 생성해서 데이터베이스에 넣는 작업
    - 명령어: INSERT INTO [테이블명] ([컬럼1],[컬럼2],...) VALUES ([값1],[값2], ...)
- Read:  데이터베이스에 있는 데이터를 조회하는 작업
    - 명령어: SELECT * FROM [테이블명] ->  저장된 정보 다 불러오는 것 (* 표시가 '전부'를 의미)
    - 일부만을 조회하고 싶으면 SELECT 다음에 해당 컬럼을 넣으면 됌. (ex. SELECT name, married FROM nodejs.users;)
    - 특정 조건을 가진 데이터를 조회하고 싶다면 
        - 1. AND로 여러 조건 묶을 수 있음 (ex. SELECT name, age FROM nodejs.users WHERE married=1 AND age>30;)
        - 2. OR로는 조건들 중 어느 하나라도 만족하는 데이터를 찾음 (ex.SELECT id, name FROM nodejs.users WHERE married=0 OR age>30;)
        - 3. 정렬을 원한다면 ORDER BY [컬럼명] [ASC|DESC] 키워드 사용 (ASC는 오름차순, DESC는 내림차순으로 SQL문 작성시 둘 중에 하나를 골라서 작성)
- Update: 데이터베이스에 있는 데이터를 수정하는 작업
    - 명령어: UPDATE [테이블명] SET [컬럼명=바꿀 값] WHERE [조건] ->조건에서 AND나 OR로 여러개 사용 가능
    - 예시: UPDATE nodejs.users SET comment='바꿀내용' WHERE id=2; -> id가 2인 로우의 컬럼을 수정 가능. 
- Delete: 데이터베이스에 있는 데이터를 삭제하는 작업
    - 명령어: DELETE FROM [테이블명] WHERE [조건]
    - 예시: DELETE FROM nodeks.users WHERE id=2; ->id가 2인 로우를 제거

# 시퀄라이즈
- 시퀄라이즈: MySQL 작업을 쉽게 할 수 있도록 도와주는 라이브러리
    - ORM(Object-relational Mapping)으로 분류. ORM은 자바스크립트 객체와 데이터베이스의 릴레이션을 매핑해주는 도구
    - 시퀄라이즈를 MySQL 뿐만 아니라 MariaDB, PostgreSQL, SQLite, MSSQL 등의 다른 데이터베이스도 같이 사용 가능
    - 사용이유: 자바스크립트 구문을 알라서 SQL로 바꿔줌. SQL언어를 직접 사용안해도 자바스크립트만으로 MySQL 조작가능.
- 시퀄라이즈 설치: 
    - 1. npm i express morgan numjucks sequelize sequelize-cli mysql2 ->시퀄라이즈에 필요한 패키지 설치
    - 2. npm i -D nodemon
    - 3. npx sequelize init -> config, models, migrations, sedders 폴더 생성
    
- mysql연결 (app.js 생성 후 익스프레스와 시퀄라이즈 연결 코드 작성)
```javascript
const express = require('express');
const path = require('path');
const morgan = require('morgan');
const numjucks = require('nunjucks'); 

const { sequelize } = require('./models');

const app = express();
app.set('port', process.env.PORT || 3001);
app.set('view engine', 'html');
nunjucks.configure('views', {
    express: app,
    watch: true,
});
sequelize.sync({force: false})
    .then(() => {
        console.log('데이터베이스 연결 성공');
    })
    .catch((err) => {
        console.error(err);
    });

app.use(morgan('dev'));
app.use(express.static(path.join(__dirname, 'public')));
app.use(express.json());
app.use(express.urlencoded({ extended: false}));

app.use((req, res, next) => {
    const error = new Error (`${req.method} ${req.url} 라우터가 없습니다.`)
    error.status= 404;
    next(error);
});

app.use((err,req,res,next) => {
    res.locals.message = err.message;
    res.locals.error = process.env.NODE_ENV !=='production' ? err : {};
    res.status(err.status ||500);
    res.render('error');
});
app.listen(app.get('port'),() => {
    console.log(app.get('port'), '번 포트에서 대기 중');
});
```

# 모델 정의하기: MySQL에서 정의한 테이블을 시퀄라이즈에서도 정의해야함
    - 시퀄라이즈는 모델과 MySQL의 테이블을 연결해주는 역할을 함
    ```javascript
    //models/user.js
    const Sequelize = require('sequelize');

    class User extends Sequelize.Model {
    static initiate(sequelize) {
        User.init({
        name: {
            type: Sequelize.STRING(20),
            allowNull: false,
            unique: true,
        },
        age: {
            type: Sequelize.INTEGER.UNSIGNED,
            allowNull: false,
        },
        married: {
            type: Sequelize.BOOLEAN,
            allowNull: false,
        },
        comment: {
            type: Sequelize.TEXT,
            allowNull: true,
        },
        created_at: {
            type: Sequelize.DATE,
            allowNull: false,
            defaultValue: Sequelize.NOW,
        },
        }, {
        sequelize,
        timestamps: false,
        underscored: false,
        modelName: 'User',
        tableName: 'users',
        paranoid: false,
        charset: 'utf8',
        collate: 'utf8_general_ci',
        });
    }

    static associate(db) {}
    };

    module.exports = User;
    ```
    - User 모델을 만들고 모듈로 exports 함. User모델은 Sequelize.Model을 확장한 클래스로 선언
    - Static initiate 메서드에서는 테이블에 대한 설정을 하고, static associate메서드에선 다른 모델과의 관계를 작성
    - 시퀄라이즈의 자료형은 MySQL의 자료형과는 조금 다름
        - VARCHAR는 STRING으로, INT는 INTEGER로, TINYINt는 BOOLEAN으로, DATETIME은 DATE로 작성
        - INTEGER.UNSIGNED는 UNSIGNED옵션이 적용된 INT, allowNull은 NOT NULL과 동일, unique는 UNIQUE옵션
    - 모델.init 메서드의 두번째 인수는 테이블 옵션
        - sequelize : static initiate 메서드의 매개변수와 연결되는 옵션으로 db.sequelize객체를 넣어야함.
        - timestamps : true이면 createdAt, updatedAt 컬럼 추가됨. 각각 로우가 생성될 때와 수정될 때의 시간이 자동으로 입력됨.false면 자동으로 날짜 컬럼을 추가하는 기능 해제
        - underscored : 테이블명과 컬럼명을 캐멀케이스로 만들었던 걸 스네이크케이스로 변경
        - modelName: 모델 이름을 설정 가능
        - paranoid: true설정시 deletedAt컬럼 생성됨(로우 삭제시 완전히 지워지지 않고 지워진 시각이 기록)
        - charset, collate: 각각 utf8, utf8_general_ci로 한글이 입력됌 
    
    ```javascript
    //comment.js
    const Sequelize = require('sequelize');

    class Comment extends Sequelize.Model {
    static initiate(sequelize) {
        Comment.init({
        comment: {
            type: Sequelize.STRING(100),
            allowNull: false,
        },
        created_at: {
            type: Sequelize.DATE,
            allowNull: true,
            defaultValue: Sequelize.NOW,
        },
        }, {
        sequelize,
        timestamps: false,
        modelName: 'Comment',
        tableName: 'comments',
        paranoid: false,
        charset: 'utf8mb4',
        collate: 'utf8mb4_general_ci',
        });
    }

    static associate(db) {
        db.Comment.belongsTo(db.User, { foreignKey: 'commenter', targetKey: 'id' });
    }
    };

    module.exports = Comment;
    ```
    - 모델 생성했다면 models/index.js와 연결함

    ```javascript
    //index.js
    const Sequelize = require('sequelize');
    const User = require('./user');
    const Comment = require('./comment');
    ...
    db.sequelize = sequelize;

    db.User = User;
    db.Comment = Comment;

    User.initiate(sequelize);
    Comment.initiate(sequelize);

    User.associate(db);
    Comment.associate(db);

    module.exports = db;
    ```
# 관계 정의하기

## 1:N
- 1:N 관계를 hasMany라는 메서드로 표현
- users 테이블의 로우 하나를 불러올 때 연결된 comments테이블의 로우들도 같이 불러올 수 있음.
- belongsto는 comments테이블의 로우를 불러올 때 연결된 users테이블의 로우 가져옴

## 1:1
- 1:1 관계를 hasOne이라는 메서드로 표현
- 1:1 관계라 해도 belongsTo와 hasOne이 반대면 안됨.

