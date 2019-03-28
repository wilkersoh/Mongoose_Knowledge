# Note-Mongo

### 连接mongodb基础设置
``` javascript

const mongodb = require('mongodb');
const MongoClient = mongodb.MongoClient;
const ObjectID = mongodb.ObjectID;

````
### destructor it 
const { MongoClient, ObjectID } = require('mongodb');
const id = new ObjectID()

*完整长这样
``` javascript 
const { MongoClient, ObjectID } = require('mongodb');


const connectionURL = 'mongodb://127.0.0.1:27017';
const databaseName = 'task-manager';

const id  = new ObjectID();

```

* findOne
*



*连接server ### 他们都有callback
``` javascript
MongoClient.connect(connectionURL, { useNewUrlParser: true }, (err, client) => {

    do something ...
})
```

-------
##### findOne
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

##### 注意 使用find 他是返回 cursor 而不是 array
``` javascript
    db.collection('users').find({age: 23}).toArray((err, users) => {
        console.log(users);
    })
 ``` 


    


