### 安装环境
ubuntu 16.04.4

### 安装bitcoin可执行文件
```
sudo add-apt-repository ppa:bitcoin/bitcoin
sudo apt-get update
sudo apt-get install bitcoind
```
### 启动btc测试节点
- btc可以启动mainnet、regtest、testnet3、simnet四种节点，bos跨链服务需要启动testnet3类型的节点(bitcoind参数类型为-testnet)
- 创建data目录，mkdir -p /data/blockchain/btc/
- 启动testnet3节点，节点启动以后会首先同步区块，需要花费几个小时的时间，启动命令如下
/usr/bin/bitcoind -rpcuser=user -reindex -txindex=1 -rpcbind=0.0.0.0 -rpcport=18666 -rpcallowip=::/0 -rpcpassword=123456 -deprecatedrpc=accounts -testnet -rest -datadir=/data/blockchain/btc/
- 获取测试链新的地址
/usr/bin/bitcoin-cli -rpcuser=user  -rpcport=18666 -rpcpassword=123456  -testnet  -datadir=/data/blockchain/btc/ getnewaddress
- 查看新地址下的btc余额，此时余额为0
/usr/bin/bitcoin-cli -rpcuser=space  -rpcport=18666 -rpcpassword=space123  -testnet  -datadir=/data/blockchain/btc/ getbalance
- testnet3测试节点需要去测试币网站领取测试币，推荐网站为
https://coinfaucet.eu/en/btc-testnet/
- testnet3测试网浏览器，在上面可以查看当前区块，测试网的转账记录也可以查到
https://live.blockcypher.com/btc-testnet/
- 从测试链向bos账号的映射btc地址转账
/usr/bin/bitcoin-cli -rpcuser=space  -rpcport=18666 -rpcpassword=space123  -testnet  -datadir=/data/blockchain/btc/ sendfrom “” “mjRg63C9JdG7CgiomU9UL4FdGKfQJJz6rR” 0.10000000
- 通过测试币浏览器即可查看此交易

*说明:btc测试网申请的测试币数目有限且申请次数有限制，所以需要谨慎使用*
	
