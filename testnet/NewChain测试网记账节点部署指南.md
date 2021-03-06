## NewChain测试网记账节点部署指南

### 1. 购买云服务器，可参考：

https://www.aliyun.com/product/ecs?spm=5176.12825654.eofdhaal5.2.36722c4aSDIOpf

### 2. 按照下面链接代码操作部署主网，启动区块同步：

https://github.com/newtonproject/newchain-deploy

### 3. 查看区块同步进度：

3.1 执行命令查看同步好的区块高度（number字段对应值）

sudo supervisorctl tail -f newchain stderr

3.2 如同步好的区块链高度与下面链接中的测试网最新区块高度一致则同步完成

https://explorer.testnet.newtonproject.org

### 4. 同步完成后执行下列命令

```bash
cd /data/newchain/testnet/ && curl -L https://release.cloud.diynova.com/newton/newchain-deploy/testnet/newchain-mine.sh -o newchain-mine.sh && chmod +x newchain-mine.sh && ./newchain-mine.sh
```

该命令会下载并运行 newchain-mine.sh 脚本，创建miner并启用挖矿

### 5. 设置keystore密码

输入两次keystore密码，保存好keystore和minnner地址

### 5. 设置keystore密码

输入两次keystore密码，保存好keystore和minnner地址

- 备份keystore

输入如下命令，获取keystore内容，

```bash
cat /data/newchain/testnet/nodedata/keystore/*
```

keystore为整个大括号及其包含的内容，需要备份好，切勿泄露给任何人

- 备份keystore的密码

输入如下命令，即可看到密码

```bash
cat /data/newchain/testnet/password.txt
```

### 6. 将填写好的如下内容提交issue，提交链接地址:

* Node name/对外显示名称：
* Minner address/进行打包出块的miner地址：
* RPC Url/RPC Url（可以是服务器地址加端口）：
* Node operator name/运行节点主体（个人、社群、组织）名称：
* Contacts such as email, telegram account/节点负责人及联系方式（手机、邮箱、微信、telegram等均可）：
* NewPay or NewMask Mainnet NEW address/用于接收测试网矿工纪念币的主网NEW地址(NewPay或NewMask)：

### 7. Minner地址粘贴替代下列代码中的000:

```bash
/data/newchain/testnet/bin/geth attach /data/newchain/testnet/nodedata/geth.ipc --exec 'clique.propose("000", true)'
```

### 8. 批准加入

将替换好自己minner地址的第7项中的命令发送至NewChain testnet nodes群请其他矿工执行该命令批准加入

### 9. 等待其他节点执行代码批准加入

### 10. 查看是否已成为记账节点

```bash
/data/newchain/testnet/bin/geth attach /data/newchain/testnet/nodedata/geth.ipc --exec 'clique.getSigners()'
```
