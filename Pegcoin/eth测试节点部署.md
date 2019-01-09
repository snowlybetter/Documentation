### 安装环境
ubuntu 16.04.4

### 安装geth可执行文件
```
ssudo add-apt-repository ppa:ethereum/ethereum
sudo apt update
sudo apt install ethereum
```
### 启动eth测试节点
- 以太坊有公开的测试网络，比较常用的有ropsten（雷电）、rinkeby。bos测试网跨链换币服务连接的是ropsten网络。两种网络的启动方式分别是geth –testnet、geth --rinkeby
- 创建data目录，mkdir -p /data/ethereum/block_data
- 启动ropsten测试网节点，节点启动以后会同步区块，可能要花费一天的时间，启动命令如下
geth --testnet --syncmode "full" --cache=512 --datadir "/data/ethereum/block_data" --rpc --rpcapi db,net,eth,web3,debug,personal,admin --rpcport 9666 --port 9548 --rpcaddr 0.0.0.0 --rpccorsdomain "*" console
- 连接geth客户端：geth attach /mnt/work/blockchain/eth/geth.ipc
- 进入geth客户端下首先创建账号personal.newAccount("ethbostest”)，其中中间为对私钥进行加密的密码
- 查看创建的账号：eth.accounts
- 在同步区块是可以通过admin.peers查看节点连接的peer信息
- 在同步区块使可以查看当前同步的区块高度：eth.blockNumber
- 在转账之前需要申请ropsten测试币用来测试，申请网址为：https://faucet.ropsten.be
- 在转账之前需要解锁钱包，user1=eth.accounts[0]—> personal.unlockAccount(user1)，输入创建账号时的密码即可解锁
- 向bos映射的地址转账：eth.sendTransaction({from:"0xee46af1d0720a9d413d4c1e0af2078e3b2de6462", to:"0xa801aeb41cc4bb0ad9451238653be26a1827d170", value:web3.toWei(0.00100000, "ether")})
- 查看地址的余额：eth.getBalance("0xee46af1d0720a9d413d4c1e0af2078e3b2de6462”)
- 转账以后可以通过ropsten测试网浏览器，查看交易的确认情况：https://ropsten.etherscan.io

*说明:eth测试网申请的测试币数目有限且申请次数有限制，所以需要谨慎使用*
