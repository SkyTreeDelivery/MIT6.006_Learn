# lec02 - Models of computation, Python cost model, document distance
代码提供了8中方法，逐步优化执行效率。

分别分析一下各自方法的时间复杂度

## 方法一：效率最低的原始方法

- 读取文件：时间复杂度未知
- 分割字符：$O(n)$，$n$为文件内字符个数
- 计数：$O(n)$，$n$为单词个数
- 排序：$O(n)$，$n$为单词个数
- 计算向量夹角：$O(n)$，$n$为向量维数

评价：基本上所有的操作都是低效的，例如排序可以用快排，计数可以用hashmap，计算向量夹角可以用GPU加速，读取文件和分割字符暂时没想好怎么优化，此外还有列表拼接导致的扩容等原因，可以提前初始化。

## 方法二：优化了list合并

方法一代码
```python
 word_list = word_list + words_in_line
```

方法二代码
```python
# Using "extend" is much more efficient than concatenation here:
 word_list.extend(words_in_line)
```

## 方法三：使用[[双指针法]]优化了向量点乘计算

方法一代码：

```python
def inner_product(L1,L2):
    """
    Inner product between two vectors, where vectors
    are represented as lists of (word,freq) pairs.

    Example: inner_product([["and",3],["of",2],["the",5]],
                           [["and",4],["in",1],["of",1],["this",2]]) = 14.0 
    """
    sum = 0.0
    for word1, count1 in L1:
        for word2, count2 in L2:
            if word1 == word2:
                sum += count1 * count2
    return sum
```

方法三代码：

```python
def inner_product(L1,L2):
    """
    Inner product between two vectors, where vectors
    are represented as alphabetically sorted (word,freq) pairs.

    Example: inner_product([["and",3],["of",2],["the",5]],
                           [["and",4],["in",1],["of",1],["this",2]]) = 14.0 
    """
    sum = 0.0
    i = 0
    j = 0
    while i<len(L1) and j<len(L2):
        # L1[i:] and L2[j:] yet to be processed
        if L1[i][0] == L2[j][0]:
            # both vectors have this word
            sum += L1[i][1] * L2[j][1]
            i += 1
            j += 1
        elif L1[i][0] < L2[j][0]:
            # word L1[i][0] is in L1 but not L2
            i += 1
        else:
            # word L2[j][0] is in L2 but not L1
            j += 1
    return sum
```

## 方法四：使用hashmap优化频数统计

方法一：

```python
def count_frequency(word_list):
    """
    Return a list giving pairs of form: (word,frequency)
    """
    L = []
    for new_word in word_list:
        for entry in L:
            if new_word == entry[0]:
                entry[1] = entry[1] + 1
                break
        else:
            L.append([new_word,1])
    return L
```

方法四：

```python
def count_frequency(word_list):
    """
    Return a list giving pairs of form: (word,frequency)
    """
    D = {}
    for new_word in word_list:
        if new_word in D:
            D[new_word] = D[new_word]+1
        else:
            D[new_word] = 1
    return D.items()
```

## 方法五：使用translate和split
使用translate方法和split方法的组合来处理word的提取。

translate方法对字符做映射处理，split负责拆分字符串，他们的效率未知。

## 方法六：使用组合排序代替插入排序
组合排序的时间复杂度为O(nlogn)，插入排序的时间复杂度为O(n^2)

方法一：
```python
def insertion_sort(A):
    """
    Sort list A into order, in place.

    From Cormen/Leiserson/Rivest/Stein,
    Introduction to Algorithms (second edition), page 17,
    modified to adjust for fact that Python arrays use 
    0-indexing.
    """
    for j in range(len(A)):
        key = A[j]
        # insert A[j] into sorted sequence A[0..j-1]
        i = j-1
        while i>-1 and A[i]>key:
            A[i+1] = A[i]
            i = i-1
        A[i+1] = key
    return A
```

方法六
```python
def merge_sort(A):
    """
    Sort list A into order, and return result.
    """
    n = len(A)
    if n==1: 
        return A
    mid = n//2     # floor division
    L = merge_sort(A[:mid])
    R = merge_sort(A[mid:])
    return merge(L,R)

def merge(L,R):
    """
    Given two sorted sequences L and R, return their merge.
    """
    i = 0
    j = 0
    answer = []
    while i<len(L) and j<len(R):
        if L[i]<R[j]:
            answer.append(L[i])
            i += 1
        else:
            answer.append(R[j])
            j += 1
    if i<len(L):
        answer.extend(L[i:])
    if j<len(R):
        answer.extend(R[j:])
    return answer
```

## 方法七：使用hashmap优化点乘
方法四：
```python
def inner_product(L1,L2):
    """
    Inner product between two vectors, where vectors
    are represented as alphabetically sorted (word,freq) pairs.

    Example: inner_product([["and",3],["of",2],["the",5]],
                           [["and",4],["in",1],["of",1],["this",2]]) = 14.0 
    """
    sum = 0.0
    i = 0
    j = 0
    while i<len(L1) and j<len(L2):
        # L1[i:] and L2[j:] yet to be processed
        if L1[i][0] == L2[j][0]:
            # both vectors have this word
            sum += L1[i][1] * L2[j][1]
            i += 1
            j += 1
        elif L1[i][0] < L2[j][0]:
            # word L1[i][0] is in L1 but not L2
            i += 1
        else:
            # word L2[j][0] is in L2 but not L1
            j += 1
    return sum
```

方法七：
```python
def inner_product(D1,D2):
    """
    Inner product between two vectors, where vectors
    are represented as dictionaries of (word,freq) pairs.

    Example: inner_product({"and":3,"of":2,"the":5},
                           {"and":4,"in":1,"of":1,"this":2}) = 14.0 
    """
    sum = 0.0
    for key in D1:
        if key in D2:
            sum += D1[key] * D2[key]
    return sum
```
## 方法八：一次读取所有字符而不是分行读取
方法一：
```python
def read_file(filename):
    """ 
    Read the text file with the given filename;
    return a list of the lines of text in the file.
    """
    try:
        f = open(filename, 'r')
        return f.readlines()
    except IOError:
        print "Error opening or reading input file: ",filename
        sys.exit()
```

方法八
```python
def read_file(filename):
    """ 
    Read the text file with the given filename;
    return a list of the lines of text in the file.
    """
    try:
        f = open(filename, 'r')
        return f.read()
    except IOError:
        print "Error opening or reading input file: ",filename
        sys.exit()
```

## 总结
1. 详细分析算法时间复杂度的步骤是分别分析每一步的执行次数和时间复杂度。**渐进复杂度的实用价值在于不需要严格的分析也能得到正确的结果**。
2. **更严格的边界是很棒的，但是不那么严格的边界也是好的，能够得到正确结果**。例如在方法一中count_frequency函数的最坏时间复杂度可以是$O(W^2)$，也可以是$O(W^2)$，$O(W^2)$比$O(W^2)$更严格。
3. 当我们分析一些时间复杂度随输入而变化的算法时，分析**最坏时间复杂度**往往是有价值的。
4. **分析时间复杂度时，可以==自定义一些变量==来代表算法中有显著差异的不同量**，例如文件距离中的word数量和vector长度，分别使用$W$和$L$表示。
5. 使用**代码分析工具**（code profiler）可以辅助开发者快速定位**性能瓶颈**。Python自带代码分析工具。
6. 使用一些**库函数**可能有助于简化代码并提高执行效率。因为有些库函数是采用C++实现，机器码的执行效率比python解释器快。
7. **渐进最优**（asymptotic optimal）指一个代码在渐进复杂度上可以优化的极限。如果一个算法必须至少遍历每个元素一次，那么他的渐进最优就是$O(n)$。