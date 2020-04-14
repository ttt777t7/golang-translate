这里主要记录golang入门教程的翻译
golang-tutorial https://golangbot.com
### 1、golang的反射
#### 反射的作用?
1.反射的主要作用,就是当程序在运行时能够获取数据类型和值
2.编写更通用的函数
#### 例子
例1
```
  package main

  import (  
      "fmt"
  )

  type order struct {  
      ordId      int
      customerId int
  }

  func createQuery(o order) string {  
      i := fmt.Sprintf("insert into order values(%d, %d)", o.ordId, o.customerId)
      return i
  }

  func main() {  
      o := order{
          ordId:      1234,
          customerId: 567,
      }
      fmt.Println(createQuery(o))
  }
```
这个例子非常简单，定义一个结构体，创建了一个接受该结构体的函数，并在main函数中调用,输出了insert into order values(1234,567)
例2
```
  package main

  type order struct {  
      ordId      int
      customerId int
  }

  type employee struct {  
      name string
      id int
      address string
      salary int
      country string
  }

  func createQuery(q interface{}) string {  

  }

  func main() {

  }
```
如果我希望编写一个函数，参数为空结构体，可以接受任何结构体类型，都可以输出： insert into order values(结构体字段,结构体字段),但遗憾的是，如果没有反射，这样的需求无法实现。用传统的方法，createQuery函数内，我们必须显性说明输出字段或者调用函数,如下所示。
```
  func createQuery(q interface{}) string {  
  i := fmt.Sprintf("insert into order values(%d, %d)", o.ordId, o.customerId)
    return i
  }
  
  func createQuery(q interface{}) string {  
  i := fmt.Sprintf("insert into order values(%s,%d,%s,%d,%s)", o.name,o.id,o.address o.salary,o.country)
    return i
  }
  
```
