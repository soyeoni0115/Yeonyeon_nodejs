5장 패키지 매니저
1) npm: Node Package Manager의 약어, 노드 패키지 매니저를 의미
대부분의 자바스크립트 프로그램은 npm에서 찾아서 설치하면 됌

2) package.json: 사용자가 다운받은 패키지의 버전을 관리하는 파일

노드 프로젝트를 시작하기 전에 폴더 내부에 무조건 package.json부터 만들고 시작해야함



3) 기본 설정 단계



    [1] npm init 실행하기

$ npm init
#콘솔창

This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and 
save it as a dependency in the package.json file.

press ^C at any time to quit.
package name: (폴더명) [프로젝트 이름 입력]
version: (1.0.0)[프로젝트 버전 입력]
description: [프로젝트 버전 입력]
entry point: index.js

test command : [엔터 키 클릭]
git repository: [엔터 키 클릭]
author: [사용자의 이름 입력]
license: (ISC)[엔터 키 클릭]
About to write to C:\Users\zerocho\npmtest\package.json:

{
"name": "npmtest",
"version": "0.0.1",
"description":"hello package.json",
"main":"index.js",
"scripts":{
	"test":"echo \"Error: no test specified\" && exit1"
},
"author":"ZeroCho",
"license":"ISC"
}

Is this ok? (yes) yes

1. package name: 패키지 이름. package.json의 name의 속성에 저장
2. version: 패키지의 버전
3. entry point: 자바스크리브 실행 파일 진입점. 보통 마지막으로 module.exports를 하는 파일 지정
4. test command: 코드 테스트 할 때 입력할 명령어 의미. package.jsonscript 속성 안의 test속성에 저장됨
5. git repository: 코드를 저장해둔 깃 저장소
6. keywords: npm공식 홈페이지에서 패키지를 쉽게 찾을 수 있게 함
7. license: 해당 패키지의 라이선스를 넣으면 됌





    [2] 패키지 설치 

        npm install [패키지 이름]-> 익스프레스 설치 목적

#package.json 파일 (npm init 실행 후)

{
    "name":"npmtest",
    "version":"0.0.1",
    "description":"hello package.json",
    "main":"index.js",
    "scripts":{
		"test" : "echo\"Error: no test specified\" && exit 1"
    },
    "author":"ZeroCho",
    "license":"ISC"
}


    - script 부분-> npm 명령어 저장해두는 부분
    - npm run [스크립트 명령어] 입력 시 해당 스크립트 실행
        ex)  해당 코드에선 npm run test 수행하면 echo “Error: no test specified"란 문자열이 콘솔에 찍히고 exit1의 의미인 에러와 함께 종료를 한다.





#npm install 패키지 후의 package.json

{
    "name":"npmtest",
    ...
    "license":"ISC",
    "dependencies" : {
		"express" : "^4.17.3",
    }
}


패키지 설치 후 package.json의 특징


1. dependencies 속성 생성됨- 그 안에 express라는 이름이랑 버전이 저장.
2. node_modules폴더 생성- 설치한 패키지들이 들어있음. express하나만 설치했는데 패키지가 여러개 들어있는 이유는 express가 의존하는 패키지들이 존재하기에
3. package-lock.json 파일 생성

    -  node_modules에 들어있는 패키지의 정확한 버전과 의존관계가 담겨있음.
    - 모듈 여러 개 동시 설치 시 npm install 패키지1 패키지2 로 나열하면 됨
    - 개발용 패키지 (개발 중에만 사용) npm install --save-dev 패키지 
4. peerDependencies가 package.json에 들어가 있는 경우가 있음.

    peerDependencies가 다른 패키지에도 있는데 버전이 다르다면 npm i --force로 강제로 모든 버전을 설치하거나 처음에 peerDependencies가 서로 충돌하는 패키지를 같이 설치 안하면 된다.

패키지 버전



패키지 버전을 1.0.8이라고 가정!
    - 1에 해당하는 첫번째 자리는 메이저 버전
    - 0에 해당하는 두번째 자리는 마이너 버전. 하위 호환이 되는 기능 업데이트를 할 때 올림.
    - 8에 해당하는 세번째 자리는 패치 버전. 기능 추가보단 기능에 문제가 있던 걸 수정한 것


6장

<익스프레스 웹 서버>
익스프레스: npm에서 서버를 제작하는 과정에서 겪게 되는 불편을 해소하고 편의 기능을 추가한 웹 서버 프레임워크
    ⁃ http 모듈의 요청과 응답 객체에 추가 기능 부여.
    ⁃ 코드를 분리하기 쉽게 만들어 관리에 용이
    ⁃ 더 이상 if문으로 요청 메서드와 주소를 구별하지 않아도 됨

<익스프레스 프로젝트 시작>



    1. learn-express 폴더 생성
    2. package.json 제일 먼저 생성 (npm init 명령어를 콘솔에서 호출 후 단계적으로 내용물 입력)
    3. scripts 부분에 start 속성 잊지 말고 삽입(nodemon app이면 app.js를 nodemon으로 실행하는 것)

//package.json

{
    "name":"leaern-express",
    "version":"0.0.1",
    "description":"익스프레스를 배우자",
    "main":"app.js",
    "scripts":{
        "start":"nodemon app"
    },
    "author":"Zerocho",
    "license":"MIT"
}




    4. app.js 작성
    -app.set('port', 포트번호)-> 서버 실행될 포트 설정..
    -app.set(키, 값)을 사용해 데이터를 저장할 수 있고, 나중에 데이터를 app.get(키)로 가져올 수 있음
    -app.get(주소, 라우터)-> 주소에 대한 get요청이 올 때 어떤 동작을 할지 적는 부분. 
    -매개변수 req는 요청에 관한 정보가 들어있는 객체, res는 응답에 관한 정보가 들어있는 객체..
    -get요청 말고도 post,put, patch, delete, options에 대한 라우터를 우한 메서드 존재. get일 때랑 똑같이 사용
    -app.listen-> 포트를 연결하고 서버 실행)

//app.js

const express = require('express');

const app= express();
app.set('port',process.env.PORT || 3000);

app.get('/', (req,res) =>{
    res.send('Hello,Express');
});

app.listen(app.get('port'), ()=> {
    console.log(app.get('port'),'번 포트에서 대기중');
});

    - 만약 문자열 대신 HTML로 응답하고 싶으면 res.sendFile 메서드 사용 (단, 파일의 경로를 path 모듈을 사용해서 지정해야됨)

// app.js -연결이 html인거

const express= require('express');
const path=require('path');

const app= express();
app.set('port',process.env.PORT||3000);
app.get('/',(req,res) =>{
    res.sendFile(path.join(__dirname, '/index.html'));
});

app.listen(app.get('port'),() =>{
    console.log(app.get('port'),'번 포트에서 대기중');
});


<미들웨어>

[1] 특징

익스프레스의 핵심으로 요청와 응답의 중간에 존재
라우터와 에러 핸들러도 미들웨어의 일종.
요청과 응답을 조작해 기능을 추가하기도 하고, 나쁜 요청을 걸러내기도 함
app.use와 함께 사용 app.use(미들웨어)
위에서 아래로 순서대로 실행되며 요청과 응답 사이에 특별한 기능을 추가할 후 있음. 


//app.js 예시

...
app.set('port',process.env.PORT||3000);

app.use((req,res,next) => {
    console.log('모든 요청에 다 실행됩니다,');
    next();
});

app.get('/',(req,res,next) =>{
    console.log('GET / 요청에서만 실행됩니다.');
    next();
}, (req,res) =>{
    throw new Error('에러는 에러 처리 미들웨어로 갑니다.')
});

app.use((err,req,res,next) =>{
    console.error(err);
    res.status(500).send(err.message);
});

app.listen(app.get('port'),() =>{
...


next라는 매개변수가 존재. -> 다음 미들웨어로 넘어가는 함수.
주소를 첫번째 인수로 넣어주지만 않으면 미들웨어는 모든 요청에서 실행되고 주소를 넣으면 해당하는 요청에서만 실행
app.use나 app.get같은 라우터에 미들웨어를 여러 개 장착할 수 있음.
에러 처리 미들웨어는 매개변수가 err,req,res,next가 있는데 4개를 다 사용하지 않아도 반드시 네 개여야 함.
err은 에러에 관한 정보가 담김.
에러 처리 미들웨어를 직접 연결하지 않아도 기본적으로 익스프레스가 처리를 하지만 실무에선 직접 연결해주는 게 좋음.



실무에서 자주 사용하는 패키지 다운

npm i morgan cookie-parser express-session dotenv


const express = require('expess');
const morgan = require('morgan');
const cookieParser = require('cookieParser');
const session = require('session');
const dotenv = require('dotenv');
const path = require('path');

dotenv.config();
const app= express();
app.set('port',process.env.PORT||3000);

//설치한 패키지들을 불러와 app.use에 연결.

app.use(morgan('dev'));
app.use('/',express.static(path.join(__dirname,'public')));
app.use(express.json());
app.use(express.urlencoded({ extended: false}));
app.use(cookieParser(process.env.COOKIE_SECRET));
app.use(session({
    resave: false,
    saveUninitialized: false;
    secret: process.env.COOKIE_SECRET,
    cookie:{
        httpOnly: true,
        secure: false,
    },
    name: 'session-cookie',
})));

app.use(req,res,next) => {
    console.log('모든 요청에 다 실행됩니다.');
}));

...

설치한 패키지들을 불러와 app.use에 연결.
dotenv패키지는 .env 파일을 읽어 process.env로 만듦



[morgan패키지]

morgan 연결 후 기존 로그 외에 추가적인 로그가 나온다
GET / 500 7.409ms - 50 ==>morgan미들웨어에서 나오는 것
http 메서드 / [주소] [http 상태코드] [응답 속도] - [응답바이트]
morgan 미들 웨어 사용 => app.use(morgan('dev'));
dev 말고도 combined, common, short, tiny 등을 넣을 수 있음.

[static 패키지]
static 미들웨어: 정적인 파일들을 제공하는 라우터 역할. 기본 제공이기에 express객체 내에서 꺼내 장착하면 됨.

app. use('요청 경로‘, express.static('실제 경로’));
app. use('/',express.static(path.join(__dirname, 'public')));

함수 인수로 정적 파일들이 담겨 있는 폴더 지정하면 됨. 현재는 public이 지정되어있음. 
주의! 실제 서버의 폴더 경로에는 public이 들어 있지만, 요청 주소에는 public이 들어있지 않음. 서버의 폴더 경로와 요청 겨오가 달라 외부인이 서버의 구조를 쉽게 파악할 수 없음. (보안에 좋음)


[body-parser]
body-parser 미들웨어: 요청의 본문에 있는 데이터를 해석해서 req.body 객체로 만들어주는 미들웨어.
보통 폼 데이터나 AJAX 요청의 데이터 처리

JSON 파싱: app.use(express.json());
URL-encoded 폼 데이터 파싱: app.use(express.urlencoded({ extended: false }));
extended: false: Node.js의 기본 querystring 모듈을 사용합니다.
extended: true: qs 모듈을 사용하여 더 복잡한 객체 파싱이 가능합니다.
주의: body-parser는 텍스트 기반의 본문만 처리합니다. 파일 업로드(multipart/form-data)는 처리하지 못하며, 이는 multer가 담당합니다.


2) cookie-parser
요청에 포함된 쿠키를 파싱하여 req.cookies 객체에서 쉽게 접근할 수 있도록 만듭니다.

JavaScript

app.use(cookieParser('나만의비밀키'));
cookieParser에 비밀 키를 전달하면, '서명된 쿠키(Signed Cookies)'를 파싱할 수 있습니다.
서명된 쿠키는 서버에서 발급한 쿠키가 클라이언트에서 변조되지 않았음을 보장하는 데 사용되며, req.signedCookies 객체를 통해 접근할 수 있습니다.
주의: cookie-parser는 쿠키를 읽고 파싱하는 역할만 합니다. 쿠키를 생성(res.cookie)하거나 삭제(res.clearCookie)하는 것은 res 객체의 메서드를 사용해야 합니다.

3) express-session (세션 관리)
로그인 정보 등 사용자에 특화된 데이터를 서버에 임시로 저장하고 관리(세션 관리)할 때 사용합니다.

app.use(session({
    resave: false,
    saveUninitialized: false,
    secret: process.env.COOKIE_SECRET, // cookieParser의 비밀 키와 동일하게 설정
    cookie: {
        httpOnly: true,
        secure: false, // 배포 시에는 true로 설정
        maxAge: 60 * 60 * 1000 // 1시간
    },
    name: 'connect.sid' // 세션 쿠키 이름 (기본값)
}));
express-session은 클라이언트에 세션 ID를 담은 쿠키(기본값 connect.sid)를 전송하고, 서버는 이 ID를 기반으로 req.session 객체에 데이터를 저장하고 관리합니다.
주요 옵션
secret: 세션 ID 쿠키를 서명하는 데 사용되는 비밀 키 (필수)
resave: false: 세션이 수정되지 않아도 강제로 다시 저장하지 않음
saveUninitialized: false: 초기화되지 않은(새로 생겼지만 내용이 없는) 세션을 저장하지 않아 불필요한 세션 생성을 막음.
store: 세션 데이터를 저장할 위치를 지정
기본값은 메모리이며, 이는 서버가 재시작되면 초기화.
운영 환경에서는 redis와 같은 외부 데이터베이스를 store로 연결하는 것이 좋음.
4) multer (파일 업로드)
multer 미들웨어: body-parser가 처리하지 못하는 multipart/form-data 형식(주로 파일 업로드)을 처리하는 미들웨어

const multer = require('multer');
const path = require('path');
const fs = require('fs');

// 'uploads' 폴더가 없으면 생성
try {
    fs.readdirSync('uploads');
} catch (error) {
    console.error('uploads 폴더가 없어 생성합니다.');
    fs.mkdirSync('uploads');
}

const upload = multer({
    storage: multer.diskStorage({
        destination(req, file, done) {
            done(null, 'uploads/'); // 저장할 경로
        },
        filename(req, file, done) {
            const ext = path.extname(file.originalname);
            const basename = path.basename(file.originalname, ext);
            // 파일명 중복 방지를 위해 현재 시간을 추가
            done(null, basename + Date.now() + ext);
        }
    }),
    limits: { fileSize: 5 * 1024 * 1024 } // 5MB 파일 크기 제한
});
설정: storage 옵션으로 파일이 저장될 위치(destination)와 파일명(filename)을 지정합니다. limits로 파일 크기 등을 제한할 수 있습니다.
사용: 라우터 미들웨어로 upload 객체의 메서드를 사용합니다.
upload.single('image'): image라는 이름의 필드(name)로 전송된 단일 파일을 처리합니다. 파일 정보는 req.file에, 텍스트 필드 정보는 req.body에 저장됩니다.
upload.array('images'): 여러 파일을 처리합니다. (req.files)
upload.fields([...]): 여러 필드의 파일들을 처리합니다. (req.files)
이전에는 upload.single()을 사용해 단일 파일을 업로드하는 방법을 알아봤습니다. multer는 여러 개의 파일을 업로드하는 다양한 시나리오에도 대응할 수 있습니다.

upload.array(fieldName): 여러 파일 업로드 (동일 필드)

HTML 폼에서 <input type="file" name="many" multiple>처럼 multiple 속성을 사용해 여러 파일을 한 번에 업로드할 때 사용합니다.

HTML 예시 (multipart.html)
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="many" multiple />
    <input type="text" name="title" />
    <button type="submit">업로드</button>
</form>
라우터 예시 (app.js)
app.post('/upload', upload.array('many'), (req, res) => {
    console.log(req.files); // 업로드된 파일 정보 배열
    console.log(req.body);  // 텍스트 필드 정보 (title)
    res.send('ok');
});




upload.fields(fieldsArray): 여러 파일 업로드 (다른 필드)
<input type="file" name="image1">, <input type="file" name="image2">처럼 서로 다른 name 속성을 가진 여러 개의 파일 입력을 처리할 때 사용합니다.

HTML 예시 (multipart.html)
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="image1" />
    <input type="file" name="image2" />
    <input type="text" name="title" />
    <button type="submit">업로드</button>
</form>
라우터 예시 (app.js)
app.post('/upload', 
    upload.fields([{ name: 'image1' }, { name: 'image2' }]), 
    (req, res) => {
        console.log(req.files.image1); // image1 파일 정보
        console.log(req.files.image2); // image2 파일 정보
        console.log(req.body);         // 텍스트 필드 정보 (title)
        res.send('ok');
    }
);
결과: req.files 객체 안에, fields 배열에 정의한 name 속성을 키로 하는 파일 정보들이 담김 (예: req.files.image1[0], req.files.image2[0])
upload.none(): 파일 없는 multipart/form-data 처리

파일 업로드는 없지만, enctype이 multipart/form-data인 폼의 텍스트 데이터만 처리하고 싶을 때 사용합니다.

HTML 예시 (multipart.html)
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="text" name="title" />
    <button type="submit">업로드</button>
</form>
라우터 예시 (app.js)
app.post('/upload', upload.none(), (req, res) => {
    console.log(req.body); // 텍스트 필드 정보 (title)
    res.send('ok');
});
결과: req.file이나 req.files는 생성되지 않으며, 오직 req.body에 텍스트 데이터만 담김
6. Express의 꽃, Router로 라우팅 분리하기
app.js 파일 하나에 app.get(), app.post() 등이 계속 늘어나면 코드가 길어지고 관리가 매우 어려워집니다. Express는 express.Router를 통해 라우팅 로직을 기능별로 별도의 파일로 분리할 수 있게 해줍니다.

1) 라우터 파일 생성하기
먼저, routes 폴더를 만들고 그 안에 기능별 라우터 파일을 생성합니다.



routes/index.js (메인 페이지 라우터)
const express = require('express');
const router = express.Router();

// GET /
router.get('/', (req, res) => {
    res.send('Hello, Express');
});

module.exports = router;
routes/user.js (사용자 관련 라우터)
JavaScript

const express = require('express');
const router = express.Router();

// GET /user/
router.get('/', (req, res) => {
    res.send('Hello, User');
});

module.exports = router;

2) app.js에서 라우터 연결하기
app.js에서는 require로 라우터 파일을 불러와 app.use()로 연결합니다. 이 때, 공통되는 주소를 app.use()의 첫 번째 인수로 지정할 수 있습니다.

// app.js

const indexRouter = require('./routes'); // ./routes/index.js와 동일
const userRouter = require('./routes/user');

// ... (세션, 쿠키파서 미들웨어) ...

// indexRouter를 / 경로에 연결
app.use('/', indexRouter); 
// userRouter를 /user 경로에 연결
app.use('/user', userRouter); 

// ... (404, 에러 핸들러) ...
app.use('/', indexRouter): indexRouter 내부의 router.get('/')는 GET / 요청을 처리합니다.
app.use('/user', userRouter): userRouter 내부의 router.get('/')는 GET /user 요청을 처리합니다.

7. 라우팅 고급 기법
라우터를 더 효율적으로 관리하는 몇 가지 유용한 기법을 소개합니다.


1) app.route(): 같은 주소, 다른 메서드 묶기
/abc라는 동일한 주소에 대해 GET, POST, PUT 등 여러 메서드를 처리할 때, app.route()를 사용하면 코드를 깔끔하게 그룹화할 수 있습니다.

기존 방식
app.get('/abc', (req, res) => { res.send('GET /abc'); });
app.post('/abc', (req, res) => { res.send('POST /abc'); });
app.route() 사용
app.route('/abc')
    .get((req, res) => { res.send('GET /abc'); })
    .post((req, res) => { res.send('POST /abc'); });


2) 라우트 매개변수 (Route Parameters)
주소의 특정 부분을 동적으로 받아 처리해야 할 때 사용합니다. (예: 사용자 ID)



// :id 부분이 매개변수입니다.
router.get('/user/:id', (req, res) => {
    // req.params 객체에 담깁니다.
    console.log(req.params.id); // '123'
    res.send('User ID: ' + req.params.id);
});
/user/123, /user/abc 등의 요청을 모두 이 라우터가 처리합니다.
주의! 라우트 매개변수를 사용하는 라우터는 일반 라우터보다 항상 뒤에 배치해야 합니다.
// (O) 올바른 순서
router.get('/user/like', ...); // 1. /like 먼저 처리
router.get('/user/:id', ...);  // 2. 그 외 /:id 처리

// (X) 잘못된 순서
router.get('/user/:id', ...);  // '/user/like' 요청도 :id가 'like'라고 인식
router.get('/user/like', ...);  // 이 코드는 절대 실행되지 않음

8. 핵심 객체: req (요청)와 res (응답)
미들웨어와 라우터가 항상 다루는 req와 res 객체에는 Express가 추가한 유용한 속성과 메서드가 많습니다.

1) req (Request) 객체의 주요 속성
req.body: body-parser나 express.json 미들웨어가 파싱한 요청 본문 데이터 (주로 POST, PUT).
req.params: 라우트 매개변수 정보 (예: /user/:id의 id 값).
req.query: 쿼리스트링 정보 (예: /search?q=hello의 { q: 'hello' }).
req.cookies: cookie-parser가 파싱한 쿠키 객체.
req.signedCookies: cookie-parser가 파싱한 서명된 쿠키 객체.
req.get(헤더이름): 요청 헤더에서 특정 값을 가져옵니다.
2) res (Response) 객체의 주요 메서드
res.send(데이터): 가장 범용적인 응답 메서드. 문자열, HTML, JSON 객체, 버퍼 등을 보냅니다.
res.json(데이터): JSON.stringify를 실행하여 JSON 형식으로 응답합니다.
res.status(코드): HTTP 상태 코드를 설정합니다. (예: res.status(404)).
res.sendFile(경로): 지정된 경로의 파일을 응답으로 보냅니다.
res.redirect(주소): 클라이언트를 지정된 주소로 리다이렉트시킵니다.
res.render(뷰이름, 데이터): 템플릿 엔진(예: Pug, EJS)을 사용하여 뷰를 렌더링합니다.
res.cookie(키, 값, 옵션): 클라이언트에 쿠키를 설정합니다.
res.clearCookie(키, 옵션): 클라이언트의 쿠키를 삭제합니다.
res.set(헤더이름, 값): 응답 헤더를 설정합니다.
이러한 res 객체의 메서드들은 **메서드 체이닝(Method Chaining)**을 지원하는 경우가 많아 코드를 간결하게 작성할 수 있습니다.

res.status(201)
   .cookie('test', 'test')
   .redirect('/admin');

9. 404 처리 및 에러 핸들러
미들웨어 스택의 마지막에는 404 처리와 에러 핸들러를 배치해야 합니다.



// ... (라우터 미들웨어들) ...

// 1. 404 처리 미들웨어
// 위쪽의 모든 라우터에서 요청을 처리하지 못한 경우 여기까지 내려옴
app.use((req, res, next) => {
    res.status(404).send('Not Found');
});

// 2. 에러 처리 미들웨어
// (err, req, res, next) 매개변수 4개를 모두 가져야 함
app.use((err, req, res, next) => {
    console.error(err);
    res.status(500).send('Internal Server Error');
});
404 미들웨어는 일반 미들웨어와 동일하며, 다른 라우터들보다 아래에 위치해야 합니다.
에러 처리 미들웨어는 err를 첫 번째 매개변수로 가지며, 가장 마지막에 위치해야 합니다.


10. 템플릿 엔진(Template Engine)이란?
HTML은 정적입니다. 만약 1,000개의 데이터를 서버에서 받아 동적으로 목록을 만들어야 한다면, 순수 HTML로는 불가능에 가깝습니다.

이때 템플릿 엔진을 사용합니다. 템플릿 엔진은 if 같은 조건문, each 같은 반복문, 그리고 변수 등을 사용할 수 있는 특별한 문법을 제공합니다. 개발자가 이 문법으로 템플릿(예: index.pug)을 만들어두면, 서버는 이 템플릿에 실제 데이터를 결합하여 최종 HTML 파일을 생성한 뒤 클라이언트에 응답합니다.

여기서는 대표적인 템플릿 엔진인 Pug(퍼그)(구 Jade)의 설치와 사용법을 알아보겠습니다.

11. Express에 Pug 설치 및 연동하기
먼저 npm으로 pug를 설치합니다.

$ npm i pug




그다음 app.js에서 Express가 Pug를 템플릿 엔진으로 사용하도록 설정합니다.

// app.js

const path = require('path');
// ...

// 1. 템플릿 파일들이 위치할 폴더를 지정합니다. (기본값: views)
app.set('views', path.join(__dirname, 'views'));
// 2. 사용할 템플릿 엔진을 pug로 설정합니다.
app.set('view engine', 'pug');

// ...




이제 res.send() 대신 res.render() 메서드를 사용할 수 있습니다.

router.get('/', (req, res, next) => {
    // views 폴더의 index.pug 파일을 렌더링합니다.
    // { title: 'Express' } 객체를 통해 변수를 전달합니다.
    res.render('index', { title: 'Express' });
});
res.render('index', ...)는 views/index.pug를 찾아 렌더링합니다.
res.render('admin/main', ...)는 views/admin/main.pug를 찾아 렌더링합니다.

12. Pug 기본 문법 마스터하기
Pug는 HTML과 문법이 많이 다릅니다. 닫는 태그가 없고, 들여쓰기로 부모-자식 관계를 구분합니다.

1) 기본 HTML 표현
들여쓰기가 핵심입니다.
div 태그는 기본값이라 생략 가능합니다.
ID: # (예: #login-button → <div id="login-button"></div>)
Class: . (예: .post-image → <div class="post-image"></div>)
태그 + ID/Class: span#highlight → <span id="highlight"></span>
속성: () 안에 작성합니다. (예: button(type='submit') 전송)
텍스트: 태그 뒤에 한 칸 띄고 작성합니다. (예: p 안녕하세요.)
여러 줄 텍스트: | (파이프) 기호를 사용합니다.
 
p
  | 안녕하세요.
  | 여러 줄을 입력합니다.
코드 스니펫

2) style 및 script 태그
태그 뒤에 .을 붙여 CSS나 JavaScript 코드를 직접 작성할 수 있습니다.

코드 스니펫

style.
  h1 { font-size: 30px; }

script.
  const message = 'Pug';
  alert(message);
3) 변수 사용하기 (Express에서 전달받은)
res.render('index', { title: 'Express' })로 전달한 title 변수는 Pug에서 다음과 같이 사용할 수 있습니다.

=변수명 (Escaped): 태그의 텍스트로 변수를 출력합니다. HTML 특수문자(, >`)는 **이스케이프(escape)**되어 안전하게 표시됩니다.
h1= title
// 결과: <h1>Express</h1>


!=변수명 (Unescaped): 변수 내용을 이스케이프하지 않고 그대로 출력합니다. HTML 태그가 포함된 변수를 렌더링할 때 사용하지만, XSS 공격에 취약할 수 있으니 주의해야 합니다.
p!= '<strong>강조</strong>'
// 결과: <p><strong>강조</strong></p>
#{변수명} (보간법): 텍스트 중간에 변수를 삽입할 때 사용합니다.
p Welcome to #{title}
// 결과: <p>Welcome to Express</p>




속성에서 사용: input(placeholder=title + ' 연습')처럼 속성값으로도 사용할 수 있습니다.
Tip: res.render 외에 res.locals 객체를 사용해 변수를 전달할 수도 있습니다. res.locals.title = 'Express'와 같이 설정하면, 해당 요청을 처리하는 모든 미들웨어와 템플릿에서 title 변수에 접근할 수 있습니다.

4) 조건문 (if, case)
if, else, case를 사용하여 조건부 렌더링을 할 수 있습니다.

if isLoggedIn
  div 로그인이 되었습니다.
else
  div 로그인이 필요합니다.

case fruit
  when 'apple'
    p 사과입니다.
  when 'banana'
    p 바나나입니다.
  default
    p 사과도 바나나도 아닙니다.
5) 반복문 (each)
each를 사용해 배열을 순회합니다. for를 써도 동일하게 작동합니다.

ul
  each fruit in ['사과', '배', '오렌지']
    li= fruit

// 인덱스 사용 시
ul
  each fruit, index in ['사과', '배', '오렌지']
    li= (index + 1) + '번째 ' + fruit

13. 레이아웃 재사용: include와 extends
웹사이트는 보통 헤더, 푸터, 내비게이션 등 공통된 레이아웃을 가집니다. Pug는 이를 효율적으로 관리하는 두 가지 방법을 제공합니다.

1) include (파일 포함)
단순히 다른 Pug 파일을 현재 위치에 삽입합니다. 헤더, 푸터 등 독립적인 컴포넌트를 관리할 때 유용합니다.

header.pug
header
  a(href='/') Home
main.pug
main
  include header // header.pug 파일 내용이 여기에 삽입됨
  h1 메인 파일
  include footer
2) extends와 block (레이아웃 상속)
더 강력한 레이아웃 관리 방식입니다. 공통의 "틀"(layout.pug)을 만들고, 페이지마다 달라지는 부분(block)만 채워 넣습니다.

layout.pug (기본 틀)
doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/style.css')
  body
    // 이 부분이 페이지마다 다른 내용으로 채워짐
    block content 
body.pug (실제 페이지)
// layout.pug 파일을 상속받음
extends layout

// layout.pug의 'block content' 부분을 이 내용으로 대체
block content
  main
    p 내용입니다.
res.render('body')를 호출하면, Pug는 body.pug가 상속한 layout.pug를 먼저 읽고, 그 안의 block content를 body.pug의 block content 내용으로 채워 넣어 최종 HTML을 완성합니다.



14. 또 다른 강력한 템플릿 엔진, Nunjucks(넌적스)
Pug가 간결한 문법을 제공하는 반면, HTML과 문법이 너무 달라 호불호가 갈리기도 합니다. Nunjucks는 Mozilla에서 만든 템플릿 엔진으로, HTML 문법을 그대로 사용하면서 Python의 Jinja2나 Twig와 유사한 강력한 기능을 제공합니다.

기존 HTML 문법에 익숙한 개발자에게 훌륭한 대안이 될 수 있습니다.

15. Nunjucks 설치 및 Express 연동
먼저 npm으로 nunjucks를 설치합니다.

$ npm i nunjucks


app.js 설정은 Pug와 조금 다릅니다. Nunjucks는 .html 확장자를 그대로 사용하는 경우가 많습니다.

// app.js

const nunjucks = require('nunjucks');
// ...

// 1. 사용할 템플릿 엔진을 'html'로 설정합니다.
// (Pug와 달리, 확장자를 njk 등으로 커스텀할 수도 있습니다)
app.set('view engine', 'html');

// 2. Nunjucks를 설정합니다.
nunjucks.configure('views', { // 템플릿 파일들이 위치할 폴더
    express: app,  // express 객체 연결
    watch: true,   // true로 설정하면 HTML 파일 변경 시 템플릿 엔진이 다시 렌더링
});

// ...

16. Nunjucks 기본 문법
Nunjucks는 변수와 로직을 위한 특별한 기호 {{ }}와 {% %}를 사용합니다.

1) 변수 사용하기 {{ }}
Express에서 res.render('index', { title: 'Express' })로 전달한 변수는 {{ }}를 사용해 출력합니다.

기본 출력 (이스케이프 O): {{ title }}
<h1>{{ title }}</h1>
HTML 이스케이프 (Escaping):
{{ '<strong>strong</strong>' }} → &lt;strong&gt;strong&lt;/strong&gt;
이스케이프 끄기 (Unescaped): 변수에 HTML 태그가 담겨있어 그대로 렌더링해야 한다면 safe 필터를 사용합니다.
<p>{{ '<strong>strong</strong>' | safe }}</p>
변수 선언: 템플릿 내부에서 set을 사용해 변수를 선언할 수 있습니다.
{% set js = 'Javascript' %}
<p>{{ js }}</p>


2) 반복문 {% for %}
{% for %}와 {% endfor %} 블록을 사용하여 배열을 순회합니다.

{% set fruits = ['사과', '배', '오렌지', '복숭아'] %}
<ul>
  {% for item in fruits %}
    <li>{{ item }}</li>
  {% endfor %}
</ul>

<ul>
  {% for item in fruits %}
    <li>{{ loop.index + 1 }}번째 {{ item }}</li>
  {% endfor %}
</ul>
3) 조건문 {% if %}
{% if %}, {% elif %}, {% else %}, {% endif %}를 사용합니다.

{% if isLoggedIn %}
  <div>로그인이 되었습니다.</div>
{% else %}
  <div>로그인이 필요합니다.</div>
{% endif %}

{% if fruit == 'apple' %}
  <p>사과입니다.</p>
{% elif fruit == 'banana' %}
  <p>바나나입니다.</p>
{% else %}
  <p>사과도 바나나도 아닙니다.</p>
{% endif %}
17. Nunjucks 레이아웃 재사용
Nunjucks도 include, extends, block을 통해 효율적으로 레이아웃을 관리합니다.

1) include (파일 포함)
Pug와 동일하게 헤더, 푸터 등 공통 요소를 삽입할 때 사용합니다.

main.html
<main>
  {% include "header.html" %}
  <h1>메인 파일</h1>
  {% include "footer.html" %}
</main>
2) extends와 block (레이아웃 상속)
Pug와 정확히 같은 개념으로, HTML 문법으로 작성된다는 점만 다릅니다.

layout.html (기본 틀)
<!DOCTYPE html>
<html>
  <head>
    <title>{{ title }}</title>
  </head>
  <body>
    {% block content %}
    {% endblock %}
  </body>
</html>




index.html (실제 페이지)
{% extends 'layout.html' %}

{% block content %}
  <h1>{{ title }}</h1>
  <p>Welcome to {{ title }}</p>
{% endblock %}
18. 템플릿 엔진과 에러 핸들러 연동
404나 500 에러가 발생했을 때, Nunjucks로 만든 에러 페이지를 보여줄 수 있습니다.

1) 404 및 에러 처리 미들웨어 수정
app.js에서 404 및 에러 처리 미들웨어가 res.render를 호출하도록 수정합니다. 이 때 res.locals를 사용해 템플릿에 변수를 전달합니다.

// app.js

// 404 처리 미들웨어
app.use((req, res, next) => {
    const error = new Error(`${req.method} ${req.url} 라우터가 없습니다.`);
    error.status = 404;
    next(error); // 에러 핸들러로 넘김
});

// 에러 처리 미들웨어 (맨 마지막에 위치)
app.use((err, req, res, next) => {
    // 템플릿에 전달할 변수 설정
    res.locals.message = err.message;
    // 개발 환경에서만 스택 트레이스 노출
    res.locals.error = process.env.NODE_ENV !== 'production' ? err : {};

    res.status(err.status || 500);
    res.render('error'); // views/error.html을 렌더링
});
2) 에러 페이지 템플릿 (error.html)
res.locals로 전달된 message와 error 변수를 사용해 에러 페이지를 만듭니다.

HTML

{% extends 'layout.html' %}

{% block content %}
  <h1>{{ message }}</h1>
  <h2>{{ error.status }}</h2>
  <pre>{{ error.stack }}</pre>
{% endblock %}
이렇게 설정하면 개발 중에는 상세한 에러 스택을 확인하고, 운영 환경에서는 사용자에게 보안에 민감한 스택 트레이스를 노출하지 않고 에러 메시지만 보여줄 수 있습니다.