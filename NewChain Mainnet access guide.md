#### NewChain-Nodes Mainnet access guide

1. Purchase Elastic Compute Service(ECS) and you may refer to following link

https://aws.amazon.com/ec2/

2. Deploy Mainnet as per following instructions to launch block synchronizing

https://github.com/newtonproject/newchain-deploy

3. Check the progress of block synchronizing

3.1 Execute following command to check the synchronized block height

sudo supervisorctl tail -f newchain stderr

3.2 Make sure block synchronization is completed and same to the latest testnet block height at below link

https://explorer.testnet.newtonproject.org

4. Execute following command after synchronizing to create minner address and keystore

cd /data/newchain/testnet/ && curl -L https://gitee.com/newtonproject/newchain-deploy/raw/master/mine_testnet.sh -o mine_testnet.sh && chmod +x mine_testnet.sh && ./mine_testnet.sh

5. Set keystore passwoard

Input keystore and keep keystore, password and minner address

6. Fill in following information and submitt issue

6.1 Node name:

6.2 Minner address:

6.3 RPC Url/RPC Url:

6.4 Node operator name:

6.5 Contacts such as email, telegram account:

6.6 NewPay orNewMask Mainnet NEW address:

7. Execute codes as per instructions in following link to upgrade read-only node to accouting node

https://github.com/newtonproject/newchain-deploy/wiki/NewChain-TestNet-mine

Copy minner address and replace 000 in following command to request other node to execute it and approve your upgrade in NewChain minner social media group

/data/newchain/testnet/bin/geth attach /data/newchain/testnet/nodedata/geth.ipc --exec 'clique.propose("000", true)'

8. Send above command containing your minner address to the social group for NewChain nodes

9. Wait for the executing code and approval from other nodes operators
