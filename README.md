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
