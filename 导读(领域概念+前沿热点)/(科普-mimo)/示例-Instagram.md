三要素：

- app
- server
- internet



# APP

## formatting

APP包含格式的代码

- 网页则是使用HTML组织格式

```python
｛Photo
	image_file: cat.png
    user_account: joe
    is_story_photo: true
｝
```





# 服务器

上传照片到服务器



## Database



### SQL

响应流程：

1. APP请求
2. 服务器查找@SQL
3. 排序：比如最新@SQL
4. 服务器发出响应/数据
5. APP：显示照片