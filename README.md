# Note-Mongo
### 想想下 数据 会有什么功能呢
* 添加数据 C - Create - get
* 读取数据 R - Read - get
* 更改数据 U - Update - put
* 删除数据 D - Deleted - delete

``` javascript
const validator = require('validator') //蛮好用的
```

<p>每个数据都会通过mongodb 自动create出一个独有的_id</p>
<p>use Robo 3T查看数据</p>

### 我也不清楚为什么大家都是用 Mongoose 而不是 Mongodb模块
*Mongoose应该比较容易实现一些资料吧

--------

``` javascript
//mongoose.js
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
```
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

### 做完Schema后，就需要使用他来创建模型了
``` javascript
const Yz = mongoose.model('CreateName', personSchema); //（如果没有这个名字它会自动在你database里创建，Schema）
 
module.exports = Yz;
```
### 使用数据库
* Create
``` javascript
//api.js 
const Yz = require('../models/mongoose')

router.post('/contact', (req, res, next)=>{
    // let yz = new Yz(req.body);
    // yz.save(); save at database after collection 
    Yz.create(req.body)
     .then((yz) => {
        res.send(yz)
     }).catch(next);
})
```
* Update
``` javascript 
router.put('/contact/:id', (req, res, next)=>{
    Yz.findOneAndUpdate({_id: req.params.id}, req.body)
      .then(() => {
          Yz.findOne({_id: req.params.id}, req.body)
            .then((yz) => 
                res.send(yz)
            );
        })
})
```
* Delete
``` javascript
router.delete('/contact/:id', (req, res, next)=>{
    Yz.findOneAndDelete({_id: req.params.id})
      .then(yz => {
          res.send(yz)
      })
})
```



### 连接mongodb基础设置 （Mongdb 版本）
``` javascript

const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;
const ObjectID = mongodb.ObjectID;

````
### destructor it 
```javascript
const { MongoClient, ObjectID } = require('mongodb');
const id = new ObjectID()
```

*完整长这样
``` javascript 
const { MongoClient, ObjectID } = require('mongodb');


const connectionURL = 'mongodb://127.0.0.1:27017';
const databaseName = 'task-manager';

const id  = new ObjectID();

```
### mongodb大多都有callback
*连接server 
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

module.exports = router; //去 主要index.js，它调用这个
```

``` javascript
//index.js

// 连接 /yzdb folder 会自动创建 如果没有的话
mongoose.connect('mongodb://localhost/yzdb', { useNewUrlParser: true});
// const router = require('./router/api')
app.use('/api', require('./router/api'));
```

--------------------------------
```

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


    


