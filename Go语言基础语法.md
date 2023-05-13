# Go语言基础语法

## Go 语言快速上手 - 基础语言

### 01 简介

#### 1.1 什么是 Go 语言

- Go 语言是谷歌出品的一门通用型计算机编程语言。有以下特性：
  1. 高性能、高并发：内嵌了对高并发的支持
  2. 语法简单、学习曲线平缓
  3. 丰富的标准库
  4. 完善的工具链：编译、代码格式化、错误检查、代码文档、包管理、代码提示
  5. 静态链接
  6. 快速编译
  7. 跨平台：Linux、WIndows、MacOS、Android、ios
  8. 垃圾回收

#### 1.2 哪些公司在使用 GO

- 字节、腾讯、哔哩哔哩、Google、Facebook等

#### 1.3 字节全面拥抱 Go 的原因

1. 最初使用的 Python，由于性能问题换成了 Go
2. C++ 不太适合在线 Web 业务
3. 早期团队非 Java 背景
4. 性能比较好
5. 部署简单、学习成本低
6. 内部 RPC 和 HTTP 框架的推广

### 02 入门

#### 2.1 开发环境

##### 安装 Golang

- 通过以下链接可以下载安装 Golang：

  [The Go Programming Language](https://go.dev/) 

  [Go下载 - Go语言中文网](https://studygolang.com/dl) 

  [七牛云 - Goproxy.cn](https://goproxy.cn/) 

##### 配置集成开发环境

- 可以使用 VScode 作为 Go 的集成开发环境

#### 2.2 基础语法

- 以 “hello world” 为例，介绍 go 的代码组成：

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main() {
  	fmt.Println("hello world")
  }
  ```

  - `package main` ：代表这个文件属于 main 包的一部分，是程序的入口文件
  - `import {"fmt"}`：导入了 “fmt” 包，用于向屏幕输入输出字符串、格式化字符串
  - `func main() {fmt.Println("hello world")}`：main 函数，用于向屏幕输出 “hello world” 字符串

##### 变量

- Go 语言是强类型语言。
- Go 语言变量的声明有两种方式：
  1. `var name = value` ：这种方式会自动推导变量的类型
  2. `name := value`

##### 常量

- Go 语言常量的声明方式：`const name = value`：会根据使用的上下文自动确定使用类型

##### if-else

- `if` 后面的条件判断不带括号，且**相应的结果语句必须带大括号**，不能和 `if` 写在同一行。

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	if 7%2 == 0 {
  		fmt.Println("7 is even")
  	} else {
  		fmt.Println("7 is odd")
  	}
  
  	if 8%4 == 0 {
  		fmt.Println("8 is divisible by 4")
  	}
  
  	if num := 9; num < 0 {
  		fmt.Println(num, "is negative")
  	} else if num < 10 {
  		fmt.Println(num, "has 1 digit")
  	} else {
  		fmt.Println(num, "has multiple digits")
  	}
  }
  ```

##### 循环

- **Go 只有 `for` 循环**。

  `for` 循环后面也不用带括号，可以用 `break`、`continue` 来对循环进行操作。

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	i := 1
  	for {
  		fmt.Println("loop")
  		break
  	}
  	for j := 7; j < 9; j++ {
  		fmt.Println(j)
  	}
  
  	for n := 0; n < 5; n++ {
  		if n%2 == 0 {
  			continue
  		}
  		fmt.Println(n)
  	}
  	for i <= 3 {
  		fmt.Println(i)
  		i = i + 1
  	}
  }
  ```

##### switch

- `switch` 后面的变量名也不需要括号。在 `switch` 的 case 中，如果没有 `break`，是不会像 `C++` 一样一直向下执行其他的 case 的。

  ```go
  package main
  
  import (
  	"fmt"
  	"time"
  )
  
  func main() {
  
  	a := 2
  	switch a {
  	case 1:
  		fmt.Println("one")
  	case 2:
  		fmt.Println("two")
  	case 3:
  		fmt.Println("three")
  	case 4, 5:
  		fmt.Println("four or five")
  	default:
  		fmt.Println("other")
  	}
  
  	t := time.Now()
  	switch {
  	case t.Hour() < 12:
  		fmt.Println("It's before noon")
  		fmt.Println(t.Hour())
  	default:
  		fmt.Println("It's after noon")
  		fmt.Println(t.Hour())
  	}
  }
  ```

##### 数组

```go
package main

import "fmt"

func main() {

	var a [5]int
	a[4] = 100
	fmt.Println("get:", a[2])
	fmt.Println("len:", len(a))

	b := [5]int{1, 2, 3, 4, 5}
	fmt.Println(b)

	var twoD [2][3]int
	for i := 0; i < 2; i++ {
		for j := 0; j < 3; j++ {
			twoD[i][j] = i + j
		}
	}
	fmt.Println("2d: ", twoD)
}
```

##### ★切片

- 切片不同于数组，它是一个可变长度的数组，可以任意更改大小。

  我们用 `make` 来创建切片。

  ```go
  package main
  
  import "fmt"
  
  func main() {
  
  	s := make([]string, 3)
  	s[0] = "a"
  	s[1] = "b"
  	s[2] = "c"
  	fmt.Println("get:", s[2])   // c
  	fmt.Println("len:", len(s)) // 3
  
  	s = append(s, "d")
  	s = append(s, "e", "f")
  	fmt.Println(s) // [a b c d e f]
  
  	c := make([]string, len(s))
  	copy(c, s)
  	fmt.Println(c) // [a b c d e f]
  
  	fmt.Println(s[2:5]) // [c d e]
  	fmt.Println(s[:5])  // [a b c d e]
  	fmt.Println(s[2:])  // [c d e f]
  
  	good := []string{"g", "o", "o", "d"}
  	fmt.Println(good) // [g o o d]
  }
  ```

##### ★map

- 实际使用过程中使用最频繁的数据结构。

  我们**用 `make(map[key]value)` 来创建 map**。

  Go 里面的 map 是完全无序的。

  ```go
  package main
  
  import "fmt"
  
  func main() {
  	m := make(map[string]int)
  	m["one"] = 1
  	m["two"] = 2
  	fmt.Println(m)           // map[one:1 two:2]
  	fmt.Println(len(m))      // 2
  	fmt.Println(m["one"])    // 1
  	fmt.Println(m["unknow"]) // 0
  
  	r, ok := m["unknow"]
  	fmt.Println(r, ok) // 0 false
  
  	delete(m, "one")
  
  	m2 := map[string]int{"one": 1, "two": 2}
  	var m3 = map[string]int{"one": 1, "two": 2}
  	fmt.Println(m2, m3)
  }
  ```

##### range

- 对于一个 `slice` 或者 `map`，我们可以用 `range` 来进行**快速遍历**。

  ```go
  package main
  
  import "fmt"
  
  func main() {
  	nums := []int{2, 3, 4}
  	sum := 0
  	for i, num := range nums {
  		sum += num
  		if num == 2 {
  			fmt.Println("index:", i, "num:", num) // index: 0 num: 2
  		}
  	}
  	fmt.Println(sum) // 9
  
  	m := map[string]string{"a": "A", "b": "B"}
  	for k, v := range m {
  		fmt.Println(k, v) // b 8; a A
  	}
  	for k := range m {
  		fmt.Println("key", k) // key a; key b
  	}
  }
  ```

##### 函数

- Go 里面的函数会返回多个值，第一个值是真正的返回结果，第二个值返回的是错误信息。

  ```go
  package main
  
  import "fmt"
  
  func add(a int, b int) int {
  	return a + b
  }
  
  func add2(a, b int) int {
  	return a + b
  }
  
  func exists(m map[string]string, k string) (v string, ok bool) {
  	v, ok = m[k]
  	return v, ok
  }
  
  func main() {
  	res := add(1, 2)
  	fmt.Println(res) // 3
  
  	v, ok := exists(map[string]string{"a": "A"}, "a")
  	fmt.Println(v, ok) // A True
  }
  ```

##### 指针

- 主要用于对传入的参数进行修改。

  ```go
  package main
  
  import "fmt"
  
  func add2(n int) {
  	n += 2
  }
  
  func add2ptr(n *int) {
  	*n += 2
  }
  
  func main() {
  	n := 5
  	add2(n)
  	fmt.Println(n) // 5
  	add2ptr(&n)
  	fmt.Println(n) // 7
  }
  ```

##### 结构体

- 结构体是带类型的字段的集合。

  ```go
  package main
  
  import "fmt"
  
  type user struct {
  	name     string
  	password string
  }
  
  func main() {
  	a := user{name: "wang", password: "1024"}
  	b := user{"wang", "1024"}
  	c := user{name: "wang"}
  	c.password = "1024"
  	var d user
  	d.name = "wang"
  	d.password = "1024"
  
  	fmt.Println(a, b, c, d)                 // {wang 1024} {wang 1024} {wang 1024} {wang 1024}
  	fmt.Println(checkPassword(a, "haha"))   // false
  	fmt.Println(checkPassword2(&a, "haha")) // false
  }
  
  func checkPassword(u user, password string) bool {
  	return u.password == password
  }
  
  func checkPassword2(u *user, password string) bool {
  	return u.password == password
  }
  ```

##### 结构体方法

```go
package main

import "fmt"

type user struct {
	name     string
	password string
}

func (u user) checkPassword(password string) bool {
	return u.password == password
}

func (u *user) resetPassword(password string) {
	u.password = password
}

func main() {
	a := user{name: "wang", password: "1024"}
	a.resetPassword("2048")
	fmt.Println(a.checkPassword("2048")) // true
}
```

##### 错误处理

- 前面说过，Go 里面的函数会返回多个值，第一个值是真正的返回结果，第二个值返回的是错误信息。

  我们可以通过函数返回的错误信息，来判断是否发生了某种错误。

  ```go
  package main
  
  import (
  	"errors"
  	"fmt"
  )
  
  type user struct {
  	name     string
  	password string
  }
  
  func findUser(users []user, name string) (v *user, err error) {
  	for _, u := range users {
  		if u.name == name {
  			return &u, nil
  		}
  	}
  	return nil, errors.New("not found")
  }
  
  func main() {
  	u, err := findUser([]user{{"wang", "1024"}}, "wang")
  	if err != nil {
  		fmt.Println(err)
  		return
  	}
  	fmt.Println(u.name) // wang
  
  	if u, err := findUser([]user{{"wang", "1024"}}, "li"); err != nil {
  		fmt.Println(err) // not found
  		return
  	} else {
  		fmt.Println(u.name)
  	}
  }
  ```

- 在这里，`nil` 用于表示是否发生了错误，如果返回的 `error` 为 `nil`，则表示没有发生错误；否则发生了错误。

  在 Go 中，为了简单和方便，**nil 被设计为一个标识符，可以用来表示某些类型的零值**。他不是一个不变的值，而是随着不同类型有不同的类型，在日常的编码中，编译器会根据上下文来推导出。

##### 字符串操作

- 在 Go 的标准库 strings 中，有非常多的字符串操作函数，如下所示：

  ```go
  package main
  
  import (
  	"fmt"
  	"strings"
  )
  
  func main() {
  	a := "hello"
  	fmt.Println(strings.Contains(a, "ll"))                // true
  	fmt.Println(strings.Count(a, "l"))                    // 2
  	fmt.Println(strings.HasPrefix(a, "he"))               // true
  	fmt.Println(strings.HasSuffix(a, "llo"))              // true
  	fmt.Println(strings.Index(a, "ll"))                   // 2
  	fmt.Println(strings.Join([]string{"he", "llo"}, "-")) // he-llo
  	fmt.Println(strings.Repeat(a, 2))                     // hellohello
  	fmt.Println(strings.Replace(a, "e", "E", -1))         // hEllo
  	fmt.Println(strings.Split("a-b-c", "-"))              // [a b c]
  	fmt.Println(strings.ToLower(a))                       // hello
  	fmt.Println(strings.ToUpper(a))                       // HELLO
  	fmt.Println(len(a))                                   // 5
  	b := "你好"
  	fmt.Println(len(b)) // 6
  }
  ```

##### 字符串格式化

- 在 Go 的标准库 fmt 中，可以用 `Printf` 对输出进行格式化：

  通过 `%+v`、`%#v` 可以获得更详细的输出。

  ```go
  package main
  
  import "fmt"
  
  type point struct {
  	x, y int
  }
  
  func main() {
  	s := "hello"
  	n := 123
  	p := point{1, 2}
  	fmt.Println(s, n) // hello 123
  	fmt.Println(p)    // {1 2}
  
  	fmt.Printf("s=%v\n", s)  // s=hello
  	fmt.Printf("n=%v\n", n)  // n=123
  	fmt.Printf("p=%v\n", p)  // p={1 2}
  	fmt.Printf("p=%+v\n", p) // p={x:1 y:2}
  	fmt.Printf("p=%#v\n", p) // p=main.point{x:1, y:2}
  
  	f := 3.141592653
  	fmt.Println(f)          // 3.141592653
  	fmt.Printf("%.2f\n", f) // 3.14
  }
  ```

##### ★JSON处理

- 通过 `json.Marshal` 函数进行 JSON 处理。

  ```go
  package main
  
  import (
  	"encoding/json"
  	"fmt"
  )
  
  type userInfo struct {
  	Name  string
  	Age   int `json:"age"`
  	Hobby []string
  }
  
  func main() {
  	a := userInfo{Name: "wang", Age: 18, Hobby: []string{"Golang", "TypeScript"}}
  	buf, err := json.Marshal(a)
  	if err != nil {
  		panic(err)
  	}
  	fmt.Println(buf)         // [123 34 78 97...]
  	fmt.Println(string(buf)) // {"Name":"wang","age":18,"Hobby":["Golang","TypeScript"]}
  
  	buf, err = json.MarshalIndent(a, "", "\t")
  	if err != nil {
  		panic(err)
  	}
  	fmt.Println(string(buf))
  
  	var b userInfo
  	err = json.Unmarshal(buf, &b)
  	if err != nil {
  		panic(err)
  	}
  	fmt.Printf("%#v\n", b) // main.userInfo{Name:"wang", Age:18, Hobby:[]string{"Golang", "TypeScript"}}
  }
  ```

##### 时间处理

- 在 Go 的标准库中，可以用 `time` 进行时间处理。

  ```go
  package main
  
  import (
  	"fmt"
  	"time"
  )
  
  func main() {
  	now := time.Now()
  	fmt.Println(now) // 2022-03-27 18:04:59.433297 +0800 CST m=+0.000087933
  	t := time.Date(2022, 3, 27, 1, 25, 36, 0, time.UTC)
  	t2 := time.Date(2022, 3, 27, 2, 30, 36, 0, time.UTC)
  	fmt.Println(t)                                                  // 2022-03-27 01:25:36 +0000 UTC
  	fmt.Println(t.Year(), t.Month(), t.Day(), t.Hour(), t.Minute()) // 2022 March 27 1 25
  	fmt.Println(t.Format("2006-01-02 15:04:05"))                    // 2022-03-27 01:25:36
  	diff := t2.Sub(t)
  	fmt.Println(diff)                           // 1h5m0s
  	fmt.Println(diff.Minutes(), diff.Seconds()) // 65 3900
  	t3, err := time.Parse("2006-01-02 15:04:05", "2022-03-27 01:25:36")
  	if err != nil {
  		panic(err)
  	}
  	fmt.Println(t3 == t)    // true
  	fmt.Println(now.Unix()) // 1683795669
  }
  ```

##### 数字解析

- 在 Go 的标准库中，可以用 `strconv` 进行数字解析操作。 （`strconv` -> `string convert`）

  ```go
  package main
  
  import (
  	"fmt"
  	"strconv"
  )
  
  func main() {
  	f, _ := strconv.ParseFloat("1.234", 64)
  	fmt.Println(f) // 1.234
  
  	n, _ := strconv.ParseInt("111", 10, 64)
  	fmt.Println(n) // 111
  
  	n, _ = strconv.ParseInt("0x1000", 0, 64)
  	fmt.Println(n) // 4096
  
  	n2, _ := strconv.Atoi("123")
  	fmt.Println(n2) // 123
  
  	n2, err := strconv.Atoi("AAA")
  	fmt.Println(n2, err) // 0 strconv.Atoi: parsing "AAA": invalid syntax
  }
  ```

##### 进程信息

- 在 Go 的标准库中，可以用 `os` 进行进程操作。

  ```go
  package main
  
  import (
  	"fmt"
  	"os"
  	"os/exec"
  )
  
  func main() {
  	// go run example/20-env/main.go a b c d
  	fmt.Println(os.Args)           // [/var/folders/8p/n34xxfnx38dg8bv_x8l62t_m0000gn/T/go-build3406981276/b001/exe/main a b c d]
  	fmt.Println(os.Getenv("PATH")) // /usr/local/go/bin...
  	fmt.Println(os.Setenv("AA", "BB"))
  
  	buf, err := exec.Command("grep", "127.0.0.1", "/etc/hosts").CombinedOutput()
  	if err != nil {
  		panic(err)
  	}
  	fmt.Println(string(buf)) // 127.0.0.1       localhost
  }
  ```

### 03 实战