<b>启动mongoose中间件有两个</b>
* pre    -- 在执行某某之前执行里面的代码
* post   -- 在执行某某之后执行里面的代码

<b>中间件有四个</b>
* init
* validate   ---- 
* save
* remove

<b>查询中间件</b>
* count
* find
* findOne
* findOneAndRemove
* findOneAndUpdate
* insertMany
* update

<p>拿schema来处理</p>
``` javascript
const carSchema = new mongoose.Schema({})

carSchema.pre('remove', function(next){}

module.exports = mongoose.model('Car', carSchema)
```

<p>model里启动这些中间件而不是instance</p>

``` javascript
const Car = require('url');

Car.remove()
```
<p>model调用了remove method前它会调用pre里的代码</p>

<b>它有前后执行顺序，如果中间件和pre，post都设置的话</b>
<p>如果数据库里有3条数据了,执行find而且中间件pre，post都设置,运行find后，它会执行</p>

``` javascript
carSchema.pre('find', function(next){
  console.log('find');
  next();
}

// 如果全部中间件都有写的话 比如 pre||post('init'), 

```
<b>find会调用以下的中间件，如果它们都有些的话</b>

<p>1. pre-find</p>
<p>2. pre-init</p>
<p>3. post-init</p>
<p>4. pre-init</p>
<p>5. post-init</p>
<p>6. pre-init</p>
<p>7. post-init</p>
<p>8. post-find</p>

<b>为什么 2 到 7 的都在循环， 因为里面有3条数据</b>

#### 大多数不会都写入那么多中间件，比如我想remove(),我针对 pre || post 设置中间件就行了




    


