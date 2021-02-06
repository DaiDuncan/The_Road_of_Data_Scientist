用`asyncio`提供的`@asyncio.coroutine`可以把一个generator标记为coroutine类型，然后在coroutine内部用`yield from`调用另一个coroutine实现异步操作。



为了简化并更好地标识异步IO，从Python 3.5开始引入了新的语法`async`和`await`，可以让coroutine的代码更简洁易读。

请注意，`async`和`await`是针对coroutine的新语法，要使用新的语法，只需要做两步简单的替换：

1. 把`@asyncio.coroutine`替换为`async`；
2. 把`yield from`替换为`await`。

```python
### 旧风格
@asyncio.coroutine
def hello():
    print("Hello world!")
    r = yield from asyncio.sleep(1)
    print("Hello again!")
    
    
### 新风格
async def hello():
    print("Hello world!")
    r = await asyncio.sleep(1)
    print("Hello again!")
```

---

注意新语法只能用在Python 3.5以及后续版本，如果使用3.4版本，则仍需使用上一节的方案。



