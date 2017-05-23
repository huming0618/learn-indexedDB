# 创建数据库

### API
`var request = window.indexedDB.open("MyTestDatabase", 3);`
- [IDBFactory.open()的实现](https://developer.mozilla.org/en-US/docs/Web/API/IDBFactory/open)
- 第二个参数为版本号, 是可选参数
- 当版本号提供时
  - 如果数据库存在但提供的版本号低于当前数据库时候，抛出异常
  - 如果数据库存在且提供的版本号高于当前数据库的时时候， 触发onupgradeneeded事件
  - 如果数据不存在， 则创建数据库为指定版本， 触发onupgradeneeded事件
- 当版本号没有提供时
  - 如果数据库存在，则使用当前数据库，没有版本升级事件
  - 如果数据不存在，则创建数据库为版本"1", 触发onupgradeneeded事件

### 创建数据库可能触发的事件'

##### `onsuccess`

##### `onerror`

##### `onupgradeneeded`
**数据库的结构调整只能在这个事件里面完成**,参见下文

> When a database is first created, its version is the integer 1. Each database has one version at a time; a database can't exist in multiple versions at once. The only way to change the version is by opening it with a greater version than the current one. This will start a versionchange transaction and fire an upgradeneeded event. **The only place where the schema of the database can be updated is inside the handler of that event.**

[链接](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Basic_Concepts_Behind_IndexedDB)

从事件中获取升级中的Transacation
```
request.onupgradeneeded = function(event){
  var tran = event.target.transaction;
  tran.oncomplete = function(){
    .... 
  }
}

```

### 注意
- 在chrome dev tool查看数据库版本时候， 需要关闭dev tool再打开后， 版本号才是更新的
- indexedDB的事件为DOM事件类型
- 在`onupgradeneeded`, 数据表对象中的transaction都指向当前数据库的changeversion的Transacation

# 删除数据库
`var request = window.indexedDB.deleteDatabase("MyTestDatabase");`
- [IDBFactory.deleteDatabase()的实现](https://developer.mozilla.org/en-US/docs/Web/API/IDBFactory/deleteDatabase)

# IDBFactory

[JSFiddle](https://jsfiddle.net/ruLw4duv/)
