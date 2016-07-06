# Blog
desc the Hbase on the AWS to collect the data

### 6-27 test the Happybase to save data to HBase with Python
### 6-28 test the AWS EC2 for the Hbase
1. 测试新加坡的 EMR，开通 3 个 Node 的服务，3 次，速度太慢了。几乎不能忍受。
1. 第一次开通 Hbase 不成功，提示帐户需要验证
2. 第二个和第三个开通后，无法使用密钥登录

### 成本
- 对于AWS 的价格还是不完全了解。
- 如果开通的服务不关闭就会一直收费，但关闭后，可能所有的测试数据就丢失了。但配置可以保存，以再通过 Clone 建立同样的硬件部署。
  而 HBase 需要至少三个节点，费用可能会有点高。所以先用 Ec2 单机测试。之后转移到集群。
	
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

## 07-01
1. 通过正则表达式解析网页
2. 可能还是需要 BeautifulSoup 工具
3. Urllib 需要用到 Urllib3，以利用其资源池. 正在了解 Google 搜索结果的获取与解析
4. EC2 Hbase & Pyhton envirement deplopment

Q:
根据我的测试，这些功能在浏览器里是可以的，各种参数也可以指定。但在使用浏览器之外的程序语言发出时，就不工作了。好象是 Google 在收到浏览器的请求后，对 URL 做了一些加密后，再转发到搜索引擎，而通过程序发送的搜索请求是没有这个加密过程支持的，国内的 Baidu 目前也是这种机制。
原因是 Google 停止了对各种自动搜索工具的 API，以前应该是 Web search API，目前已经被 Customer Search API 所替代，目的就是防止大量的scrapy 程序通过 Google 收集数据。因此把这些服务转变成了收费的服务。

> https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=lenovo%20thinkpad

这个 Link 在 Browser 里是可以工作，但在程序（ex. Python)中发出这个请求到 Google，就只有一些无用的信息。之前有一些工具，例如 Urllib2 可以伪装浏览器，但我的测试结果是，这些功能已经被 Google 封闭。

例如，使用这段代码，可以得到 Header，和 Status 都是正常的情况下，返回的内容与浏览器中搜索的返回内容完全不同：
```python
import urllib

google = urllib.urlopen('https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=lenovo%20thinkpad')
print 'http header:\n', google.info()
print 'http status:', google.getcode()
print 'url:', google.geturl()
for line in google: 
    print line,
google.close()
```
我会继续测试这个功能，也请你们的技术员帮助验证。

# 07-03
测试了 AWS 的 Hadoop 集群功能，在 AWS 上使用集群的方法

# 07-05
测试了基于种 Google 的搜索方式，目前 Google 在搜索服务上除了 Web 页面外，采用了 Customer Searching Engine 服务，所以如果不能按 Google 最新的方式填写参数，则不能欺骗 Google 服务器通过搜索得到结果。
今天终于解决了这个困扰多天的问题，并整理了各种可能的 Google Header 中可以包括的 User-agent，但由于收集的内容较多，没能完全测试。

# 2016年7月6日

1. 整理 AWS 中 EMR 使用的文档，参见 [How to setup the Hbase on AWS](./AWS%20EMR%20HBase.md)
1. 随后整理 Google 搜索的文档。
1. 对 Hbase 数据库的表结构进行初步的设计和测试。

