北京大学肖臻老师《区块链技术与应用》笔记 - ETH篇

### 1.0 ETH概述篇

BTC和ETH为最主要的两种加密货币，BTC称为区块链1.0，以太坊称为区块链2.0。之前文章中提出了比特币设计中存在某些不足，以太坊便对其进行了改进。例如：出块时间、共识协议、mining puzzle（对内存要求高，反ASIC芯片使用）

未来，以太坊还将会用权益证明(POS)替代工作量证明(POW)。
此外，以太坊增加了对**智能合约（smart contract）**的支持。

#### 1.1 为什么要开发“智能合约”

BTC本身是一个去中心化的货币，在比特币取得成功之后，很多人就开始思考：除了货币可以去中心化，还有什么可以去中心化？以太坊的一个特性就是增加了对去中心化的合约的支持。
如果说比特币系统本身是一个货币应用，以太坊则由于智能合约，升级成为了一个平台，用户可以依据该平台自行开发业务应用。

#### 1.2 关于BTC和ETH

BTC的发明人为中本聪(疑似日本人)，ETH为Vitalik Buterin受到BTC启发发明出来的““下一代加密货币与去中心化应用平台””。BTC中货币最小单位为“聪”，最少的钱为一聪；ETH中货币最小单位为“Wei”，最少的钱为一Wei。

#### 1.3 去中心化的合约

首先，讨论去中心化货币。货币本身由政府发行，政府公信力为其背书，BTC通过技术手段取代了政府的职能。

现实生活中，我们经常提到“契约”或“合约”。合约的有效性也是需要政府进行维护的，如果产生纠纷需要针对合法性合同进行判决。ETH的设计目的就是，通过技术手段来实现取代政府对于合约的职能。当然不是所有的合约都可以量化，可以对较为逻辑性强、量化的合同进行智能合约化。

那么，去中心化的合约有什么好处？

首先，去中心化的货币有什么好处？比如跨国转账，用传统的方式就很麻烦，用加密货币则方便很多。那同样对于合约来说，若合同签署方并非一个国家，来自世界各个国家，没有统一的司法部门（如：面向世界的众筹），若出现合同纠纷打起官司会很麻烦。故用技术手段编写无法修改的合约，所有人只能按照相关参与方执行，无法违约。



### 2.0 ETH账户篇

BTC系统是基于交易的账本，系统中并未显示记录账户有多少钱，只能通过UTXO进行推算。这种方式的好处是隐私保护比较好，你有多少钱，自己可能都说不清，别人就更不知道了。但实际中，使用起来较为别扭。

A转给B钱的时候，需要说明币的来源。实际中只需要存钱说明来源，花钱则不用。此外，账户中的钱在花的时候，必须一次性全部花出去。

如图1，B收到A的10个BTC，他想要给C3个BTC，如果按照1中方式，其余7个比特币会以交易费的形式给挖出区块的矿工。

因此，为了避免这种情况，所以采用2中方式，将3个BTC转给C，将剩余7个BTC转到B的另一账户D上面。很多比特币钱包就采用这种方式，每次转账就生成一个自己的新的地址，有利于隐私保护。
[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200224131750105.png)](https://img-blog.csdnimg.cn/20200224131750105.png)

以太坊系统则采用了基于账户的模型，与现实中银行账户相似。系统中显示记录每个账户以太币的数量，转账是否合法只需要查看转账者账户中以太币是否足够即可，同时也不需要每次全部转账。同时，这也也天然地防范了双花攻击。因为花钱的时候不需要像比特币一样说明币的来源，如果你花两次就扣两次钱就好了。

当然，以太坊发这种模式也存在缺点，这种模式存在**重放攻击**（reply attack）的缺陷。A向B转账，过一段时间，B将A的交易重新发布，从而导致A账户被扣钱两次。双花攻击和重放攻击是对称的，双花攻击说的是花钱的人不诚实，花过的钱继续花一次，重放攻击说的是收钱的人不诚实，收到过的钱再继续收一次。在比特币中会有重放攻击吗，不会，因为收到过的交易信息再广播一次是很显然的double spending。

> 为了防范重放攻击，给账户交易添加计数器记录该账户有史以来一共发布过多少个交易，转账时候将转账次数（nonce）成为交易内容的一部分，一起包含进去都是受到发布交易者的签名保护的。
> 系统中全节点维护账户余额和该计数器的交易数，从而防止本地篡改余额或进行重放攻击。如果要进行重放攻击，B把A的第221笔交易再广播出去，那全节点对比维护的nonce值发现221笔交易已经完成过了，就不会再执行这笔交易。

以太坊系统中存在两类账户：**外部账户**和**合约账户**。

1. 外部账户：类似于BTC系统中公私钥对。掌握某个私钥就拥有账户控制权。存在账户余额balance和计数器nonce。
2. 合约账户：并非通过公私钥对控制。(不能主动发起交易，只能接收到外部账户调用后才能发起交易或调用其他合约账户)其除了balance和nonce之外还有code(代码)、storage(相关状态-存储)、每个变量的取值。

创建合约时候会返回一个地址，就可以对其调用。调用过程中，代码不变但状态会发生改变。

**为什么要做以太坊，更换为基于账户的模型而不是沿袭BTC系统？**

比特币基于交易的模型隐私保护比较好，每次交易支持更换账户，但以太坊是为了支持智能合约，对于合约来说，要求参与方的身份较为稳定。比如，智能合约用于金融衍生品期货，那对于账户来说，不能身份突然变掉了吧，对于合约账户来说，如果地址变了，那投入到原来合约的钱也就找没用了。



### 3.0 ETH数据结构篇

> 在以太坊中，有**三棵树**的说法，分别是状态树、收据树和交易树。了解了这三棵树，就弄清楚了以太坊的基础数据结构设计。
> 而以太坊实现的是一个"平台性"的应用，其复杂性必然较高。因此，其内部数据结构设计也存在一定复杂度。对此，ETH数据结构篇将花费较多篇幅进行编写。

#### 3.1 引入

首先，我们要实现从账户地址到账户状态的映射。在以太坊中，账户地址为160位，表示为40个16进制数。状态包含了余额(balance)、交易次数(nonce)，合约账户中还包含了code(代码)、存储(stroge)。

- 直观地来看，其本质上为Key-value键值对，所以直观想法便用哈希表实现。若不考虑哈希碰撞，查询直接为常数级别的查询效率。
  但采用哈希表，难以提供Merkle proof。Merkel proof用来证明tree里面有什么，使用Merkle Tree的另一个目的是使各个节点的状态一致。

#### 3.2 思考如何组织账户的数据结构

1. 我们能否像BTC中，将哈希表的内容组织为Merkle Tree？这里我个人的理解是，把哈希表中的键值对就像交易的内容一样放在叶子节点。
   比如你和一个人要签合同，希望他能证明一下他有多少钱，证明账户余额。一种方法是把哈希表组成一个Merkle tree， 算出一个根哈希值，放在区块头中公布出去，根哈希值是正确的就能保证底下的树没用被篡改。但当新区块发布，哈希表内容会改变，再次将所有的账户状态组织为新的Merkle Tree？如果这样，每当产生新区块(ETH中新区块产生时间为10s左右)，都要重新组织Merkle Tree，注意是对账户余额的状态做一个Merkle Tree，账户可以是无限的，所以这不现实。
   
   那为什么比特币系统就直接用了Merkle Tree？比特币系统中是对一个区块中的交易内容进行MT，不是整个账户的余额，区块包含上限为4000个交易左右，所以Merkle Tree不是无限增大的。并且每个区块里的交易信息大多数与上一个区块不一样，所以每个区块建立一个Merkel Tree是正常的，但是在ETH中，账户余额发生变化的仅仅为很少一部分数据，我们每次重新构建Merkle Tree代价很大。
   
2. 那我们不要哈希表了，直接使用Merkle Tree，每次修改只需要修改其中一部分即可，这个可以吗？这里我个人的理解是，把一个账户地址和其所对应的状态直接放在叶子节点上。
   但是，Merkle Tree并未提供一个高效的查找和更新的方案。此外，将所有账户构建为一个大的Merkle Tree，必须进行排序（Sorted Merkle Tree），因为账户如果散乱放在叶节点上，那构建的Merkle Tree不唯一，那不利于全节点的状态一致性。但是BTC中为什么不用排序，因为每个挖矿的节点打包交易的顺序确实不一样，但是大家最终会同步获得记账权的节点的块信息，当然就不需要排序。如果以太坊也像比特币一样把账户的状态每次在本地组装成一个Merkle Tree放在区块中然后再发布出去，首先这样的Merkle Tree的占用量很大，其次每次就修改很少的一部分账户余额，但还要把所有的账户状态建立树再打包没必要。

3. 那么经过排序，使用Sorted Merkle Tree可以吗？
   新增账户，由于其地址随机，插入Merkle Tree时候很大可能在Tree中间，发现其必须进行重构。所以Sorted Merkle Tree插入、删除(实际上可以不删除)的代价太大。

既然哈希表和 Merkle Tree都不可以，那么我们看一下实际中以太坊采取的数据结构：MPT。

> 注意：BTC系统中，虽然每个节点构建的Merkle Tree不一致（不排序），但最终是获得记账权的节点的Merkle Tree才是有效的。

#### 3.3 一个简单的数据结构——trie(字典树、前缀树)

如下为一个通过5个单词组成的trie数据结构（只画出key，未画出value）

![image-20220321150307585](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220321150307585.png)

特点：

1. trie中每个节点的最大分支数目取决于Key值中每个元素的取值范围(图例中最多26个英文字母分叉+一个结束标志位)。在以太坊中地址表示成40位16进制，16再加上一个结束标志位，一共17个分支。
2. trie查找效率取决于key的长度，键值越长查找访问的内存次数就要更多。在以太坊中，地址长度为40个16进制的数，key值固定是40。（以太坊的地址是公钥取哈希截取后面一半，得到160位的地址。）
3. 理论上使用哈希表可能会出现哈希碰撞，意思是两个地址可能不一样但映射到了同一个地方，而trie上面不会发生碰撞。
4. 给定输入，无论如何顺序插入，构造的trie都是一样的。
5. 更新操作局部性较好。

那么trie有缺点吗？
它的缺点是存储浪费。很多节点只存储一个key，但其“儿子”只有一个，出现一脉单传的情况，过于浪费，对于一些节点需要进行合并。因此，为了解决这一问题，我们引入**Patricia tree/trie**。

#### 3.4 Patricia trie(Patricia tree)

Patricia trie就是进行了路径压缩的trie。如上图例子，进行路径压缩后如下图所示：

![image-20220321151447430](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220321151447430.png)

需要注意的是，如果新插入单词，原本压缩的路径可能需要扩展开来。那么，需要考虑什么情况下路径压缩效果较好？树中插入的键值分布较为稀疏的情况下，可见路径压缩效果较好。比如有几个单词很长的时候。

![image-20220321151903374](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220321151903374.png)

那在以太坊系统中，路径是否稀疏呢？160位的地址存在2^160 种，该数实际上已经非常大了，和账户数目相比，可以认为地址这一键值非常稀疏。为什么要这么稀疏呢？防止账户碰撞（比地球爆炸的概率还要小）。
**因此，我们可以在以太坊账户管理种使用Patricia tree这一数据结构！但实际上，在以太坊种使用的并非简单的PT(Patricia tree),而是MPT(Merkle Patricia tree)。** 把普通指针换成哈希指针。

#### 3.5 Merkle Tree 和 Binary Tree

区块链和链表的区别在于区块链使用哈希指针，链表使用普通指针。
同样，Merkle Tree 相比 Binary Tree，也是普通指针换成了哈希指针。

所以，以太坊系统中可如此，将所有账户组织为一个经过路径压缩和排序的Merkle Tree，其根哈希值存储于block header中。

> BTC系统中只有一个交易组成的Merkle Tree，而以太坊中有三个(三棵树)。
> 也就是说，在以太坊的block header中，存在有三个根哈希值。

**根哈希值的用处：**

1. 防止篡改。
2. 提供Merkle proof，可以证明账户余额，轻节点可以进行验证。
3. 证明某个账户是否存在，例如你想向这个账户转一笔钱，想先知道这个账户是否在全节点中存在。如果存在，就把这个分支作为Merkle Proof发过去。

以太坊用的不是原生版的MPT（Merkle Patricia tree）用的是modified MPT。

#### 3.6 Modified MPT (Modified Merkle Patricia tree)

下图为以太坊中使用的MPT结构示意图。右上角表示四个账户(为了直观，显示较少)和其状态(只显示账户余额)。（需要注意这里的指针都是哈希指针）

![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200225193700264.png)

Extension Node: 如果这个节点出现了路径压缩就会是Extension Node。

nibble:十六进制数。

每次发布新区块，状态树中部分节点状态会改变。但改变并非在原地修改，而是新建一些分支，保留原本状态。如下图中，仅仅有新发生改变的节点才需要修改，其他未修改节点直接指向前一个区块中的对应节点。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200225193719146.png)

所以，系统中全节点并非维护一棵MPT，而是每次发布新区块都要新建MPT。只不过大部分节点共享，只有少数发生变化的节点要新建分支。

> 为什么要保存原本状态？为何不直接修改？
> 为了便于回滚（roll back）。如下1中产生分叉，而后上面节点胜出，变为2中状态。那么，下面节点中状态的修改便需要进行回滚。因此，需要维护这些历史记录。不想比特币简单脚本，如果要发生回滚只需要对账户重新加减，但以太坊中有智能合约，无法轻易的对代码的结果产生回滚。
> [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200225193738472.png)](https://img-blog.csdnimg.cn/20200225193738472.png)

#### 3.7 通过代码看以太坊中的数据结构

1. block header中的数据结构

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200225193754852.png)](https://img-blog.csdnimg.cn/20200225193754852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

ParentHash:前一个区块的块头的哈希值。

Root：状态树的根哈希值。

MixDigest：是根据Nonce经过哈希运算算出来的一个值

2. 区块结构

   [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200225193824148.png)](https://img-blog.csdnimg.cn/20200225193824148.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

   

3. 区块在网上真正发布时的信息

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200225193837431.png)](https://img-blog.csdnimg.cn/20200225193837431.png)

> **最后说明**
> 状态树中保存Key-value对，key就是地址，而value状态通过RLP(Recursive Length Prefix，一种进行序列化的方法)编码序列号之后再进行存储。

### 4.0交易树和收据树

每次发布一个区块时，区块中的交易会形成一颗Merkle Tree，即交易树。此外，以太坊还添加了一个收据树，每个交易执行完之后形成一个收据，记录交易相关信息。也就是说，交易树和收据树上的节点是一一对应的。
由于以太坊智能合约执行较为复杂，通过增加收据树，便于快速查询执行结果。

交易树和收据树都是MPT(Merkle Patricia tree)，而BTC中都采用普通的MT(Merkle Tree)。可能就仅仅是为了三棵树代码比较统一，便于管理，不一定非要有其他的原因。

MPT的好处是支持查找操作，通过键值沿着树进行查找即可。对于状态树，查找键值为账户地址；对于交易树和收据树，查找键值为交易在发布的区块中的序号，交易的排列顺序是由发布交易的那个节点决定的。

交易树和收据树只将当前区块中的交易组织起来，而状态树将所有账户的状态都包含进去，无论这些账户是否与当前区块中交易有关系。多个区块的状态树是共享节点，只有改变状态的那个节点需要新建分支，而交易树和收据树依照区块独立。

交易树和收据树的用途：

1. 向轻节点提供Merkle Proof。
2. 更加复杂的查找操作(例如：查找过去十天所有跟某个智能合约相关的交易；过去十天的众筹事件等)

#### Bloom filter(布隆过滤器)

支持较为高效查找某个元素是否在某个集合中。
最笨的方法：元素遍历，复杂度为O(n)，而且需要存储整个交易列表——轻节点不能用
改进的方法：给一个大的集合，计算出一个紧凑的“摘要”。

> 例：如下图，给定一个数据集，其中含义元素a、b、c，通过一个哈希函数H()对其进行计算，将其映射到一个其初始全为0的128位的向量的某个位置，将该位置置为1。将所有元素处理完，就可以得到一个向量，则称该向量为原集合的“摘要”。可见该“摘要”比原集合是要小很多的。
> 假定想要查询一个元素d是否在集合中，假设H(d)映射到向量中的位置处为0，说明d一定不在集合中；假设H(d)映射到向量中的位置处为1，有可能集合中确实有d，也有可能因为哈希碰撞产生误报。
> [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/2020022718395131.png)](https://img-blog.csdnimg.cn/2020022718395131.png)

Bloom filter特点：有可能出现误报，但不会出现漏报。
Bloom filter变种：不是采用一个哈希函数，是采用一组哈希函数进行向量映射，如果出现哈希碰撞，不会所有的哈希函数都出现碰撞。

如果集合中删除元素该怎么操作？
无法操作。也就是说，简单的Bloom filter不支持删除操作。如果想要支持删除操作，需要将记录数不能为0和1，需要修改为一个计数器(需要考虑计数器是否会溢出)。就变复杂了，与设计初衷相违背。

#### 4.1 以太坊中的Bloom filter的作用

每个交易完成后会产生一个收据，收据包含一个Bloom filter来记录交易类型、地址等信息。在区块block header中也包含一个总的Bloom filter，其为该区块中所有交易的Bloom filter的一个并集。

那如何查过去十天与这个智能合约相关的所有交易呢？

先查一下哪个区块的块头中有我需要的交易的Bloom filter，如果块头中的Bloom filter中有的话，再去查找这个区块里面包含的交易，所对应的收据树里面的每个收据的Bloom filter，也可能都没有，则发生了误报；如果有的话，我们再找到相对应的交易直接再进行确认。
好处是通过Bloom filter这样一个结构，快速大量过滤掉大量无关区块，从而提高了查找效率。

#### 4.2 补充

以太坊的运行过程，可以视为**交易驱动的状态机**，通过执行当前区块中包含的交易，驱动系统从当前状态转移到下一状态。当然，BTC我们也可以视为**交易驱动的状态机**，其状态为UTXO。对于给定的当前状态和给定一组交易，可以确定性的转移到下一状态(保证系统一致性)。

问题1：A转账到B，有没有可能收款账户不包含再状态树中？
可能。因为以太坊中账户可以节点自己产生，只有在产生交易时才会被系统知道。
问题2：可否将每个区块中状态树更改为只包含和区块中交易相关的账户状态？(大幅削减状态树大小，且和交易树、收据树保持一致)
不能。这样设计要查找账户状态很不方便，如果A要转账10个ETH，那系统首先要查A有没有10个ETH，就需要一直往前找，找到最近的一个包含A的区块才能转账。如果要向一个新创建账户转账，因为需要知道收款账户的状态，才能给其添加金额，但由于其是新创建的账户，所有需要一直找到创世纪块才能知道该账户为新建账户。

#### 4.3 从代码中看具体的数据结构

- 交易树和收据树的创建过程

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200227184002796.png)

DeriveSha 来得到交易树和收据树的根哈希值。

CalcUncleHash 得到哈希值。

![image-20220322131737732](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322131737732.png)

derive_sha.go中，DeriveSha函数把Transactions和Receipts建为trie。

![image-20220322131932068](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322131932068.png)

而trie的数据结构是MPT

![image-20220322132018634](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322132018634.png)

这是Receipt的数据结构，每个交易执行完成后形成一个收据，记录了这个交易的执行结果。

这个Bloom域就是指Bloom filter，log域是个数组，每个收据可以包含多个log。这些收据的Bloom filter就是根据这些log产生出来的。

![image-20220322132451939](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322132451939.png)

这是区块块头的数据结构。这里的Bloom就是每个区块的Bloom filter，是由每个区块中所有收据的Bloom filter合并出来的。

![image-20220322132642304](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322132642304.png)

再来看第一张图，CreateBloom函数用来创建BlockHeader中的Bloom域，这个Bloom Filter由这个块中所有receipts的Bloom Filter组合得到。

![image-20220322132819019](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322132819019.png)

CreateBloom函数的参数是这个区块的所有收据，for循环是遍历所有的收据，并对每个收据调用LogsBloom函数获得这个收据的Bloom filter，用Or操作合并起来。得到整个区块的Bloom filter。

![image-20220322133205175](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322133205175.png)

LogsBloom的作用是生成每个收据的Bloom filter，参数是这个收据的Logs数组，这个函数有两层for循环，外层for循环对Logs数组中每个log进行处理，首先对这个log的地址取哈希后加入到Bloom filter里面，这里的bloom9是Bloom filter用的哈希函数。然后内层循环把这个log中包含的每个Topics加入到Bloom filter里面，这样就得到了这个收据的Bloom filter。

![image-20220322133844974](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322133844974.png)

bloom9是Bloom filter中使用的哈希函数，这里的bloom9函数是把输入映射到digest的三个位置，也就是说把三个位置都置为1，b为32个字节的哈希值，循环中把32个字节的哈希值取前6个字节，每两个字节组成一组，&2047意味着对2048取余。因为以太坊中Bloom filter的长度为2048位。最后一行，把1左移这么多位，通过Or运算，合并到上一轮得到的bloom filter里面，这样经过3轮循环把3个位置置为1后返回所创建的bloom filter。

![image-20220322135238601](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220322135238601.png)

BloomLookup为查询bloom filter里是否包含了我们感兴趣的topic，先使用Bloom9函数把topic转化为big.Int，然后把它和bloom filter取and操作，因为bloom filter一定会包含其他topic的Bloom filter，取and就是把这个cmp“提取出来”，再与自身的big.Int比较是否相等。

### 5.0 ETH中GHOST协议篇

BTC系统中出块时间为10min，而以太坊中出块时间被降低到15s左右，虽然有效提高了系统反应时间和吞吐率，却也导致系统临时性分叉变成常态，且分叉数目更多。这对于共识协议来说，就存在很大挑战。在BTC系统中，不在最长合法链上的节点最后都是作废的，但如果在以太坊系统中，如果这样处理，由于系统中经常性会出现分叉，则矿工挖到矿很大可能会被废弃，这会大大降低矿工挖矿积极性。而对于个人矿工来说，和大型矿池相比更是存在天然劣势。正常来说，挖矿的收益应该是个体在整体算力所占的比例是多少，收益就是多少。当分叉出现的时候，大矿池一定会按照自己原先挖的区块上继续往下挖，而同时一个个体矿工挖到的是分叉的区块，那别人也会去沿着大矿池挖到的块挖，因为这个块成为最长合法链的概率是最高的。并且大型矿池的网络比较好，别的矿工更容易收到它挖的块。故个体矿工非常占劣势，由此可见挖矿时间也不是说越短就越好。

对此，以太坊设计了新的公式协议——`GHOST协议`(该协议并非原创，而是对原本就有的Ghost协议进行了改进)。

#### 5.1 GHOST协议

**GHOST协议最初版本**

如图，假定以太坊系统存在以下情况，A、B、C、D在四个分支上，最后，随着时间推移B所在链成为最长合法链，因此A、C、D区块都作废，但为了补偿这些区块所属矿工所作的工作，给这些区块一些“补偿”，并称其为"Uncle Block"（叔父区块）。
规定E区块在发布时可以将A、C、D叔父区块包含进来，A、C、D叔父区块可以得到出块奖励的7/8，而为了激励E包含叔父区块，规定E每包含一个叔父区块可以额外得到1/32的出块奖励。为了防止E大量包含叔父区块，规定一个区块只能最多包含两个叔父区块，因此E在A、C、D中最多只能包含两个区块作为自己的出块奖励。
[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228133558197.png)](https://img-blog.csdnimg.cn/20200228133558197.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

> 假定一个矿工挖出了B，此时他沿着其所在链继续挖，在挖E的时候，则可以将A包含进区块挖矿，若挖矿过程中又听到C也是“叔辈”，则可以停止挖矿，将C包含进来重新组织成一个新区块重新挖矿，实际中，由于挖矿过程的`无记忆性`，这样并不会降低成功挖到矿的概率。

**最初版本缺陷：**

1. 因为叔父区块最多只能包含两个，如图出现3个怎么办？或者自己的区块E已经发布完了，然后才收到叔父区块，已经来不及了，那这个新听到的区块（A或者C或者D）又变成了丢弃的区块，怎么解决？
2. 矿工出于商业利益，故意不包含叔父区块，导致叔父区块（对手）可以得到的7/8出块奖励没了，而自己仅仅损失1/32。如果甲、乙两个大型矿池存在竞争关系，那么他们可以采用故意不包含对方的叔父区块，因为这样对自己损失小而对对方损失大。

**Ghost协议新的版本**

如下图中1为对上面例子的补充，F为E后面一个新的区块。因为规定E最多只能包含两个叔父区块，所以假定E包含了C和D。此时，F也可以将A认为自己的的叔父区块(实际上并非叔父辈的，而是爷爷辈的)。如果继续往下挖，F后的新区块仍然可以包含B同辈的区块(假定E、F未包含完)。这样，就有效地解决了上面提到的最初Ghost协议版本存在的缺陷。因为如果你不包含上一辈的叔父区块，那下一个区块不一定是你挖出来的了，别人就会去包含你刚没包含的那个区块。损人不利己。
[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228133625207.png)](https://img-blog.csdnimg.cn/20200228133625207.png)
但这样仍然存在一定的问题。

我们将“叔父”这个概念进行扩展，但问题在于，**“叔父”这一定义隔多少代才好呢**？如果隔个好几千代，那就在很久之前在挖矿难度低的时候，发布很多叔父区块，期待被包含。
如下图所示，M为该区块链上的一个区块，F为其严格意义上的叔父，（图别看叉了，M和F只差了一辈，把下面一行往右移动些更好），E为其严格意义上的“爷爷辈”。以太坊中规定，如果M包含F辈区块，则F获得7/8出块奖励；如果M包含E辈区块，则F获得6/8出块奖励，以此类推向前。直到包含A辈区块，A获得**2/8**出块奖励，再往前的“叔父区块”，对于M来说就不再认可其为M的"叔父"了。
对于M来说，无论包含哪个辈分的“叔父”，得到的出块奖励都是1/32出块奖励。
也就是说，叔父区块的定义是和当前区块在七代之内有共同祖先才可（合法的叔父只有6辈）。
[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/2020022813374149.png)](https://img-blog.csdnimg.cn/2020022813374149.png)
这样，就方便了全节点进行记录，因为如果包含了隔好几千代的叔父区块，那它要维护的状态就太多了，你发布的区块包含着叔父区块，其他节点也是要验证的。此外，设计7代之内的逐级递减的奖励可以鼓励一旦出现分叉尽早进行合并。（但是M去合并无论上哪辈的区块得到的奖励都是1/32，不是逐级递减的，怎么就能鼓励尽早合并呢？我这里理解的是，A到F的分叉区块很有可能有和M是一个矿池的，M越早合并自己挖出来的分叉区块，奖励越多）

#### 5.2 以太坊中的奖励

BTC：静态奖励(出块奖励)+动态奖励(交易费，占据比例很小约占出块奖励的1%)。
ETH：静态奖励(出块奖励+包含叔父区块的奖励)+动态奖励(汽油费，占据比例很小，叔父区块得不到汽油费。)
BTC中为了人为制造稀缺性，比特币每隔一段时间出块奖励会降低，最终当出块奖励趋于0后会主要依赖于交易费运作。而以太坊中并没有人为规定每隔一段时间降低出块奖励。

以太坊中包含了叔父区块，要不要执行叔父区块中的交易？
不应该，叔父区块和主链上区块有可能包含有冲突的交易。而且我们前文也提到，叔父区块是没有动态奖励的。因此，一个节点在收到一个叔父区块的时候，只检查区块合法性（是否符合挖矿难度要求，只查header就行了。）而不检查其中交易的合法性。

当然，对于分叉后的堂哥区块（叔父的儿子）怎么办？例如下图所示，A->F该链并非一个最长合法链，所以B->F这些区块怎么办？该给挖矿补偿吗？
如果规定将下面整条链作为一个整体，给予出块奖励，这一定程度上鼓励了分叉攻击（降低了分叉攻击的成本，因为即使攻击失败也有奖励获得）。因此，ETH系统中规定，只认可A区块为叔父区块，给予其补偿，而其后的区块全部作废。
[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228133827874.png)](https://img-blog.csdnimg.cn/20200228133827874.png)

#### 5.3 以太坊真实数据

> [Etherscan网站](https://cn.etherscan.com/)，该网站可以实时观看以太坊的数据。以下截图为我于2020/2/28截的图，和肖老师视频中截图存在一定差异。但具体内容基本一致。

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/2020022813410842.png)](https://img-blog.csdnimg.cn/2020022813410842.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

这里是叔父区块的情况，第一列Block Height就是区块的序号，看#1，这个区块的序号比UncleNumber小了2个，隔了两代，奖励6/8的区块奖励。在例子中是3个ETH为出块奖励，3*6/8=2.25个ETH。其他也是类似。

![image-20220323145613093](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220323145613093.png)

左边：看Block Reward最后加了0.09375，说明这个区块引入了一个叔父区块，并且对应的叔父区块获得了2.25Ether的奖励，说明隔了两代。

右边：看Block Reward最后加了0.1875，说明这个区块引入了两个叔父区块，并且对应的叔父区块获得了4.875Ether(2.625+2.25)，包含了一个隔一代和一个隔两代的叔父区块。

![image-20220323150056747](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220323150056747.png)

### 6.0 ETH挖矿算法篇1

在之前的BTC篇中，介绍了比特币系统中使用的挖矿算法。挖矿这一过程，虽然并没有创造什么实际价值，但挖矿本身维持了比特币系统的稳定。总体来说，比特币系统中的挖矿算法较为成功，并未发现大的漏洞。
当然，比特币系统的挖矿算法也存在一定问题，其中最为突出的就是导致了挖矿设备的专业化，普通计算机用户难以参与进去，导致了挖矿中心化的局面产生，而这与“去中心化”这一理念相违背。
因此，在比特币之后包括以太坊在内的许多加密货币针对该缺陷进行改进，希图做到ASIC Resistance(抗拒ASIC专用矿机)。由于ASIC芯片相对普通计算机来说，算力强但访问内存性能弱，因此常用的方法为Memory Hard Mining Puzzle，即增加对内存访问的需求。

#### 6.1 LiteCoin（莱特币）

> 莱特币百度百科：[click here](https://baike.baidu.com/item/莱特币/4414009?fr=aladdin)
> 莱特币中国官网：[click here](https://litecoin.org/cn/)

莱特币曾一度成为市值仅次于比特币的第二大货币。其基本设计大体上和比特币一致，但针对挖矿算法进行了修改。
莱特币的puzzle基于Scrypt。Scrypt为一个对内存性能要求较高的哈希函数，之前多用于计算机安全密码学领域。

**莱特币挖矿算法基本思想**

1. 设置一个很大的数组，按照顺序填充伪随机数。

> 因为哈希函数的输出我们并不能提前预料，所以看上去就像是一大堆随机的数据，因此称其为“伪随机数”。实际上其实是不能用真正的随机数，用真正随机数的话也没法进行验证。

Seed为种子节点，通过Seed进行一些运算获得第一个数，之后每个数字都是通过前一个位置的值取哈希得到的。
可以看到，这样的数组中取值存在前后依赖关系。
[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228171425644.png)](https://img-blog.csdnimg.cn/20200228171425644.png)

2. 在需要求解Puzzle的时候，随机从数组中读一个数开始，每次读取位置与前一个数相关。例如：第一次，从A位置读取其中数据，根据A中数据计算获得下一次读取位置B，再读取B；第二次，从B位置读取其中数据，根据B中数据计算获得下一次读取位置C，再读取C；
[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228171439422.png)](https://img-blog.csdnimg.cn/20200228171439422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

**分析**

如果数组足够大，对于挖矿矿工来说，必须保存该数组以便查询，否则每次不仅计算位置，还要根据Seed计算要读取位置中的数据，才能查询到对应位置的数据。这对于矿工来说，计算复杂度大幅度上升。
当然，矿工可以选择只保存一部分数据，例如：只保存奇数位置数据，偶数位置需要时再根据前一个奇数位置数据计算即可，从而对内存空间大小减少了一半(计算复杂度提高一点，但内存减少一半)。

> 核心思想：不能仅仅进行运算，增加其对内存的访问，从而实现对ASIC芯片不友好。

这个想法有问题吗？看似蛮不错的，使得ASIC矿机挖矿变得不友好，但该方法对Puzzle验证并不是很友好。想要验证该Puzzle，也需要存储该数组，因此对于轻节点来说，并不友好(系统中绝大多数节点为轻节点)。
因此，莱特币真正应用来说，数组大小不敢设置太大。例如：对于计算机而言，1G毫无压力，而对于手机APP来说，1G占据空间就过大了。所以，实际中，莱特币系统设计的数组大小仅仅128K大小。起初莱特币发行时，不仅希望能够抗拒ASIC，还希望能抗拒GPU。但实际中，后来慢慢出现了GPU挖矿，再后来，ASIC芯片挖矿也出现了。实际应用中，莱特币的设计并未起到预期作用，也就是说，128k对于ASIC Resistance来说过小了。

> 莱特币的这一设计是好事还是坏事？
> 从其并未起到预期作用来看，当然是一件坏事，但换个角度来思考，早期通过宣传这一设计目标，有效吸引了大批矿工参与，解决了莱特币“能启动”问题，“能启动问题”就是刚开始挖矿的太少的话，系统会不安全。

此外，莱特币和比特币另一区别为出块时间，莱特币为2.5min，为比特币的1/4。除了这些不同外，这两种货币基本一样。

#### 6.2 以太坊

以太坊的理念与莱特币相同，都是Memory Hard Mining Puzzle，但具体设计上与莱特币不同。

**以太坊挖矿算法基本思想**

以太坊中，设计了两个数据集，一大一小。小的为16MB的cache，大的数据集为1G的dataset(DAG)。其关系为，1G的数据集是通过16MB数据集生成而来的。

> 思考为何要设计一大一小两个数据集？
> 为了便于进行验证，轻节点保存16MB的Cache进行验证即可，而矿工为了挖矿更快，减少重复计算则需要存储1GB大小的大数据集。

16MB的小Cache数据生成方式与莱特币中生成方式较为类似。

1. 数组的生成方式是通过Seed进行一些运算获得第一个数，之后每个数字都是通过前一个位置的值取哈希获得的。
2. (不同)：
   - 莱特币：直接从数组中按照伪随机顺序读取一些数据进行运算。
   - 以太坊：先生成一个更大的数组(注：以太坊中这两个数组大小并不固定，因为考虑到计算机内存不断增大，因此该两个数组需要定期增大)
     [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228171502473.png)](https://img-blog.csdnimg.cn/20200228171502473.png)
3. 大的DAG生成方式：
   大的数组中每个元素都是从小数组中按照伪随机顺序读取元素、计算位置、读取元素以此类推，方法同莱特币中相同。其实读取的位置是按照要填的大数组的位置“i”取哈希得到，如第一次读取A位置数据，对当前哈希值更新迭代算出下一次读取位置B，再进行哈希值更新迭代计算出C位置元素。如此来回迭代读取256次数，最终算出一个数作为DAG中第一个元素64字节，DAG中每个元素生成方式都依次类推。cache固定，DAG跟着也是固定的，也可以直接根据cache来计算想要得到DAG的某个位置的数。
   [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228171529478.png)](https://img-blog.csdnimg.cn/20200228171529478.png)

**分析**

轻节点只保存小的cache，验证时进行计算即可。但对于挖矿来说，如果这样则大部分算力都花费在了通过Cache计算DAG上面，因此，其必须保存大的数组DAG以便于更快挖矿。求puzzle时是不需要保存这个cache的，只需要保存DAG。

> 以太坊挖矿求解puzzle过程：
> 根据区块block header和其中的Nonce值计算一个初始哈希mix，根据其映射到某个初始位置A，读取A位置的数，和mix一起计算哈希更新mix，再读取A位置的下一个数，和mix一起计算哈希更新mix。根据mix读取下一个要读取的位置B。以此类推循环64次，共读取128个数。
> [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228171549400.png)](https://img-blog.csdnimg.cn/20200228171549400.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)
> 最后，计算出一个哈希值与挖矿难度目标阈值比较，若不符合就重新更换Nonce，重复以上操作直到最终计算哈希值符合难度要求或当前区块已经被挖出。

### 7.0 ETH挖矿算法篇2

####  7.1 伪代码理解以太坊挖矿算法

mkcache:根据一个seed，填充整个cache数组。

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172022692.png)](https://img-blog.csdnimg.cn/20200228172022692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)



calc_dataset_item：通过cache来生成大数据集中的第i个元素，基本思想是通过伪随机的顺序读取cache中的256个数，每次读取的位置是由上一个读取的数计算得到的。第一个要从cache读取的数据的位置由初次mix决定，而mix是由"i'决定的，“i”每次都是不同的，所以mix也一定不同。（不同的mix可能对应初次读取的cache数组下标一样的，但是第二个读取的数字下标mix也会参与，所以第一个读取的位置虽然可以相同，但后续的读取顺序是不同的。也就是mix决定了初始读取的位置也决定了读取顺序。）

get_int_from_item：从哈希值解析出位置下标，就是用当前算出来的哈希值mix求出下一个要读取的位置cache_index。

make_item：利用两个哈希值得到新的哈希值，是用cache中这个位置cache_index的数和当前的哈希值mix计算出下一个哈希值也叫mix。

[![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228172037463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)](https://img-blog.csdnimg.cn/20200228172037463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)



calc_dataset：填充完整个DAG数组，就是不断调用calc_dataset_item。

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172049350.png)](https://img-blog.csdnimg.cn/20200228172049350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)



hashimoto_full：矿工用来挖矿的函数，header是当前要生成的区块的块头，（挖矿只需要用到块头是因为这样轻节点只需要保存块头就可以验证这个区块是否符合挖矿难度。）nonce就是当前尝试的nonce值，full_size是大数据集中元素的个数，元素的个数每3万个区块会增加一次，增加原始大小的1/128，dataset就是前面生成的大数据集DAG。

挖矿过程：首先根据块头的信息和初始的nonce生成一个哈希值mix，然后要经过64轮循环，每次循环中首先根据当前哈希值mix值得到大数据中的元素下标，然后读取下标所指向的元素数据再和当前哈希值mix更新哈希值mix，与生成DAG类似使用了make_item函数。利用刚更新好的mix和数组下标，得到数组下标后一位的值和mix更新后的mix。循环64次结束后返回一个哈希值用来和目标阈值做比较。虽然每次循环中读取DAG的两个相邻位置的哈希值，但这两个哈希值的生成过程是独立的，每个都是由16M的cache中256个数生成的，而且这256个数读取顺序是按照伪随机的顺序产生的，没什么联系。

hashimoto_light是轻节点用来验证的函数，header是收到矿工挖到区块的块头，nonce是包含在这个块头里的nonce，而full_size仍然是大数据集中的元素个数，cache是用来验证用的16M的cache。calc_dataset_item是通过cache计算大数据中第dataset_index的元素，轻节点虽然没能保存大数据集，但是它能每次独立的计算具体位置的元素是什么。它与挖矿的区别在于，DAG的某个位置的元素是从cache推出来的，而挖矿是直接取读取DAG的。

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172115780.png)](https://img-blog.csdnimg.cn/20200228172115780.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)



mine: 挖矿函数，就是不断的修改nonce值，从0到2的64次方。先随机生成一个nonce值，再不断+1尝试。

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172129582.png)](https://img-blog.csdnimg.cn/20200228172129582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)



所有函数的汇总，解释了读取顺序的随机性，和验证只需要保存cache，而挖矿需要DAG输入。

[![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172141186.png)](https://img-blog.csdnimg.cn/20200228172141186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

目前以太坊挖矿以GPU为主，可见其设计较为成功，这与以太坊设计的挖矿算法(Ethash)所需要的大内存具有很大关系。
1G的大数组与128k相比，差距8000多倍，即使是16MB与128K相比，也大了一百多倍，可见对内存需求的差距很大(况且两个数组大小是会不断增长的)。
当然，以太坊实现ASIC Resistance除了挖矿算法设计之外，还存在另外一个原因，即其预期从**工作量证明(POW)转向权益证明(POS)**

#### 7.2 权益证明（POS：Proof of State）

权益证明：按照所占权益投票进行共识达成，类似于股份制有限共识按照股份多少投票，权益证明不需要挖矿。
而这对于ASIC矿机厂商来说，就好比一把悬在头上的达摩克利斯之剑。因为ASIC芯片研发周期很长，成本很高，如果以太坊转入权益证明，这些投入的研发费用将全部白费(ASIC矿机只能用于挖特定的加密货币)。
但实际上，以太坊目前仍然是POW挖矿共识机制。在设计之初，以太坊开发者就设想要从POW转向POS，并为了防止有矿工不愿意转埋下了一颗“难度炸弹”。但截至目前，以太坊仍然基于POW共识机制。

> 其实很多时候，面对一些问题转换思路就能得到很好的解决方案。如这里，如果按照原本思想，通过不断改进挖矿算法来达成ASIC Resistance，无疑是比较难的。而这里通过不停宣传要转向POS来不断吓阻矿工，使得矿工不敢擅自转入ASIC挖矿，从而实现了ASIC Resistance。

#### 7.3 预挖矿（Pre-Mining）

以太坊中采用的预挖矿的机制。这里“预挖矿”并不挖矿，而是在开发以太坊时，给开发者预留了一部分货币。以太坊的早期开发者，目前就很有钱了。
而比特币并未采用这一模式，所有比特币都是通过挖矿产生的。但早期挖矿难度容易，所有中本聪本人本来就有很多币。
和Pre-Mining对应，还有Pre-Sale，Pre-Sale指的是将预留的货币出售掉用于后续开发，类似于拉风投或众筹。目前，各类加密货币很多，存在一部分货币就在采用Pre-Sale来获取资金，如果此时买入，后续如果该货币取得成功，同样可以获得很大收益。

#### 7.4 以太坊统计数据

1. 以太坊中以太币供应量(2018年)
   饼状图中，总量大约1亿，蓝色部分都是Pre-Mining产生的（接近3/4）。黑色部分为出块奖励产生的以太币，绿色为叔父区块产生的奖励以太币。挖矿挖的再努力，关键还是不能输在起跑线上。。
   [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172242273.png)](https://img-blog.csdnimg.cn/20200228172242273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)
   
2. 最大的25个矿池挖矿算力比重(2018年)

   可以看出挖矿集中化比较高。

   [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172259489.png)](https://img-blog.csdnimg.cn/20200228172259489.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

3. 以太币价格变化情况(至2018年)
   可见，2017年以太坊才开始大涨。
   [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172312186.png)](https://img-blog.csdnimg.cn/20200228172312186.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

4. 以太币市值变化情况(至2018年)
   [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172406126.png)](https://img-blog.csdnimg.cn/20200228172406126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

5. 以太币Hash Rate变化情况(至2018年)

   不同数字货币之间的哈希率是不能比的，比如以太坊中尝试一个nonce的难度比比特币的要大的多。

   [![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200228172448932.png)](https://img-blog.csdnimg.cn/20200228172448932.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70)

#### 7.5 其他观点

本篇中挖矿算法设计一直趋向于让大众参与，这一才是公平的。且由于参与人员的分散，算力分散，也进一步使得系统更安全。
但同样一件事物，从不同观点看就有不同的看法。也有人认为**让普通计算机参与挖矿是不安全的，像比特币那样，让中心化矿池参与挖矿才是安全的。为什么呢？**
因为要攻击系统，需要购入大量只能进行特定货币挖矿的矿机通过算力进行强行51%攻击，而攻击成功后，必然导致该币种的价值跳水，攻击者投入的硬件成本将会全部打水漂。而如果让通用计算机也参与挖矿，发动攻击成本便大幅度降低，目前的大型互联网公司，将其服务器聚集起来进行攻击即可，而攻击完成后这些服务器仍然可以转而运行日常业务。因此，也有人认为，在挖矿上面，ASIC矿机“一统天下”才是最安全的方式。



### 8.0 ETH挖矿难度的调整

比特币是每隔2016个区块来调整挖矿的难度，目标是维持出块时间平均在10分钟左右，以太坊是每个区块都有可能调整挖矿难度，调整的方法也比较复杂，而且还改过好几个版本，包括以太坊的黄皮书和实际代码也有一些出入，我们这部分以代码为准。

#### 8.1 区块难度公式

H：指当前一个区块。Hi：当前区块的序号。D(H)：当前区块的难度。

max括号里的第一部分我们叫基础部分，为了维持出块时间大约在15s左右，后面跟着的espilon为第二部分，也叫做难度炸弹，目的是未来向权益证明过度。

我们先来看第一部分，是在父区块难度的基础上进行调整。第一部分的D0是难度下线，无论怎么调整最小也不会低于这个难度。

![在这里插入图片描述](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/87488ed5bcac4c6eae7232ba3611a714.png)



这里的x是调整的力度，是父区块的难度除以2048，无论是上调还是下调都是按照这个x整数进行调整的。

下面一个公式是和两个因素有关，一个是出块时间，另一个是有没有叔父区块（就是当前挖矿的前一个父区块，它有没有叔父区块）。因为如果当前挖的区块的前一个区块包括了叔父区块，那货币的发行量增多了，所以当前挖的区块的难度就要提高一个档次。max括号中的前面一部分有可能正的有可能负的，如果负的话难度要往下调，最多一次性下调整99个单位。每个单位是父区块难度的2048分之一。所以一次性下调难度最多是2048分之99。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/75fd11261ca14c7d848f697dc4d3909a.png)



y：有叔父区块的话y=2，没用叔父区块的话y=1。所以y一定是常数。后面的部分比y大的话，减出来是负的，难度要下调。相反，后面这一项比前面要小的话，减出来就是正的，难度上调。

Hs是当前区块的时间戳，P(H)Hs是父区块的时间戳。这两块相减就是出块时间的时间间隔。

假设出块时间在1到8秒之间，除以9再向下取整，等于0，假设y=1，最终答案等于1。说明难度要上调一个单位。

假设出块时间在9到17秒之间，除以9再向下取整，等于1，假设y=1，最终答案等于0。说明难度不变。

假设出块时间在18到26秒之间，除以9再向下取整，等于2，假设y=1，最终答案等于-1。说明难度要下调一个单位。

如果出块时间非常非常长，那最终一次性下调难度也不会大于99个单位。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/bba9a1503e7c457888450af1a2d6b896.png)

#### 8.2 难度炸弹

以太坊在设计之初就计划要逐步从POW（工作量证明）转向POS（权益证明），而权益证明不需要挖矿，这就带来一个问题——已经在挖矿设备上投入大量资金的矿工会不会联合起来抵制这个转换，从POW转向POS需要通过硬分叉来实现，这样就可能导致社区分裂，以太坊可能分裂成两条平行的链。
在以太坊早期时，区块号较小，难度炸弹计算所得值较小，难度调整级别基本上通过难度调整中的自适应难度调整部分决定，而随着越来越多区块被挖出，难度炸弹的威力开始显露出来，这也就使得挖矿变得越来越难，从而迫使矿工愿意转入POS。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/a5b6298ae3a94225b9578e99304d2f6e.png)

#### 8.3 难度炸弹的调整

因为开发者低估了POS的设计难度，导致其迟迟没有设计出来，但是难度炸弹的威力已经开始显现出来，系统的出块时间已经开始逐渐变长，本来要15秒出块的，后来变长到了30秒，但矿工还需要继续挖矿。最后在EIP当中决定回退难度炸弹中的区块号，因此我们可以看到，上图中第二项的fake block number（假的区块号），该数对当前区块编号减去了三百万，也就是相当于将区块编号回退了三百万个。从而降低了出块的难度。当然，为了保持公平，也将出块奖励从5个以太币减少到了3个以太币。
下图显示了难度调整对难度炸弹难度影响的结果：

![img](https://img-blog.csdnimg.cn/c58ddf3d5ec344bcaf8e9ebaf9d8fc2c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAUG9sYXJEYXku,size_20,color_FFFFFF,t_70,g_se,x_16)

#### 8.4 以太坊发展的四个阶段

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/7053e4fe9d2d40c191c0c4bbafcb7e28.png)

降低难度炸弹同时也降低了ETH奖励，因为回调是突然的，不调整奖励的话对调整前的矿工来说是不公平的。本来挖的很辛苦只得到了5个ETH，突然下调难度了，别人轻轻松松也挖到5个，那是不公平的。同时也维持了货币的总供应量。

#### 8.5 具体的代码实现

**难度计算公式**

bigTime：当前区块的时间戳。bigParentTime：父区块的时间戳。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/45b30edba1fe43fabd4645a07041fed3.png)

**基础部分的计算**

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/c9f6a0fc7b284ef5921f3aa1b5b2a3fa.png)

**难度炸弹的计算**

为什么减去的是2999999，假如当前的区块号正好是3百万，那么父区块的序号是2999999，它这里判断的是父区块的序号，如果满足条件，则让父区块的序号减去2999999。则父区块的序号为0（区块号其实是从1开始的，逻辑上父区块的序号变成了0），当前的区块序号就变成了1。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/c1bf94c7af4f423baeef104dd601e204.png)

#### 8.6 以太坊实际统计数据（截止2018年）

**挖矿难度变化曲线**
断崖式下跌是难度炸弹的调整，下跌前难度的增长像是曲线。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/7605462fd22c4e668b376159a2de5127.png)

**出块时间**

早期稳定在15秒左右，大概在17年中旬5、6月的时候，出块时间开始大幅增长，最高到达了30秒左右。最后挖矿难度虽然回到了之前的水平，但是出块的时间还是在15秒左右，说明难度变大的同时，算力也增强了。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/c685f4495ebf4fa3a17b5a1e36733516.png)

**实际区块例子**

最长合法链对于以太坊来说也叫作最难合法链。总难度最大的合法链，每个区块的难度反应的是挖出这个区块所需要的工作量，总难度最大就是挖出这条链的总的工作量最大，一般来说靠后的区块挖出来的工作量是比较大的。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/0a4ae65439a44903bac390a02f534360.png)

### 9.0 权益证明

权益证明（POS——Proof of stake）

#### 9.1 BTC能耗

目前比特币和以太坊都是基于工作量证明的共识机制，这种共识机制对电力的浪费非常严重。
下图为比特币系统电力消耗随着时间变化的情况。y轴的单位为Twh，1Twh = 10^9 Kwh,1Kwh就是我们平时生活中常说的“一度电”。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/728a3f56754547549c5a6d013f40fc0f.png)

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/0b04540189de4d008d3d8ad16096dec5.png)

可以看到比特币系统的能耗是相当大的，每个交易的平均能耗是1000度电，但是尽管成本很高，却仍然存在利润空间。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/4537c783fe6f44968753b42efdb7aab5.png)

#### 9.2 以太坊能耗

以太坊能耗也是随时间增长的，中间有一点波动。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/48e6fb6eb51b4e7b9219ecdb627af276.png)

以太坊的出块时间短，所以每个交易的平均能耗比比特币少。

![img](https://img-blog.csdnimg.cn/484db51b22c44988abbe0ac2b35b7ff4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAUG9sYXJEYXku,size_20,color_FFFFFF,t_70,g_se,x_16)

比特币和以太坊的能耗相加当做一个国家来看。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/bac412421ef64cb3b4cb65faba1bcafb.png)

#### 9.3 权益证明

**思考**

挖矿消耗的这些能源是必须的吗？矿工为什么要挖矿？
矿工挖矿是为了取得出块奖励，获取收益。而系统给予出块奖励的目的是激励矿工参与区块链系统维护，进行记账，而挖矿本质上是看矿工投入资金来决定的(投入资金买设备->设备决定算力->算力比例决定收益)。

那么，为什么不直接拼“钱”呢？既然是用钱购买矿机进行挖矿比拼算力，那么为什么不直接将钱投入到系统开发和维护中，而根据投入钱的多少来进行收益分配呢？这就是权益证明的基本思想。

采用权益证明的货币，一般在正式发行之前会先预留一些货币给开发者，而开发者也会出售一些货币换取开发所需要的资金，在系统进入稳定状态后，每个人都按照持有货币的数量进行投票。

**优点**

省去了挖矿的过程，减少了能耗，温室气体的排放。

POW中维护其安全的资源没有形成闭环，它需要通过现实中的货币去购买矿机，这也就导致只要有人想要攻击，只需要外部聚集足够资金就可以成功。而对于POS，要想发动攻击的话需要得到这个币种发行量一半以上的份额才行，发动攻击的资源只能从这个加密货币的内部才可以得到（闭环）。无论这个人再外面有多少钱都不会对这个加密货币系统造成直接的影响。必须要有足够的钱去买足够的币才能够发起攻击，一旦有人大量买入这种加密货币，价格会大涨，早期的开发者和投资者会认为这不是坏事情，正好可以大赚一笔，这就类似于股份制公司遭受恶意的股份收购。

POS与POW并不是互斥的，有的加密货币采用一种混合模型，即仍然需要挖矿，但挖矿难度跟你持有的币的数量有关，持有的币越多，挖矿越简单。当然，如果就这么简单的设计是有问题的，即持有币数量最多的人每次挖矿都是最容易的。所以，有的加密货币要求投入的币会被锁定一段时间不能重复使用，比如：挖当前区块投入一定数量的币用于降低挖矿难度，等这个区块发布后，这些币会被锁定一段时间，下次再挖的时候这些币就不能再用了，要过多少个区块之后才能再用。这个也叫做Proof of Deposit。

**问题**

权益证明的应用仍然存在很多挑战，其中一个就是“两边下注”的问题。（nothing at stake）

如下图所示，区块链系统产生了分叉，存在两个区块A和B竞争主链时，如果是工作量证明的话，同时挖A、B两条链会因为算力分散导致挖到区块的概率降低，但是采用权益证明的话，在A和B同时进行了下注。最终A区块胜出，那么他能够获得A区块相应收益，而在B区块进行投票放入的“筹码”只记录在下面的分叉上并不影响你在上面分叉上的使用，这也就导致其每次都能获得收益。


![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20b73894fa584ca9930e8f6f3ae65a2b.png)



#### 9.4 以太坊准备采用的权益证明协议

以太坊中，准备采用的权益证明协议为Casper the Friendly Finality Gadget(FFG)，该协议在过渡阶段是要和POW结合使用的。为工作量证明提供Finality。

Finality是一种最终的状态，包含在Finality中的交易不会被取消。

单纯基于挖矿的交易是有可能被回滚的，比特币中规定要等六个区块来防止被回滚，但这只是说明回滚的概率比较小，但是只要攻击者的算力足够强（占到50%以上）仍然可能回滚该交易。所以单纯基于挖矿的区块链是缺乏这种Finality的。

Casper协议引入一个概念：Validator(验证者)，一个用户想要成为Validator，需要上交一笔ETH“保证金”，这笔保证金会被系统锁定。Validator的职责是推动系统达成共识，投票决定哪一条链成为最长合法链，投票权重取决于保证金数目。挖矿的时候每挖出一百个区块就作为一个**epoch**，然后通过投票决定其能不能成为一个Finality。

投票时采用**two-phase commit**，第一轮投票是**Prepare Message**，第二轮投票时**Commit Message**，Casper规定每一轮投票都要得到2/3以上的验证者才能通过（按照保证金的金额大小计算）实际系统中不再区分这两个Message，而且把epoch从100个区块减到50个区块，且只需要一轮投票（对于上一个epoch是Commit Message，对下一个epoch是Prepare Message），要连续两轮投票都得到2/3以上的多数才算有效。

原始版本：

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/4203deafac174c9da3d1663e2abac2d3.png)

优化后：

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/5ce9b47c3109474eac984b6fec3866c0.png)

矿工挖矿会获得出块奖励，而验证者也会得到相应奖励。当然，为了防止验证者的不良行为，规定其被发现时要受到处罚。例如,某个验证者“行政不作为”，不参与投票导致系统迟迟无法达成共识，这时会扣掉部分保证金；如果某个验证者“乱作为”，给两个有冲突的分叉都进行投票（两边下注），被发现后没收全部保证金。没收的保证金被销毁，从而减少系统中货币总量。验证者存在“任期”，在任期结束后，进入“等待期”，在此期间等待其他节点检举揭发是否存在不良行为进行惩处，若通过等待期，则可以取回保证金和应得的奖励。

Q：通过验证者达成的Finality有没有可能被推翻？
A：如果发动攻击的组织仅仅作为矿工，无论他算力多强，但如果没有验证者作为同伙是无法推翻的，必须在系统中，存在大量“验证者”对前后两个有冲突的Finality都下注。比如，一个Finality已经被超过2/3的验证者投票通过了（比如一共就A、B、C三人，A和B投了同意这个Finality的票），现在矿工发动分叉攻击，分叉链成为了最长合法链，验证者同时为了帮助分叉，投票给分叉链（这个时候可能是B和C投给了分叉链），也就是说，至少1/3（B两边都投了）（上面的链投了2/3，下面的链也投了2/3，至少有1/3的重叠了）的验证者两侧都投票。而这一旦被发现，这1/3验证者的保证金将会被没收。

以太坊系统设想，随着时间推移，挖矿奖励逐渐减少而权益证明奖励逐渐增多，从而实现POW到POS的过渡，最终实现完全放弃挖矿。

为什么以太坊不从一开始就用权益证明呢？
因为权益证明还不是很成熟，工作量证明是很成熟的，经过了时间的检验（bug bounty）。
EOS加密货币，即“柚子”，就是采用权益证明的共识机制，其采用的是DPOS：Delegated Proof of Stake。该协议核心思想是通过投票选21个超级节点，再由超级节点产生区块。但目前，权益证明仍然处于探索阶段。

#### 9.5 其他观点

前面的基本观点都是基于“挖矿消耗大量电能，而这是不好的”这一观点，但也有人持有相反观点。
他们认为其所消耗的电能所占比值并不大，而且其对于环境的影响是有限的。挖矿提供了将电能转换为钱的手段，而电能本身难以传输和存储，一般来说，白天所发的电不足，晚上所发的电又多于实际需求，很多大型数据中心要建在电比较便宜的地方，就是因为传输数据比传输电要容易。因此，挖矿为将多余的电能转换为有价值的货币提供了很好的解决手段。也就是说挖矿消耗电能可以有效消耗过剩产能，带动当地经济发展。

### 10.0 智能合约

#### 10.1 简介

智能合约：运行在区块链系统上的一段代码，代码逻辑定义了合约内容。

智能合约的账户保存了合约当前的运行状态：

```
balance：当前余额
nonce：交易次数
code：合约代码
storage：存储，数据结构为一棵MPT
```

智能合约编写代码为Solidity，其语法与JavaScript很接近。下图是拍卖合约的代码例子，Solidity是面向对象的语言，这里的contract相当于C++的class类。Solidity是强类型语言，这里的类型和普通的编程语言像C++是比较接近的，比如uint是无符号整数。address是Solidity所特有的，后面会讲地址类型的成员变量和成员函数。

接下来是两个事件event，事件的作用是记录日志的，HighestBidIncreased是拍卖的最高出价，如果有人出了新的最高价，我们记录一下参数是address bidder出价人，金额是amount。Pay2Beneficiary 它的参数是winner最终拍下的人，amount金额。

event上面两行的mapping是一个哈希表，保存了一个地址address到uint的映射，Solidity语言中的它不支持遍历，如果你想遍历哈希表中所有的元素，你就需要自己想个办法来记录哈希表中有哪些元素，我们这里是用bidders这个数组来建立的。Solidity里面的数组是可以固定长度的也可以动态改变长度。这里是一个动态改变长度的数组。比如你想往数组里面增加一个元素，用push操作，bidders.push(bidder)，在数组的末尾新增加一个出价人。你想要知道这个数组有多少个元素，用它的length域，bidders.length。如果需要固定长度，就例如address[1024] bidders。

再往下是它的构造函数，solidity语言定义构造函数有两种方法，一种是像C++一样定义一个contrast同名的函数，这个函数可以有参数但不可以有返回值。新版本（在这里是0.4.21）版本推荐的是图中的定义方式，用constructor定义，构造函数只有在合约创建的时候调用一次，构造函数只能有一个。

接下来是三个成员函数，这三个函数都是public，说明其他账户可以调用这些函数，示例中的函数都没有参数，注意bid()这个函数，它有一个标志叫payable，后面会解释什么意思。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200304162806439.png)

#### 10.2 账户调用

调用智能合约其实和转账是类似的，比如A发起一个交易转账给B，如果B是一个普通的账户，那么这就是一个普通的转账交易，如果B是一个合约账户的话，那相当于发起一次合约账户的调用。

##### 10.2.1 外部账户调用合约账户

图例中sender address是发起调用的地址，to contract address 是被调用的合约的地址，调用的函数看（Tx DATA）这个地方给出了要调用的函数，如果这个函数有参数的话，参数的取值也是在这个DATA域中说明的。

中间这一行是调用的参数，VALUE是发起调用的时候转过去多少钱，GAS USED是我这个转账花了多少的汽油费，GAS PRICE是单位汽油的价格，GAS LIMIT是我这笔交易我最多愿意支付多少汽油，后面会详细讲汽油费的事情。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200304191834619.png" alt="img"  />

##### 10.2.2 合约账户调用合约账户

合约账户之间也可以进行调用。其调用方式如下：

1、直接调用

A这个合约的foo函数的作用只是写条Log，LogCallFoo是一个事件，emit的作用是调用这个事件写一个Log，对于程序的运行逻辑是没有影响的。

B这个合约的函数是把A转化成一个实例，再去调用其中的foo这个函数。

错误处理：直接调用的方式，一方产生异常会导致另一方也进行回滚操作。

<img src="https://img-blog.csdnimg.cn/20200304192100140.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L011X1hpYW95ZQ==,size_16,color_FFFFFF,t_70" alt="img" style="zoom:67%;" />

2、address类型的call()函数调用

addr.call()，图例中addr.call(funcsig,"call foo by func call")这个函数的第一个的参数funcsig是要调用的那个函数foo(string)的签名，第二个是要调用的参数"call foo by func call"。

错误处理：address.call()的方法，如果调用过程中被调用合约产生异常，会导致call()返回false，但发起调用的函数不会抛出异常，而是继续执行。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200304192237645.png" alt="img" style="zoom:67%;" />

3、代理调用delegatecall()

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/2020030419262145.png)

和call()调用基本一致，区别在于其并不会切入被调用合约的上下文中。

##### 10.2.3 关于Payable

凡是要接受外部转账的函数都必须标志成payable，否则你给这个函数转去钱的话会引发错误处理，会抛出异常。如果不需要外部转账这个函数就不需要写成payable。

如下，成员函数中的第一个函数bid()，有一个payable修饰。该例中背景为拍卖，bid()为出价，你调用这个函数的时候要把这个出价对应的币也转过去，存储到合约里，锁定在那里一直到合约结束，因此需要payable进行标记。

withdraw()为其他未拍卖到的人将锁定在智能合约中的钱取出的函数，其不涉及转账，因此不需要payable进行标记。

![image-20220325131434375](C:/Users/zhijia_soft/AppData/Roaming/Typora/typora-user-images/image-20220325131434375.png)

##### 10.2.4 fallback()函数

这个函数没有名字，没有参数，也没有返回值。注意，fallback这个关键字不出现在这个函数名中间。fallback中文其中有个意思是“应急计划”，当没有什么函数可以调用的时候就调用它。比如A向B转账，但没有在data域中说明要调用哪个函数，或说明的要调用函数不存在，此时调用fallback()函数。这也是为什么这个函数没有参数，因为本来就没法提供参数。

fallback()函数也同样可以标注成payable，一般情况下都会写成payable。因为一个合约其他函数没有标志成payable，fallback()函数也没有标志成payable，那它没有任何能接受转账的能力。如果向这个合约转账，data域为空，调用fallback()函数，会抛出异常。

另：转账金额和汽油费是不同的。汽油费是为了让矿工打包该交易，而转账金额是单纯为了转账，其可以为0，但汽油费必须给。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200305112942373.png" alt="img" style="zoom:67%;" />

#### 10.3 智能合约创建与运行

智能合约的创建是由某个外部账户发起一个转账交易到0x0地址，转账金额是0，把要发布的代码放到data域里面。EVM设计思想类似于JAVA中的JVM，便于跨平台增强可移植性，通过加一层虚拟机，对智能合约的运行提供一个一致性的平台。所以EVM有时叫做world wide computer。EVM中寻址空间256位，像之前我们看到的uint就是256位的，个人PC一般是32或者64位。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200306151126702.png" alt="img" style="zoom: 33%;" />

#### 10.4 汽油费

比特币的设计理念是简单，脚本语言很有限，比如说不支持循环，而以太坊要提供一个图灵完备的编程模型（Turing-complete Programming Model），很多功能在比特币平台上实现起来很困难，甚至是实现不了的，但到了以太坊上就很容易。但这也导致一些问题，出现了死循环怎么办？当一个全节点收到一个对智能合约调用，怎么知晓其是否会导致死循环。

事实上，无法预知其是否会导致死循环，实际上，该问题是一个停机问题（Halting Problem），停机问题是不可解的，从理论上可以证明不存在一个算法，能够对任意给定的输入程序判断出这个程序是否会停机。因此，以太坊引入汽油费机制将该问题扔给了发起交易的账户，遏制发起交易的人滥用资源。第二个目的是给矿工消耗的计算资源的一些补偿。注意这里的问题不是NPC的，NPC的问题是可解的，只不过它没有多项式时间的解法，很多NPC问题有指数时间的解法，比如说哈密尔顿回路问题，判断一个图有没有哈密尔顿回路，这个其实是很容易解的，如果不考虑复杂度的话，想到一个解法是很容易的，比如可以把所有的可能性枚举一遍。

我们来看下交易的数据结构txdata。AccountNonce是交易序号，用于防止第2章说的**重放攻击**（reply attack）。Price和GasLimit就是和汽油费相关的，GasLimit是我这个交易愿意支付的最大汽油量，Price是单位汽油的价格，这两个乘在一起是这笔交易可能消耗的最大汽油费。Recipient是收款人的地址。Amount是转账金额，把Amount这么多的钱转给Recipient。从代码中可以看到转账金额和汽油费是分开的。

![image-20220325140137206](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220325140137206.png)

当一个全节点收到一个对智能合约的调用，先按照GasLimit算出可能花掉的最大汽油费，然后一次从发起调用的这个账户上把钱扣掉，再根据实际执行情况，算出实际花掉的汽油费，多出来的部分会退掉，但如果给少了会发生回滚。EVM中不同指令消耗的汽油费是不一样的，简单的指令很便宜（比如加法减法），复杂的（比如取哈希）或者需要存储状态的指令就很贵，如果只是需要读取公共数据那些指令可以是免费的。

在一个区块头（Block Header）中有GasLimit和GasUsed两个域。GasUsed是这个区块中所有交易中所消耗的汽油费加在一起。但是，GasLimit不是这个区块中所有交易中的GasLimit加在一起。下面来说说这个的用意。发布区块需要消耗一定的资源，那要不要对这个区块所消耗的资源有一个限制，比特币中对发布的区块也是有一个限制的是大小的限制，最多不能超过1M。因为如果一个区块没有任何的限制，那一个矿工可能把很多的交易打包成一个区块发布出去，那这个超大的区块在区块链上会消耗很多的资源。比特币的交易较为简单，基本上可以用这个交易中所包含的字节数看出消耗的资源是多少，但是在以太坊中不一样，有些交易在字节数上看起来是很小的，但是消耗的资源很大，比如它可能调用别的合约之类的。所以要根据交易的具体操作来收费，这个就是汽油费。所以在区块头中的GasLimit是这个区块中所有交易能消耗的汽油费的一个上限。

![image-20220325143843402](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220325143843402.png)

相对于比特币中写死的1M大小的区块容量上限，以太坊中的这个GasLimit的这个上限是浮动的，矿工可以自己进行调整，可以在上一个区块的GasLimit的基础上，上调或者下调1/1024的GasLimit。不要觉得1/1024很小，由于出块速度快，如果每个区块都选择上调，那很快就能翻一翻。有的矿工觉得要上调，有的矿工觉得要下调，在这种机制下，得出的GasLimit是所有矿工觉得合理的GasLimit的一个平均值。

**汽油费是怎么扣掉的？**

全节点收到调用智能合约的要求的时候，从本地维护的状态树中把对应的账户余额减掉，如果GasLimit有剩余的再把余额加上去一点点。智能合约的执行都是在改本地的数据结构，只有获得记账权之后才可以把本地的状态同步出去，变成区块链上的共识。别的没有获得记账权的节点，需要把自己刚做的候选区块的交易给扔掉，把获得记账权的节点发布的区块信息在本地重新执行一遍，更新自己本地的三棵树。

#### 10.5 错误处理

以太坊中交易具有原子性，要么全执行，要么全不执行，不会只执行一部分，这个交易包含普通的转账交易，也包含对智能合约的调用。所有如果在执行智能合约的过程当中，出现任何错误会导致整个交易的执行回滚，退回到开始执行之前的状态，就好像这个交易就完全没有执行过。什么情况下会出现错误？一种是汽油提供的不够，Gaslimit用完了，已经消耗掉的汽油费是不会退回的，不然会有恶意节点反复去调用计算量很大的合约，但汽油费给的不够，如果汽油费还能退回来，对攻击者没有任何损失，对矿工来说白白浪费了很多资源。另一种遇到异常的情况是，遇到了assert()、require()或者revert()语句，assert()、require()这两条语句都是用来判断某种条件，如果条件不满足就会抛出异常。assert()类似C++，处理内部错误，require()处理外部错误，比如拍卖合约中的require(now <= auctionEnd)，如果符合条件就继续运行，如果拍卖都已经结束了，你还在出价，就会抛出异常。revert()是执行到这个语句就会发生回滚。Solidity里面没有类似Java可以自定义的try-catch()结构。

![image-20220325141853132](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220325141853132.png)

嵌套调用：一个合约调用另一个合约中的函数。

嵌套调用是否发生回滚，取决于调用方式。如果是直接调用的方式会出现连锁式的回滚，如果是call()的方式就不会引起连锁式的回滚，只会使当前的调用失败返回一个false的返回值。需要注意的是，有些情况下从表面上来看你没有调用任何一个函数，比如一个合约向一个合约账户直接转账，没有调用任何函数，因为fallback函数的存在，就有会调用到fallback函数，这个也叫嵌套调用。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200306153636245.png" alt="img" style="zoom: 33%;" />

#### 10.6 一些问答

**1、是先挖矿还是先执行交易和智能合约？**

先执行交易内容和智能合约，更新完三颗树得到根哈希值，这样区块头的内容才能确定，然后才能挖矿尝试各个nonce。

**2、会不会有全节点收到新的区块，不在本地重新验证一遍，直接继续挖？**

不行，如果这个节点跳过了验证的步骤，那它的三棵树的状态就无法更新，无法继续挖矿了。发布的区块里面可没有三棵树状态的内容，不能直接拿过来，只能自己去算。

**3、发布到区块链中的交易是不是都是成功执行的？如果智能合约执行出现了错误要不要也发布到区块链上面去？**

不一定都是成功执行的，执行错误的结果也要发到区块链上，否则汽油费扣不掉。怎么知道一个交易是不是执行成功了呢？每个交易执行完成后形成一个收据，其中status这个域是记录交易执行的情况。

![image-20220325155435469](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220325155435469.png)

**4、智能合约支持多线程吗？**

solidity不支持多线程，因为以太坊是交易驱动的状态机，这个状态需要完全确定的。多线程的问题在于多个核对内存访问的顺序不同的话，执行的结果可能是不一样的。除了多线程外其他造成执行结果不一致的行为还有随机数等。

**5、智能合约能获得的信息：**

智能合约的执行必须是确定性的，所以它不能通过像系统调用那样获得环境信息，因为每个全节点执行的环境很可能是不一样的。所以只能得到一些固定的状态信息。

智能合约可以获得的区块信息：

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328104210890.png" alt="image-20220328104210890" style="zoom:67%;" />

智能合约可以获得的调用信息：

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328104246017.png" alt="image-20220328104246017" style="zoom: 80%;" />

msg.sender和tx.origin的区别：msg.sender指的是调用当前合约的发起方，可以是合约账户。但是tx.origin指的是最初发起整个交易的账户。
msg.gas是当前的调用还剩余多少汽油费，这决定了我还能做哪些操作，包括你还想调用其他合约前提是还有足够的汽油费剩下来。
msg.data是所谓的DATA数据域，调用了哪些函数和函数参数的取值。
msg.sig是数据域的前四个字节，函数标志符。
now是当前区块的时间戳和block.timestamp是一个意思。智能合约无法获得很精确的时间，只能获得当前区块的时间。

#### 10.7 地址类型

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328104535893.png" alt="image-20220328104535893" style="zoom:80%;" />

```
<address>.balance：是成员变量，剩下的都是成员函数。uint256是成员变量的类型，不是函数调用。
<address>.transfer():转账，是指当前合约往address这个地址转入多少钱。address是转入地址，不是转出地址。转出地址是当前使用这个函数的合约。
<address>.call():调用，是当前的合约去调用address这个合约。同样的发起方是合约，接收方是address。
<address>.delegatecall():调用，不需要切换到被调用的函数环境中，使用当前合约的余额、存储这些状态去运行就可以了。
```

如果转账的时候，对方没有定义fallback函数所引起的错误会不会造成连锁回滚，这取决于是怎么转账的。（和调用会不会造成回滚类似，取决于是怎么调用的。）

转账有三种方式：1、transfer()会导致连锁性回滚，类似于直接调用。2、send()有返回值，不会导致连锁性回滚。3、call.value(uint256 amount)()，第一个参数填转账金额，第二个如果不调用函数可以是空的，不会导致连锁性回滚。transfer和send就是发送转账用的，call.value本来是函数调用的，也可以用来转账。transfer和send只能发一点点汽油2300个单位，收到调用的合约基本上干不了什么事，只能写写日志，call.value是把当前发起调用的合约剩余所有的汽油全部发过去，对方用完之后返回来还能接着运行。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/20200314174230829.png" alt="img" style="zoom:80%;" />



#### 10.8 智能合约的例子--拍卖

##### 10.8.1 例子的代码详解

拍卖的规则：
拍卖有个受益人beneficiary，也就是实际卖东西的人。auctionEnd，拍卖的结束时间。highestBidder，最高出价的人。拍卖结束之前每个人都可以去出价，竞拍的时候为了保证诚信，需要把出价的ETH转账过去，锁在里面，直到拍卖结束。拍卖过程中不运行中途退出。拍卖结束后最高出价人的钱将转给受益人，其他没有竞拍成功的人，可以把钱取回来。竞拍是可以多次出价的，中间加价只需要补差价就可以。出价要有效的话，要比最高出价的要高才行。

constructor构造函数会记录下收益人是谁，结束时间。

![image-20220325165321938](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220325165321938.png)

这里是拍卖要用的两个函数，bid()和auctionEnd()。

![image-20220328132015751](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328132015751.png)

bid()函数是竞拍的时候用的，你要竞拍就发起一个交易调用合约中的bid()函数，bid()函数并没有参数，一般来说，你参与交易需要告诉对方你出的价格是多少，那这个价格从哪里体现了呢。在msg.value里面，这个是随交易发送过去的wei的数量。代码详解如下：

首先先检查拍卖是否结束了，require(now <= auctionEnd);

再检查这次出价和之前所有的出价之和是不是高于最高出价者，require(bids[msg.sender]+msg.value > bids[highestBidder])。如果没有出过价钱，bids[msg.sender]是个哈希表，哈希表查询不到时会返回0。

如果bids[msg.sender]等于0，说明此人之前没有出过价钱，需要把拍卖者的信息msg.sender加到bidders[]数组里。因为Solidity的哈希表不支持遍历，你要遍历哈希表的话你要保存一下他包含哪些元素。

highestBidder=msg.sender记录下新的出价人是谁。bids[msg.sender] += msg.value更新最新的出价。

emit HighestBidIncreased(msg.sender, bids[msg.senger])。写条日志，更新下最高的出价人和出的价钱。

![image-20220328132028848](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328132028848.png)

auctionEnd()是拍卖结束后的函数，代码详细解释如下：

首先查一下拍卖是不是已经结束了，require(now > auctionEnd);

然后判断一下这个函数是不是已经被调过了，require(!ended)，如果被调过了就不用再调一遍了。

beneficiary.transfer(bids[highestBidder]);把最高出价人所对应的出的最高的金额转账给受益人。

对于没有竞拍成功的人，用一个循环把他们出过的价钱退回给bidder，bidder.transfer(bids[bidder]);

最后标注一下这个函数已经执行完了，写一个AuctionEnded竞拍结束的日志。

##### 10.8.2 合约的安全

以上的合约其实有安全漏洞。看以下的黑客合约hackV1，当然这是一个合约账户，黑客需要从外部账户中去调用这个合约中的hack_bid函数，然后这个函数再去调用拍卖合约中的bid()函数，把黑客的外部账户的钱转到黑客合约中再转到拍卖合约中的bid()函数，来参与拍卖。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328133728997.png" alt="image-20220328133728997" style="zoom: 50%;" />

hack_bid(address addr)参数是拍卖合约的地址，转成拍卖合约的一个实例，调用拍卖合约的bid()函数，把钱发送过去。好像这样也没什么问题，但是最后退款会有问题，我们来看退款的函数。

<img src="C:/Users/zhijia_soft/AppData/Roaming/Typora/typora-user-images/image-20220328140522823.png" alt="image-20220328140522823" style="zoom:50%;" />

退款到黑客合约的钱会有什么情况？bidder.transfer()转账的时候不会调用黑客合约的任何一个函数，所有会去调用黑客合约的fallback()函数，但是黑客合约也没有定义fallback()函数。所以，转账会抛出异常，transfer()函数会引起连锁式的回滚，整个参与拍卖的人都会收不到钱。对黑客来说，可能只是为了捣乱没写fallback()函数，也可能是忘记写fallback()函数。

注意，在给每个bidder转账退款的时候，是全节点去修改本地的数据结构，并不是形成一条条新的交易到区块链上。

所以现实中出现这种情况怎么办，答案是没有办法，黑客合约已经参与了竞拍，合约内容无法改，拍卖合约同样无法改。Code is law，代码就是法律。没有人能篡改规则，规则有了漏洞也无法改了。

能在合约中留个后门给创建者超级用户的权力吗，比如在拍卖合约的构造函数里添加一个owner，记录owner的地址，允许它做类似系统管理员的操作，比如可以任意转账把钱转到哪个地址都行。但这个和去中心化的理念是背道而驰的。

下面是拍卖合约的第二个版本，由竞拍者自己取回钱。

![image-20220328141659440](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328141659440.png)

withdraw():先检查拍卖是否结束，再检查这个人是否是最高出价者，如果是的话不能给他，因为他的钱要给受益人。然后再看这个人的账户的余额是不是正的，把这个人的账户余额转给msg.sender这个人，再把他的账户余额归0，免的他下次再来取一次钱。

pay2Beneficiary():把最高出价给受益人。

但这样改也是有问题的，因为addr.call.value()()会触发addr里面的fallback()函数，而黑客可以在自己的黑客合约中的fallback()函数中做手脚。如下图所示。

![image-20220328144534675](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328144534675.png)

黑客在fallback()函数中又调用了拍卖合约的withdraw()把钱取了一遍。但是在拍卖合约中withdraw()函数对账户余额的清零操作是在交易完成之后才开始。而转账的语句已经陷入到了与黑客合约的递归调用之中，根本执行不到下面清零的操作。

递归到什么时候结束？1、拍卖合约的余额不够了。2、汽油费不够了，每次递归调用要消耗汽油费的。3、调用栈溢出了。所以在黑客合约里有这样的判断条件，当前的拍卖合约的余额大于出价（即还能支持转账），当前调用的gas剩余是大于6000的，调用栈小于500。

怎么修改？最简单的办法是先清零再转账，先把bids[msg.sender]=0。就像pay2Beneficiary()函数，把最高出价者的余额归零，bids[highestBidder]通过哈希表修改余额，然后再转账，如果转账失败了，再把bids[highestBidder]的余额恢复。

![image-20220328145132474](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328145132474.png)

以上是和其他合约的经典编程模式，先要判断条件，然后再改变条件，最后再和别的合约发生交互。在区块链上任何未知的合约都可以是有恶意的，所以每次需要转账或者调用别的合约的时候都要提醒自己，这个合约有可能会反过来调用自己当前的合约，并且修改状态。

还有一种修改的办法，就是不要使用call.value的方式，用send()或者transfer()，因为这两个函数转账发送过去的汽油费只有2300个单位，这个不足以让接受的合约再发起一个新的调用。

![image-20220328145752739](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328145752739.png)

### 11.0 The DAO 事件

以太坊实现了去中心化的合约，有人想既然去中心化这么好，为什么不把所有的东西都改成去中心化呢？有人提出口号：decentralized everthing。DAO(Decentralized Autonomous Organization)去中心化的自治组织就是在这个背景下诞生的。传统社会下，组织都是建立在某种法律文件下的，比如说可以有个章程规范组织的行为，有时候还可能到政府登记注册。那DAO就是把组织的规章制度写在代码里，通过区块链的共识协议来维护这种组织的正常执行。

在2016年5月，出现了一个致力于众筹投资的DAO，它的名字为The DAO。DAO是一个通用的概念，凡是去中心化的自治组织都可以叫做DAO，The DAO 只是指具体的这个DAO，它的工作原理有点像众筹的投资基金，本质是允许在以太坊上的一个智能合约。如果你想参与The DAO，那你可以把以太币发给智能合约，然后可以换回The DAO的代币。需要决定投资哪个项目的时候，是大家投票决定的，手里的代币越多，投票的权重越大，最后有了收益也是按照智能合约中规定的规章制度进行的。

工作原理有点像DAC(Decentralized Autonomous Corporation)去中心化的自治公司，一般来说，DAC是出于盈利目的，DAO的话可以是出于非盈利性目的，比如某种公益事业。DAC虽然有公司这个词，但是在现实社会中，他没有公司应有的法人地位，一般来说也没有像董事长、CEO这样的职务。

The DAO出现的时候很风靡，因为从来没有如此民主的众筹方式，一个月的时间筹集到了当时价值1.5亿美元的以太币。当时媒体都在预测，未来几年The DAO的影响里会有多么多么的大，有的人甚至说The DAO在3到5年后的影响力甚至会超过以太坊本身。遗憾的是，The DAO一共只存活了3个月。

如果你是The DAO的投资者，你怎么取回的The DAO的收益，比如你已经换回了The DAO的代币，过一段时间你需要用钱了，想把以前投资的以太币换回来，怎么办？这个在The DAO的基金里是通过拆分的方式解决的。叫做：split DAO。拆分这个做法不止是取回自己的收益也是一种建立子基金的方式。拆分完之后得到个child DAO。设计理念是这样的，The DAO投资项目是靠大家手里的代币去投票，如果有一小部分人他的投资理念和其他人不一样怎么办。这一小部分人可以用拆分的办法从The DAO独立出来，成立一个自己的子基金，叫做child DAO。拆分的时候，他们手中的代币会被收回，换成相应数量的以太币，把相应的以太币打到子基金里，然后他们就可以投自己想投的项目了。拆分的极端例子就是单个的投资者成立一个子基金，然后在子基金里就可以把所有的钱投给他自己，这是投资者取回投资和收益的唯一途径，它没有像之前所说的withdraw函数。拆分前有7天的辩论期，拆分完之后有28的锁定期。就是这28天的锁定期给了后面事故中的补救时间。

拆分的理念并没有错，而且是民主制度的进一步体现，民主制度并不是绝对的少数服从多数，而是说也要尊重少数人选择的权力。拆分的理念没有错，那问题出在哪里呢？问题就出在split DAO的实现上。

![image-20220328161933823](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220328161933823.png)

从withdrawRewardFor这个语句开始，首先把钱还给调用这个函数的人，然后把The DAO中的总金额减少相应的数量，再把调用者的账户清0。在上一节讲过，正确的操作是先把账户清0，然后再转账。黑客就是利用这个漏洞进行的重入攻击，转走了5千万美元的以太币，差不多1/3。

引起了以太坊社区的大恐慌，币价的大跳水。一派认为，需要回滚交易，成立的子基金有28天的锁定期，所以黑客还没有办法取走以太币，还有时间可以采取补救措施；另一派认为，不需要做任何补救措施，因为黑客的行为没有违法，code is law。在疑似黑客的公开信中，黑客声称自己没有做错任何事情，既然代码里写的可以让我重复多次取钱，我就没有违反任何法律。这方认为，不应该回滚交易，区块链最重要的特性是不可篡改性，如果出了问题就回滚，怎么能叫不可篡改呢。而且这次出问题的只是以太坊上的一个应用而已，以太坊本身的代码没有问题，如果每个智能合约出了问题都回滚的话，不就乱套了吗。

以太坊的开发团队是支持采取补救措施的，主要是这个事情的影响太大了，占总以太币流通量的百分之十几。too big too fail，太大了以至于不能倒。那现在怎么补救呢？

比如说从黑客盗取以太坊的区块的前一个区块开始分叉，让分叉链更长，这样行不行。如果这样，不光是黑客的交易回滚了，区块上所有的交易都回滚了，会影响大量正常交易的人，所以这样做是不行的。要回滚的话只能是精确定位，只能是针对黑客盗取以太币的那些交易。怎么操作呢？以太坊的团队制度了两步走的方案。第一步，首先要锁定黑客的账户。第二步，把盗取的以太币设法退回去，清退The DAO基金上的这些钱。

第一步，怎么锁定这些账户。以太坊团队发布了一条软件升级，增加了一条规则，凡是和The DAO这个基金上的账户相关的，不能做任何的交易。许多矿工都升级了软件。这个是软分叉，软件升级是增加了一条判断的规则，新矿工挖出来的区块，旧矿工是认可的，旧矿工可以继续在这个区块后来开始挖，旧矿工挖出来的区块，新矿工有可能不认可，这样对系统只会造成临时性的分叉。遗憾的是，升级的软件有个问题，那就是判断是否和The DAO相关的交易这个步骤还要不要收取汽油费，如果不收取的话，可能有恶意的攻击者不断的发放非法的交易，拒绝服务(Deny of service)，浪费矿工的资源，反正对攻击者来说成本很低。以太坊的这个升级没有收汽油费，受到了Deny of service，矿工之后就受不了了，纷纷重新使用升级前的软件。于是软分叉的方案失败了。

这个时候形式严峻了，软分叉的方案失败了，剩余时间不多了，以太坊团队想软的既然不行，那就来硬的了，设计了一个硬分叉的方案，通过软件升级的方法，把The DAO账户上的所有资金强行转到另外一个新的智能合约上去，新的智能合约就只有一个功能，那就是退钱！退回以太币。这个为什么是硬分叉？本来的转账是要有合法的签名，而这个升级的转账是没有合法的签名的，所以新矿工挖出来的区块，旧矿工是不认可的。挖到第192万个区块的时候，自动执行这条交易。

由此，社区彻底分成了两派，以太坊团队还写了一个合约来进行投票，用手中的以太币去投票，结果显示大多数人支持硬分叉，于是绝大数矿工升级了软件。最后硬分叉没有出现意外，成功了。但故事没有结束。

反对的认为，参与投票的人并不是很多，此外，投票的结果就一定是正确的吗。旧的那条链并没有消亡，还有矿工在上继续挖，挖出来的币叫做ETC经典以太坊(Ethereum classic)，并在交易所上市交易了。一部分人挖是由于投机，一部分人挖则是出于信仰，坚持这种纯而又纯的去中心化理念，认为旧链才是正宗的以太坊，根正苗红，那些搞硬分叉的是在搞修正主义。

刚开始硬分叉的时候带来了一些问题，比如重放攻击，在新链上的合法交易放到旧链上去同样是合法的，反过来旧链上的合法交易放在新链上也是可以执行的。后来给两条链增加了chainID来解决。

为什么都是针对的都是所有的账户，而不只是对应黑客的账户？

如果只冻结黑客的账户，由于合约的bug修不了，那人人都可以成为黑客去继续盗取，所以必须把与DAO相关的所有账户都冻结了。

### 12.0 反思

1、智能合约的反思——智能合约真的智能吗？(Is smart contract really smart?)

首先我们必须了解智能合约里面并没有用到任何人工智能的技术，所以有人认为应该把它叫做自动合约，按照事先写好的代码，自动执行某些操作，现实世界当中，有什么自动合约的例子吗？ATM取款机可以看作物理世界上的一个自动合约，按照事先规定好的逻辑去做某些事情。所以智能合约其实并不智能，而且挺笨的，一旦写好了之后就改不了了，就是作为代码合同。(Smart contract is anything but smart.)

2、不可篡改性是把双刃剑(Irrevocability is a double edged sword.)

一般来说我们提到区块链的不可篡改性都认为这个是区块链的一个优点，很多区块链的应用都利用了不可篡改的特性，比如防伪、溯源。但是从“The DAO”事件当中，可以看出不可篡改性其实是一个双刃剑。一方面，不可篡改性增加了合约的公信力，但另一方面，不可篡改性也意味着如果规则中有漏洞，我们想要修补这个漏洞，想软件升级都是很困难的。The DAO的盗币事件有传闻说The DAO的开发团队在发生盗币事件前几天已经收到了有关智能合约中存在安全漏洞的消息，但是没有来的及发布更新后的软件。这个如果是对于一个中心化的系统，大家可能会觉得是很难想象的，如果你发现你的软件中的安全漏洞，你干嘛不及时发布一个安全补丁呢。但是问题在于在区块链的世界里你怎么发布补丁，软件更新需要硬分叉来实现，还需要得到大部分矿工的支持。无论是比特币还是以太坊，硬分叉都不是随便搞的，这次以太坊搞的硬分叉，最后就造成了两条平行的链，而且要搞硬分叉要说明理由，否则别的矿工为什么要升级你的软件，而你一旦说明理由的话，那就会把安全漏洞的信息泄露出去，那么有恶意的攻击者在还没有来得及升级软件之前抢先发动攻击，这些都是区块链上不可篡改性带来的一些问题。

不可篡改性还导致另一个问题，即使我们发现了这个安全漏洞，已经有人进行恶意攻击了，我们想要冻结账户，终止交易都是很困难的。这个和我们如常生活的体验不太一样的地方，比如说你的银行卡的信息被泄露了，第一反应是通知银行，冻结账户，修改密码。很多人在刚接触区块链的时候也是同样的反应，发现比特币账户的私钥泄露出去了，怎么办，赶紧通知谁把账户给冻结了，没有办法冻结，要冻结的话只能软分叉，实质上要发布一个软件更新，有关要冻结的账户的交易都不予执行，这才可以冻结，对于个人来说，你的私钥泄露出去了搞一个软分叉，这是不可能的，对于普通人只能尽快将剩下的资金转到安全账户。

与之相关的一个问题，智能合约发布到区块链上就无法阻止别人对它的调用，比如说这次的盗币事件，黑客偷走了差不多1/3的以太币，还有2/3的以太币还在The DAO相关的子基金里，同样存在着安全的风险。我们传统直观的认为，智能合约出问题了，那就不能让别人去调用它了。但在区块链上你没办法组织别人对它的调用，你要阻止的话又要软分叉，增加一条新的规则，凡是调用这个智能合约的交易都不予执行。所以还剩的2/3的资金应该也及时转走，怎么转走？利用黑客的这个漏洞把钱转走。当时就有人建议这么做的，比如说你是个好人，你创建一个新的智能合约，把这个事情公布给大家，说现在要利用黑客的漏洞把剩下的钱转移到安全的智能合约，手段和重入攻击是一样的，只不过你的目的是好的，这个新的合约的目的是将来把钱退给大家。

3、没有什么是真的不可篡改的(Nothing is irrevocability.)

理论上，没有什么什么是绝对不可篡改的，比如分叉攻击可以回滚交易。“The DAO”事件中，以太坊开发团队通过软件升级方式强行改变了某些账户的状态，所以不能迷信不可篡改的特性。毕竟代码是死的人是活的，没有什么是绝对改不了的，连宪法都可以修宪，比如美国的宪法修正案，修宪是很难的，但是有必要的时候还是可以改的。区块链上是一样的，想要篡改是比较难的，但是遇到重大事件，要是想改还是改的了的。

4、合约语言设计的反思(Is solidity the right programming language?)

为什么会出现重入攻击这样的事情，从某种意义上来说，solidity语言上有一些反自然的特性，我们一般的理解是我给你转账，你是一个被动的接收者，你不可能再回来调用我。但solidity语言的特性是，我给你转账等于调用了你的fallback()函数，虽然表面上我没有调用任何的函数，结果你还可以倒过来调用我。这个和平时的生活体验不一样，所以容易忽视这样的安全漏洞。

有人提议应该改进为函数式编程语言，这样比较安全，不容易出现漏洞。而且从长远来看，要实现理论上证明智能合约的正确性。如果能够用形式化验证（formal verification）的方式，证明一段程序的正确性，那么将能够解决智能合约的漏洞问题，这个有些人曾经认为是智能合约的一个终极目标。但是形式化验证距离实用仍有很大的距离，一般只能证明一些逻辑简单的正确性，而且在证明过程要屏蔽掉很多实现上的细节，这样导致的结果就是即使这个程序从理论上可以证明的正确的，实际跑起来可能仍然有问题。从语言设计上来说，solidity有很多值得改进的地方，但是不是说我们就该用函数式编程语言，就应该用formal proof的方法证明程序的正确性，这个还有待探讨。

语言设计上的第二个反思是，编写智能合约的语言应该有什么表达能力。我们前面讲过比特币和以太坊的区别，比特币的脚本语言就是很简单的，表达能力很差，而以太坊的编程语言叫图灵完备的，凡是计算机能完成的任务，语言都能实现起来，但是图灵完备的表达能力是不是一个好事情。出现智能合约的漏洞后，有些人认为，应该选取一个表达能力适中的语言，它可以实现智能合约的功能，又不容易出现漏洞。但是难以设计出适当的语言，因为设计语言的时候很难预料将来所有可能出现的事情，也很难预料将来可能出现的所有安全攻击。

我们说比特币脚本是很简单的，但在实际应用中大部分矿工也只是接收几个常用的脚本，有一个安全脚本的白名单，如果交易的脚本不在白名单里面，很多矿工缺省情况下是不接受的。联想到现实中的合同，也会出现由于不严谨出现一些纠纷，所以通常使用模板写合同来规避这些问题。智能合约可以参考这种在模板基础上书写合约的方法，以后应该也会出现类似写智能合约的专门机构。所以大家不要对智能合约出现了各种各样的问题就对它丧失信心，智能合约的历史相对来说比较短的，最终还是会走向成熟。

5、开源软件漏洞的反思(Many eyeball fallacy.)

去中心化的系统一般都是开源的，因为需要所有节点执行同样的操作，才能达成共识。开源的一个好处就是增加合约的公信力，接受群众的监督；有人认为另一个好处是不容易出现安全漏洞，因为全世界有这么多双眼睛在看着这个代码，但实际情况并不如此，我们也看到了智能合约的代码出现了各种各样的漏洞，而且这个问题不是智能合约所特有的，其他开源的软件也有类似的问题。那为什么全世界有这么多双眼睛看着还出现了这么多漏洞？


理论上代码是开源的，但实际上去研究代码的人是很少的，例如像“The DAO”这样涉及到财产安全的项目，你要把你的钱投进去，投钱之前是不是需要检查下这个智能合约靠不靠谱，但是很多人都没有仔细看，看的人可能也没有足够的安全知识去检测里面的安全漏洞。有很多手机上的区块链钱包也是开源的，也是涉及到财产安全的，那么有多少人是去看了钱包的源代码的，有多少是看的懂的，大家都认为别人一定看过了，实际上可能没有谁认真看过，所以不要认为开源软件一定比不开源的软件安全，有很多人用的软件也不一定没有安全漏洞，历史上有很多开源软件的安全漏洞是很多年后发现的。所以这些关键性应用我们还是要小心仔细，需要自己去检查下智能合约是否有安全问题。

6、去中心化的反思(What does decentralization mean?)

区块链技术的追随者一般都是去中心理念的拥护者，这些人呢对于现实生活中中心化的管理方式不满意，所以在区块链的世界中去寻找一种全新的管理方式。这也是为什么以太坊开发团队在推出了硬分叉的解决方案后引发了这么大的争议，有很多人认为又回到了中心化的老路上面，你开发团队凭什么用一个软件升级就把一个人账上的钱转走，这个岂不是比中心化更中心化，在中心化的社会里要没收一个人的财产还需要通过法律各种各样的程序，28天以内还不一定能完成。

但是回想一下这个硬分叉的过程，是不是就是以太坊的开发团队说的算的。首先他们搞了一个投票，最后是大部分人投票支持硬分叉，但是更重要的是以太坊的团队是没有办法强迫大家支持这个投票结果，最后硬分叉能够成功是因为什么，是因为90%以上的矿工升级了软件，用行动支持了硬分叉，即使是这样还有少部分的矿工不支持硬分叉的方案，继续留在旧链上挖矿，那也是他们的自由，以太坊的团队没有什么办法强制他们转过来。所以说去中心化并不是说全自动化，让机器决定一切，不能有人为的干预，去中心化也不是说已经制定的规则就不能修改，而是说对规则的修改需要用去中心化的方式完成。我们平时说是用脚投票，区块链的世界里是用挖矿来投票的。这次硬分叉为什么成功，是因为绝大多数矿工认为以太坊的开发团队所做的决定是符合大部分人的利益的，大家不要低估了广大部分矿工的思想觉悟。如果以太坊开发团队为了一己私利做了什么决定，那广大工人阶级是不会跟着他们这样干的，这是关于去中心化的反思。

关于分叉的事情，我们一般认为分叉是件坏事，但是你仔细想一下分叉恰恰是去中心化的体现。在一个中心化的系统里，你是没有办法分叉的，你可以选择放弃，但是你不能选择分叉。比如以太坊创始人V神的故事，19岁的他非常喜欢打魔兽世界，直到有一天暴雪公司把他最喜欢的技能去掉了，他非常生气，多次找暴雪公司的人反馈，没有得到任何满意的结果，他一气之下就不玩了，后来他就想为什么会出现这种情况， 本质上是因为这个游戏是一个去中心化的游戏，决定权在公司，普通用户没有任何办法，所以他决定要去创建一个去中心的平台，用户不满意就有选择分叉的权利。所以存在分叉的选项恰恰是民主的体现。

7、去中心化与分布式的反思(decentralized ≠ distributed)

一个去中心化系统必然是分布式的，但是分布式系统不一定是去中心化的，即使这个系统运行在成千上万的计算机上，如果这些计算机都是由同一个组织所管辖的，那么也不能叫做去中心化的。在分布式的平台上可以运行一个中心化的应用，也可以运行一个去中心化的应用。

比特币和以太坊都是交易驱动的状态机（state machine），他的特点是让系统中几千台计算机做同一个操作，付出很大的代价来维护状态的一致性，而这个并不是分布式系统常用的工作模式，大多数的分布系统是让不同机器做不同的事情。然后再把各台机器的结果汇总起来，得到最后的结果，这样做的目的是为了比单机的速度要快。比如说1台计算机要完成的任务，可能需要一个星期，用10台计算机的分布式集群，可能一天就完成了，最理想的状况是得到线性加速，就是10台计算机的速度比1台计算机的速度要快10倍，但是实际应用当中，线性加速是很难达到的，因为存在任务切分，任务通讯，结果汇总都是有overlap，所以实际应用当中10台计算的速度可能相当于1台计算机的6、7倍，但是任然要比1台计算机要强，这样分布式系统才有意义。

状态机的模式不是这样的，状态机的目的不是比1台计算的处理速度快，而是为了容错。状态机最早的应用场景是什么，是一些mission critical applications(关键运算应用)，比如像airtraffic control, stock exchange, space shuttle，这些是一些状态机常见的应用场景，这些场景的特点是什么？这些应用程序必须无间断的向外提供服务，所以需要好几台计算机重复同一种操作，这样即使1台计算机宕机，这样还能向外提供服务，这是状态机模式的原理。这样付出的代价是什么，效率很低，几台机器合在一起比一台机器还要慢，因为它要同步状态，而且集群里的机器越多速度越慢，所以传统利用状态机的模型里面机器的数量是比较少的，有很多就是个位数的。像比特币、以太坊这样上千台机器重复同一组操作，在以前是从来没有过的。所以不要以为比特币和以太坊是分布式系统的常态，我们理解了这点就能理解智能合约比较适用的场景是什么，不要把智能合约EVM平台当成大规模计算或者大规模存储等服务，如果你这么做的话，不光是速度很慢，而且也是非常贵的，因为要耗很多汽油费、智能合约是用来编写控制逻辑的，只有那些需要在互不信任的实体之间建立共识的操作，才需要写在智能合约里。如果你需要大规模计算服务的话，可以用亚马逊的云服务平台。

### 13.0 ETH-美链 事件

2018年4月发生的事件，美链是发行在以太坊上的代币，这些代币没有自己的区块链，而是以智能合约的形式运行在以太坊的EVM平台上。发行这个代币的智能合约，对应的是以太坊状态树的一个节点，这个节点有它自己的账户余额，就相当于这个智能合约一共有多少个以太币，就是发行这个代币的智能合约它的资产有多少个以太币，然后在这个合约里每个账户上有多少个代币，这个是作为存储树中的变量，存储在智能合约的账户里。代币的发行、转账、销毁都是通过调用智能合约中的函数来实现的，这个也是跟以太坊上的以太币不太一样的地方，它不像以太坊一样需要挖矿来维护一个底层的基础链，像以太坊上每个账户有多少个以太币，这个是直接保存在状态树中的变量，然后以太坊上面两个账户转账是通过发布一个交易到区块链上，这个交易会打包到发布的区块链上面，而代币发生转账的话实际上就是智能合约上面两个账户之间发生转账，通过调用智能合约上的函数，就可以完成了。每个代币都可以制定自己的发行规则，比如某个代币是1个以太坊兑换100个代币，那么比如说从某个外部账户转1个以太币给这个智能合约，这个智能合约就可以给你在这个智能合约里的代币账户上发送100个代币，每个代币账户上有多少个代币的信息都是维护在存储树里面，发行这个代币的智能合约的存储树里面。

以太坊平台的出现让发行代币提供了方便，包括以前说的eos，这个在上线之前也是作为以太坊上的代币形式，上线的意思是有自己的基础链了，不用依附在以太坊上了。以太坊发行代币的标准为ERC20(Ethereum Request for Comments)。

<img src="https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220330142549281.png" alt="image-20220330142549281" style="zoom:33%;" />

这个是batchTransfer的函数：

![image-20220330142830043](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/image-20220330142830043.png)

这个函数有两个参数，第一个参数是一个数组，接收者地址的数组，第二个参数value是转账的金额，给每个人转多少。

uint256 amount = uint256(cnt) * _value;  //计算一共要转的总金额

require(cnt > 0 && cnt <= 20);  //检查接收者的数目不超过20个。

require(_value > 0 && balances[msg.sender] >= amount);  //检查发起调用函数的这个账户是否有这么多钱。

balances[msg.sender] = balances[msg.sender].sub(amount); //把发起调用的账户余额减去amount。

下面用一个循环给每个接收者接收value这么多的代币。

那么，问题出在哪？

uint256 amount = uint256(cnt) * _value; 当value的值很大的时候可能会发生溢出，那么amount算出来可能是个很小的一个值，所以从调用者的代币中减的时候是很小一部分的代币，但还是给每个receivers增加那么多value的代币。最后系统中相当于多发行了许多的代币。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/f55e5c1e99a8432a8d6acc1e0c82f10e.png)

第0号参数是_receivers数组在参数列表中的位置，这里是16进制的，0040对应的是4乘16=64，第一个参数出现在第64个字节的位置，一行有64个数字，1个16进制的数字需要用4个二进制的数字来表示，64乘4=256位，也就是说一行有32个字节。所以是从[2]行开始表示receivers，表示了数组的长度是2，[3] [4]两行是接受的地址。[1]行表示value的值。注意，[1]行的参数是80000...再乘以接收者2个，算出来的溢出正好是0。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/9d06b76928764975ba773bdc03d00b41.png)

红框中可以看到每个地址都是接收了很大数量的代币。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/5a70f1fdaaaf46ce80e05ea13cdfc5ed.png)

攻击使代币的价格造成致命性的打击，差不多快要归零了。

![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/403dda3b203144c494e31cb1491d54dd.png)

代币上市的交易所在发生攻击后暂停提币的功能，并且回滚了交易。

**反思**

在进行数学运算的时候一定要考虑溢出的可能性。solidity有一个safeMath库，里面提供的操作运算都会自动检测有没有出现溢出。

C语言里，两个数相乘会有一定的精度损失，再除以一个数，不一定会得到和另外一个数一模一样的数。但是在solidity里面是不存在的，因为两个数都是256位的整数，整数先进行乘法，再进行除法。

batchTransfer的加法和减法都用的safeMath库，只有乘法不小心没有使用，结果酿成了悲剧。曾经有人怀疑是不是故意的，但从事件的结果来看又不像使故意的。
![img](https://raw.githubusercontent.com/YiJingGuo/ETH-Learning-Notes/main/img/1b8c372e474a411f9f57800bc2daa534.png)

mul()函数中，先用a*b = c，再用c/a看看是否等于b，如果发生溢出的话，assert()会抛出异常。

### 14.0 总结

之前的章节说的技术方面的比较多，最后一节我们讲讲应用层面的东西。

现在社会上对区块链的争议是非常大的，有很多对区块链的质疑其实也是有道理的，为什么会有那么多人会质疑这个技术，其中一个原因是区块链的概念被滥用了。有些人把什么问题都往区块链上放，无论是效率上的问题，还是监管上的问题，好像区块链是解决一切问题的法宝，无论什么问题放在区块链上都可以解决了，这个是不对的。

接下来举个例子，国外有人提出把保险理赔业务放在区块链上，原因是现有的保险理赔业务非常的慢，可能需要几个星期甚至更长的时间，所以他们觉得放在区块链上之后，因为区块链的转账速度，比如说比特币等6个区块确认，大概一个小时的时间就可以完成，这比保险理赔几个星期的速度要好很多了，这个应用场景有什么问题吗？保险理赔的速度慢并不是支付技术的局限性，就是普通的银行转账就行，是因为理赔的内容需要人工审核才会比较慢，而在这方面上区块链并没有好的优势。

还要人提出用区块链做防伪溯源的，比如有人提出把有机蔬菜生成的全过程放在区块链上，他们认为区块链是不可篡改的，在区块链上可以查到有机蔬菜生产的全过程，所以是很好的应用场景，这个应用场景有什么问题吗？这个应用本身没有问题，但是是不是说只要你用区块链把生成的全过程记录下来了就能保证你买到的蔬菜就是有机的，这个是不一定的。如果这块地是施过化肥的，或者撒上农药的，但是被当作有机蔬菜记录在区块链里，那么区块链技术本身是检测不出来的，同样在运输销售过程中被人掉包了，这个也不是区块链能检测出来的。区块链的不可篡改性只是说这个内容写到区块链上是不可篡改的，但是写入到区块链的时候，本身写的就是假的内容，这个是没有办法检测出来的。

还有一些区块链技术的争议是和信任机制相关的，区块链的一个共识机制的目的是要在互不信任的实体之间建立共识，有些人认为这个本身就是一个伪命题，因为互不信任的实体之间是没有办法进行实体交易的。比如说网上购物，假设有某个电商网站，它是去中心化的，你不信任它，那还怎么会在上面买东西呢。比如说你把比特币付给这个电商网站，对方不给你发货怎么办，或者收到之后发行有质量问题怎么办。在一个中心化的世界里，你可以通过各种机构比如说信用卡有一些保护措施，在去中心化的世界这些是没有办法做到的。这个质疑有一定道理，但是中心化和去中心化的界限不是黑白分明的，在一个成功的商业模式里面，既可以有中心化的成分也可以有去中心化的成分，比特币只不过是一种支付方式，并不是说采用比特币作为支付方式的商业模式本身也必须是去中心化的。比如像亚马逊这样一个中心化的网站，他将来也可以采用比特币作为一种支付方式。

与之相关的一个问题是区块链的不可篡改性，比如说网上购物的例子，交易已经写在区块链上不可撤销了，但商品有问题要退货怎么办。但其实包括信用卡支付，退款也不是撤销原来的支付，而是在应用层发起一个新的交易。用比特币支付也是同样的道理的。

还要一些是和法律的保护相关的，传统的支付方式法律是有保护的，而区块链目前是缺乏监管的，有些认为这是好事情，去中心化可以不受到监管，但监管本身不一定是坏事情，没有法律监管同时也意味着没有司法保护。这里我们要注意的是，这些法律的监管和支付技术本身是没有什么关系的，信用卡被盗刷有些国家提供保护，但也有一些国家不提供保护。更重要的是比特币本来就不应该是和已有的支付方式进行竞争，在已有的支付方式已经解决的很好的领域不应该使用比特币。那哪些领域可以用呢，互联网把信息传播的很好，但对于跨界支付难度就要大很多，现有的体系当中支付渠道和信息传播渠道是分开的。有人说下一代互联网是价值交互网络，现在的互联网可以说是信息传播网络。未来的发展趋势就是支付渠道和信息传播渠道可以融合在一起，使得价值交互可以和信息传播一样的方便。

还要一些质疑是和支付效率相关的，有些人认为加密货币的支付方式是十分低效的还耗能。第一，加密货币本来就不是用于和已有的支付方式做竞争的。第二，随着技术和共识协议的发展，已经有加密货币的效率大大提高了。第三，我们评价一个支付方式的好坏要和特定的历史条件下去评判，加密货币在某些场景下已经是非常高效的了。

还有一些质疑是和智能合约相关的，有那么多安全漏洞，还不如用法律合同，老百姓还能看的懂。但程序化是个大趋势，Software is eating the world.(软件将会改变颠覆世界)任何事件领域在转型的早期都会有一些问题，这是正常的。将来会出现智能合约常用的一些模板。但是不要以为智能合约和去中心化会解决任何问题。比如The DAO如果没有出问题，它的模式也不一定好，虽然他是由投票来决定投资很民主，但是民主就一定是好事情吗，大多数人的决定就一定是正确的吗？

```
Democracy is the worst form of Government except for all those other forms that have been tried from time to time...
```

丘吉尔说的，民主制度不是一种最完美的制度，就像高考不是一种最好的选拔人才的制度，但相对于其他制度是不错的。但不要以为用民主制度，投票能解决所有的问题，如果真是这样的话，那这个世界太简单了。The DAO的例子也是一样的，去中心化就一定是好事情吗，不一定的，这些风投的投资基金需要去调查许多方面，绝不是像智能合约中简单的投投票就完成了。历史上98 99年的互联网淘金热的时候，有些人就是把所以的概念都往互联网上靠，互联网相关的股票涨的很厉害。当时有人在互联网上卖狗粮，赔的一塌糊涂，因为狗粮很沉，当时的邮费就要不少钱。不要因为某种商业模式用去中心化的概念包装一下就把它捧上天，中心化和去中心化的管理方式其实是各有利弊的，要具体问题具体分析。像The DAO这个项目本就不应该那么热。

