编写Python脚本来获取Google搜索结果的示例

这篇文章主要介绍了编写Python脚本来获取Google搜索结果的示例,也是利用Python编写爬虫的一个简单实现,需要的朋友可以参考下

前一段时间一直在研究如何用python抓取搜索引擎结果，在实现的过程中遇到了很多的问题，我把我遇到的问题都记录下来，希望以后遇到同样问题的童鞋不要再走弯路。

1. 搜索引擎的选取

　　选择一个好的搜索引擎意味着你能够得到更准确的搜索结果。我用过的搜索引擎有四种：Google、Bing、Baidu、Yahoo!。 作为程序员，我首选Google。但当我看见我最爱的Google返回给我的全是一堆的js代码，根本没我想要的搜索结果。于是我转而投向了Bing的阵营，在用过一段时间后我发现Bing返回的搜索结果对于我的问题来说不太理想。正当我要绝望时，Google拯救了我。原来Google为了照顾那些禁止浏览器使用js的用户，还有另外一种搜索方式，请看下面的搜索URL：

https://www.google.com.hk/search?hl=en&q=hello

　　hl指定要搜索的语言，q就是你要搜索的关键字。 好了，感谢Google，搜索结果页面包含我要抓取的内容。

　　PS: 网上很多利用python抓取Google搜索结果还是利用 https://ajax.googleapis.com/ajax/services/search/web... 的方法。需要注意的是这个方法Google已经不再推荐使用了，见 https://developers.google.com/web-search/docs/ 。Google现在提供了Custom Search API， 不过API限制每天100次请求，如果需要更多则只能花钱买。

2. Python抓取并分析网页

　　利用Python抓取网页很方便，不多说，见代码:

def search(self, queryStr):
   queryStr = urllib2.quote(queryStr)
   url = 'https://www.google.com.hk/search?hl=en&q=%s' % queryStr
   request = urllib2.Request(url)
   response = urllib2.urlopen(request)
   html = response.read()
   results = self.extractSearchResults(html)
　　第6行的 html 就是我们抓取的搜索结果页面源码。使用过Python的同学会发现，Python同时提供了urllib 和 urllib2两个模块，都是和URL请求相关的模块，不过提供了不同的功能，urllib只可以接收URL，而urllib2可以接受一个Request类的实例来设置URL请求的headers，这意味着你可以伪装你的user agent 等(下面会用到)。

　　现在我们已经可以用Python抓取网页并保存下来，接下来我们就可以从源码页面中抽取我们想要的搜索结果。Python提供了htmlparser模块，不过用起来相对比较麻烦，这里推荐一个很好用的网页分析包BeautifulSoup，关于BeautifulSoup的用法官网有详细的介绍，这里我不再多说。

　　利用上面的代码，对于少量的查询还比较OK，但如果要进行上千上万次的查询，上面的方法就不再有效了， Google会检测你请求的来源，如果我们利用机器频繁爬取Google的搜索结果，不多久就Google会block你的IP，并给你返回503 Error页面。这不是我们想要的结果，于是我们还要继续探索

　　前面提到利用urllib2我们可以设置URL请求的headers,  伪装我们的user agent。简单的说，user agent就是客户端浏览器等应用程序使用的一种特殊的网络协议， 在每次浏览器（邮件客户端/搜索引擎蜘蛛）进行 HTTP 请求时发送到服务器，服务器就知道了用户是使用什么浏览器（邮件客户端/搜索引擎蜘蛛）来访问的。 有时候为了达到一些目的，我们不得不去善意的欺骗服务器告诉它我不是在用机器访问你。

　　于是，我们的代码就成了下面这个样子:

user_agents = ['Mozilla/5.0 (Windows NT 6.1; WOW64; rv:23.0) Gecko/20130406 Firefox/23.0', \
     'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:18.0) Gecko/20100101 Firefox/18.0', \
     'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/533+ \
     (KHTML, like Gecko) Element Browser 5.0', \
     'IBM WebExplorer /v0.94', 'Galaxy/1.0 [en] (Mac OS X 10.5.6; U; en)', \
     'Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.1; WOW64; Trident/6.0)', \
     'Opera/9.80 (Windows NT 6.0) Presto/2.12.388 Version/12.14', \
     'Mozilla/5.0 (iPad; CPU OS 6_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) \
     Version/6.0 Mobile/10A5355d Safari/8536.25', \
     'Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) \
     Chrome/28.0.1468.0 Safari/537.36', \
     'Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.0; Trident/5.0; TheWorld)']
 def search(self, queryStr):
   queryStr = urllib2.quote(queryStr)
   url = 'https://www.google.com.hk/search?hl=en&q=%s' % queryStr
   request = urllib2.Request(url)
   index = random.randint(0, 9)
   user_agent = user_agents[index]
   request.add_header('User-agent', user_agent)
   response = urllib2.urlopen(request)
   html = response.read()
   results = self.extractSearchResults(html)
　　不要被user_agents那个list吓到，那其实就是10个user agent 字符串，这么做是让我们伪装的更好一些，如果你需要更多的user agent 请看这里 UserAgentString。

17-19行表示随机选择一个user agent 字符串，然后用request 的add_header方法伪装一个user agent。

　　通过伪装user agent能够让我们持续抓取搜索引擎结果，如果这样还不行，那我建议在每两次查询间随机休眠一段时间，这样会影响抓取速度，但是能够让你更持续的抓取结果，如果你有多个IP，那抓取的速度也就上来了。

　　github上有本文所有源代码，需要的同学可从下面的网址下载:

https://github.com/meibenjin/GoogleSearchCrawler