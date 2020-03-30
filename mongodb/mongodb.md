# mongodb

## 1. 数据库操作

 * 查看数据库

   ```
   show dbs
   ```

 * 选择创建数据库

   ```
   use 数据库名称
   ```

* 删除数据库

  ```
  db.drowDatabase()
  ```

## 2. 集合操作

* 创建集合

  ```
  db.createCollection(集合名字)
  ```

* 查看集合

  ```
  show collections
  ```

* 删除集合

  ```
  db.集合名称.drop()
  ```

* 增加数据

  ```
  db.集合名称.insert({插入内容})
  ```

* 更改数据

  ``` 
  db.集合名称.update(<query>,<update>,[multi<bool>])
  // multi默认为false，只修改一条数据
  
  db.集合名称.update(<query>,{$set:{}},[multi<bool>])
  // 只修改$set中的文档，而不是修改整行
  ```

* 删除数据

  ``` 
  db.集合名称.remove(<query>,{justOne: true})	// 只删除一条
  db.集合名称.remove(<query>)				   // 删除全部
  ```

  