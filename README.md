# naivecoin-go

go version naivecoin.

## 环境配置

1. 从官网下载最新的Golang环境，截止到今天（2020/04/22）应该是Go 1.14版本。
2. 看一下你的环境变量，系统变量里的 Path 里应该已经自动注入了 Go 的 bin 文件夹。你需要注意的是用户变量里的 GOPATH，这个位置一般用来存放你的第三方依赖，默认位置是你的个人文档，你也可以修改到你喜欢的位置去。

## 项目编译与初始化
1. 打开工程目录，首先需要下载第三方依赖。在根目录位置打开 CMD 命令行或者 powershell，输入
    ```
    go mod download
    ```
    但是需要注意的是，go mod 会从官方依赖中心或者 github 上拉取依赖，这个对中国大陆地区尤其是你们那块儿的网络极不友好（原因你懂的）。这时你要配置一下依赖代理，在命令行输入
    ```
    $env:GOPROXY = "https://goproxy.cn"
    ```
    就行了。你再试试下载是不是行了。只要输入 download 之后没有报错就跳到等待输入状态就说明下载好了。
2. 接着就可以编译项目了，仍是在工程根目录，命令行输入
    ```
    go build .\src
    ```
    你就能在目录下发现一个叫 src.exe 的文件，这就是编译结果了。
3. 在执行 exe 之前，先在工程根目录下创建一个叫 data 的文件夹，这个文件夹用来存放 bolt 数据库和 dat 配置文件。本来这个工作是应该代码初始化时自动执行的，但是我懒啊，忘记做了，你自己手动建吧。

## 执行程序
1. 在命令行里执行
    ```
    .\src
    ```
    就能启动程序。如果命令里不加任何参数的话，你就会看到一大堆 help 提示告诉你命令的格式，这是我自己写的，哈哈。
2. 要创造一个区块链加密货币，首先需要来几个公钥地址，创造命令是
    ```
    .\src createaddress
    ```
    完事了你就能看到这种结果
    ```
    Your new address: 15hUVxUUrSajsonsUK3Er56X7zJNxjBMw5
    ```
    你的结果肯定和我上面的不一样，都是随机的嘛，意思到了就行了。你也可以多整几个地址，就重复执行上面的命令就行了。然后你输入
    ```
    .\src listaddress
    ```
    就能看到你本地的所有公钥地址了。
3. 下面是创造创世区块。创世区块是区块链的首部区块，你需要指定创世区块的创币奖励给谁，我设定会给挖矿人奖励 10 块钱。就这样
    ```
    .\src initchain -address 15hUVxUUrSajsonsUK3Er56X7zJNxjBMw5
    ```
    在 `-address` 后面跟的就是你刚才创建的任意一个地址就行了。
4. 有了创世区块，你的链里就开始有货币了。你就可以把货币转来转去玩一玩，就像这样
    ```
     .\src send -from 15hUVxUUrSajsonsUK3Er56X7zJNxjBMw5 -to 1AKfY7NnEYg65dCMviwCvmHLp1VfyKRFAP -amount 1
    ```
    `-from` 后面接的是币的来源，来源地址里一定要有钱，否则会报错的。`-to` 接的是钱的去向，代码只会检查这个地址是不是 Base58 语法上合法的，但是没有办法检测这个地址到底是不是真的对应着一个人和一个账户。这个是区块链的特点，即使是比特币也是这样。`-amount` 后面接你要转多少钱，这个额度不能超过你来源拥有的钱的总额。
    本来按照比特币的系统，一个区块里面可以包含多笔交易，但是我懒啊，就把它简化成一个交易一个区块了。
5. 这时你可以把区块链打印出来看一看到底长啥样
    ```
    .\src printchain 
    ```
    打印结果是我精心排版的，好看吧。也可以看一看交易的内容
    ```
    .\src printtrans
    ```
    你也可以看看某个地址里面有多少钱，比如
    ```
    .\src getbalance -address 15hUVxUUrSajsonsUK3Er56X7zJNxjBMw5
    ```
6. 这个代码还有一些功能是未完成的，毕竟我要做毕设了没空写了。我在这里告诉你哪些还没做。一方面是没有把区块链中节点的角色分离，一般至少要把挖矿节点和交易节点分离才好；一方面是还没有做通信功能，现在的项目就只能本地玩一玩，没法多节点跑；还有交易签名的部分做了一半，还没有完全做完。

给武威师兄，希望你玩的开心。
