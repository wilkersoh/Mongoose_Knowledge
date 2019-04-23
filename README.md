<b>什么是Model的方法？它不是instance实例哦</b>

``` javascript
const iamModel = require('dbUrl');
```
<p>iamModel就是model，还没生成去 instance！</p>
<p>以下要写1个还是4个parameter是看个人，每个做法都不一样，cb就是直接执行了</p>

* findByIdAndRemove | (id, options, callback)         | if one parameter return Promise 
* findByIdAndUpdate | (id, update, options, callback) |
* find              | (condition, projection,opts, cb)| 找全部documents


``` javascript
// findByIdAndRemove
router.delete('/yz/:id', (req, res) => {
    Yz.findByIdAndRemove({_id: req.params.id})
      .then(function(yz){
          res.send(yz)
      });
})
```
<p>为什么Update我需要做多一次findOne呢？虽然在database里已经更新了，但我的Postman软件里那边不会给我通知它更新的资料,第二个findOne可以说是再db里找多一次，然后他就会显示那个已经更新的资料</p>

``` javascript 
// findByIdAndUpdate
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
<p>Find是照全部资料</p>

``` javascript 
// find
Yz.find({}).then(function(yzs){
        res.send(yzs);
 ```




