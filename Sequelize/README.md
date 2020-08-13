# Sequelize

## Sequelize란?

node에서 RDB의 필요할때 사용한다. DB에 연결해서 쿼리를 직접 만들어 날려도 되지만, ORM을 사용하면 수 배 이상의 생산성을 가질 수 있다.

Sequelize.js는 Node.js 기반의 ORM(Object-Releational-Mapping)이다. 공식적으로 PostgreSQL, MySQL, MariaDB, SQLite, MS-SQL을 지원한다.



## 설치 및 연결

Sequelize.js는 npm으로 다음과 같이 쉽게 설치할 수 있다.

```
npm install sequelize
```

Sequelize는 node 패키지로 node-mysql을 포함하고 있지 않으므로 node-mysql도 수동으로 설치해주어야 한다.

각각의 DB에 따라 설치를 따로 해줘야 한다. 나는여기서 mysql2를 설치했다

```shell
npm install mysql2
npm install pg pg-hstore
npm install sqlite3
npm install tedious //MS SQL
```



mysql과 연결하기 위한 Sequelize 

#### models/index.js

```javascript
// 데이터베이스 접속 설정
const sequelize = new Sequelize( process.env.DATABASE,
process.env.DB_USER, process.env.DB_PASSWORD,{
    host: process.env.DB_HOST,
    dialect: 'mysql',
    timezone: '+09:00', //한국 시간 셋팅
    operatorsAliases: Sequelize.Op,
    pool: {
        max: 5,
        min: 0,
        idle: 10000
    }
});

// index.js를 제외한 파일들을 확인하면서 테이블을 생성
fs.readdirSync(__dirname)
    .filter(file => {
        return file.indexOf('.js')&& file !== 'index.js'
    })
    .forEach(file => {
        var model = sequelize.import(path.join(__dirname,
            file));
            db[model.name] = model;
    });

// 테이블 간의 관계를 작동시킨다.
Object.keys(db).forEach(modelName => {
    if("associate" in db[modelName]){
        db[modelName].associate(db);
    }
});


```

#### app.js

```javascript
/*
require ...
*/
// 뒤에 생략하면 자동으로 index.js로 된다.
const db = require("./models")

/*
app function 
...
*/
this.dbConnection();
// DB연결
dbConnection(){
        // DB authentication
        db.sequelize.authenticate()
        .then(() => {
            console.log('Connection has been established successfully.');
        })
        .then(() => {
            console.log('DB Sync complete.');
        })
        .catch(err => {
            console.error('Unable to connect to the database:', err);
        });
    }
```

