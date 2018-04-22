# 螺旋矩阵

## 什么是螺旋矩阵？
> 螺旋矩阵是指一个呈螺旋状的矩阵，它的数字由第一行开始到右边不断变大，向下变大，向左变大，向上变大，如此循环。

## 实现思路

> 根据规律可以看出，每一层都是按照右->下->左->上的顺序进行递增，由此可见可以用列表和递归的方式每一层每一层的实现，直到中间完全剩下一个（奇数行），或者递归实现填满（偶数行）。

## 在Python中的实现：


```python
def main():
    lines = int(input('请输入螺旋矩阵的行数：'))
    total_matrix = [[0] * lines for i in range(lines)]
    show_num = 1
    col = lines - 1
    row = lines - 1
    start_line = 0

    def print_ju(start_line, col, row, show_num, ):
        if row == 0:
            if lines % 2 != 0:
                total_matrix[lines // 2][lines // 2] = lines * lines
        else:
            for i in range(start_line, col):  # 打印上横行
                total_matrix[start_line][i] = show_num
                show_num += 1
            for i in range(start_line, row, 1):  # 打印右竖行
                total_matrix[i][col] = show_num
                show_num += 1
            for i in range(row, row - col + start_line, -1):  # 打印下横行
                total_matrix[row][i] = show_num
                show_num += 1
            for i in range(col, col - row + start_line, -1):  # 打印左边竖行
                total_matrix[i][row - col + start_line] = show_num
                show_num += 1
            return print_ju(start_line + 1, row - 1, col - 1, show_num)

    print_ju(start_line, row, col, show_num)
    for i in total_matrix:
        for x in i:
            print(format(x, '3'), end=' ')
        print()


if __name__ == '__main__':
    main()

```

    请输入螺旋矩阵的行数：10
      1   2   3   4   5   6   7   8   9  10 
     36  37  38  39  40  41  42  43  44  11 
     35  64  65  66  67  68  69  70  45  12 
     34  63  84  85  86  87  88  71  46  13 
     33  62  83  96  97  98  89  72  47  14 
     32  61  82  95 100  99  90  73  48  15 
     31  60  81  94  93  92  91  74  49  16 
     30  59  80  79  78  77  76  75  50  17 
     29  58  57  56  55  54  53  52  51  18 
     28  27  26  25  24  23  22  21  20  19 
    
