# fabric 背书策略

* [1. 用途](#1.用途)
    * [1.1 交易流程回顾](#1.1交易流程回顾)

## 1.用途

### 1.1交易流程回顾

Hyperledger Fabric 区块链网络交易的执行分为一下几个步骤。Endorse 与 Committer 都是 Hyperledger Fabric 区块链网络中 Peer 节点的具体角色。与背书策略强相关的是**第 3 步**。

1. Client 构造交易并发往 Endorser 节点，Endorser 节点执行交易并调用系统链码 ESCC 对交易签名（背书），结果返回 Client
2. Client 将交易响应发送给 Orderer 节点进行排序出块，Orderer 节点将交易打包到区块中，广播给网络上的 Committer 节点
3. Committer 节点收到区块后，对区块内交易逐一验证，其中一个重要的步骤是调用系统链码 VSCC 校验交易是否符合指定的 Endorsement 策略，最后将区块追加到区块链上。

![Fabric交易流程图](/Users/wangruidong/workSpace/lib_wang/knowledge-base/pic/Fabric交易流程图.webp)


### 1.2 作用
系统链码 VSCC 的作用包括，背书策略为 VSCC 提供2、3的准则。

1. 验证 Endorser 节点对交易的签名
2. 验证是否有足够数量的节点对交易背书
3. 背书信息来自指定源
## 2 用法
### 2.1 实体的定义
**实体( Principal )**：一个实体由 MSP 与 ROLE 定义，即某个组织内的某个角色。其中ROLE支持四种 client, peer, admin, member。例如 Org0.admin 表示 Org0 这个 MSP 下的任意管理员; Org1.member 表示 Org1 这个 MSP 下的任意成员。

### 2.2 表达式
Endorsement Policy 的语法结构如下所示。EXPR 可以是是 AND 或者 OR 逻辑符。E 是实体或者嵌套的表达式。

```shell
// 基础表达式形式
EXPR(E[, E...])
// 需要三个实体都提供签名
AND('Org1.member', 'Org2.member', 'Org3.member')
// 需要两个实体任一提供签名
OR('Org1.member', 'Org2.member')
// 需要Org1的admin提供签名，或者Org2，Org3的member同时提供签名
OR('Org1.admin', AND('Org2.member', 'Org3.member'))
```

### 2.3 示例
#### 2.3.1 在 cli 命令行中使用

```shell
# 实例化
$ peer chaincode instantiate -C <channelid> -n mycc -P "AND('Org1.member', 'Org2.member')"
# 升级
$ peer chaincode upgrade -o orderer.example.com:7050 --tls $CORE_PEER_TLS_ENABLED --cafile $ORDERER_CA -C $CHANNEL_NAME -n mycc -v 2.0 -c '{"Args":["init","a","90","b","210"]}' -P "OR ('Org1MSP.peer','Org2MSP.peer','Org3MSP.peer')"
```

#### 2.3.2 在 SDK 使用

Fabric 为SDK提供了另一套独立的简单语法来指定背书策略。
Node中可以创建如下对象，示例策略对应命令行的：

>AND('ordererOrg.admin', OR(peerOrg1.member, peerOrg2.member))

```json
{
  identities: [
    // 以下几项自动编号为[0,1,2]
    { role: { name: "member", mspId: "peerOrg1" }},
    { role: { name: "member", mspId: "peerOrg2" }},
    { role: { name: "admin", mspId: "ordererOrg" }}
  ],
  policy: {
    // n-of 指定需要组内多少个进行签名， 1-of 等价于 OR, max-of 等价于AND，此处2与后面的组相同，因此是AND
    "2-of": [
      // 对应编号2的身份
      { "signed-by": 2},
      // 嵌套
      { "1-of": [{ "signed-by": 0 }, { "signed-by": 1 }]}
    ]
  }
}
```

Java中创建如下 .yaml 文件，并调用 ChaincodeEndorsementPolicy.fromYamlFile() 进行解析即可。示例策略对应命令行的 ：

> OR(OR('Org1MSP.member', 'Org1MSP.admin'), OR('Org2MSP.member', 'Org2MSP.admin'))

```shell
# 指定策略中会用到的角色
identities: 
    # Org1MSP 中的 member
    user1: {"role": {"name": "member", "mspId": "Org1MSP"}} 
    # Org2MSP 中的 member
    user2: {"role": {"name": "member", "mspId": "Org2MSP"}}
    # Org1MSP 中的 admin
    admin1: {"role": {"name": "admin", "mspId": "Org1MSP"}}
    # Org2MSP 中的 admin
    admin2: {"role": {"name": "admin", "mspId": "Org2MSP"}}

policy:
    # n-of 指定需要组内多少个进行签名， 1-of 等价于 OR, max-of 等价于AND
    1-of:
      # 嵌套
      - 1-of:
        # user1 即上面角色中的 user1
        - signed-by: "user1"
        - signed-by: "admin1"
      - 1-of:
        - signed-by: "user2"
        - signed-by: "admin2"
```

## 3. 注意点

### 3.1 默认策略
Chaincode需要在部署时指定 Endorsement Policy ，否则默认的背书策略为通道中组织的任意成员( “any member of the organizations in the channel”)。例如，通道中有两个组织 Org1, Org2 ，则默认策略为 OR('Org1.member', 'Org2.member')。

### 3.2 新增实体
对于 Chaincode 实例化后才加入的组织，其节点可以进行 Chaincode 查询操作，但是不能对背书交易进行提交，此时需要修改 Chaincode 的背书策略以增加该组织相关节点的权限。我们可以通过 Upgrade Chaincode 来达到为其指定新的背书策略的目标。

### 3.3 交易验证失败
如1.1所述，Hyperledger Fabric 区块链网络的交易执行到第3步时， Orderer 节点已经出块，交易包含在区块中。若此处仅仅是区块内部的交易验证失败，不会影响区块上链，只是对于该交易会标记一个交易失败的验证码（ValidationCode: "0" 表示交易成功，"非0"表示交易无效），对于背书策略不符的情况，验证码为 ENDORSEMENT_POLICY_FAILURE(10)。

