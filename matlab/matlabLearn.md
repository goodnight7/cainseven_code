[TOC]

----

# MATLAB ABC

1. 绘制正弦曲线和余弦曲线。
```
x=[0:0.5:360]*pi/180;

plot(x,sin(x),x,cos(x));
```

1. 求方程 3x 4 +7x 3 +9x 2 -23=0的全部根。
```
p=[3,7,9,0,-23]; %建立多项式系数向量

x=roots(p) %求根
```

1. 求积分
```
quad('x.*log(1+x)',0,1)
```

1. 求解线性方程组。
```
a=[2,-3,1;8,3,2;45,1,-9];

b=[4;2;17];

x=inv(a)*b
```

1. 帮助窗口 
```
helpwin
helpdesk
doc

help [file] [func]

lookfor [-all] [keyword]
```

# 变量

1. 计算表达式的值,并显示计算结果。
```
x=1+2i; 
y=3-sqrt(17); 
z=(cos(abs(x+y))-sin(78*pi/180))/(x+abs(y))
```

1. 内存
```
clear % 清除内存

who % 显示所有变量名称

whos % 显示所有变量明细 

% 存取变量为文件
save filename [变量名表] [-append] [-ascii]
% 不带扩展名 .mat
% 追加
% 默认二进制

load filename [变量名表] [-ascii]
```

1. 函数 
MATLAB提供了许多数学函数,函数的自 变量规定为矩阵变量,运算法则是将函数 逐项作用于矩阵的元素上,因而运算的结 果是一个与自变量同维数的矩阵。 
函数使用说明: 
    * 三角函数以弧度为单位计算。 

    * abs函数可以求实数的绝对值、复数的模、 字符串的ASCII码值。

    * 用于取整的函数有fix、floor、ceil、 round,要注意它们的区别。 

    * rem与mod函数的区别。rem(x,y)和 mod(x,y)要求x,y必须为相同大小的实矩阵 或为标量。

1. 矩阵
    1. 矩阵的建立
        1. 直接输入法
        最简单的建立矩阵的方法是从键盘直接输入矩阵的元素。具体方法如下:将矩阵的元素用方括号括起来,按矩阵行的顺序输入各元素同一行的各元素之间用空格或逗号分隔,不同行的元素之间用分号分隔。

        1. 利用M文件建立矩阵

        1. 利用冒号表达式建立一个向量
        冒号表达式可以产生一个行向量,一般格式是:
        `e1:e2:e3`
        其中e1为初始值,e2为步长,e3为终止值。在MATLAB中,还可以用linspace函数产生行向量。其调用格式为:
        `linspace(a,b,n)`
        其中a和b是生成向量的第一个和最后一个元素,n是元素总数。显然,`linspace(a,b,n)`与`a:(b-a)/(n-1):b`等价。

    1. 矩阵的拆分
        1. 元素
        通过下标引用矩阵的元素,例如 `A(3,2)=200 `采用矩阵元素的序号来引用矩阵元素。矩阵元素的序号就 是相应元素在内存中的排列顺序。在MATLAB中,矩阵 元素按列存储,先第一列,再第二列,依次类推。例如 
        ```
        A=[1,2,3;4,5,6];
         A(3) 
         ans =2 
         ```
         显然,序号(Index)与下标(Subscript )是一一对应的,以 m×n矩阵A为例,矩阵元素
         `A(i,j)`的序号为`(j-1)*m+i`。其 相互转换关系也可利用`sub2ind`和`ind2sub`函数求得。

        1. 矩阵拆分
            1. 冒号表达式
                1. `A(:,j)`表示取A矩阵的第j列全部元素;
                `A(i,:)`表示A矩阵第i行的全部元素;
                `A(i,j)`表示取A矩阵第i行、第j列的元素。
                1. `A(i:i+m,:)`示取A矩阵第i~i+m行的全部元素;
                `A(:,k:k+m)`表示取A矩阵第k~k+m列的全部元素;
                `A(i:i+m,k:k+m)`表示取A矩阵第i~i+m行内,并在第k~k+m列中的所有元素。

        1. 利用空矩阵删除矩阵的元素
        在MATLAB中,定义[]为空矩阵。给变量X赋空矩阵的语句为`X=[]`。注意,`X=[]`与`clear X`不同,clear是将X从工作空间中删除,而空矩阵则存在于工作空间中,只是维数为0。

    1. 特殊矩阵
        1. 通用的特殊矩阵
        常用的产生通用特殊矩阵的函数有:
        zeros:产生全0矩阵(零矩阵)。
        ones:产生全1矩阵(幺矩阵)。
        eye:产生单位矩阵。
        rand:产生0~1间均匀分布的随机矩阵。
        randn:产生均值为0,方差为1的标准正态分布随机矩阵。
        常用的函数还有`reshape(A,m,n)`,它在矩阵总元素保持不变的前提下,将矩阵A重新排成m×n的二维矩阵。

            1. 分别建立3×3、3×2和与矩阵A同样大小的零矩阵。
                1. 建立一个3×3零矩阵。
                `zeros(3)` 
                1. 建立一个3×2零矩阵。 
                `zeros(3,2)` 
                1. 设A为2×3矩阵,则可以用`zeros(size(A))`建立 一个与矩阵A同样大小零矩阵。
                ``` 
                A=[1 2 3;4 5 6]; %产生一个2×3阶矩阵A 
                zeros(size(A)) %产生一个与矩阵A同样大小的零矩阵
                ```

            1. 建立随机矩阵:
                1. 在区间[20,50]内均匀分布的5阶随机矩阵。
                `x=20+(50-20)*rand(5)`
                1. 均值为0.6、方差为0.1的5阶正态分布随机矩阵。
                `y=0.6+sqrt(0.1)*randn(5)`
        1. 特殊矩阵
            1. 魔方矩阵
            魔方矩阵有一个有趣的性质,其每行、每列及两条对角线上的元素和都相等。对于n阶魔方阵,其元素由1,2,3,...,n2共n2个整数组成。MATLAB提供了求魔方矩阵的函数magic(n),其功能是生成一个n阶魔方阵。
            将101~125等25个数填入一个5行5列的表格中,使其每行每列及对角线的和均为565。
            `M=100+magic(5)`
            1. 范得蒙矩阵
            范得蒙(Vandermonde)矩阵最后一列全为1,倒数第二列为一个指定的向量,其他各列是其后列与倒数第二列的点乘积。可以用一个指定向量生成一个范得蒙矩阵。在MATLAB中,函数vander(V)生成以向量V为基础向量的范得蒙矩阵。例如,A=vander([1;2;3;5])即可得到上述范得蒙矩阵。
            1. 希尔伯特矩阵
            在MATLAB中,生成希尔伯特矩阵的函数是hilb(n)。使用一般方法求逆会因为原始数据的微小扰动而产生不可靠的计算结果。MATLAB中,有一个专门求希尔伯特矩阵的逆的函数invhilb(n),其功能是求n阶的希尔伯特矩阵的逆矩阵。
            求4阶希尔伯特矩阵及其逆矩阵。
            命令如下:
            ```
            format rat

            H=hilb(4)

            H=invhilb(4)
            ```
            1. 托普利兹矩阵托普利兹(Toeplitz)矩阵除第一行第一列外,其他每个元素都与左上角的元素相同。生成托普利兹矩阵的函数是`toeplitz(x,y)`它生成一个以x为第一列,y为第一行的托普利兹矩阵。这里x, y均为向量,两者不必等长。`toeplitz(x)`用向量x生成一个对称的托普利兹矩阵。例如`T=toeplitz(1:6)`
            1. 伴随矩阵
            MATLAB生成伴随矩阵的函数是compan(p),其中p是一个多项式的系数向量,高次幂系数排在前,低次幂排在后。例如,为了求多项式的`x3-7x+6`的伴随矩阵,可使用命令:
            `p=[1,0,-7,6];compan(p)`
            1. 帕斯卡矩阵
            我们知道,二次项(x+y)n展开后的系数随n的增大组成一个三角形表,称为杨辉三角形。由杨辉三角形表组成的矩阵称为帕斯卡(Pascal)矩阵。函数pascal(n)生成一个n阶帕斯卡矩阵。
















