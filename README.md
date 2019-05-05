<b>什么是Model的方法？它不是instance实例哦</b>

``` javascript
const iamModel = require('dbUrl');
```
<p>iamModel就是model，还没生成去 instance！</p>
<p>以下要写1个还是4个parameter是看个人，每个做法都不一样，cb就是直接执行了</p>

* findByIdAndRemove | (id, options, callback)    | if one parameter return Promise 
* findByIdAndUpdate
* find          
* findOne
* populate 牵连到ObjectId, ref | 为了返回两个Schema的数据


##### findByIdAndRemove
1. new: true 返回更新的资料而不是旧的  default:false
2. runValidators: true update it before run validation

``` javascript
router.delete('/yz/:id', (req, res) => {
    Yz.findByIdAndRemove({_id: req.params.id})
      .then(function(yz){
          res.send(yz)
      });
})
```
<p>为什么Update我需要做多一次findOne呢？虽然在database里已经更新了，但我的Postman软件里那边不会给我通知它更新的资料,第二个findOne可以说是再db里找多一次，然后他就会显示那个已经更新的资料</p>
#####  findByIdAndUpdate
* (id, update, options, callback) | opts- new: true 返回更新的资料而不是旧的


``` javascript 
router.put('/yz/:id', (req, res) => {
    Yz.findByIdAndUpdate({_id: req.params.id}, req.body)
      .then(function(){
          Yz.findOne({_id: req.params.id})
        .then(function(yz){
            res.send(yz)
        })
    })
})
```
##### find
* | (condition, projection,opts, cb)| 找全部documents

``` javascript 
Yz.find({}).then(function(yzs){
        res.send(yzs);
 ```
 
##### findOne
* | (condition, projection, opts, cb) | 找一个
<p>在condition里，如果要找的一个Obj是nested里两个资料，需要这样写<p>
<p> {“String”： 值， “String”： 值} </p>

``` javascript
// nested document
{ item: "journal", instock: [ { warehouse: "A", qty: 5 }, { warehouse: "C", qty: 15 } ]

Model.findOne( { "instock.qty": 5, "instock.warehouse": "A" } )
```


------------------

##### populate
<p>关于当这个Schema的某个obj的_id是指向另一个Schema里的_id时候，你要获取当前的Schema资料和指向另一个Schema的资料</p>
<p>什么能让Schema的_id是指向另外一个Schema的_id呢？</p>
<p>就是使用 type: Schema.ObjectId, ref: 'theSchema'啊</p>

``` javascript
// Schema.ObjectId || ref: 'otherSchemaName'
const bookSchema = new mongoose.Schema({
  author:{
    // 这是只要这个author 会生成一个id 但是这个id 是指向另一个Schema的_id
    // ref 是ObjectId指向哪里，所以他们连个是一起出现的
    type: mongoose.Schema.Types.ObjectId,
    ref: 'populateMethod'
  }
})
```

``` javascript
const populateMethod = new mongoose.Schema({
  name:{type: String},
  pwd: String,
})
```
<p>如果要找到当前和某个Schema里资料</p>

``` javascript
bookSchema.findById(req.params.id).populate('author').exec()
```





