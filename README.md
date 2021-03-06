``` bash
----------master ------------ CRUD
     |----method ------------ Schema.virtual
     |----middleware -------- pre || post
     |----Model.method ------ update || delete
     |----CRUD --------------  Model.method
```

* Schema概念
* Model概念
* 连接数据库

### 想想下 数据 会有什么功能呢
* 添加数据 C - Create - get
* 读取数据 R - Read - get
* 更改数据 U - Update - put
* 删除数据 D - Deleted - delete

<p>1. 创建Schema</p>   -- const schemaName = new mongoose.Schema({})
<p>2. 转成model</p>    -- const Model = mongoose.model('dbName', schemaName)
<p>3. 做成实例</p>     -- const instance = new Model({})

#### 為什麼使用mongoose而不是mongoDB呢？
<p>Mongoose是MongoDB的一个对象模型工具，是基于node-mongodb-native开发的MongoDB nodejs驱动，可以在异步的环境下执行。同时它也是针对MongoDB操作的一个对象模型库，封装了MongoDB对文档的的一些增删改查等常用方法，让NodeJS操作Mongodb数据库变得更加灵活简单</p>


``` javascript
//mongoose.js
const Schema = mongoose.Schema;
```
<p>使用保密的行为</p>

``` javascript
// 用 process secure的话需要 dotenv插件， 然后在.env 里储存token
if(process.env.NODE_ENV !== 'production'){
     require('dotenv').config()
}
const mongoose = require('mongoose');

mongoose.connect(process.env.DATABASE_URL, {useNew...})
 .then(xx => console.log("success connected"));

// .env
DATABASE_URL=mongodb://localhost/yzdb
```
<p>这样也是没问题得~</p>

``` javascript
//app.js 在执行 连接server的地方，那样的话 就会跑server 这个也一起读到了

// 连接 /yzdb folder 会自动创建 如果没有的话
// 'mongodb://127.0.0.1:27017/yzdb',{....};
// const db = require('./url/your mongoose key')
// const mongoose.connect(db, {...same below}); 也行

mongoose.connect('mongodb://localhost/yzdb', { useNewUrlParser: true, useCreateIndex: true});

// const router = require('./router/api')
app.use('/api', require('./router/api'));
```

``` javascript
const validator = require('validator') //蛮好用的
```

<p>每个数据都会通过mongodb 自动create出一个独有的_id</p>
<p>use Robo 3T查看数据</p>

--------

##### 记得要创建 Schema 样板 数据都是跟着它的规定执行
``` javascript
const personSchema = new Schema({
    type: {
        type: String,
        default: "Chinese" // 可以设置 default 来之mongoose
    },
    name: {
       type: String,
       required: true,
       trim: true,
    },
    age: {
       type: Number,
       trim: true,
       validate(value){ // 可以设置 validate 来之 mongoose
        if(value < 0){
          alert('Provide a valid number')
         }
       }
    },
    email: {
      type: String,
      required: true,
      trim: true,
      validate(value){
        if(!validator.isEmail(value)){ //通过validator 去验证这些资料
           alert('Provide a valid email')
        }
      }
    }
})
```

### 做完Schema后，就需要使用他来创建模型了

``` javascript
const Yz = mongoose.model('CreateName', personSchema); //（如果没有这个名字它会自动在你database里创建，Schema）
 
module.exports = Yz;
```

--------------------------------




### 连接mongodb基础设置 （Mongdb 版本）
``` javascript

const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;
const ObjectID = mongodb.ObjectID;

````
### destructor it 

``` javascript
const { MongoClient, ObjectID } = require('mongodb');
const id = new ObjectID()
```
完整长这样

``` javascript 
const { MongoClient, ObjectID } = require('mongodb');


const connectionURL = 'mongodb://127.0.0.1:27017';
const databaseName = 'task-manager';

const id  = new ObjectID();

```
### mongodb大多都有callback
连接server

``` javascript
MongoClient.connect(connectionURL, { useNewUrlParser: true }, (err, client) => {

    const db = client.db(databaseName)
    
    do something ...
})
```

-------

##### findOne （寻找一个）

``` javascript
MongoClient.connect(connectionURL, { useNewUrlParser: true }, (err, client) => {
    if(err){
        console.log('Unable to connect the database!')
    }

    const db = client.db(databaseName)
    
    db.collection('users').findOne({_id: new ObjectID('5c9ca38544a3722c489b8719')}, (err, user) => {
        if(err) throw err;
        console.log(user);
    })
  
})  


##### 注意 使用find 他是返回 cursor 而不是 array (寻找多个)
``` javascript
    db.collection('users').find({age: 23}).toArray((err, users) => {
        console.log(users);
    })
 ``` 
 
 #### Update Operate 使用在update数据身上
 * $set 更改value
 * $inc 增加 1 如下
 <p>还有一些可以在网路上找来看</p>
 
 ``` javascript 
     const updatePromise = db.collection('users').updateOne({
        _id: new ObjectID('5c9ca463d88e00040089d387')
    },{
        $inc: {
            age: 1
        }
    })

    updatePromise.then((result) => {
        console.log(result);
    }).catch((err) => {
        console.log(err)
    })
   ```


    


