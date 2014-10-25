Bean-query
==========

[Click Here](./README_en.md) for English version.

BeanQuery 是一个把对象转换为Map的Java工具库。支持选择Bean中的一些属性，对结果进行排序和按照条件查询。不仅仅可以作用于顶层对象，也可以作用于子对象。

BeanQuery的使用非常简单也很直接，例子代码如下：
```java
//静态导入BeanQuery
import static cn.jimmyshi.beanquery.BeanQuery.*;


//使用 select、from、where、orderBy、desc和asc来组装一个Query，然后执行execute方法来获得结果。
List<Map<String, Object>> result = select("price,name,mainAuthor.name as mainAuthorName")
    .from(bookCollection)
    .where(
        //for books name is Book2 or starts with Book1
        anyOf(
            value("name", startsWith("Book1")),
            value("name", is("Book2"))
        ),
        //for books price between (53,65)
        allOf(
            value("price", greaterThan(53d)),
            value("price",lessThan(65d))
        )
    )
    .orderBy("name").desc()
    .execute();
```

在上面的例子中，bookCollection的内容如下所示(json格式)
```json
[
  {
    "price":55.55,
    "name":"Book1",
    "mainAuthor":{
      "name":"Book1-MainAuthor",
      "address":{
        "address":"Shenzhen Guangdong China",
        "postCode":"518000"
      },
      "birthDate":"1982-01-30T14:52:39"
    }
  },
  {
    "price":52.55,
    "name":"Book12",
    "mainAuthor":{
      "name":"Book1-MainAuthor",
      "address":{
        "address":"Shenzhen Guangdong China",
        "postCode":"518000"
      },
      "birthDate":"1982-01-30T14:52:39"
    }
  },
  {
    "price":53.55,
    "name":"Book13",
    "mainAuthor":{
      "name":"Book13-MainAuthor",
      "address":{
        "address":"Shenzhen Guangdong China",
        "postCode":"518000"
      },
      "birthDate":"1982-01-30T14:52:39"
    }
  },
  {
    "price":60.0,
    "name":"Book14",
    "mainAuthor":{
      "name":"Book14-MainAuthor",
      "address":{
        "address":"Shenzhen Guangdong China",
        "postCode":"518000"
      },
      "birthDate":"1982-01-30T14:52:39"
    }
  },
  {
    "price":50.55,
    "name":"Book15",
    "mainAuthor":{
      "name":"Book1-MainAuthor",
      "address":{
        "address":"Shenzhen Guangdong China",
        "postCode":"518000"
      },
      "birthDate":"1982-01-30T14:52:39"
    }
  },
  {
    "price":77.77,
    "name":"Book3",
    "mainAuthor":{
      "name":"Book3-MainAuthor",
      "address":{
        "address":"Shenzhen Guangdong China",
        "postCode":"518005"
      },
      "birthDate":"1982-01-30T14:52:39"
    }
  }
  ,
  {
    "price":66.66,
    "name":"Book2",
    "mainAuthor":{
      "name":"Book2-MainAuthor",
      "address":{
        "address":"Shenzhen Guangdong China",
        "postCode":"518005"
      },
      "birthDate":"1982-01-30T14:52:39"
    }
  }
]
```
执行完之后，则result的内容如下所示(json格式)
```json
[
  {
    "price":60.0,
    "name":"Book14",
    "mainAuthorName":"Book14-MainAuthor"
  },
  {
    "price":53.55,
    "name":"Book13",
    "mainAuthorName":"Book13-MainAuthor"
  },
  {
    "price":55.55,
    "name":"Book1",
    "mainAuthorName":"Book1-MainAuthor"
  }
]
```
