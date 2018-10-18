---
layout: blog  
note: true  
title:  "Python爬虫实例 — 从百度贴吧下载多页话题内容"  
tags:  
- 网络爬虫  
- python  
background: blue  
background-image: http://pf6qvqv35.bkt.clouddn.com/CrawlerCrawlerCoverpage.jpg  
date:   2018-10-17 21:31   
category: 网络爬虫
---

上周网络爬虫课程中，留了一个实践：从百度贴吧下载多页话题内容。我完成的是从贴吧中一个帖子中爬取多页内容，与老师题目要求的从贴吧中爬取多页话题还是有一定区别的，况且，在老师讲评之后，我瞬间就发现了自己跟老师代码之间的差距了，我在代码书写上还是存在很多不规范不严谨的地方，而且也没有体现出面向对象的思想，所以，重新将这个题目做一遍，学习一下大佬是怎么写的。

## 实例：从百度贴吧下载多页话题内容 
先了解一下百度贴吧 （ [http://tieba.baidu.com/f?](http://tieba.baidu.com/f?)）我们定义几个函数：
* loadPage(url) 用于获取网页 
* writePage(html,filename) 用于将已获得的网页存储为本地文件 
* tiebaCrawler(url,beginpPage,endPage,keyword)用于调度，提供需要抓取的页面URLs
* main：程序主控模块，完成基本命令行交互接口


```python
import urllib.request
import urllib.parse

def loadPage(url):
    '''
        Function: Fetching url and accessing the webpage content
        url: the wanted webpage url
    '''
    headers = {
            'Accept': 'text/html',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.106 Safari/537.36',
    }
    print('To send HTTP request to %s ' % url)
    request = urllib.request.Request(url, headers=headers)
    
    return urllib.request.urlopen(request).read().decode('utf-8')

def writePage(html, filename):
    '''
        Function : To write the content of html into a local file
        html : The response content
        filename : the local filename to be used stored the response
    '''
    print('To write html into a local file %s ...' % filename)
    with open(filename, 'w', encoding='utf-8') as f:
        f.write(str(html))
    print('Work done!')
    print('---'*10)

def tiebaCrawler(url, beginPage, endPage, keyword):
    '''
        Function: The scheduler of crawler, is used to access every wanted url in turns
        url: the url of tieba's webpage
        beginPage: initial page
        endPage: end page
        keyword: the wanted keyword
    '''
    for page in range(beginPage,endPage+1):
        pn = (page - 1) * 50
        queryurl = url + '&pn=' + str(pn)
        filename = keyword + str(page) + '_tieba.html'
        writePage(loadPage(queryurl), filename)
        

if __name__ == '__main__':
    kw = input('Please input the wanted tieba\'s name:')
    beginPage = int(input('The beginning page number:'))
    endPage = int(input('The ending page number:'))
    
    # 百度贴吧查询 url 例子：http://tieba.baidu.com/f?ie=utf-8&kw=%E8%B5%B5%E4%B8%BD%E9%A2%96&red_tag=m2239217474
    url = 'http://tieba.baidu.com/f?ie=utf-8&'
    key = urllib.parse.urlencode({'kw':kw})
    queryurl = url + key
    
    tiebaCrawler(queryurl, beginPage, endPage, kw)
```

    Please input the wanted tieba's name:赵丽颖
    The beginning page number:1
    The ending page number:3
    To send HTTP request to http://tieba.baidu.com/f?ie=utf-8&kw=%E8%B5%B5%E4%B8%BD%E9%A2%96&pn=0 
    To write html into a local file 赵丽颖1_tieba.html ...
    Work done!
    ------------------------------
    To send HTTP request to http://tieba.baidu.com/f?ie=utf-8&kw=%E8%B5%B5%E4%B8%BD%E9%A2%96&pn=50 
    To write html into a local file 赵丽颖2_tieba.html ...
    Work done!
    ------------------------------
    To send HTTP request to http://tieba.baidu.com/f?ie=utf-8&kw=%E8%B5%B5%E4%B8%BD%E9%A2%96&pn=100 
    To write html into a local file 赵丽颖3_tieba.html ...
    Work done!
    ------------------------------
    

## 心得收获
1. 将功能尽量拆分开来，每个函数只做一件事儿，控制好函数的输入和输出，体现面向对象的思想。
2. 每个函数都要写备注（格式如上文代码中），讲清楚两件事，a.函数是做什么用的？b.函数的参数各表示什么？交代清楚了这些，不仅可以大大增加代码的可读性，还可以督促自己规范代码。
