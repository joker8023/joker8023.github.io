# 介绍

## 前言

- Go（又称 Golang）是 Google 开发的一种静态强类型、编译型、并发型，并具有垃圾回收功能的编程语言。
- 罗伯特·格瑞史莫、罗勃·派克（Rob Pike）及肯·汤普逊于 2007 年 9 月开始设计 Go，稍后 Ian Lance Taylor、Russ Cox 加入项目。Go 是基于 Inferno 操作系统所开发的。Go 于 2009 年 11 月正式宣布推出，成为开放源代码项目，支持 Linux、macOS、Windows 等操作系统。 在 2016 年，Go 被软件评价公司 TIOBE 选为“TIOBE 2016 年最佳语言”。

## 语言特色

- 简洁、快速、安全
- 并行、有趣、开源
- 内存管理、数组安全、编译迅速

## 性能对比

> 以下性能对比数据来自https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/go.html，截取部分算法展示

### regex-redux（正则替换算法）

| 语言 | 时间(/s) | 占用内存 | CPU 负载        |
| ---- | -------- | -------- | --------------- |
| Go   | 3.61     | 374,916  | 47% 34% 20% 64% |
| Java | 5.70     | 656,328  | 89% 76% 79% 75% |

### binary-trees（二叉树）

| 语言 | 时间(/s) | 占用内存  | CPU 负载        |
| ---- | -------- | --------- | --------------- |
| Go   | 6.74     | 280,176   | 98% 98% 98% 97% |
| Java | 2.50     | 2,487,096 | 88% 75% 75% 79% |

> 各个语言都在优化程序的运行速度，但在多核运算以及低内存运算 方面，golang 还是优势比较明显

## 谁在用

- Google ![image](https://cdn.yuanmedia.top/encrypt/google.png)
- Facebook ![image](https://cdn.yuanmedia.top/encrypt/facebook.png)
- 阿里巴巴 ![image](https://cdn.yuanmedia.top/encrypt/alibaba.png)
- 腾讯 ![image](https://cdn.yuanmedia.top/encrypt/tencent.png)
- 京东 ![image](https://cdn.yuanmedia.top/encrypt/jd.png)

现在很多公司都开始尝试 Golang，除了上面提到的，还有美团、滴滴、新浪以及七牛等。一般的选择，都是选择用于自己公司合适的产品系统来做，比如消息推送的、监控的、容器的等，Golang 特别适合做网络并发的服务，这是他的强项，所以也是被优先用于这些项目。

其次就是用于替换一些以前使用 PHP、Python、C/C++的项目，这些迁移到 Golang 还是比较容易的，不过目前旧系统迁移的不是太多，主要是新系统的使用。

# 安装

点击 https://golang.google.cn/dl/
下载对应版本的文件，直接安装就可以了

```
package main

import "fmt"

func main() {
   /* 这是我的第一个简单的程序 */
   fmt.Println("Hello, World!")
}
```

## Go 语言数据类型

| 类型       | 类型和描述                                                                                                                                 |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| 布尔型     | 布尔型的值只可以是常量 true 或者 false。一个简单的例子：var b bool = true。                                                                |
| 数字类型   | 整型 int 和浮点型 float32、float64，Go 语言支持整型和浮点型数字，并且支持复数，其中位的运算采用补码。                                      |
| 字符串类型 | 字符串就是一串固定长度的字符连接起来的字符序列。Go 的字符串是由单个字节连接起来的。Go 语言的字符串的字节使用 UTF-8 编码标识 Unicode 文本。 |
| 派生类型   | 包括：(a) 指针类型（Pointer）(b) 数组类型(c) 结构化类型(struct)(d)Channel 类型(e) 函数类型(f) 切片类型(g)接口类型（interface）(h) Map 类型 |

### 定义变量

| 类型  | 描述 |
| ----- | ---- |
| var   | 变量 |
| const | 常量 |

```
package main
import "fmt"
func main() {

    // 声明一个变量并初始化
    var a = "RUNOOB"
    fmt.Println(a)

    //也可以写成
    a := "RUNOOB"
    fmt.Println(a)

    // 没有初始化就为零值
    var b int
    fmt.Println(b)

    // bool 零值为 false
    var c bool
    fmt.Println(c)

    // 定义多个变量
    a,b := 1,2
    fmt.Println(a,b)
}
```

## 运算符

| 运算符 | 描述 | 实例                |
| ------ | ---- | ------------------- |
| +      | 相加 | A + B 输出结果 30   |
| -      | 相减 | A - B 输出结果 -10  |
| \*     | 相乘 | A \* B 输出结果 200 |
| /      | 相除 | B / A 输出结果 2    |
| %      | 求余 | B % A 输出结果 0    |
| ++     | 自增 | A++ 输出结果 11     |
| --     | 自减 | A-- 输出结果 9      |

```
package main

import "fmt"

func main() {

   var a int = 21
   var b int = 10
   var c int

   c = a + b
   fmt.Printf("第一行 - c 的值为 %d\n", c )
   c = a - b
   fmt.Printf("第二行 - c 的值为 %d\n", c )
   c = a * b
   fmt.Printf("第三行 - c 的值为 %d\n", c )
   c = a / b
   fmt.Printf("第四行 - c 的值为 %d\n", c )
   c = a % b
   fmt.Printf("第五行 - c 的值为 %d\n", c )
   a++
   fmt.Printf("第六行 - a 的值为 %d\n", a )
   a=21   // 为了方便测试，a 这里重新赋值为 21
   a--
   fmt.Printf("第七行 - a 的值为 %d\n", a )
}
```

## Go 语言条件语句

### if 语句

流程图如下
![image](https://cdn.yuanmedia.top/encrypt/if.png)

```
package main

import "fmt"

func main() {
   /* 定义局部变量 */
   var a int = 10

   /* 使用 if 语句判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" )
   }
   fmt.Printf("a 的值为 : %d\n", a)
}
```

### if else 语句

流程图如下
![image](https://cdn.yuanmedia.top/encrypt/if_else.png)

```
package main

import "fmt"

func main() {
   /* 局部变量定义 */
   var a int = 100;

   /* 判断布尔表达式 */
   if a < 20 {
       /* 如果条件为 true 则执行以下语句 */
       fmt.Printf("a 小于 20\n" );
   } else {
       /* 如果条件为 false 则执行以下语句 */
       fmt.Printf("a 不小于 20\n" );
   }
   fmt.Printf("a 的值为 : %d\n", a);

}
```

### switch 语句

流程图如下
![image](https://cdn.yuanmedia.top/encrypt/swtich.png)

```
package main

import "log"

func main() {
	// 等级
	grade := "A"

	switch grade {
	case "A":
		log.Println("您的分数大于90分")
	case "B":
		log.Println("您的分数大于80分")
	case "C":
		log.Println("您的分数大于70分")
	case "D":
		log.Println("您的分数大于60分")
	default:
		log.Println("您不及格")
	}

}

```

## Go 语言循环语句

流程图如下

![image](https://cdn.yuanmedia.top/encrypt/for.png)

### 无限循环

```
package main

import "fmt"

func main() {
    for true  {
        fmt.Printf("这是无限循环。\n");
    }
}
```

### 带条件循环

```
package main

import "log"

func main() {
	i := 1
	for i < 10 {
		log.Printf("i 为 %d", i)
		i++
	}
}

```

### break

```
package main

import "log"

func main() {
	i := 1
	for {
		log.Printf("i 为 %d", i)
		i++
		if i >= 10 {
			break
		}
	}
}


```

### continue

```
package main

import "log"

func main() {
	i := 0
	for i < 100 {
		i++
		if i < 20 {
			continue
		}
		log.Printf("i 为 %d", i)

	}
}

```
