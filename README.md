* .then method
* async await method

### 使用数据库
* Create
``` javascript
//api.js 
// Schema become model then module.exports to here
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

module.exports = router; //去 主要index.js，它调用这个

```

------------------
<p>Aysnc Await</p>
<p>记得要用 try catch哦</p>
<p>记得get的话，是拿到资料过后render去页面，而不是转跳redirect去页面</p>

``` javascript
router.post('/', async (req, res) => {
  const book = new Book({
    // title is from tag name
    title: req.body.title
  })
  try {
       const newBook = await book.save()
       res.redirect('/somewhere')
  } catch {
      res.redirect('/somewhere')
  }
})

router.delete('/:id', async (req, res) => {
  const book = await book.findById(req.params.id);
  try {
       await book.remove()
     res.redirect('/homepage')
  } catch {
     res.redirect('/somewhere')
  }

})
```

<p>如何更新资料呢？首先通过get的edit url资料然后才会put去更新啊</p>

``` html
<a href="/books/id/edit>Edit></a>
```
<p>其实他就是通过url来定义你的routes是做什么行为，就那么简单</p>
<p>当点击进upated就是put</p>
<p>记得get就是render啊，它拿到id了才知道是哪个对象过后在页面才能写那个对象的资料</p>

``` javascript
// 点击 edit button 他就是读哪里有这个path，找到这个就进来了
router.get('/:id/edit', async (req, res) => {
    try{
        // 寻找url  .id的 
        const author = await Author.findById(req.params.id);
        res.render('authors/edit', {
            author: author
        })
    } catch {
        res.redirect('/authors')
    }

})

// update
router.put('/:id/edit', async (req, res) => {
    let author;
    try {
        // 找url里的 id 在 database 
        author = await Author.findById(req.params.id)
        // form name里写的 资料 
        author.name = req.body.name;
        // 储存进database 更新了
        await author.save()
        res.redirect(`/authors/${author.id}`)
  
    } catch {
        // do something
    }
})
```




