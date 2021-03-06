## 什么是区块链

### 定义

区块链技术自身仍然在飞速发展中，目前还缺乏统一的规范和标准。

[wikipedia](https://en.wikipedia.org/wiki/Block_chain_(database) 给出的定义为：

> A blockchain —originally, block chain —is a distributed database that maintains a continuously-growing list of data records hardened against tampering and revision. It consists of data structure blocks—which hold exclusively data in initial blockchain implementations, and both data and programs in some of the more recent implementations—with each block holding batches of individual transactions and the results of any blockchain executables. Each block contains a timestamp and information linking it to a previous block.

最早区块链技术出现在比特币项目。作为比特币背后的分布式记账平台，区块链在无集中式监管的情况下，稳定运行了近八年时间，支持了海量的交易记录，并未出现严重的漏洞。

*注：比特币历史上唯一已知的漏洞事件曾导致比特币的恶意增发，但问题很快被发现并修正，相关非法交易被撤销。*

公认的最早关于区块链的描述性文献是中本聪所撰写的 [比特币：一种点对点的电子现金系统](https://bitcoin.org/bitcoin.pdf)，但该文献重点在于讨论比特币系统，实际上并没有明确提出区块链的定义和概念。在其中，区块链被描述用于记录比特币交易的账目历史。

![古老的账本](_images/ledger.jpg)

记账技术历史悠久，[现代复式记账系统](https://zh.wikipedia.org/wiki/%E5%A4%8D%E5%BC%8F%E7%B0%BF%E8%AE%B0)（Double Entry Bookkeeping）是由意大利数学家卢卡·帕西奥利，1494 年在《Summa de arithmetica, geometrica, proportioni et proportionalità》一书中最早制定。复式记账法将对账验证功能引入记账过程，提升了记账的可靠性。从这个角度来看，**区块链是首个自带对账功能的数字记账技术实现**。

更广泛意义地看，区块链属于一种分布式的记录技术。参与到系统上的节点，能获取到历史记录信息；区块链数据由所有节点共同维护，每个参与维护节点都能复制获得一份完整记录的拷贝。

跟传统的数据库技术相比，其特点应该包括：

* 维护一条不断增长的链，只可能添加记录，而发生过的记录都不可篡改；
* 去中心化，或者说多中心化，无集中的控制，实现上尽量分布式；
* 可以通过密码学的机制来确保交易无法抵赖和破坏，并尽量保护用户信息和记录的隐私性。

更进一步的，还可以将智能合约跟区块链结合到一起，让其提供除了交易功能外更灵活的合约功能，执行更为复杂的操作（实际上，比特币区块链已经支持简单的脚本计算）。这样扩展之后的区块链，已经超越了单纯数据记录的功能了，实际上带有点“普适计算”的意味了。

从技术特点上，可以看到现在区块链技术的三种典型应用场景：

| 定位 | 功能 | 智能合约 | 一致性 | 权限 | 类型 | 性能 | 代表 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 公信的数字货币 | 记账功能 | 不带有或较弱 | PoW | 无 | 公有链 | 较低 | 比特币 |
| 公信的交易处理 | 智能合约 | 图灵完备 | PoW、PoS | 无 | 公有链 | 受限 | 以太坊 |
| 带权限的交易处理 | 商业处理 | 多种语言，图灵完备 | 多种，可插拔 | 支持 | 联盟链 | 可扩展 | Hyperledger |

### 基本原理

区块链的基本原理理解起来并不难。基本组件包括：

* 交易：对账本状态的改变；
* 区块：记录交易和状态，是对当前账本状态的一次确认；
* 链：状态变化的日志记录。

把区块链作为一个状态机，则每次交易就是试图改变一次状态，每次生成区块就是参与者对于其中包括的所有交易改变状态的结果确认。

![区块链示例](_images/simpleBlockchain.png)

在实现上，首先假设存在一个分布式的数据记录池（这方面的技术相对成熟），这个池子只允许添加、不允许删除，其结构是一个线性的链表，由一个个“区块”串联组成，这也是其名字“区块链”的来源。新的数据要加入，必须作为一个新的区块来加入。而这个块是否合法，可以通过一些手段快速检验出来。

那么比特币网络如何使用了区块链技术？比特币网络将每 10 分钟内所有的交易记录都打包在一起（此外还要包括当前区块的哈希值等），这些信息组成一个块。然后，网络中所有的成员都可以试图来找到一个 nonce 串放到区块里，之后将所有区块里信息进行 hash 计算，让 hash 结果满足一定条件（比如小于某个值）。一旦算出来就可以进行全网广播，大家拿到这个算出来的结果，进行验证，发现确实符合约定条件了，就承认这个区块是一个合法的新区块，新的区块被添加到链上。

因为算出来的概率要从数学上进行保证，比如每 10 分钟平均算出来一个。所以保证了区块链每 10 分钟增加一个块。算出来的这个人将得到放进块里的所有交易的管理费和协议固定发放的奖励费（目前是 12.5 比特币，每四年减半）。也即俗称的挖矿。

很自然会有人问，能否进行恶意操作来破坏整个区块链系统或者获取非法利益。比如不承认别人的结果，拒绝别人的交易等。实际上，因为系统中存在大量的用户，而且用户默认都只承认他看到的最长的链。只要不超过一半（概率意义上越少肯定越难）的用户协商，最终最长的链将很大概率上是合法的链，而且随着时间增加，这个概率会越大。例如，经过 6 个块后，即便有一半的节点联合起来想颠覆被确认的结果，其概率将为 $$\frac{1}{2}^6 = 1.6\%$$，即低于 $$\frac{1}{60}$$ 的可能性。

*注：熟悉 [Git](https://git-scm.com) 的人，应该会赞叹两者在设计上的异曲同工之妙。*

### 分类

根据参与者的不同，可以分为公开（Public）链、联盟（Consortium）链和私有（Private）链。

公开链，顾名思义，任何人都可以参与使用和维护，典型的如比特币区块链，信息是完全公开的。

如果引入许可机制，包括私有链和联盟链两种。

私有链，则是集中管理者进行限制，只能得到内部少数人可以使用，信息不公开。

联盟链则介于两者之间，由若干组织一起合作维护一条区块链，该区块链的使用必须是有权限的管理，相关信息会得到保护，典型如银联组织。

目前来看，公开链将会更多的吸引社区和媒体的眼球，但更多的商业价值应该在联盟链和私有链上。

根据使用目的和场景的不同，又可以分为以数字货币为目的的货币链，以记录产权为目的的产权链，以众筹为目的的众筹链等。

