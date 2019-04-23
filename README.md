
### Schema.Method
* virtual

``` javascript
const carSchema = new mongoose.Schema({
  model: {
    brand: String,
    color: String
  }
});

const Car = mongoose.model('dataBaseName', carSchema)

let instance = new Car({
  model: { brand: "toyota", color: "blue" }
})
```
<p>如果想要拿到Car的model资料</p>

``` javascript
console.log(`${instance.model.brand} and ${instance.model.color}`); // toyota and blue
```

<p>Virtual能够帮助我们虚拟化它，他是它不存在database里,通过get方法给它赋值</p>

``` javascript
carSchema.virtual('carDetail').get(function() {
  return `${instance.model.brand} and ${instance.model.color}`;
});

console.log(instance.carDetail); // toyota and blue
```





    


