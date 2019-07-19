# MySql


## 1.sql – 多语言数据库设计的最佳实践是什么？

为每个多语言对象创建两个表。
例如。第一个表只包含语言中性数据(主键等)，第二个表每个语言包含一个记录，包含本地化数据和语言的ISO代码。

在某些情况下，我们添加一个DefaultLanguage字段，以便如果没有可用于指定语言的本地化数据，我们可以回退到该语言。

例：

```

Table "Product":
----------------
ID                 : int
<any other language-neutral fields>


Table "ProductTranslations"
---------------------------
ID                 : int      (foreign key referencing the Product)
Language           : varchar  (e.g. "en-US", "de-CH")
IsDefault          : bit
ProductDescription : nvarchar
<any other localized data>
```
使用这种方法，可以处理所需的多种语言(无需为每种新语言添加其他字段)。

## 2.动态内容国际化,数据库设计

 - 使用智能翻译。
   例如使用Google Translate API等。
 - 独立建表
   为每个语种设计一套数据库，维护“内容”时，每种语言都维护进去，table_cn, table _en 
 - 数据映射，2个表做映射
   适用于支持多种语言的数据库，扩展性强，需要多次查询数据库。
   例如使用一个数据字典来定义每一种语言，然后在维护每一种语言进去，建立两张表，Language和Content，Language相当于一个数据字典，列出系统用到的所有语种。Content表就是维护的内容——也就是我们想解决的多语种显示的东西。两张表表样如下：
   
   ```
   Language
  id----|---langeuage-----|---description----
  1-----|---------zh--------|------中文---------
  2-----|---------en--------|------英文---------
  3-----|---------fr---------|------法文---------
  
  
  Content
id----|---content----------------|---languageId------
1-----|----你好！----------------|------1------------
2-----|----How are you!---------|------2------------
3-----|----fa wen bu hui---------|------3------------
   ```
   
   这样Content表中就保存了需要语种的所有的语言信息（当然要维护进去），然后再根据用户的需要，对应Language表选择出对应语种的Content内容。
