# big_screen 特点

便利性工具, 结构简单  => 只需传数据就可以实现数据大屏展示



# 安装环境

```shell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple flask
```



# 输入数据

@me|反馈：不止这些输入数据

```python
self.echart1_data = {
            'title': '行业分布',
            'data': [
                {"name": "商超门店", "value": 47},
                {"name": "教育培训", "value": 52},
                {"name": "房地产", "value": 90},
                {"name": "生活服务", "value": 84},
                {"name": "汽车销售", "value": 99},
                {"name": "旅游酒店", "value": 37},
                {"name": "五金建材", "value": 2},
            ]
        }
        self.echart2_data = {
            'title': '省份分布',
            'data': [
                {"name": "浙江", "value": 47},
                {"name": "上海", "value": 52},
                {"name": "江苏", "value": 90},
                {"name": "广东", "value": 84},
                {"name": "北京", "value": 99},
                {"name": "深圳", "value": 37},
                {"name": "安徽", "value": 150},
            ]
        }
        self.echarts3_1_data = {
            'title': '年龄分布',
            'data': [
                {"name": "0岁以下", "value": 47},
                {"name": "20-29岁", "value": 52},
                {"name": "30-39岁", "value": 90},
                {"name": "40-49岁", "value": 84},
                {"name": "50岁以上", "value": 99},
            ]
        }
        self.echarts3_2_data = {
            'title': '职业分布',
            'data': [
                {"name": "电子商务", "value": 10},
                {"name": "教育", "value": 20},
                {"name": "IT/互联网", "value": 20},
                {"name": "金融", "value": 30},
                {"name": "学生", "value": 40},
                {"name": "其他", "value": 50},
            ]
        }
```



## 本地运行

```shell
cd big_screen-master;
python app.py;
```



# 结果展示

[Link: 视频](https://www.zhihu.com/zvideo/1302897123357339648)



# 在线部署

```shell
nohup python app.py  #在后台运行，关闭连接之后一样会运行

### 查看进程
ps -ef | grep python


### 如果发生错误的话，我们是无法知道哪里出错的，这时我们指定日志输出文件
nohup python -u app.py > robot.log 2>&1 &


### 停止在线运行
kill PID
```





# #小结

原理：基于Flask web服务，将所有的图都集合在两个网页

- 但是为什么显示中国地图呢？



