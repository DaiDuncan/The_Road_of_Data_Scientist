提供了常用的摘要算法如MD5，SHA1等等

- 摘要算法又称散列算法，哈希算法

注意：MD5是散列算法，不是加密算法



很多网站的密码都是经过MD5散列处理的，为的是保护用户账号的安全

但太多的人就是喜欢用简单的密码，比如123456，而123456 经过MD5散列后的值永远是 e10adc3949ba59abbe56e057f20f883e ，于是黑客就通过撞库，得到了用户的密码

- 网上一些所谓“解密”MD5的网站，仅仅是收集了大量的散列后的值而已，不是解密，而是撞库





用于加密相关的操作，代替了md5模块和sha模块，主要提供 SHA1, SHA224, SHA256, SHA384, SHA512 ，MD5 算法

```python
import hashlib
 
# ######## md5 ########
hash = hashlib.md5()	# 创建md5对象
# help(hash.update)
hash.update(bytes('123456', encoding='utf-8'))	# update方法只接收bytes类型数据作为参数
print(hash.hexdigest())	# 得到散列后的字符串
print(hash.digest())
 
### Output
MD5散列前为 ：123456
MD5散列后为 ：e10adc3949ba59abbe56e057f20f883e


######## sha1 ########
 
hash = hashlib.sha1()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
 
# ######## sha256 ########
 
hash = hashlib.sha256()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
 
 
######### sha384 ########
 
hash = hashlib.sha384()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
 
######### sha512 ########
 
hash = hashlib.sha512()
hash.update(bytes('admin', encoding='utf-8'))
print(hash.hexdigest())
```

以上加密算法虽然依然非常厉害，但时候存在缺陷，即：通过撞库可以反解。

所以，有必要对加密算法中添加**自定义key**再来做加密。

```python
import hashlib
 
# ######## md5 ########
 
hash = hashlib.md5(bytes('898oaFs09f',encoding="utf-8"))
hash.update(bytes('admin',encoding="utf-8"))
print(hash.hexdigest())
```

python内置还有一个 hmac 模块，它内部对我们**创建 key 和 内容** 进行进一步的处理，然后再加密

```python
import hmac
 
h = hmac.new(bytes('898oaFs09f',encoding="utf-8"))
h.update(bytes('admin',encoding="utf-8"))
print(h.hexdigest())
```





