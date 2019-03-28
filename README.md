# Note-Mongo

### 连接mongodb基础设置
``` javascript

const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;

const connectionURL = 'mongodb://127.0.0.1:27017';
const databaseName = 'task-manager';

```

*连接server ### 他们都有callback
``` javascript
MongoClient.connect(connectionURL, { useNewUrlParser: true }, (err, client) => {
    if(err){
        console.log('Unable to connect the database!')
    }

   // 连接上 数据库
   const db = client.db(databaseName);
    
   db.collection('users').insertMany([{
        name: 'Jen',
        age: 25
    },{
        name: 'mami',
        age: 63
    }], (err, result) => {
        if(err){
            return console.log('Unable to received data')
        }
        console.log(result.ops)
    })
})
```
    


