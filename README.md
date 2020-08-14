# ContractTool

1.vscl简介

voc主链合约语言为VSCL（VOC  Smart  Contract Language）是一门面向合约的、为实现智能合约而创建的高级编程语言。这门语言受到了 C++，Python 和 Javascript 语言的影响，设计的目的是能在VOC虚拟机（VVM）上运行。

Vscl 是静态类型语言，支持继承、库和复杂的用户定义类型等特性。

2.合约开发工具：

目前尝试 Vscl 编程的最好的方式是使用 [vscode](https://code.visualstudio.com/)+[vscl](https://github.com/vocchain/ContractTool/blob/master/vscl-0.0.12.vsix)扩展插件

插件下载链接：

![image-20200730173742411](D:\文档\文档\markdown\区块链\VOC\Untitled.assets\image-20200730173742411.png)



2.合约编辑

![image-20200730173952218](D:\文档\文档\markdown\区块链\VOC\Untitled.assets\image-20200730173952218.png)

示例代码：

```json
pragma vscl ^0.4.21;


contract TokenVRC03 {
    string public name;
    string public symbol;
    uint public decimals = 18; 
    uint public totalSupply;
    mapping (address => uint) public balances;

    event Sent(address from, address to, uint amount);

    function TokenVRC03(uint initialSupply, string tokenName, string tokenSymbol,uint decimal){
          decimals=decimal;
        totalSupply = initialSupply * 10 ** uint(decimals);
        balances[msg.sender] = totalSupply;
        name = tokenName;
        symbol = tokenSymbol;
    }
    
 function transfer(address receiver, uint amount) public {
        if (balances[msg.sender] < amount) return ;
        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```

3.合约编译

合约编译使用vscl合约编译工具volc 

下载链接：https://github.com/vocchain/ContractTool/blob/master/volc.zip

```
 # 1.windows
./vocl.exe vrc03.vol
# 编译将生成两个文件
VRC03Token.bin   //合约二进制代码（部署合约的code）
VRC03Token.abi   //合约二进制接口文件

# 2. linux
./vocl vrc03.vol
# 编译将生成两个文件
VRC03Token.bin   //合约二进制代码（部署合约的code）
VRC03Token.abi   //合约二进制接口文件
```

4. 合约部署

合约部署需要使用voc-cli, 请使用`voc-cli -h`查看使用帮助

下载链接：

linux:https://github.com/vocchain/ContractTool/blob/master/voc-cli

windows:https://github.com/vocchain/ContractTool/blob/master/voc-cli_windows.zip

```
 .\voc-cli.exe

      USAGE
      voc-cli COMMAND [ARGMENTS] [OPTIONS]

      [COMMAND]
      import   import voc privkey    //导入私钥
      contract create or call smart contract   //创建或调用合约
      vrc03   creaate or send vrc03 tokens   //开发中
      help    show more infomation for command

      [OPTION]
      -h help
```



```
1 windos部署：
1.导入需要部署合约的账号
voc-cli.exe   import VOC2bZcHrZXXXXXXXUP     password
2.部署合约
./voc-cli.exe   contract create -c 合约code（编译生成的bin）  -a  合约参数（根据abi生成） -n 合约名称 --account 你的账号

2 linux
1.导入需要部署合约的账号
voc-cli  import VOC2bZcHrZXXXXXXXUP     password
2.部署合约
./voc-cli   contract create -c 合约code（编译生成的bin）  -a  合约参数（根据abi生成） -n 合约名称 --account 你的账号
```

