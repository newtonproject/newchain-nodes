## NewChain-Nodes Mainnet access guide

### 1. Purchase Elastic Compute Service(ECS) and you may refer to following link

https://aws.amazon.com/ec2/

### 2. Deploy Mainnet as per following instructions to launch block synchronizing

https://github.com/newtonproject/newchain-deploy/blob/master/README-en.md

### 3. Check the progress of block synchronizing

3.1 Execute following command to check the synchronized block height

sudo supervisorctl tail -f newchain stderr

3.2 Make sure block synchronization is completed and same to the latest mainnet block height at below link

https://explorer.newtonproject.org

### 4. Execute following command after synchronizing to create minner address and keystore

```bash
cd /data/newchain/mainnet/ && curl -L https://release.cloud.diynova.com/newton/newchain-deploy/mainnet/newchain-mine.sh -o newchain-mine.sh && chmod +x newchain-mine.sh && ./newchain-mine.sh
```

### 5. Set keystore passwoard

Input keystore and keep keystore, password and minner address

### 6. Fill in following information and submitt issue

* Node name:
* Minner address:
* RPC Url/RPC Url:
* Node operator name:
* Contacts such as email, telegram account:
* NewPay or NewMask Mainnet NEW address:

### 7. Copy minner address and replace 000 in following command

```bash
/data/newchain/mainnet/bin/geth attach /data/newchain/mainnet/nodedata/geth.ipc --exec 'clique.propose("000", true)'
```

### 8. Send above command containing your minner address to the social group for NewChain nodes

### 9. Wait for the executing code and approval from other nodes operators
