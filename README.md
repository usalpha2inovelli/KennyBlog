# Blog
desc the Hbase on the AWS to collect the data

### 6-27 test the Happybase to save data to HBase with Pythin
### 6-28 test the AWS EC2 for the Hbase
1. 测试新加坡的 EMR，开通 3 个 Node 的服务，3 次，速度太慢了。几乎不能忍受。
1. 第一次开通 Hbase 不成功，提示帐户需要验证
2. 第二个和第三个开通后，无法使用密钥登录

### 成本
	对于AWS 的价格还是不完全了解。
	如果开通的服务不关闭就会一直收费，但关闭后，可能所有的配置数据就丢失了。
  而 HBase 需要至少三个节点，费用可能会有点高。
	
## 6-30
  AWS 一直不能测试。
  目前在做程序设计
  可能的方案：
  1. 利用 Pythonr Urllib 库，直接从 google 解析搜索得到的 Link，存入 Hbase 或 Redis，然后从连接取内容存到 Hbase
  1. 存取 Link 到 redis 时，可以做重复数据删除，避免取到重复的内容。
  1. 利用 Urllib 做解析，需要正则表达式对 Goolge 搜索页内容做 Link 提取。或者使用类似 BeatifulSoup 的框架来解析内容，从以后可能用到多线程或布式数据收集，暂时先不考虑复杂方案。尽量用最简单的 Urllib，自己多写代码。
  1. 需要测试 python 在 AWS 上的环境
  1. 需要测试 Hbase 在 AWS 的表现
  > 需要了解如何通过 Google 一次取到固定数量，并限定搜索的条件，通过程序代码


