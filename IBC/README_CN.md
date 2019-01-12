
# IBC 用户协议
## 简介

当将一条链上的token信息向本链和对方链的ibc.token合约注册此token信息后，ibc系统开始接受对此token的跨链交易。ibc系统支持两端链上任意多种token的跨链需求。

ibc系统在一条链上只有一个ibc.token合约，原链上被映射的多种token都会在本合约中进行管理。

对需要跨链的token合约的需求：
- 第一：其transfer接口必须和 “eosio.token”合约的transfer接口定义完全相同
- 第二：其transfer函数中必须包含 require_recipient( to ); 语句

在一个token被注册后
- eosio用户调用被承接token的transfer接口并提供适当的memo信息，即可完成资产从原链到token映射链的转移。
- eosio用户调用ibc.token合约transfer接口并提供适当的memo信息，即可完成映射资产转回原链的操作。

因此用户使用现有的手机app钱包即可完成跨链资产，但需要现有的钱包增加支持ibc.token合约，支持也很简单，因为ibc.token合约的transfer接口定义和eosio.token的transfer定义完全相同，手机app钱包也可以提供专有的的界面，增强用户体验。

/**
 * ---- ibc memo format of transfer action ----
 * memo string must start with "ibc", and the rest of the string should be composed of key-value pairs
 * key-value pairs are separated by spaces, key and value are separated by equal-sign
 * currently supported keys include: "receiver,r","notes". other keys will be ignored
 *
 * examples:
 * "ibc receiver=youraccount"
 * "ibc r=youraccount"
 * "ibc r=youraccount notes=this is notes txt"
 *
 * key details:
 * "receiver,r": required,   must be a valid eosio name. The name should exist on peerchain, if not, this ibc transaction will failed finally.
 * "notes":      optional,   user defined string, must be the last item if have, all content after "notes=" is the value of "notes"
 */


## 被承兑token合约transfer 接口说明

源码链接:https://github.com/boscore/bos.contracts/blob/master/ibc.token/src/ibc.token.cpp#L367

当用户将资产从其原链转移到对方链上时调用。

即调用 token合约的transfer接口，格式为[from ,to, quantity,memo]

其中to必须为本条链上ibc系统的ibc.token合约名，例如“ibc2token555”

其中memo的格式如下：

memo需要以“ibc ”开头，之后的字符串由key=value的格式组成，如果提供了note键，则note=之后的所有字符都属于此建的value；必须提供receiver键，并提供一个有效的eosio名字。

有效的memo示例如下:
```
"ibc receiver=bosaccounta"
"ibc r=bosaccounta note=happy new year 2019"
```
一个有效的transfer命令如下
```
cleos transfer fromsomeone1 ibc2token555 "1.0000 EOS" "ibc r=bosaccounta"
```
当调用被承兑token的transfer接口，并且to为ibc.token合约时，memo必须以ibc 或 local开头，否则会失败，当以local开头时，ibc.token合约不做处理，因为不是跨链交易，这个目的是允许直接给ibc.token合约账户转账，而与跨链无关。
