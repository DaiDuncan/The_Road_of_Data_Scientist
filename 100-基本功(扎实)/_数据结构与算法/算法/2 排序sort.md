学习排序算法的目的不是自己实现排序算法在工作中使用，而是通过练习排序算法来提升思维逻辑能力和组织代码的能力

![image-20210124150945350](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210124150945.png)

放在内存的称为内排序，需要使用外存的称为外排序

- 外部排序？



## 时间复杂度记忆

| **排序算法**     | **平均时间复杂度** | **最坏时间复杂度** | **最好时间复杂度** | **空间复杂度** | **稳定性** |
| ---------------- | ------------------ | ------------------ | ------------------ | -------------- | ---------- |
| **冒泡排序**     | O(n²)              | O(n²)              | O(n)               | O(1)           | 稳定       |
| **直接选择排序** | O(n²)              | O(n²)              | O(n)               | O(1)           | 不稳定     |
| **直接插入排序** | O(n²)              | O(n²)              | O(n)               | O(1)           | 稳定       |
| **快速排序**     | O(nlogn)           | O(n²)              | O(nlogn)           | O(nlogn)       | 不稳定     |
| **堆排序**       | O(nlogn)           | O(nlogn)           | O(nlogn)           | O(1)           | 不稳定     |
| **希尔排序**     | O(nlogn)           | O(ns)              | O(n)               | O(1)           | 不稳定     |
| **归并排序**     | O(nlogn)           | O(nlogn)           | O(nlogn)           | O(n)           | 稳定       |
| **计数排序**     | O(n+k)             | O(n+k)             | O(n+k)             | O(n+k)         | 稳定       |
| **基数排序**     | O(N*M)             | O(N*M)             | O(N*M)             | O(M)           | 稳定       |

1 归并排序可以通过手摇算法将空间复杂度降到O(1)，但是时间复杂度会提高。

2 基数排序时间复杂度为O(N*M)，其中N为数据个数，M为==数据位数==。



- 时间复杂度

冒泡、选择、直接排序需要两个for循环，每次只关注一个元素，平均时间复杂度为O(n²)）（一遍找元素O(n)，一遍找位置O(n)）

快速、归并、希尔、堆==基于二分思想==，log以2为底，平均时间复杂度为O(nlogn)（一遍找元素O(n)，一遍找位置O(logn)）





- 稳定性

排序算法的稳定性：排序前后==相同元素的相对位置不变==，则称排序算法是稳定的；否则排序算法是不稳定的





# 交换排序

## 冒泡排序bubble

核心思想：相邻的两个数据进行比较，确保较大的那个数在右侧

- 第一次：找到了max(功能可复用)

```python
def pop_sort(lst):
    for i in range(len(lst)-1, 0, -1):	#逆序：第一个max_index在末尾
        move_max(lst, i)


def move_max(lst, max_index):
    """
    将索引0到max_index这个范围内的最大值移动到max_index位置上
    :param lst:
    :param max_index:
    :return:
    """
    for i in range(max_index):
        if lst[i] > lst[i+1]:
            lst[i], lst[i+1] = lst[i+1], lst[i]


if __name__ == '__main__':
    lst = [7, 1, 4, 2, 3, 6]
    pop_sort(lst)
    print(lst)
```



![img](https://img-blog.csdn.net/20161009190728886)





## 快速排序quick：基准值分区 + 递归

1. 从待排序数组中随意选中一个数值，==作为基准值==
2. 移动待排序数组中的元素，使得基准值左侧的数值都小于等于它，右侧的数值大于等于它
3. 基准值将原来的数组分为两部分，针对这两部分，重复步骤1，2， 3

=> 必然要使用递归算法

```python
### 基于基准值(随意选择)进行分区
# partition函数返回基准值最后的索引
def partition(lst,start,end):
    """
    用lst[start] 做基准值，在start到end这个范围进行分区
    """
    pivot = lst[start]

    while start < end :
        while start < end and lst[end] >= pivot:
            end -= 1
        lst[start] = lst[end]

        while start < end and lst[start] <= pivot:
            start += 1
        lst[end] = lst[start]

    lst[start] = pivot
    return start	


### 递归部分
def my_quick_sort(lst,start,end):
    if start>= end:
        return

    index = partition(lst,start,end)
    my_quick_sort(lst,start,index-1)
    my_quick_sort(lst,index+1,end)
```

分区虽然没有让整个数组变得有序，但是让基准值找到了自己应该在的位置，对左右两侧重复分区动作，每一次分区动作都至少让一个元素找到自己应该在的位置。



验证代码：

```python
if __name__ == '__main__':
    lst = [4,3,2,4,1,5,7,2]
    my_quick_sort(lst,0,len(lst)-1)
    print lst
```

![img](https://img-blog.csdn.net/20161009191035090)









# 选择排序

## 直接选择排序select：每一轮选最值

先从0这个位置到n这个位置找出最小值，然后将这个最小值与a[0]交换，然后呢，a[1]到a[n]就是我们接下来要排序的序列

- 每一次，我们都从序列中找出一个最小值，然后把它与待定序列的第一个元素交换位置
- 这样下去，待排序的元素就会越来越少，直到最后一个

```python
def select_sort(lst):
    for i in range(len(lst)):
        min = i
        for j in range(min,len(lst)):
            # 寻找min 到len(lst)-1 这个范围内的最小值
            if lst[min] > lst[j]:
                min = j
        lst[i], lst[min] = lst[min], lst[i]

lst = [2,6,1,8,2,4,9]
select_sort(lst)
print lst
```

![img](https://img-blog.csdnimg.cn/20201009100055434.gif)





## 堆排序heap sort

堆是一种数据结构，分最大堆和最小堆，最大（最小）堆是一棵每一个节点的键值都不小于（大于）其孩子（如果存在）的键值的树

- 大顶堆是一棵完全二叉树，同时也是一棵最大树
- 小顶堆是一棵完全二叉树，同时也是一棵最小树。



子节点：假设当前节点的序号是i

- 左子节点则是 i*2 +1
- i*2 + 2

不是所有节点都有子节点，假设一个堆的长度为n，那么$\frac{n}{2}-1$及以前的节点都是有子节点的







建立堆的过程：每个节点都和自己的两个子节点中最大的那个节点交换位置就可以了 => 节点值较大的那个就会不停的向上调整

```python
def adjust_heap(lst, i, size):
    lchild = 2 * i + 1      # 计算两个子节点的位置
    rchild = 2 * i + 2
    max = i
    if i < size // 2:
        if lchild < size and lst[lchild] > lst[max]:
            max = lchild
        if rchild < size and lst[rchild] > lst[max]:
            max = rchild

        # 如果max != i成立,那么就说明,子节点比父节点大,要交换位置
        if max != i:
            lst[max], lst[i] = lst[i], lst[max]
            # 向下继续调整,确保子树也符合堆的定义
            # 另外一边的节点必定与父节点的关系已经明确
            adjust_heap(lst, max, size)

def build_heap(lst):
    for i in range((len(lst)//2)-1, -1, -1):	#从一半(有子节点)到0
        adjust_heap(lst, i, len(lst))
        print(lst)   # 此处输出,可以观察效果

lst = [3,9,2,6,1,4,8]
build_heap(lst)
print(lst)
```

![img](https://img-blog.csdn.net/20161009191011299)





### 最大堆

对于最大堆，堆顶的元素一定是整个堆里最大的，但是，如果我们去观察，整个堆并不呈现有序的特性

- 只保证：==父节点==比子节点更大

```text
[9, 6, 8, 3, 1, 4, 2]
```

![image-20210124100434969](https://cdn.jsdelivr.net/gh/DaiDuncan/PicUploader/img/20210124100435.png)

```python
### 注意：以下基于python2 => 最新版的print需要添加()
def adjust_heap(lst, i, size):
    lchild = 2 * i + 1      # 计算两个子节点的位置
    rchild = 2 * i + 2
    max = i
    if i < size // 2:
        if lchild < size and lst[lchild] > lst[max]:
            max = lchild
        if rchild < size and lst[rchild] > lst[max]:
            max = rchild

        # 如果max != i成立,那么就说明,子节点比父节点大,要交换位置
        if max != i:
            lst[max], lst[i] = lst[i], lst[max]
            # 向下继续调整,确保子树也符合堆的定义
            adjust_heap(lst, max, size)

def build_heap(lst):
    for i in range((len(lst)//2)-1, -1, -1):
        adjust_heap(lst, i, len(lst))
        print lst   # 此处输出,可以观察效果


lst = [3,9,2,6,1,4,8]
def heap_sort(lst):
    size = len(lst)
    # 建立一个堆,此时lst[0]一定是最大值
    build_heap(lst)
    print '*'*20
    for i in range(size-1, -1, -1):
        # 将lst[0] 与lst[i] 交换,这样大的数值就按顺序排到了后面
        lst[0], lst[i] = lst[i], lst[0]
        # 交换后,重新将0到i这个范围内的数据调整成堆,这样lst[0]还是最大值
        # 只需要调整栈顶和最后一个节点
        adjust_heap(lst, 0, i)	
        print lst,i  # 此处输出可以查看效果

heap_sort(lst)
print '*'*20
print lst
```











# 插入排序

## 直接插入排序insert

每一步都将一个待排数据按其大小插入到==已经排序的数据==中的适当位置，直到全部插入完毕



函数默认列表lst从0到index-1都是有序的，现在只需要将索引index位置上的数据插入到合适的位置就可以了

- 第1个元素单独看做一个数列，它本身就是有序的

```python
def insert(lst, index):
    """
    列表lst从索引0到索引index-1 都是有序的
    函数将索引index位置上的元素插入到前面的一个合适的位置
    :param lst:
    :param index:
    :return:
    """
    if lst[index-1] < lst[index]:
        return

    tmp = lst[index]
    tmp_index = index
    while tmp_index > 0 and lst[tmp_index-1] > tmp:
        lst[tmp_index] = lst[tmp_index-1]
        tmp_index -= 1
    lst[tmp_index] = tmp


def insert_sort(lst):
    for i in range(1, len(lst)):
        insert(lst, i)


if __name__ == '__main__':
    lst = [1, 6, 2, 7, 5]
    insert_sort(lst)
    print(lst)
```



<img src="https://img-blog.csdn.net/20161009190855230" alt="img" style="zoom:80%;" />



## 希尔排序：分组间隔 + 组内插入排序

又称缩小增量排序：对插入算法的改进

待排序列==基本有序==的情况下，插入算法的效率非常高，那么希尔排序就是利用这个特点对插入算法进行了改造升级



1 分組

关键在于对待排序列进行分组，这个分组并不是真的对序列进行了拆分，而仅仅是虚拟的分组

待排序数组为：`4,1,67,34,12,35,14,8,6,19`



2 第一轮希尔排序

首先，用10/==2== = 5, 这里的5就是缩小增量排序中的那个“增量”

从第0个元素开始，每个元素都与自己距离为5的元素分为一组，那么这样一来分组情况就是

```text
4   35
1   14
67  8
34  6
12  19
```

所谓的分组，仅仅是逻辑上的分组，这10个元素仍然在原来的序列中

上面一共分了5组，每一组都进行插入排序，67 和 8 交换位置，34 和6 交换位置



序列变为：

```text
4, 1, 8, 6, 12, 35, 14, 67, 34, 19
```



3 第二轮希尔排序

上一轮排序时，增量为5，那么这一轮增量为5/2 = 2

```text
4 8 12 14 34
1 6 35 67 19

=> 4, 1, 8, 6, 12, 19, 14, 35, 34, 67
```



4 第三轮希尔排序

上一轮排序时，增量为2，这一轮增量为2 /2 = 1，当增量为1的时候，其实就只能分出一个组了

这样，就完全的退化成插入排序了，但是，由于已经进行了两轮希尔排序，==使得序列已经基本有序了==，那么此时进行插入排序，效果就会非常好



最终实现代码：

```python
lst = [4,1,67,34,12,35,14,8,6,19]
length = len(lst)
step = length//2

while step > 0:

    for i in range(step):
        # 插入排序
        for j in range(i+step, length, step):	#按照step的间隔来分组
            if lst[j] < lst[j-step]:
                tmp = lst[j]
                k = j-step

                while k >= 0 and lst[k] > tmp:
                    lst[k+step] = lst[k]
                    k -= step

                lst[k+step] = tmp
    step //= 2 #缩小增量

print lst
```

![img](https://img-blog.csdnimg.cn/img_convert/8cdd90954fa4e527190a7a7b48e0dfab.png)

<img src="https://img-blog.csdn.net/20170804062400495?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXVzaGl5aTY0NTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="这里写图片描述" style="zoom:80%;" />



# 归并排序merge Divide&Conque

- Divide：向下分组(单个元素) 
- Conque：向上合并(合并有序集合)

## 合并两个有序集合

有两个有序的序列，分别为 [1,4,7] ,[2,3,5],现在请考虑将这两个序列合并成一个有序的序列

1. 首先创建一个新的序列，分别从两个序列中取出第一个数，1和2，1比2小，把1放到新的序列中
2. 第一个序列中的1已经放到新序列中，那么拿出4来进行比较，2比4小，把2放到新的序列中
3. 第二个序列中的2已经放到新序列中，那么拿出3来进行比较，3比4小，把3放到新的序列中
4. 第二个序列中的3已经放到新序列中，那么拿出5来进行比较，4比5小，把4放到新的序列中
5. 第一个序列中的4已经放到新序列中，那么拿出7来进行比较，5比7小，把5放到新的序列中

分别从两个序列中拿出一个数来进行比较，小的那一个放到新序列中，然后，从这个小的数所属的序列中拿出一个数来继续比较

```python
def merge_lst(left_lst,right_lst):
    left_index, right_index = 0, 0
    res_lst = []
    while left_index < len(left_lst) and right_index < len(right_lst):
        # 小的放入到res_lst中
        if left_lst[left_index] < right_lst[right_index]:
            res_lst.append(left_lst[left_index])
            left_index += 1
        else:
            res_lst.append(right_lst[right_index])
            right_index += 1

    # 循环结束时,必然有一个序列已经都放到新序列里,另一个却没有
    if left_index == len(left_lst):
        res_lst.extend(right_lst[right_index:])
    else:
        res_lst.extend(left_lst[left_index:])

    return res_lst
```



两个序列未必是有序的，没关系，就拿A序列来说，我把A序列再分一次，分成A1,A2，如果A1，A2有序我直接对他们进行合并，A不就变得有序了么，但是A1，A2未必有序啊，没关系，我继续分，直到分出来的序列里只有一个元素的时候，一个元素，就是一个有序的序列啊，这个时候不就可以合并了

一层一层的分组，分到最后，一个组里只有一个元素，终于符合合并的条件了，再一层一层的向上合并

```python
def merge_lst(left_lst,right_lst):
    left_index, right_index = 0, 0
    res_lst = []
    while left_index < len(left_lst) and right_index < len(right_lst):
        # 小的放入到res_lst中
        if left_lst[left_index] < right_lst[right_index]:
            res_lst.append(left_lst[left_index])
            left_index += 1
        else:
            res_lst.append(right_lst[right_index])
            right_index += 1

    # 循环结束时,必然有一个序列已经都放到新序列里,另一个却没有
    if left_index == len(left_lst):
        res_lst.extend(right_lst[right_index:])
    else:
        res_lst.extend(left_lst[left_index:])

    return res_lst


def merge_sort(lst):
    if len(lst) <= 1:
        return lst

    middle = len(lst)//2
    left_lst = merge_sort(lst[:middle])
    right_lst = merge_sort(lst[middle:])
    return merge_lst(left_lst, right_lst)


if __name__ == '__main__':
    lst = [19,4,2,8,3,167,174,34]
    print merge_sort(lst)
```

<img src="https://img-blog.csdn.net/20161009190940095" alt="img" style="zoom:80%;" />





# 桶排序

桶排序是计数排序的变种，把计数排序中相邻的m个”小桶”放到一个”大桶”中，在分完桶后，对每个桶进行排序（一般用快排），然后合并成最后的结果





## 计数排序

- 找出待排序的数组中最大和最小的元素 
- 统计数组中每个值为i的元素出现的==次数==，存入数组C的第i项 => C[i]也就是第i个值出现的次数
- 对所有的==计数==累加（从C中的第一个元素开始，每一项和前一项相加）
- 反向填充==目标数组==：将每个元素i放在新数组的第C[i]项，每放一个元素就将C[i]减去1，直到C[i]为0，然后i+1
  - 终止条件：所有的计数累加和为0时，说明元素已经全部取出

<img src="https://img-blog.csdn.net/20170809224042941?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQveXVzaGl5aTY0NTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="这里写图片描述" style="zoom:80%;" />

```c++
//程序1：
#define NUM_RANGE (100)    //预定义数据范围上限，即K的值
 
void counting_sort(int *ini_arr, int *sorted_arr, int n)  //所需空间为 2*n+k
{  
       int *count_arr = (int *)malloc(sizeof(int) * NUM_RANGE);  
       int i, j, k;  
 
       //初始化统计数组元素为值为零 
       for(k=0; k<NUM_RANGE; k++){  
               count_arr[k] = 0;  
       }  
       //统计数组中，每个元素出现的次数    
       for(i=0; i<n; i++){  
               count_arr[ini_arr[i]]++;  
       }  
 
       //统计数组计数，每项存前N项和，这实质为排序过程
       for(k=1; k<NUM_RANGE; k++){  
               count_arr[k] += count_arr[k-1];  
       }  
 
       //将计数排序结果转化为数组元素的真实排序结果
       for(j=n-1 ; j>=0; j--){  
           int elem = ini_arr[j];          //取待排序元素
           int index = count_arr[elem]-1;  //待排序元素在有序数组中的序号
           sorted_arr[index] = elem;       //将待排序元素存入结果数组中
           count_arr[elem]--;              //修正排序结果，其实是针对算得元素的修正
       }  
       free(count_arr);  
}  
 
//程序2：C++(最大最小压缩桶数)
public static void countSort(int[] arr) {
        if (arr == null || arr.length < 2) {
            return;
        }
        int min = arr[0];
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            min = Math.min(arr[i], min);
            max = Math.max(arr[i], max);
        }
        int[] countArr = new int[max - min + 1];
        for (int i = 0; i < arr.length; i++) {
            countArr[arr[i] - min]++;
        }
        int index = 0;
        for (int i = 0; i < countArr.length; i++) {
            while (countArr[i]-- > 0) {
                arr[index++] = i + min;
        }
```

![img](https://www.runoob.com/wp-content/uploads/2019/03/countingSort.gif)



## 基数排序

 ①LSD–Least Significant Digit first 从低位（个位）向高位排
 ②MSD– Most Significant Digit first 从高位向低位（个位）排

将所有待比较数值（正整数）统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后，数列就变成一个有序序列

一个正整数有几位：

````python
### 方法1：借助字符串
print(len(str(98372)))

### 方法2：以10为底的log
import math
print(int(math.ceil(math.log(98372, 10))))	#log(98372, 10) 的值是小于5的，而ceil返回的是上入整数
````

从低位到高位输出一个正整数的每一位

```python
a = 98372
index = 1
while True:
    b = a%(10**index)	#取余数：得到个位
    c = b//(10**(index-1))	#个位剔除：十位变成个位
    if c == 0 and b == a:
        break
    print(c)
    index += 1
```





假设有一个序列 `lst = [3,56,1,24,134,67,356,321]` 它是无序的

分配10个桶，编号从0到9，现在，请将个位数字是0的放在0号桶里，个位数字是1的放在1号桶里，以此类推，最后，这10个桶里的情况是下面的样子

```text
[[], [1, 321], [], [3], [24, 134], [], [56, 356], [67], [], []]
```

- 个位数有序

按照从小到大的顺序从桶里把数字都取出来，是这个样子，我们叫它序列 A

```text
[1, 321, 3, 24, 134, 56, 356, 67]
```



- 十位数有序

再分配10个桶，编号从0到9，对于序列A，将十位数字是0的放在0号桶里，十位数字是1的放在1号桶里，以此类推，如果一个数字没有十位，那就是0呗

```text
[[1, 3], [], [321, 24], [134], [], [56, 356], [67], [], [], []]
```

```text
[1, 3, 321, 24, 134, 56, 356, 67]
```

………………



```python
import math
def sort(a, radix=10):
    """a为整数列表， radix为基数"""
    K = int(math.ceil(math.log(max(a), radix))) # 最大值有几位数
    bucket = [[] for i in range(radix)] # 不能用 [[]]*radix：相当于浅拷贝
    for i in range(1, K+1): # K次循环
        for val in a:
            bucket[val%(radix**i)//(radix**(i-1))].append(val) # 取整数第K位数字 （从低到高）
        del a[:]
        for each in bucket:
            a.extend(each) # 桶合并
        bucket = [[] for i in range(radix)]	#bucket清零初始化

lst = [3,56,1,24,134,67,356,321]
sort(lst)
print(lst)
```



# 应用|排序算法的选择

在数据量小的时候快速排序当属第一，堆排序最差，但==随着数据的不断增大==归并排序的性能会逐步赶上并超过快速排序，性能成为三种算法之首。

可能在数据量大到一定数量时，快速排序的堆栈开销比较大，所以在性能上大打折扣，甚至堆排序的性能也能好过它，但总体上来说快速排序表现的还是比较优秀的。





# #参考文献

[Link: 酷Python](http://www.coolpython.net/python_primary/sort/index.html)

[Link: 排序算法时间复杂度、空间复杂度、稳定性比较](https://blog.csdn.net/pange1991/article/details/85460755)

[Link: 可视化 算法与数据结构](https://visualgo.net/zh)

