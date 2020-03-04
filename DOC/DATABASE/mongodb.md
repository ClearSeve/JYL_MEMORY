mongodb
=======

环境配置
--------

环境变量path: C:\\Program Files\\MongoDB\\Server\\4.0\\bin

安装启动服务：

mongod.exe --install

net start MongoDB

移除服务：

mongod.exe --remove

连接：

mongo.exe

mongod.cfg文件（路径需要手动创建）：

storage:

dbPath: c:\\data\\db

数据库操作
----------

collection ：数据库表/集合（table）

document：数据记录行/文档（row）

field ：数据字段/域（column）

| show dbs                                                            | 显示所有数据库                                                  |
|---------------------------------------------------------------------|-----------------------------------------------------------------|
| db                                                                  | 显示当前使用的数据库名称                                        |
|                                                                     |                                                                 |
| use jyl                                                             | 创建/选择数据库jyl                                              |
| db.dropDatabase()                                                   | 删除当前使用的数据库                                            |
|                                                                     |                                                                 |
| show collections                                                    | 显示当前使用的数据库中的所有集合名                              |
| db.createCollection("jylcl")                                        | 创建集合jylcl                                                   |
| db.jylcl.drop()                                                     | 删除集合jylcl                                                   |
|                                                                     |                                                                 |
| db.jylcl.insert({'name':'jyl','age':1})                             | 向集合jylcl插入数据，不存在则创建                               |
|                                                                     |                                                                 |
| db.jylcl.remove({'name':'jyl'})                                     | 按照条件删除集合中的文档                                        |
|                                                                     |                                                                 |
| db.jylcl.update({'name':'jyl'},{\$set:{'age':1}})                   | 修改集合中的数据                                                |
|                                                                     |                                                                 |
| db.jylcl.find()                                                     | 查找集合中所有数据，find().pretty()以格式化的方式来显示所有文档 |
| db.jylcl.find({'name':'jyl'}) db.jylcl.find({'name':'jyl','age':1}) | 按照条件查找集合中的文档                                        |
