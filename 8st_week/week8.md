# 프로젝트 세팅 및 패키지 설치
- 먼저 프로젝트 폴더(`nodebird`)를 생성하고 초기화를 진행
```bash
mkdir nodebird
cd nodebird
npm init -y
```

## 필요 패키지 설치
- 서버 구동, DB 연결, 인증, 이미지 처리에 필요한 라이브러리들을 한 번에 설치

핵심 패키지
    ```bash
    npm i express cookie-parser express-session morgan multer dotenv nunjucks
    ```
데이터베이스 관련 (MySQL, Sequelize)
    ```bash
    npm i sequelize mysql2 sequelize-cli
    ```
로그인/인증 관련 (Passport)
    ```bash
    npm i passport passport-local passport-kakao bcrypt
    ```
개발용 패키지 (코드 수정 시 서버 자동 재시작)
    ```bash
    npm i -D nodemon
    ```


## 디렉토리 구조 생성

다음과 같이 폴더들을 미리 만들어둡니다.

```bash
`config`: DB 설정
`models`: DB 모델
`routes`: 라우터
`views`: 화면(HTML)
`passport`: 인증 전략
`public`: CSS, 정적 파일
`controllers`: 컨트롤러 (로직 분리)
'seeders' : init 시 자동 생성되는 폴더
'.env' : 비밀 키 모음
```

# 기본 서버 설정 (app.js)
- 서버의 진입점인 `app.js`를 작성.
- 각종 미들웨어와 라우터, 패스포트 설정을 연결하는 중심 파일

 app.js
 ```javascript
const express = require('express');
const cookieParser = require('cookie-parser');
const morgan = require('morgan');
const path = require('path');
const session = require('express-session');
const nunjucks = require('nunjucks');
const dotenv = require('dotenv');
const passport = require('passport');

dotenv.config();
const pageRouter = require('./routes/page');
const authRouter = require('./routes/auth');
const postRouter = require('./routes/post');
const userRouter = require('./routes/user');
const { sequelize } = require('./models');
const passportConfig = require('./passport');

const app = express();
passportConfig(); // 패스포트 설정
app.set('port', process.env.PORT || 8001);
app.set('view engine', 'html');
nunjucks.configure('views', {
  express: app,
  watch: true,
});

// DB 연결 (sync)
sequelize.sync({ force: false })
  .then(() => {
    console.log('데이터베이스 연결 성공');
  })
  .catch((err) => {
    console.error(err);
  });

app.use(morgan('dev'));
app.use(express.static(path.join(__dirname, 'public')));
app.use('/img', express.static(path.join(__dirname, 'uploads'))); // 업로드 이미지 제공
app.use(express.json());
app.use(express.urlencoded({ extended: false }));
app.use(cookieParser(process.env.COOKIE_SECRET));
app.use(session({
  resave: false,
  saveUninitialized: false,
  secret: process.env.COOKIE_SECRET,
  cookie: {
    httpOnly: true,
    secure: false,
  },
}));
app.use(passport.initialize());
app.use(passport.session());

// 라우터 연결
app.use('/', pageRouter);
app.use('/auth', authRouter);
app.use('/post', postRouter);
app.use('/user', userRouter);

// 404 처리 미들웨어
app.use((req, res, next) => {
  const error = new Error(`${req.method} ${req.url} 라우터가 없습니다.`);
  error.status = 404;
  next(error);
});

// 에러 처리 미들웨어
app.use((err, req, res, next) => {
  res.locals.message = err.message;
  res.locals.error = process.env.NODE_ENV !== 'production' ? err : {};
  res.status(err.status || 500);
  res.render('error');
});

app.listen(app.get('port'), () => {
  console.log(app.get('port'), '번 포트에서 대기 중');
});

```

그 외로 기본적인 라우터와 템플릿 엔진을 만들어보면..

routes폴더
    page.js
        profile, join, 메인으로 페이지 3개로 구성
        res.locals 안에 user, followingCount, followercount, followingIdList 변수 설정
        컨트롤러 존재 - 라우터의 미들웨어를 다른 곳에서 불러옴 ( 프로젝트에 controllers 폴더 만들고 page.js 따로 만듦)

views 폴더
    layout.html
        기본적인 틀을 설정
        로그인, 회원가입 등 메인 화면 구성
        if문으로 렌더링 시 user가 있으면 사용자 정보와 팔로잉, 팔로워 수를 보여주고, 존재하지 않으면 로그인 메뉴를 보여줌
    main.html
        user변수가 존재할 때 게시글 업로드 폼을 보여줌
        렌더링 시 twits 배열 안의 요소들을 읽어 게시글로 만들음
        팔로우의 여부와 작성자가 자기 자신임을 구분하는 구문을 작성하여 역할을 분리함
    profile.html
        사용자의 팔로워와 사용자가 팔로잉 중인 목록을 보여줌
    join.html
        회원가입하는 폼을 보여줌
    error.html
        서버에 error가 발생했을 때 에러 내역을 보여줌

public 폴더
    main.css
        전체적인 디자인을 보여줌


=> 다 설정 후 서버 실행

npm start

# 데이터베이스 모델링 (Sequelize)
- models구조
    - user.js
    - post.js
    - hashtag.js
    - index.js


## User 모델 (models/user.js)
- 사용자 정보를 담음. SNS 로그인(카카오)을 위해 `provider`, `snsId` 컬럼을 둠

models/user.js
```javascript
const Sequelize = require('sequelize');

class User extends Sequelize.Model {
  static initiate(sequelize) {
    User.init({
      email: {
        type: Sequelize.STRING(40),
        allowNull: true,
        unique: true,
      },
      nick: {
        type: Sequelize.STRING(15),
        allowNull: false,
      },
      password: {
        type: Sequelize.STRING(100),
        allowNull: true,
      },
      provider: {
        type: Sequelize.ENUM('local', 'kakao'),
        allowNull: false,
        defaultValue: 'local',
      },
      snsId: {
        type: Sequelize.STRING(30),
        allowNull: true,
      },
    }, {
      sequelize,
      timestamps: true,
      paranoid: true,
      modelName: 'User',
      tableName: 'users',
      charset: 'utf8',
      collate: 'utf8_general_ci',
    });
  }

  static associate(db) {
};

module.exports = User;

```

## Post 모델 (models/post.js)
- 게시글 내용과 이미지 경로를 저장
- 게시글 등록자의 아이디를 담은 컬럼은 나중에 관계를 설정할 때 시퀄라이즈가 알아서 생성

models/post.js
```javascript
const Sequelize = require('sequelize');

class Post extends Sequelize.Model {
  static initiate(sequelize) {
    Post.init({
      content: {
        type: Sequelize.STRING(140),
        allowNull: false,
      },
      img: {
        type: Sequelize.STRING(200),
        allowNull: true,
      },
    }, {
      sequelize,
      timestamps: true,
      paranoid: false,
      modelName: 'Post',
      tableName: 'posts',
      charset: 'utf8mb4',
      collate: 'utf8mb4_general_ci',
    });
  }

  static associate(db) {
    db.Post.belongsTo(db.User);
    db.Post.belongsToMany(db.Hashtag, { through: 'PostHashtag' });
  }
};

module.exports = Post;
```

## Hashtag 모델 (models/hashtag.js)

태그 검색을 위해 별도로 저장합니다.

> **models/hashtag.js**

```javascript
const Sequelize = require('sequelize');

class Hashtag extends Sequelize.Model {
  static initiate(sequelize) {
    Hashtag.init({
      title: {
        type: Sequelize.STRING(15),
        allowNull: false,
        unique: true,
      },
    }, {
      sequelize,
      timestamps: true,
      paranoid: false,
      modelName: 'Hashtag',
      tableName: 'hashtags',
      charset: 'utf8mb4',
      collate: 'utf8mb4_general_ci',
    });
  }

  static associate(db) {
    db.Hashtag.belongsToMany(db.Post, { through: 'PostHashtag' });
  }
};

module.exports = Hashtag;
```
```
## Hashtag 모델 (models/hashtag.js)
- 태그 이름을 저장, 나중에 태그로 검색하기 위해 따로 분리

```javascript
const Sequelize = require('sequelize');

class Hashtag extends Sequelize.Model {
  static initiate(sequelize) {
    Hashtag.init({
      title: {
        type: Sequelize.STRING(15),
        allowNull: false,
        unique: true,
      },
    }, {
      sequelize,
      timestamps: true,
      paranoid: false,
      modelName: 'Hashtag',
      tableName: 'hashtags',
      charset: 'utf8mb4',
      collate: 'utf8mb4_general_ci',
    });
  }

  static associate(db) {}
};

module.exports = Hashtag;
```



index.js
작성한 모델들을 시퀄라이즈에 등록하고 관계를 연결.

# 로그인 구현 (Passport)
- 로그인 인증을 담당할 Passport 설정을 진행합니다.


## Passport 설정 (passport/index.js)
- 로그인 시(`serializeUser`)와 매 요청 시(`deserializeUser`) 어떻게 동작할지 정의합니다.
- passport 모듈 설치 후 미리 app.js와 연결

passport/index.js
```javascript
const passport = require('passport');
const local = require('./localStrategy');
const kakao = require('./kakaoStrategy');
const User = require('../models/user');

module.exports = () => {
  passport.serializeUser((user, done) => {
    done(null, user.id); // 세션에 user.id만 저장
  });

  passport.deserializeUser((id, done) => {
    User.findOne({where: { id })
      .then(user => done(null, user)) // req.user에 저장
      .catch(err => done(err));
  });

  local();
  kakao();
};
```


## 로컬 로그인 구현
로컬 로그인: SNS 서비스 로그인이 아닌 자체적 회원가입을 진행한 후 하는 로그인을 의미
    - Passport에서 구현하려면 passport-local 모듈이 필요한데 이미 설치함. 로컬 로그인 전략만 세우면 됨.
    - 그 전에 회원가입, 로그인, 로그아웃 라우터를 먼저 만듦.

passport/localStrategy.js
```javascript
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
const bcrypt = require('bcrypt');
const User = require('../models/user');

module.exports = () => {
  passport.use(new LocalStrategy({
    usernameField: 'email',
    passwordField: 'password',
    passReqToCallback: false,
  }, async (email, password, done) => {
    try {
      const exUser = await User.findOne({ where: { email } });
      if (exUser) {
        const result = await bcrypt.compare(password, exUser.password);
        if (result) {
          done(null, exUser);
        } else {
          done(null, false, { message: '비밀번호가 일치하지 않습니다.' });
        }
      } else {
        done(null, false, { message: '가입되지 않은 회원입니다.' });
      }
    } catch (error) {
      console.error(error);
      done(error);
    }
  }));
};
```

작동순서
1.사용자 데이터베이스에서 일치하는 이메일이 있는지 확인
2.존재하면 bcrypt 함수의 compare함수로 비밀번호를 비교
3.비밀번호까지 일치하면 done함수의 두번째 인수로 사용자 정보를 넣어 보냄

## 카카오 로그인 구현
카카오 로그인

사용자가 번거롭지 않게 검증된 SNS에 로그인을 맡길 수 있음
회원 가입 절차가 따로 없음

```javascript
const passport = require('passport');
const KakaoStrategy = require('passport-kakao').Strategy;
const User = require('../models/user');

module.exports = () => {
  passport.use(new KakaoStrategy({
    clientID: process.env.KAKAO_ID,
    callbackURL: '/auth/kakao/callback',
  }, async (accessToken, refreshToken, profile, done) => {
    console.log('kakao profile', profile);
    try {
      const exUser = await User.findOne({
        where: { snsId: profile.id, provider: 'kakao' },
      });
      if (exUser) {
        done(null, exUser);
      } else {
        const newUser = await User.create({
          email: profile._json && profile._json.kakao_account && profile._json.kakao_account.email,
          nick: profile.displayName,
          snsId: profile.id,
          provider: 'kakao',
        });
        done(null, newUser);
      }
    } catch (error) {
      console.error(error);
      done(error);
    }
  }));
};
```


그 후 카카오 로그인 아우터를 만들어야 함

1.로그아웃 라우터 아래에 추가
2.추가한 auth라우터를 app.js에 연결
3.kakaoStrategy.js에서 사용하는 clientId 발급
4.카카오 개발자 계정과 카카오 로그인용 애플리케이션 등록 필요
5.로그인 후 애플리케이션 메뉴에 가서 애플리케이션 추가하기 
6.카카오용 NodeBird앱 생성
7.앱 생성 후 REST API키를 복사 후 .env파일에 넣음
8.Web플랫폼 등록시 사이트 도메인에 로컬주소 삽입 후 저장

# 게시글 작성 및 이미지 업로드
- npm i multer

## Post 라우터 (routes/post.js)
- `multer`를 설정하고 이미지를 업로드하는 라우터입니다.

routes/post.js
```javascript
const express = require('express');
const multer = require('multer');
const path = require('path');
const fs = require('fs');
const { afterUploadImage, uploadPost } = require('../controllers/post');
const { isLoggedIn } = require('../middlewares');

const router = express.Router();

try {
  fs.readdirSync('uploads');
} catch (error) {
  console.error('uploads 폴더가 없어 생성합니다.');
  fs.mkdirSync('uploads');
}

const upload = multer({
  storage: multer.diskStorage({
    destination(req, file, cb) {
      cb(null, 'uploads/');
    },
    filename(req, file, cb) {
      const ext = path.extname(file.originalname);
      cb(null, path.basename(file.originalname, ext) + Date.now() + ext);
    },
  }),
  limits: { fileSize: 5 * 1024 * 1024 },
});

// 이미지 업로드 라우터
router.post('/img', isLoggedIn, upload.single('img'), afterUploadImage);

// 게시글 업로드 라우터
const upload2 = multer();
router.post('/', isLoggedIn, upload2.none(), uploadPost);

module.exports = router;
```

## Post 컨트롤러 (controllers/post.js)
실제 게시글 저장 및 해시태그 추출 로직

controllers/post.js
```javascript
const { Post, Hashtag } = require('../models');

exports.afterUploadImage = (req, res) => {
  console.log(req.file);
  res.json({ url: `/img/${req.file.filename}` });
};

exports.uploadPost = async (req, res, next) => {
  try {
    const post = await Post.create({
      content: req.body.content,
      img: req.body.url,
      UserId: req.user.id,
    });
    const hashtags = req.body.content.match(/#[^\s#]+/g);
    if (hashtags) {
      const result = await Promise.all(
        hashtags.map(tag => {
          return Hashtag.findOrCreate({
            where: { title: tag.slice(1).toLowerCase() },
          });
        })
      );
      await post.addHashtags(result.map(r => r[0]));
    }
    res.redirect('/');
  } catch (error) {
    console.error(error);
    next(error);
  }
};
```

- POST /post/img 라우터
    - 이미지 하나를 업로드받은 뒤 이미지의 저장 경로를 클라이언트로 응답
- POST /post 라우터
    - 게시글 업로드를 처리하는 라우터

Controllers/page.js
1. 게시글 작성 기능 추가되어 메인페이지 로딩 시 메인 페이지와 게시글을 함께 로딩
2. 데이터베이스에서 게시글을 조회한 뒤 결과를 twits에 넣어 렌더링
3. 조회 시 게시글 작성자의 아이디와 닉네임을 JOIN해서 제공


# 팔로우 기능 구현 및 해시태그 검색 기능 추가
- 사용자 아이디를 받아 팔로잉을 추가하는 라우터와 컨트롤러

routes/user.js
```javascript
const express = require('express');
const { isLoggedIn } = require('../middlewares');
const { follow } = require('../controllers/user');

const router = express.Router();

router.post('/:id/follow', isLoggedIn, follow);

module.exports = router;

controllers/user.js
const User = require('../models/user');

exports.follow = async (req, res, next) => {
  try {
    const user = await User.findOne({ where: { id: req.user.id } });
    if (user) {
      await user.addFollowing(parseInt(req.params.id, 10));
      res.send('success');
    } else {
      res.status(404).send('no user');
    }
  } catch (error) {
    console.error(error);
    next(error);
  }
};
```
그 후로...
- 팔로잉이 생기면 req.user에 팔로워, 팔로잉 목록을 저장
- req.user 바꾸기 위해 passport/index.js에 있는 deserializeUser조작
- 사용자 정보 조회 시 팔로잉 목록과 폴로워 목록도 조회
- 팔로잉/팔로워 숫자와 팔로우 버튼을 표시하기 위해 routes/page.js 수정
- GET /hashtag 라우터에서 해시태그 이름을 받고 해시태그 값이 없으면 메인 페이지로 이동. 존재 시 getPosts메서드로 모든 게시물 가져옴
- routes/post.js 와 routes/user.js를 app.js에 연결


