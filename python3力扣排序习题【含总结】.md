# python3排序习题（leetcode）【含总结】
## 1，2545. 根据第 K 场考试的分数排序

[*2545. 根据第 K 场考试的分数排序 - 力扣（LeetCode）*](https://leetcode.cn/problems/sort-the-students-by-their-kth-score/)

班里有 m 位学生，共计划组织 n 场考试。给你一个下标从 0 开始、大小为 m x n 的整数矩阵 score ，其中每一行对应一位学生，而 score[i][j] 表示第 i 位学生在第 j 场考试取得的分数。矩阵 score 包含的整数 互不相同 。
另给你一个整数 k 。请你按第 k 场考试分数从高到低完成对这些学生（矩阵中的行）的排序。

![image](http://l1q.one:8080/i/2023/07/21/h8grdf.png)

**思路：**
按某一列排序。从大到小（反序）。

**题解：**

```
class Solution:
    def sortTheStudents(self, score: List[List[int]], k: int) -> List[List[int]]:
        score.sort(key = lambda s:-s[k])
        return score
```

**分析：**
***【按某列排序】***对于score.sort(key=lambda s: -s[k])，当key参数指定为一个函数时，sort()将使用该函数生成的键来排序列表，即根据列表每个元素的第k个值来进行排序。
s[k]中，s代表列表中的每个子列表，而k表示列的索引，即要引用的元素在每个子列表中的位置。s[k]引用了列表中的每个子列表的第k列元素。
注意，该方法排序的顺序是从小到大。
通过取负值 -s[k]，我们反转了排序顺序，使得较大的值在前面，较小的值在后面。

**另一个例子：**
假设我们有一个包含学生信息的列表 students，每个学生的信息都是一个元组，包含了学生的姓名、年龄和成绩。我们想要根据学生的成绩来对列表进行排序。

示例代码：

```
students = [("Alice", 20, 90), ("Bob", 18, 85), ("Charlie", 19, 95)]

students.sort(key=lambda s: -s[2])

print(students)
```

这里我们使用 lambda 函数 lambda s: -s[2] 来作为 key 函数，表示根据每个学生的成绩（元组中的第3个元素）进行排序。

结果输出如下：

```
[('Charlie', 19, 95), ('Alice', 20, 90), ('Bob', 18, 85)]
```

## 2，2418. 按身高排序
*https://leetcode.cn/problems/sort-the-people/*

给你一个字符串数组 names ，和一个由 **互不相同** 的正整数组成的数组 heights 。两个数组的长度均为 n 。
对于每个下标 i，names[i] 和 heights[i] 表示第 i 个人的名字和身高。
请按身高 **降序** 顺序返回对应的名字数组 names 。

![image](http://l1q.one:8080/i/2023/07/21/r2scvw.png)

**思路：**
给两个list，用zip按下标一对一组合，合并为一个二维list，按二维list中的第2列[1]大小排序，从大到小（反序）。需要返回数组 names，故*拆分，再zip重组 names heights。

**题解：**

```
class Solution:
    def sortPeople(self, names: List[str], heights: List[int]) -> List[str]:
        idx = list(map(list, zip(names, heights)))
        idx.sort(key = lambda i:-i[1])
        names, heights = zip(*idx)
        return names
```

**分析：**
idx = list(map(list, zip(names, heights)))：
zip(names, heights): 将两个列表 names heights 对应位置的元素组合成一个元组。例如，如果names=['John', 'Mary', 'Alice']，heights=[180, 165, 175]，那么zip(names, heights)将生成一个元组列表 [('John', 180), ('Mary', 165), ('Alice', 175)]。

list(map(list, zip(names, heights))): 使用list函数与map函数，将上一步得到的元组转换为列表，得到最终的列表idx，其中每个元素是由名字和身高组成的列表。
两次list：第1次，map使用list函数把列表中元组逐个转化为列表，[ , ], [ , ], [ , ]；第二次，将列表组合为列表，[[ , ], [ , ], [ , ]]

idx.sort(key = lambda i:-i[1])：
指定按身高降序排序，即高的在前面。（利用lambda排序的方法可见【2545. 根据第 K 场考试的分数排序】[leetcode2545. 根据第 K 场考试的分数排序 – iAstralFloder](http://43.138.144.65/2023/07/21/leetcode2545-%e6%a0%b9%e6%8d%ae%e7%ac%ac-k-%e5%9c%ba%e8%80%83%e8%af%95%e7%9a%84%e5%88%86%e6%95%b0%e6%8e%92%e5%ba%8f/)）

names, heights = zip(*idx)：
使用zip函数将排序后的idx列表进行拆分，分别赋值给names和heights两个变量（数组），实现将排序后的名字和身高分开。



## 3，1051.高度检查器

[1051. 高度检查器](https://leetcode.cn/problems/height-checker/)

学校打算为全体学生拍一张年度纪念照。根据要求，学生需要按照 非递减 的高度顺序排成一行。
排序后的高度情况用整数数组 expected 表示，其中 expected[i] 是预计排在这一行中第 i 位的学生的高度（下标从 0 开始）。
给你一个整数数组 heights ，表示 当前学生站位 的高度情况。heights[i] 是这一行中第 i 位学生的高度（下标从 0 开始）。
返回满足 heights[i] != expected[i] 的 下标数量 。

![image](http://l1q.one:8080/i/2023/07/23/fp4ye8.png)

**题解：**

```
class Solution:
​    def heightChecker(self, heights: List[int]) -> int:
​        count = 0
​        expected = sorted(heights)
​        for i in range(len(heights)):
​            if heights[i] != expected[i]:
​                count += 1
​        return count
```

**注意：**
sorted()与sort()
for i in range(len(heights))的使用
count = 0 初始化

## 总结

### （1）sort()与sorted()

1. `sort()` 直接修改原始列表，而 `sorted()` 则是返回一个新的已排序列表，不会修改原始列表。
2. `sort()`没有返回值，是None，不能赋值给一个新的变量。例如：
```
my_list = [3, 1, 2]
my_list.sort()
print(my_list)
```
`sorted()` 返回一个已排序的新列表，并且不会修改原始列表。可以将返回的已排序列表赋值给另一个变量，例如：`sorted_list = sorted(my_list)`。

根据实际情况来选择使用 `sort` 还是 `sorted`：如果需要保留原始列表的顺序或同时使用原始列表和已排序列表，那么应该使用 `sorted`。如果只需要对原始列表进行排序并且不关心原始顺序，那么可以使用 `sort` 以节省空间和时间。

### （2）使用lambda排序的正序与反序
例子：
```
numbers = [5, 2, 9, 1, 7]
numbers.sort(key=lambda x: x, reverse=True)# 法①
# 法② numbers.sort(key=lambda x: -x)
print(numbers) 
# 输出：[9, 7, 5, 2, 1]
```

### （3）zip与*（重组与拆分）
例子：
```
class Solution:
    def sortPeople(self, names: List[str], heights: List[int]) -> List[str]:
        idx = list(map(list, zip(names, heights)))
        idx.sort(key = lambda i:-i[1])
        names, heights = zip(*idx)
        return names
```
*："unpacking（解包）"操作符。它可以用于将可迭代对象（如列表、元组、集合等）中的元素拆分成独立的元素。例如：

```
numbers = [1, 2, 3]
a, b, c = numbers

print(a, b, c)
```

输出结果为：

```
1 2 3
```
zip：将按索引位置将多个列表重新组合成一个元组的列表，每个元组包含了来自不同列表相同索引位置上的值。例如：

```
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]

zip_result = zip(names, ages)
print(list(zip_result))
```

输出结果为：

```
[("Alice", 25), ("Bob", 30), ("Charlie", 35)]
```

zip 将列表 names 和 ages 按索引位置进行了重新组合。
