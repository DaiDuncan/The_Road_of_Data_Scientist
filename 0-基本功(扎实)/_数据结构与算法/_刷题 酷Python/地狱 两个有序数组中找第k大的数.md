已知有两个从小到大的有序数组，求两个数组的第k大的数。

```text
[1, 4, 6, 8, 12, 15, 18, 20, 28, 29]
[2, 5, 7, 10]
第8大的数是10
```



## me

选择middle index：然后只比较边界值lst[middle]，根据大小进行取舍

​	但由于边界情况的存在，需要调整：

- 比如某一个数组的长度不够，甚至为0



递归思路：取舍后，参数k调整 => 转化为规模更小的子问题

```python
def select_k(lst_1, lst_2, k):
    ### 递归返回条件
    if len(lst_1) == 0:
        return lst_2[k-1]
    elif len(lst_2) == 0:
        return lst_1[k-1]
    elif k == 1:
        if lst_1[0] <= lst_2[0]:
            return lst_1[0]
        else:
            return lst_2[0]

    key_idx = k // 2
    if k > len(lst_1) + len(lst_2):
        return "k is too big"
    elif key_idx >= len(lst_1):
        key_idx = len(lst_1)-1
    elif key_idx >= len(lst_2):
        key_idx = len(lst_2)-1

    ##print("key_index {}" .format(key_idx))

    if lst_1[key_idx] > lst_2[key_idx]:
        #idx = search_idx(lst_2, lst_1)
        del lst_1[0:key_idx] #左闭右开
        del lst_2[0:key_idx+1]

    elif lst_1[key_idx] < lst_2[key_idx]:
        #idx = search_idx(lst_1, lst_2)
        del lst_1[0:key_idx+1] #左闭右开
        del lst_2[0:key_idx]

    elif lst_1[key_idx] == lst_2[key_idx]:
        del lst_1[0:key_idx+1]
        del lst_2[0:key_idx]

    k = k - key_idx*2 - 1
    ##print("k {}" .format(k))
    ##print(lst_1)
    ##print(lst_2)
    return select_k(lst_1, lst_2, k)
```





## 参考思路

基于有序的特点

​	假设数组分别是a b,令middle = k/2， middle_ex = k - middle。比较a[middle]和b[middle]的值。

- 如果a[middle - 1] == b[middle_ex - 1]，那么a[middle-1]不正好是第k大的数么，因为 k= middle_ex + middle,且两个数组都有序。
- 如果a[middle - 1] < b[middle_ex - 1]，让a = a[middle:],前面的那些元素都可以舍弃了， 问题转变成从a 和 b这两个数组里找到第 k - middle 大的值
- 如果a[middle - 1] > b[middle_ex - 1]，让b = b[middle_ex:]，前面那些元素都可以舍弃了，问题转变成从a 和 b这两个数组里找到第 k - middle_ex大的值



改进的地方：

- 固定长度最小的数组：要让数组长度更小的为a
- 计算middle时，其实要考虑middle是否比a(长度小的数组)的长度小

```python
# coding=utf-8


def find_kth(left_lst, left_len, right_lst, right_len, k):
    """
    从left_lst 和 right_lst中寻找第k大的数
    :param left_lst: 长度小的那个数组
    :param left_len:
    :param right_lst: 长度达的那个数组
    :param right_len:
    :param k:
    :return:
    """
    # 确保left_lst长度小于 right_lst 长度
    if left_len > right_len:
        return find_kth(right_lst, right_len, left_lst, left_len, k)

    # 长度小的数组已经没有值了,从right_lst找到第k大的数
    if left_len == 0:
        return right_lst[k-1]

    # 找到第1 大的数,比较两个列表的第一个元素,返回最小的那个
    if k == 1:
        return min(left_lst[0], right_lst[0])

    # k >> 1 ,其实就是k/2
    middle = min(k >> 1, left_len)
    middle_ex = k - middle
    # 舍弃left_lst的一部分
    if left_lst[middle-1] < right_lst[middle_ex-1]:
        return find_kth(left_lst[middle:], left_len-middle, right_lst, right_len, k-middle)
    # 舍弃right_lst 的一部分
    elif left_lst[middle-1] > right_lst[middle_ex-1]:
        return find_kth(left_lst, left_len, right_lst[middle_ex:], right_len-middle_ex, k-middle_ex)
    else:
        return left_lst[middle-1]


if __name__ == '__main__':
    left_lst = [1, 4, 6, 8, 12, 15, 18, 20, 28, 29]
    right_lst = [2, 5, 7, 10]
    k = 8
    print find_kth(left_lst, len(left_lst), right_lst, len(right_lst), k)

    # 合并后排序,找第k大的数
    lst = left_lst + right_lst
    lst.sort()
    print lst[k-1]
```