---
layout: blog
comments: true
code: true
title:  "Database Query TIP 10.23"
tags:
- database
- mybatis
- mysql

background-image: https://d1.awsstatic.com/asset-repository/products/amazon-rds/1024px-MySQL.ff87215b43fd7292af172e2a5d9b844217262571.png
date:   2020-10-24 16:02:00
category: code

---

1. 判断一个可能为空的字段不为某个值:

```sql
select * from tbl
where tbl_col_1 <> 'CERTAIN_VALUE'
```

如果某行该字段的值为null

```sql
select * from 
where IFNULL(tbl_col_1, '') <> 'CERTAIN_VALUE'
```

如果不做判空处理，会导致数据库认为null 并非"不等于"待比较值，而无法返回预期结果。

具体是否需要增加IFNULL语句，需对应到具体业务场景，由null值的具体语义决定。



2. CDATA标签

被`<![CDATA[]]>`这个标记所包含的内容将表示为**纯文本**，比如`<![CDATA[<]]>`表示文本内容`“<”`。

在xml中, `<`, `>`, `&`等字符不可以直接存入，必须采用转义的方式

**为了方便起见**，使用`<![CDATA[]]>`来包含不被xml解析器解析的内容。但要注意的是：

- 此部分不能再包含`”]]>”`；

- 不允许嵌套使用；
- `”]]>”`这部分不能包含空格或者换行。

