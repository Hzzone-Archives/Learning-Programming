####list中的一些常用方法
```Python
l = [1, 2, 3, '4']
l.append('6') # append在list末尾添加项
print(l) # [1, 2, 3, '4', '6']
print(len(l)) # 5  len()返回list的长度
l.insert(4, '5') # insert在指定位置插入项
print(l) # [1, 2, 3, '4', '5', '6']
print(l.index(3)) # 2 index返回第一次出现指定项的位置
print(l.count(3)) # 1 count返回出现指定项的次数
l.remove('6') # 删除特定项
print(l) # [1, 2, 3, '4', '5']
l.reverse() # 将list翻转
print(l) # ['5', '4', 3, 2, 1]
```

----
####range的使用
* `range`函数返回一个`range`对象，必须强制转化成`list`才能使用
* `range(n)` 产生一个从0到n-1的序列
* `range(begin, end)` 产生一个从begin到end-1的序列
* `range(begin, end, interval)` 产生一个begin开始，每个元素相差interval的序列   

使用如下：
```Python
print(list(range(8))) # [0, 1, 2, 3, 4, 5, 6, 7]
print(list(range(1, 8))) # [1, 2, 3, 4, 5, 6, 7]
print(list(range(1, 8, 2))) # [1, 3, 5, 7]
```

-----
####for循环的使用
```Python
l = [1, 2, 3, 4]
for x in l:
    print(x) # 循环打印list
for x in range(5):
    print('hello world') # 打印五次hello world
for x in range(0, 20, 2): # 只打印0-20的偶数
    print(x)
```

----
####实战：计算器
```Python
while True:
    user_input = input('please input:\nadd sub multi div\nquit to exit program\n')
    s = 'result: '
    if user_input == 'add':
        print("please input two numbers")
        x1 = float(input())
        x2 = float(input())
        print(s + str(x1+x2))
    elif user_input == 'sub':
        print("please input two numbers")
        x1 = float(input())
        x2 = float(input())
        print(s + str(x1-x2))
    elif user_input == 'multi':
        print("please input two numbers")
        x1 = float(input())
        x2 = float(input())
        print(s + str(x1*x2))
    elif user_input == 'div':
        print("please input two numbers")
        x1 = float(input())
        x2 = float(input())
        print(s + str(x1/x2))
    elif user_input == 'quit':
        break
    else:
        continue
```