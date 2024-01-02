# EFCore

##### 基本介绍

- EFCore是微软开发的ORM数据库工具框架，可以让开发者用对象操作的形式操作关系型数据库。
- EFCore支持目前常用的大多数据库系统，例如SQLserver、Mysql、SQLtie等。
- 推荐使用CodeFirst，对SQLserver支持最好。

- > [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)数据库生成工具包
  >
  > - 数据库连接字符串
  >
  > ```c#
  > "server=;database=EFCore;uid=sa;pwd=123;Trust Server Certificate=True;MultipleActiveResultSets=true"
  > ```
  >
  > 
  >
  > - 生成数据库，生成一次迁移
  >
  > ```c#
  > dotnet ef migrations add (名称类似与GIT中推送的附加信息)
  > ```
  >
  > - 推送到数据库
  >
  > ```c#
  > dotnet ef database update
  > ```

##### 插入数据

```c#
using var cxt = new BookDbContext();
var b1 = new Book()
{
    BookName = "三国演义",
    Author = "罗贯中",
    Price = 100,
    PubTime = DateTime.Now
};
var b2 = new Book()
{
    BookName = "水浒传",
    Author = "施耐庵",
    Price = 120,
    PubTime = DateTime.Now
};
var b3 = new Book()
{
    BookName = "西游记",
    Author = "吴承恩",
    Price = 80,
    PubTime = DateTime.Now
};
var b4 = new Book()
{
    BookName = "红楼梦",
    Author = "曹雪芹",
    Price = 200,
    PubTime = DateTime.Now
};
var b5 = new Book()
{
    BookName = "金瓶梅",
    Author = "兰陵笑笑生",
    Price = 50,
    PubTime = DateTime.Now
};
cxt.Books.AddRange(b1, b2, b3, b4, b5);
cxt.SaveChanges();//这里推荐使用异步方法
```

##### 查询数据

```c#
//查询价格大于80的书
var books = cxt.Books.Where(b => b.Price > 90);
foreach (var book in books)
{
    Console.WriteLine(book.BookName);
  //三国演义、水浒传、红楼梦
}

//根据价格排序
var books = cxt.Books.OrderBy(b => b.Price);
foreach (var book in books)
{
    Console.WriteLine(book.BookName);
  //金瓶梅、西游记、三国演义、水浒传、红楼梦
}
```

##### 修改数据

- 首先需要把要修改的数据查询出来，然后在对其进行修改，最后执行保存方法

```c#
//找到一本书，改变它的价格
var books = cxt.Books.Single(b=>b.BookName=="三国演义");
Console.WriteLine(books.Price);//100
books.Price= 200;
cxt.SaveChanges();
var book = cxt.Books.Single(b=>b.BookName=="三国演义");
Console.WriteLine(book.Price);//200
```



##### 删除数据

- 现在的程序中，都在执行软删除，在操作上用户删除了某一项数据，但是都后台代码中并非删除了这条数据，而是隐藏它，可以设计一个字段用来实现。

```c#
var books = cxt.Books.Single(b=>b.BookName=="三国演义");
cxt.Books.Remove(books);
cxt.SaveChanges();
```

