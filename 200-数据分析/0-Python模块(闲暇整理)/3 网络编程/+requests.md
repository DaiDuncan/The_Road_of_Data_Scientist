Python内置的urllib模块，用于访问网络资源。但是，它用起来比较麻烦，而且，缺少很多实用的高级功能。

更好的方案是使用requests。它是一个Python第三方库，处理URL资源特别方便。





# Requests (第三方库)

发起HTTP请求，并获取请求的返回值

Requests 是使用 **Apache2** Licensed 许可证的，基于Python开发的HTTP 库。

其在Python内置模块的基础上进行了高度的封装，使用Requests可以轻而易举的完成**浏览器可有的任何操作**。

```python
# 安装模块
pip3 install requests

# 1无参数实例
import requests
 
ret = requests.get('https://github.com/timeline.json')
print(ret.url)
print(ret.text)
 
# 2有参数实例
#GET请求
import requests
 
payload = {'key1': 'value1', 'key2': 'value2'}
ret = requests.get("http://httpbin.org/get", params=payload)
 
print(ret.url)
print(ret.text)

==============================================
# 基本POST实例
import requests
 
payload = {'key1': 'value1', 'key2': 'value2'}
ret = requests.post("http://httpbin.org/post", data=payload)
 
print(ret.text)
 
    
# 发送请求头和数据实例
#POST请求
import requests
import json
 
url = 'https://api.github.com/some/endpoint'
payload = {'some': 'data'}
headers = {'content-type': 'application/json'}
 
ret = requests.post(url, data=json.dumps(payload), headers=headers)
 
print(ret.text)
print(ret.cookies)

==============================================
#其他请求
requests.get(url, params=None, **kwargs)
requests.post(url, data=None, json=None, **kwargs)
requests.put(url, data=None, **kwargs)
requests.head(url, **kwargs)
requests.delete(url, **kwargs)
requests.patch(url, data=None, **kwargs)
requests.options(url, **kwargs)
 
# 以上方法均是在此方法的基础上构建
requests.request(method, url, **kwargs)
```







# requests

通过GET访问一个页面，只需要几行代码：

```python
>>> import requests
>>> r = requests.get('https://www.douban.com/') # 豆瓣首页
>>> r.status_code
200
>>> r.text
r.text
'<!DOCTYPE HTML>\n<html>\n<head>\n<meta name="description" content="提供图书、电影、音乐唱片的推荐、评论和...'
```





对于带参数的URL，传入一个dict作为`params`参数：

```python
>>> r = requests.get('https://www.douban.com/search', params={'q': 'python', 'cat': '1001'})
>>> r.url # 实际请求的URL
'https://www.douban.com/search?q=python&cat=1001'
```





requests自动检测编码，可以使用`encoding`属性查看：

```python
>>> r.encoding
'utf-8'
```





无论响应是文本还是二进制内容，我们都可以用`content`属性获得`bytes`对象：

```python
>>> r.content
b'<!DOCTYPE html>\n<html>\n<head>\n<meta http-equiv="Content-Type" content="text/html; charset=utf-8">\n...'
```





对于特定类型的响应，例如JSON，可以直接获取：

```python
### json格式的数据
>>> r = requests.get('https://query.yahooapis.com/v1/public/yql?q=select%20*%20from%20weather.forecast%20where%20woeid%20%3D%202151330&format=json')
>>> r.json()
{'query': {'count': 1, 'created': '2017-11-17T07:14:12Z', ...
```





需要传入HTTP Header时，我们传入一个dict作为`headers`参数：

```python
>>> r = requests.get('https://www.douban.com/', headers={'User-Agent': 'Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit'})
>>> r.text
'<!DOCTYPE html>\n<html>\n<head>\n<meta charset="UTF-8">\n <title>豆瓣(手机版)</title>...'
```



要发送POST请求，只需要把`get()`方法变成`post()`，然后传入`data`参数作为POST请求的数据：

```python
>>> r = requests.post('https://accounts.douban.com/login', data={'form_email': 'abc@example.com', 'form_password': '123456'})
```



requests默认使用`application/x-www-form-urlencoded`对POST数据编码。如果要传递JSON数据，可以直接传入json参数：

```python
params = {'key': 'value'}
r = requests.post(url, json=params) # 内部自动序列化为JSON
```



类似的，上传文件需要更复杂的编码格式，但是requests把它简化成`files`参数：

```python
>>> upload_files = {'file': open('report.xls', 'rb')}
>>> r = requests.post(url, files=upload_files)
```

在读取文件时，注意务必使用`'rb'`即二进制模式读取，这样获取的`bytes`长度才是文件的长度。

把`post()`方法替换为`put()`，`delete()`等，就可以以PUT或DELETE方式请求资源。





除了能轻松获取响应内容外，requests对获取HTTP响应的其他信息也非常简单。例如，获取响应头：

```python
>>> r.headers
{Content-Type': 'text/html; charset=utf-8', 'Transfer-Encoding': 'chunked', 'Content-Encoding': 'gzip', ...}
>>> r.headers['Content-Type']
'text/html; charset=utf-8'
```





requests对Cookie做了特殊处理，使得我们不必解析Cookie就可以轻松获取指定的Cookie：

```python
>>> r.cookies['ts']
'example_cookie_12345'
```





要在请求中传入Cookie，只需准备一个dict传入`cookies`参数：

```python
>>> cs = {'token': '12345', 'status': 'working'}
>>> r = requests.get(url, cookies=cs)
```





最后，要指定超时，传入以秒为单位的timeout参数：

```python
>>> r = requests.get(url, timeout=2.5) # 2.5秒后超时
```





# #应用

## 多核版知乎爬虫

1个进程分析图片地址；

2个进程8线程下载图片； 

1个进程4线程解决第一次下载失败的图片；

主进程的主线程实现程序监听，判断是否终止系统，优雅退出程序！

需要在F盘新建一个img文件夹（可自己修改）

```python
import requests,json,os,threading,time,logging
from bs4 import BeautifulSoup
from multiprocessing import Process,Queue
from multiprocessing.queues import Empty
from urllib.request import urlretrieve


logging.basicConfig(level=logging.ERROR,filename='failed_img.log')

# 抓取地址
answers_url='https://www.zhihu.com/api/v4/questions/29815334/answers?data[*].author.follower_count%2Cbadge[*].topics=&data[*].mark_infos[*].url=&include=data[*].is_normal%2Cadmin_closed_comment%2Creward_info%2Cis_collapsed%2Cannotation_action%2Cannotation_detail%2Ccollapse_reason%2Cis_sticky%2Ccollapsed_by%2Csuggest_edit%2Ccomment_count%2Ccan_comment%2Ccontent%2Ceditable_content%2Cvoteup_count%2Creshipment_settings%2Ccomment_permission%2Ccreated_time%2Cupdated_time%2Creview_info%2Crelevant_info%2Cquestion%2Cexcerpt%2Crelationship.is_authorized%2Cis_author%2Cvoting%2Cis_thanked%2Cis_nothelp%2Cis_labeled&limit=5&offset=0&platform=desktop&sort_by=default'

# 待下载图片队列
img_queue = Queue()
# 下载图片失败队列
bad_queue = Queue()

# 请求头
headers={
    'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:64.0) Gecko/20100101 Firefox/64.0',
    'Host':'www.zhihu.com'
}


'''
请求知乎回答数据
:param answers_url: 知乎问题的第一个回答请求地址
:param imgq: 待下载图片队列
:param count: 递归结束标识，用于判断是否最后一页
:return: 
'''
def getR(answers_url, imgq, count=0):

    while True:
        try:
            r = requests.get(answers_url, headers=headers)
        except BaseException:
            continue
        else:
            rj = str(r.json()).replace("True", "\"true\"").replace("False", "\"false\"")
            rj = json.loads(json.dumps(eval(rj)))

            for x in rj['data']:
                content = x['content']
                soup = BeautifulSoup(content, 'lxml')
                # 查找img标签并且class属性等于origin_image zh-lightbox-thumb的元素
                for img in soup.find_all('img', attrs={'class': 'origin_image zh-lightbox-thumb'}):
                    # 保存图片到img_queue队列
                    imgq.put(img['data-original'])
                    print('保存%s到img_queue' % os.path.split(img['data-original'])[1])
            if rj['paging']['is_end'] != 'true':
                answers_url = rj['paging']['next']
            else:
                print('request_pro 执行完毕')
                # 最后一页
                break


'''
下载进程：分别创建四个线程，用于下载图片
:param imgq:    待下载图片队列
:param badq:    下载图片失败队列
:param pname:   进程名称
:return: 
'''
def download_pro(imgq,badq,pname):

    # 这里设置name可用于区分是哪个进程的哪个线程
    t1 = threading.Thread(target=download,args=(imgq,badq),name='%s - 下载线程1' % pname)
    t2 = threading.Thread(target=download,args=(imgq,badq),name='%s - 下载线程2' % pname)
    t3 = threading.Thread(target=download,args=(imgq,badq),name='%s - 下载线程3' % pname)
    t4 = threading.Thread(target=download,args=(imgq,badq),name='%s - 下载线程4' % pname)

    t1.start()
    t2.start()
    t3.start()
    t4.start()

    t1.join()
    t2.join()
    t3.join()
    t4.join()


'''
下载图片函数
:param imgq:    待下载图片队列
:param badq:    下载图片失败队列
:return: 
'''
def download(imgq,badq):

    thread_name = threading.current_thread().name
    while True:
        try:
            # 设置timeout=10,10秒后队列都拿取不到数据，那么数据已经下载完毕
            c = imgq.get(True, timeout=10)
            name = os.path.split(c)[1]
        except Empty:
            print('%s 执行完毕' % thread_name)
            # 退出循环
            break
        else:
            imgcontent = requests.get(c)
            ''' 图片请求失败,将图片地址放入bad_queue,等待bad_pro处理'''
            if imgcontent.status_code != 200:
                print('%s 下载%s失败' % (thread_name,name))
                badq.put(c)
                continue
            with open(r'F:\img\%s' % name, 'wb') as f:
                f.write(imgcontent.content)
                print('%s 下载%s成功' % (threading.current_thread().name, name))


'''
创建四个线程用于尝试bad_queue队列中的图片，
:param badp: bad_queue
:return: 
'''
def again_pro(badq):

    b1 = threading.Thread(target=again_download, args=(badq,),name='重下线程1')
    b2 = threading.Thread(target=again_download, args=(badq,),name='重下线程2')
    b3 = threading.Thread(target=again_download, args=(badq,),name='重下线程3')
    b4 = threading.Thread(target=again_download, args=(badq,),name='重下线程4')

    b1.start()
    b2.start()
    b3.start()
    b4.start()


'''
尝试重复下载bad_queue队列中的图片
:param badq: bad_queue
:return: 
'''
def again_download(badq):

    while True:
        c = badq.get(True)
        name = os.path.split(c)[1]
        # 尝试重复下载5次，如果5次均下载失败，记录到错误日志中
        for x in range(5):
            try:
                # 个人感觉使用urlretrieve下载的成功率要高些，哈哈^_^
                urlretrieve(c,r'F:\img\%s'%name)
            except BaseException as e:
                if x==4:
                    logging.error('%s 重复下载失败,error：[%s]'% (c,repr(e)))
                # 下载失败; 再次尝试
                continue
            else:
                # 下载成功；跳出循环
                print('%s 在第%s次下载%s成功' % (threading.current_thread().name,(x+1),name))
                break


'''
监听器，用于关闭程序，10秒刷新一次
:param imgq: img_queue
:param badq: bad_queue
:return: 
'''
def monitor(imgq,badq):

    while True:
        time.sleep(10)
        if imgq.empty() and badq.empty():
            print('所有任务执行完毕，40秒后系统关闭')
            count = 39
            while count > 0:
                time.sleep(1)
                print('距离系统关闭还剩：%s秒'%count)
                count-=1
            break


if __name__=='__main__':

    # 请求数据进程
    request_pro = Process(target=getR, args=(answers_url, img_queue))

    ''' 下载图片进程[first]和[second] '''
    download_pro_first = Process(target=download_pro, args=(img_queue,bad_queue,'download process first'))
    download_pro_second = Process(target=download_pro, args=(img_queue,bad_queue,'download process second'))

    # 处理[first]和[second]进程下载失败的图片
    bad_pro = Process(target=again_pro,args=(bad_queue,))
    bad_pro.daemon = True

    # req_pro进程启动
    request_pro.start()
    print('3秒之后开始下载 >>>>>>>>>>>>>>>')
    time.sleep(3)

    # first 和 second 进程启动
    download_pro_first.start()
    download_pro_second.start()

    # 重复下载进程启动
    bad_pro.start()

    # 监听线程：判断系统是否退出
    request_pro.join()
    print('监听线程启动 >>>>>>>>>>>>>>>')
    sys_close_t = threading.Thread(target=monitor,args=(img_queue,bad_queue,))
    sys_close_t.start()
```





# #参考文献

[Link: 教程|廖雪峰](https://www.liaoxuefeng.com/wiki/1016959663602400/1183249464292448#0)