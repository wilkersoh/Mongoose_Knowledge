# Note-Mongo
### 想想下 数据 会有什么功能呢
* 添加数据
* 读取数据
* 更改数据
* 删除数据
<p>每个数据都会通过mongodb 自动create出一个独有的_id</p>
<p>use Robo 3T查看数据</p>

### 连接mongodb基础设置
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


    


