# [Win + anaconda](https://www.youtube.com/watch?v=QJgjcnuQqNI)

## 0 准备工作

0.1 安装Python的选择

- 跳过安装VSCode

python3.7有版本问题 => 历史版本：安装python3.6



0.2 anaconda命令终端

```shell
# (自动提示)更新conda版本
$ conda update -n base -c defaults conda
```



0.3 管理环境 => 不同的项目、代码所需的开发环境不一样

```shell
# 示例：创建pytorch环境
$ conda create -n pytorch python=3.7	#自动安装基本库

# 应用：激活环境
$ conda activate <env>
$ conda deactivate

# 查看：环境已有的包
$ pip list
$ conda list
```



## 1 安装pytorch

1.1 [pytorch官网](https://pytorch.org/get-started/locally/) => 安装命令

| 设置            | 说明                                                 |
| --------------- | ---------------------------------------------------- |
| 版本            | 稳定版：推荐1.1以上(加入训练过程的信息显示)          |
| 操作系统        |                                                      |
| 安装方式        | Win => 推荐conda<br />Linux => 推荐pip               |
| 语言            | python版本/c++                                       |
| CUDA：GPU的型号 | 任务管理器/设备管理 => Geforce官网查看：是否支持CUDA |
|                 | 情况1：没有英伟达，或者只有集显 => 选择`None`        |
|                 | 情况2：有Nividia显卡 => 推荐9.2(高版本有问题)        |

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517153005.png" alt="image-20210517153005285" style="zoom:80%;" />

```python
conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch
```



1.2 安装过程

![image-20210517153546532](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517153546.png)

如果安装过程比较慢：直接使用下载好的软件 => 复制到anaconda的安装目录 `packages`

- cudatoolkit 9.2
- pytorch

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517153835.png" alt="image-20210517153835510" style="zoom:80%;" />

## 2 检查|是否安装成功

```shell
#==# python
$ python	#显示版本号和输入控制符>>>
$ import torch
$ torch.cuda.is_available()	#返回True
```



## 补充|显卡

意义：训练加速(不影响pytorch的使用)

0 设置

- 显卡的类型@`任务管理器 设备-GPU正常显示`

  <img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517151427.png" alt="image-20210517151427150" style="zoom:80%;" />

- CUDA Toolkit



1 显卡的驱动

注意驱动的版本：

- CUDA 9.2 以上只支持大于396.26的显卡驱动
- => NVIDIA可更新驱动版本

```python
nvidia-smi
```

![image-20210517153125725](https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517153126.png)

NVIDIA可更新驱动版本

<img src="https://raw.githubusercontent.com/DaiDuncan/PicUploader/main/img2/20210517153210.png" alt="image-20210517153209577"  />



