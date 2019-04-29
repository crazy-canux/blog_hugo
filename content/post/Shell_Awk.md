---
title: "Awk"
date: 2016-12-14T00:55:37+08:00
categories: ["Linux"]
tags: ["shell"]
keywords: []
author: "Canux"
draft: false
---

# awk

awk 是一门编程语言

awk/nawk/gawk/mawk：比sed更高级的流编辑工具，是sed和grep的升级版，主要用于数据流处理。
nawk： new awk。
mawk： awk的解释器
gawk是gnu的awk，功能更全面。

awk命令格式：
awk [-v var=value [-F *] [–] '/pattern/ {action}'  file
awk [-v var=value [-F *] [-f scriptfile ...] [–] file
awk [-v var=value [-F *] [-] 'BEGIN {} /pattern/ {action} END {}'  file
BEGIN { }  在读取输入之前就操作
END { }    在读物输入之后操作
awk的指令需要用单引号包围；
模式需要用/pattern/包围；
过程需要用{command1；command2}包围，多个过程需要用；隔开。

脚本中传递参数格式：
awk [-f scriptfile]OR['/pattern/{action}'] val1=value1 val2=value2 … file1   vala=valuea valb=valueb... file2 ...
如果通过shell传参数，把value改成$n即可。
可以通过命令返回值作为参数value。
也可以使用环境变量作为value，也可以给awk的环境变量赋值。
可以在任何位置定义变量（‘ ‘ 之后；-v之后；BEGIN中；{}中；END中）。
只有在-v和在BEGIN中定义的变量能在BEGIN中使用。
在END中定义的变量只能在END中使用，其它位置定义的变量都可以在{}中使用。
在任何位置定义的变量在END中都可以使用。

Options:
-f: 指定运行脚本文件中的命令,可以指定多个脚本。
-F: 指定输入字段分隔符，默认是空格键和制表符。
-v  $val=value  定义变量

awk中的由空格或制表符分隔的单元作为一个字段
$0	当前行的文本内容
$1	第一个字段的文本内容
$n	第n个字段的文本内容

awk中的常量和变量：
常量：字符串型和数字型，字符串常量必须用“”引号引用。
变量：变量名大小写敏感，字母或下划线开头，可以包含数字。
awk回自动将变量根据环境初始化为空串或0.

awk中的命令：

print命令：
print a,b   参数用，隔开，打印出来的就是用空格隔开的,eg：a b.
print a“string”b  参数中用双引号打印出来就是原样输出,eg:astringb.
Print     打印匹配到的行
print “”  打印一个空行

printf命令：
格式化输出，和c语言中的printf类似。
printf(format[,arguments])
format是用“”引起来的格式
arguments是一个可选的参数列表
%s:字符串
%d：十进制整数
%f：浮点格式，默认精度小数点后6位。
%%：打印%
|%mX|:对于格式X按照默认的右对齐，精确到m位，左边补空格
|%-mX|：对于格式X按照左对齐，精确到m位，右边补空格
“%*.*g”， m， n， $val ： 动态指定精度，总共m位，小数点后n位。
\t：跳格
\n：换行

next命令：
取得下一条记录

delete命令：
删除

exit命令：
退出输入记录的处理，进入END。
exit n   设置awk的退出状态。

awk中的内置的变量：

FS  定义输入记录字段分隔符，默认是空格
OFS 输出记录字段分隔符，默认是空格
NF	当前输入记录的字段个数（列号）
RS  输入记录分隔符，默认是换行符
ORS 输出记录分隔符，默认是换行符
NR	读入的输入记录的个数(行号）
FILENAME 当前输入文件的名字
CONVFMT 控制类型转换，默认为%.6g
在BEGIN{}中可以给变量赋值；在{}中用$来引用这些变量；在END{}中可以打印这些变量。

运算符：

算数操作符：
+ - * / % ^

赋值操作符：
++ –-
+= -= *= /= %= ^=

关系操作符：
< > <= >=
==(注意和赋值=的区别） !=
~（匹配） !~（不匹配）

布尔操作符：
&& 逻辑与，优先级高于||
|| 逻辑或
！ 逻辑非

awk内置的语句：
和c语言相似

条件语句：
if (expression)
    action1
[else if (expression2) {
    action2
    action3
    ...
}]  多个动作需要用{}包围
[else if () {action1;action2;...}]  多个动作写在一行需要用；隔开。
[else if () action ; else action]  在同一行用；结束一个语句

也可以用条件运算符代替条件语句：
expression ? Action1 : action2

循环语句：
while (condition)
    action
或
while (condition) {
    action1
    action2
    …
}
或
while (condition) { action1 ; action2 ; … }

do
    action
while (condition)
或
do {
    action1
    action2
    …
} while (condition)
或
do { action1;action2;...} while (condition)

for (set_counter; test_counter; increment_counter)
    action

break:退出循环

continue：终止本次循环，进入下一次循环

数组：
array[subscript] = value
awk中的数组直接给数组元素赋值，不用指定数组大小。通过下标访问。

关联数组：
awk中的数组是关联数组，也就是可以用数值和字符作为下标来访问。

in操作符用来测试variable是否是数组array的下标，如果是条件为真。
for (variable in array)
    print variable: array[variable]
if (variable in array)
    print variable: array[variable]

删除数组元素：
delete array[subscript]

系统变量的数组：
ARGV：
命令行参数数组，下标从0开始，不包括awk脚本本身和任何调用awk脚本指定的选项。
akw -f awk.sh …    #不包括-f和awk.sh

ENVIRON：
环境变量数组，下标是环境变量的名字，元素是环境变量的值。

awk内置的函数：
函数分为算数函数和字符串函数。

算数函数有9个：
sin(x)：返回x的正弦
cos(x)：返回x的余弦
atan2(y,x)：返回y/x的反正切
exp(x)：返回e的x次幂
log(x)：返回以e为底的x的自然对数
sqrt(x)：返回x的平方根
int(x)：返回x的整数部分
rand()：返回伪随机数r，0=< r <1
srand(x)：建立rand（）的新的种子数

字符串函数：
gsub(r,s,t) 在字符串t中用s替换和正则表达式r匹配的所有字符串，返回替换的个数。
gsub(r,s) 相当于t=$0
sub(r,s,t)用s替换正则表达式在t中的首次匹配。成功返回1，失败返回0.
sub(r,s)相当于t=$0

substr(s,p)返回s中从位置p开始的子串
substr(s,p,n)返回s中从位置p开始最大长度为n的子串。

index(s,t)返回子串t在s中的位置，如果没有指定s或没有匹配项返回0.也就是返回t中的首字母在s中是第几个字符，如果首字母重复出现返回第一个的位置。

length(s)   返回字符串s长度，没有指定s返回$0的长度，\n \t \r 空格都算一个字节。

match(s,r)如果正则表达式r在s中出现，返回出现的起始位置，没有匹配返回0.

split(string, array, separator)
将字符串string分解到数组array中，数组下标从1到n，string根据指定的分隔符来分解，如果没有指定分隔符，默认为FS。返回数组中元素个数。

sprintf(“format”, expr)和printf一样。

tolower(s)将s中的所有大写字母转换成小写并返回新的字符串
toupper(s)将s中的左右小写字母转换成大写并返回新的字符串

其它函数：
getline命令（其实是个函数）：
获取下一条记录给$0,成功返回1，到达EOF返回0，失败返回-1.
getline可以从文件中读取：getline < “filename”,每次读一行。
getline从标准输入读取：getline < “-”,每次从stdin读取一行。
将输入赋给一个变量：getline var_name.
从管道读取：”command”|getline；“command”|getline var_name;把一个命令结果输出给getline.

close()：关闭打开的文件和管道

“command” | ...
close(“command”)关闭输入管道

...| “command“ > filename
close(“command > filename”)关闭输出管道

system()：执行一个命令。
system(“command options”),返回命令的退出状态。

可以定义自己的函数：
function yourfunctionname(parameter-list) {
    statements
}
可以使用系统定义的变量、语句、函数来定义自己的函数。
使用自己定义的函数可以将函数写成一个单独脚本，使用-f选项来指定多个脚本进行调用。

