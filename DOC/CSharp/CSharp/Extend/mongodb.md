# mongodb

```
var collection = database.GetCollection<BOOK>("jylcl");
//BsonDocument
//var document = new BsonDocument{{ "name", "a" },{ "Price", 10 }, };

//插入数据
BOOK bk = new BOOK();
bk.Name = "a";
bk.Price = 10;
collection.InsertOne(bk);

bk = new BOOK();
bk.Name = "b";
bk.Price = 15;
collection.InsertOne(bk);

bk = new BOOK();
bk.Name = "c";
bk.Price = 17;
collection.InsertOne(bk);


bk = new BOOK();
bk.Name = "d";
bk.Price = 25;
collection.InsertOne(bk);



//查询
var filter = Builders<BOOK>.Filter.Eq("Name", "a");
var result = collection.Find(filter).ToList();
foreach (var item in result) { }

var builder = Builders<BOOK>.Filter;
filter = builder.Gt("Price", 10) & builder.Lt("Price", 20);//10 > val && val < 20
result = collection.Find(filter).ToList();


//删除
filter = Builders<BOOK>.Filter.Eq("Name", "a"); 
var delResult = collection.DeleteOne(filter);
```