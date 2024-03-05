# Flow_Crawler - 交互式Web网站爬虫
<img alt="Release" src="https://img.shields.io/badge/python-3.8+-blue">
<img alt="Release" src="https://img.shields.io/badge/pyppeteer-1.0.2-blueviolet">

> ​		Flow Crawler是一款交互式爬虫工具，不仅可以爬取页面中的超链接，还能获取通过页面的各种交互事件触发所发起的请求，支持代理设置，主要目的是配合WAF进行白流量测试。目前支持以下事件的触发：
>
> - [x] Form表单；
> - [x] 各种标签的onclick事件；
> - [x] a标签内的JavaScript代码;
> - [ ] 下拉列表等隐藏式交互事件触发；
> - [ ] 不带有onclick属性的按钮交互事件触发；
> - [ ] 非form表单的输入框输入；
> - [ ] 其他;

## Topology

此脚本需要连接外网环境，父节点为目标网站，爬虫递归下去对主网站包含的链接都进行访问，对每个子节点网站中的各种事件都进行触发，包括但不限于 输入框，button等。此爬虫和internet中间需要部署一个透明桥waf，由于爬虫流量都是模拟用户正常点击流量，所以一旦waf发生告警则基本确定是误报。

## Installation

```bash
# 安装python依赖包
pip install -r requirements.txt
# 爬取http://example.com
python crawler.py http://example.com
```

## Options

```
可选参数：
  -h, --help             帮助显示此帮助消息并退出
  -t , --timeout         所有页面请求的超时时间，默认为5秒
  --cookie               设置http请求Cookie
  --proxy-server         设置爬虫的代理地址，爬虫会将所有请求转发至该服务端口
  --headless             浏览器的无头操作模式，添加此选项后不会显示浏览器界面
  --exclude-links        如果链接包含此关键字，支持正则表达式
  --crawl-link-type      爬网程序爬网的网络资源类型，默认爬取xhr、fetch、document
  --prohibit-load-type   禁止加载的网络资源类型，默认禁止加载image,media,font,manifest，该配置选项是优先级最高的
  --crawl-external-links 设置是否爬取外部链接爬网，默认情况下，仅对同一网站链接进行爬网（不推荐）
  --intercept-request    开启http请求拦截，只有当该选项开启后，--prohibit-load-type参数才生效

```
> 注意：若配置了`prohibit-load-type`配置中包含`document`类型，则`crawl-link-type`配置中的`document`类型则失效。

## Examples

```bash
# 默认启动方式
python3 crawler.py http://example.com

# 不爬取带有以下关键字的链接，支持正则表达式
python3 crawler.py http://example.com --exclude-links "logout|Logout"

# 设置Cookie，则爬虫所有的请求都会携带该Cookie
python3 crawler.py http://example.com --cookie "PHPSESSID=gbo3fci62fpig5vp4fq6a950h2; security=impossible"

# 设置代理
python3 crawler.py http://example.com --proxy-server "127.0.0.1:8000"
```

## Todo


- [ ] 性能优化，合理使用协程加快爬取速度并增加访问频率参数；
- [ ] 解决pyppeteer模块再拦截请求时，对于重定向的请求无法正确处理，导致暂时先关闭对资源类型加载的控制；
- [ ] 增加script（在考虑是否使用装饰器实现）加载模块，用于自定义操作，例如识别到用户登录界面，若有对应脚本可进行爆破;

