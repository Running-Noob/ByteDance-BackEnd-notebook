# Go语言工程实践

## Go语言进阶与依赖管理

### 01 语言进阶

#### 并发vs并行

1. 并发：多线程程序在一个核的CPU上运行
2. 并行：多线程程序在多个核的CPU上运行

- Go 可以充分发挥多核优势，高效运行

#### 1.1 Goroutine

- **协程**：用户态，轻量级线程，栈 KB 级别
- **线程**：内核态，线程跑多个协程，栈 MB 级别

<img src="img/Go语言工程实践/协程-线程.png" style="zoom: 50%;" />

- 在Go语言中，为了创建协程，只需要在调用的函数前面**添加 `go` 关键字**即可。

#### 1.2 CSP

- CSP（Communicating Sequential Processes）

  Go 提倡通过通信共享内存而不是通过共享内存而实现通信。

  <img src="img/Go语言工程实践/CSP.png" style="zoom: 67%;" />

#### 1.3 Channel

- 通过 `make(chan 元素类型, [缓冲大小])` 创建

  - 无缓冲通道：`make(chan int)` 
  - 有缓冲通道：`make(chan int, 2)` 

  <img src="img/Go语言工程实践/channel.png" style="zoom:67%;" />

#### 1.4 并发安全 Lock

- 为了保证协程并发的安全进行，需要进行加锁操作。

#### 1.5 WaitGroup

- 用 `WaitGroup` 来优雅地实现并发进程的同步。

  `WaitGroup` 包含有一个计数器：

  1. 开启协程，计数器 +1；
  2. 协程执行结束，计数器 -1；
  3. 主协程会阻塞直到计数器为 0（即计数器为 0 表示所有子协程执行完毕）。

### 02 依赖管理

#### 2.1 Go依赖管理演进

- GOPATH -> Go Vendor -> Go Module
  - 不同环境（项目）依赖的版本不同
  - 控制依赖库的版本

##### 2.1.1 GOPATH

- 环境变量 `$GOPATH` ：

  <img src="img/Go语言工程实践/GOPATH.png" style="zoom: 67%;" />

  GOPATH 问题在于无法实现 package 的多版本控制。

##### 2.1.2 Go Vendor

- 项目目录下增加 vendor 文件，所有依赖包副本形式放在 `$ProjectRoot/vendor` 

  通过每个项目引入一份依赖的副本，解决了多个项目需要同一个 package 依赖的冲突问题。

- 依赖寻址方式：vendor => GOPATH（先 vendor，vendor 没有再 GOPATH）

##### 2.1.3 Go Module

- 通过 `go.mod` 文件管理依赖包版本。通过 `go get/go mod` 指令工具管理依赖包。

#### 2.2 Go Module依赖管理三要素

1. 配置文件，描述依赖：`go.mod`
2. 中心仓库管理依赖库：`Proxy` 
3. 本地工具：`go get/go mod` 

##### 2.2.1 依赖配置-go.mod

```go
module github.com/Moonlight-Zhao/go-project-example		//依赖管理基本单元

go 1.16		//原生库

require (	//单元依赖
	github.com/gin-contrib/sse v0.1.0 // indirect
	github.com/gin-gonic/gin v1.3.0 // indirect
	github.com/go-playground/validator/v10 v10.10.0 // indirect
	github.com/goccy/go-json v0.9.6 // indirect
    ...
)
```

- 单元依赖标识：`[Module Path][Version/Pseudo-version]` 

##### 2.2.2 依赖配置-version

- 语义化版本：`${MAJOR}.${MINOR}.${PATCH}` 
  - 不同的 `MAJOR` 之间的代码是隔离的
  - `MINOR` 用于新增函数等修改
  - `PATCH` 用于补丁，代码bug的修复

##### 2.2.3 依赖配置-indirect

- A->B->C：
  - A->B 直接依赖
  - **A->C 间接依赖（indirect）**

##### 2.2.4 依赖分发-回源

- 如果将依赖包托管在第三方平台上，会有几个问题：
  1. 无法保证构建稳定性：增加/修改/删除软件版本
  2. 无法保证依赖可用性：删除软件
  3. 增加第三方压力：代码托管平台负载问题

##### 2.2.5依赖分发-Proxy

- 为了解决 `2.2.4` 提出的问题，要用到 Proxy。
  - Proxy 会**缓存依赖库**，实现稳定、可靠的分发。
  - 可以将 Proxy 看作是依赖源和开发平台之间的一个“中间商”，该“中间商”会**存储依赖源的各种版本库**，以防在依赖源发生改变后，开发平台无法使用最初依赖的版本库的问题。
