
## 1. 如何查看当前节点同步情况？

### 方法一

1.执行命令：

```bash
sudo supervisorctl tail -f newchain stderr
```

可查看日志及已同步好的区块高度（number字段对应值）。

2. 对比上述已同步好的区块高度与测试网区块链浏览器 https://explorer.testnet.newtonproject.org/ 显示的最新区块高度对比，即可知是否完成同步。

### 方法二

执行命令：

```
/data/newchain/testnet/bin/geth attach /data/newchain/testnet/nodedata/geth.ipc --exec eth.syncing
```

* 如果已经同步成功，则返回 false
* 如果还在同步，则显示同步进程

```
{
  currentBlock: 800454,
  highestBlock: 18508008,
  knownStates: 0,
  pulledStates: 0,
  startingBlock: 0
}
```

其中，
* currentBlock: 本节点的区块高度
* highestBlock: 整个网络最高区块高度


## 2. 如何更改记账节点收益地址keystore对应的密码？

2.1 查询之前Keystore密码

如果忘记您之前的密码， 输入命令 `cat /data/newchain/testnet/password.txt`

2.2 更改密码

假设您的记账节点地址为 0x1111111111111111111111111111111111111111， 新密码为 `123456`

替换如下命令中的 `0x1111111111111111111111111111111111111111` 为您自己的记账节点地址，执行该命令

```bash
/data/newchain/testnet/bin/geth --nousb --config /data/newchain/testnet/conf/node.toml account update 0x1111111111111111111111111111111111111111
```
该命令会更改keystore密码，您需要先输入您之前的密码（如果您原keystore密码为空，则直接回车即可），
之后输入您的新密码 `123456`

2.3 替换 password.txt 密码

执行如下命令：

```bash
echo "123456" > /data/newchain/testnet/password.txt
```

2.4 重启 NewChain

```bash
sudo supervisorctl restart newchain
```

2.5 查看日志

```bash
sudo supervisorctl tail -f newchain stderr
```

## 3. 如何重置记账节点收益地址keystore对应的地址？

3.1 暂停现在运行的节点

```
sudo supervisorctl stop newchain
```

3.2 创建新地址

```
/data/newchain/testnet/bin/geth --config /data/newchain/testnet/conf/node.toml account new
```

之后您需要输入您的新地址的keystore的密码，需要重复输入两次

之后就会看到您的地址，在`Public address of the key`之后，以0x开头

3.3 更改password.txt里的密码

假设您的新密码为 `123456`，则执行如下命令：

```
echo "123456" > /data/newchain/testnet/password.txt
```

您实际运行时需要替换`123456`为您的keystore密码

3.4 更改supervisor配置文件

假设您的新地址为 `0x1111111111111111111111111111111111111111`，则需要执行如下命令：


```
sed  -i "s,command=.*,command=/data/newchain/testnet/bin/geth --config /data/newchain/testnet/conf/node.toml --mine --unlock 0x1111111111111111111111111111111111111111 --password /data/newchain/testnet/password.txt --allow-insecure-unlock --miner.gastarget 100000000,"  /etc/supervisor/conf.d/newchain.conf
```

**务必替换0x1111111111111111111111111111111111111111为您自己的地址**

3.5 更新并重启

```
sudo supervisorctl update
```

3.6 重新投票

联系节点负责人协助您进行投票，把新地址设置为miner


## 4. 如何查看当前链上的投票情况？

运行命令

```
wget https://github.com/newtonproject/newcommander/releases/download/v1.2.0/newcommander -O newcommander && chmod +x newcommander && ./newcommander block list 100 -i https://global.rpc.mainnet.newtonproject.org --clique
```

结果显示如下：

```txt
23014823 0xD72EC21B3EE4432078845fBCb42F1FA9DF07Fb7E
23014822 0xCde11735471E6485a127CDb69a903C451d572B10 Auth 0x23C9CAbF3bEb6F202fcC08674491B46d0dee103E
23014821 0xc96cf8B25216dAf7B9E0651269801fF40D57Aad1 Auth 0x23C9CAbF3bEb6F202fcC08674491B46d0dee103E
23014820 0xc509eD6788A908cf6d8de2544feE65485cb01aaa Auth 0x34622a0e2a729646ea446d365473f14282F8c72e
23014819 0xc4e90c46BcD004A5CaaBa6FCF853616a407DF07c
23014818 0xbc2A2A06F5ca3F7Ba0d86d67930Af9230444ca04
23014817 0xB1b01E0b80C61F8443503bAC080E20c4115140FC Auth 0x34622a0e2a729646ea446d365473f14282F8c72e
23014816 0x9F8A9645eF5843E2619248377c82A6Fee6d9168e
23014815 0x8a4bbc3599a119Ad8c9C45e43521dbF1e803dC85 Auth 0x34622a0e2a729646ea446d365473f14282F8c72e
23014814 0x7C7c649C2CC0893aD8b9e4493041f9Bd47aE241C Auth 0x34622a0e2a729646ea446d365473f14282F8c72e
23014813 0x77833CF74612FE91E56d64BFD1813598a438b35e
23014812 0x74678c66849C4a6968D9C6E36c1FDe1f142Fd00F
23014811 0x6feb7CB7401e0A54d04C11C4d10480DF74750818 Auth 0x23C9CAbF3bEb6F202fcC08674491B46d0dee103E
23014810 0x6D000Ba3d02F905A39200E2d0b9541CedfC6722F
23014809 0x61dc321651a830a10c553c50E2041AFa4065811B Auth 0x23C9CAbF3bEb6F202fcC08674491B46d0dee103E
23014808 0x46212dAb2792d6E44B70Ae8F3bF103291e659D4f Auth 0x23C9CAbF3bEb6F202fcC08674491B46d0dee103E
23014807 0x418507821BB1EDD2dAe9Da7b652F0CDb126547BE Auth 0x34622a0e2a729646ea446d365473f14282F8c72e
23014806 0x3e7B03FEffb346E8B3018A17Da838ff33739E6FC
23014805 0x3BD2Ab6e5D23F747fCe0c3e14e39b90dc0E2A4fa Auth 0x23C9CAbF3bEb6F202fcC08674491B46d0dee103E
23014804 0x3AA88416999fd042b3771aE17e08538595715C1d
23014803 0x3519E7395C02e517390f880080e4529b384B80f2 Auth 0x23C9CAbF3bEb6F202fcC08674491B46d0dee103E
23014802 0x283449AcE26bd3955d8726e5D1a5180a17f29278
23014801 0x261873e91AB3451EF79e426E511ff8F38A171AE7 Auth 0x34622a0e2a729646ea446d365473f14282F8c72e
23014800 0x16f5a49DfAA65B40eE24AF100C91ca0500DDAa80
```

其中，第一列是区块高度，第二列是块miner地址，第三列是参与了投票，Auth为提议增加新矿工，第四列为提议增加的矿工地址；

比较第二列与当前节点列表，可以确认具体投票miner的信息







