<b>什么是Model的方法？它不是instance实例哦</b>

``` javascript
const iamModel = require('dbUrl');
```
<p>iamModel 就是 model，还没生成去 instance！</p>

##### 创建method类似js prototype
##### toObject() 转换去 JS的 String


##### 创建method类似js prototype
* Schema.statics.methodName ----这个是for Model方法
* Schema.methods.methodName ----这个是for Instance方法

``` javascript
// User 是 model
await User.findByCredentials();
```
<p>所以要建立方法，需要使用 statics，如以下</p>

##### toObject() 转换去 JS的 String
<p>时常都是会利用这个去删除一些database里的资料</p>

``` javascript
// user is instance
user.getPublicProfile()

//db file
userSchema.methods.getPublicProfile = function (){
    const user = this;
    const userObject = user.toObject();

    delete userObject.password;
    delete userObject.tokens;

    return userObject
}
```






