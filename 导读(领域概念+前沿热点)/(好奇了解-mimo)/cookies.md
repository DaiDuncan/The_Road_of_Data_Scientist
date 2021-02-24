# what|是什么？

plain text：存储在本地local PC中

```python
"user=kevinMcK; expires=Wed, 26 Dec 2019 12:00:00 UTC;"
```



浏览器请求服务时，服务器返回页面的同时也包含了cookies => 浏览器显示页面并且保存

- 至少有一对name-value
- 文件大小<4KB



## tracking pixel

网页上的一小块：基于tracking domain要求，添加cookie

通常不可见：但仍可以请求tracking server并且保存tracking cookie

```html
<imd height="0" width="0" alt="" src="http://track.com">
```





# 效果

通常只在一个domain上有效，比如来自getmimo.com的cookie只在`getmimo.com`上有效

- 在google.com无效
- 在app.getmimo.com无效



可以设置：对所有的subdomains有效

比如设置为`facebook.com`都有效：

- 那么about.facebook.com
- blog.facebook.com
- ads.facebook.com都有效



# 作用

感兴趣的对象：

- 企业
- 广告商
- 政府

### tracking cookie

通常用来标记用户和行为

- online status
- user interface设置

```python
TrackingID=3984720234; Ip=11.0.1.1; origin=stuff.com
```





### unique authentication cookie

绑定用户：这样不需要每一次重新登录 `UserID=B76SDFE44LN`

- 防止盗取账户：检查是否是相同的浏览器/地址等

设置expiry time到期时间



### 第三方tracking tools

记录不同网站的活动





# 创建cookie

```javascript
function addCookie(cname, value){
    document.cookie = cname + "=" + value + ";"
}

addCookie("username", "kevinMcallister");

console.log(document.cookie);
```

