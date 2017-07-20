### MongoDB

> ODM: 对象数据库管理

#### 文档

{"name": "tom", age: 1} 表示为对象

#### 集合

集合是一组文档，如同关系数据库中的表, 多个集合组成数据库，一个MongoDB实例可以承载多个数据库

#### mongo shell

```
show dbs
show collections

use <dbname> 切换数据库
db.getName() 查看当前数据库
db.dropDatabase() // 删除数据库
db.stats() 当前db信息
db.getMongo() 当前db链接地址

db.createCollection('user')
db.createCollection(“collName”, {size: 20,capped: 5, max: 100}); 创建collection
db.getCollectionNames(); 当前db所有集合
db.printCollectionStats(); //显示聚集索引状态
增删改查
db.userInfo.find(); 查找
db.userInfo.save({name: 1, age: 2}) 新增
db.userInfo.insert({name: 1})
db.users.update({age: 25}, {$set: {name: 'changeName'}}, false, true); 第三个参数默认false, true表示找不到则传入这个数据。第四个参数默认false，表示只修改找到的第一条数据，否则全局修改
db.userInfo.remove({name: 1}) 删除

```



#### mongoose

```
mongoose.connect('mongodb://localhost/myblog') // 连接.myblog是db名
const db = mongoose.connection; // 监听
db.on('error', console.error);
db.on('open', () => {
  initialize();
});

const Schema = mongoose.Schema;
const userSchema = new Schema({
  name: { type: String },
  pwd: String
}); // 类似于collection的规范

const User = mongoose.model('User', userSchema); 生成collection
const user = new User({
  name: 1,
  pwd: 1
}); 生成model, use可操作


```

报错处理

> `mongoose.connect('mongodb://localhost/myblog')`会报错因为不识别localhost ——>`mongoose.connect('mongodb://127.0.0.1/myblog')`

>promise需要全局替代
>
>`mongoose.Promise = global.Promise;`

