
## 一、概况

## 二、基础知识

### 1.变量格式
* 建议最好不要使用下划线加大写字母的标示符，因为Lua的保留字也是这样的。
* Lua 区分大小写语言
* 不允许使用特殊字符，如 @, $, 和 % 
* 默认情况下，变量认为是全局的

### 2.数据类型
Lua中有8个基本类型分别为：nil、boolean、number、string、userdata、function、thread和table。

重点理解：function、thread、table

### 3.变量
Lua 变量有三种类型：全局变量、局部变量、表中的域。

Lua 中的变量全是全局变量，那怕是语句块或是函数里，除非用 local 显式声明为局部变量。

局部变量的作用域为从声明位置开始到所在语句块结束。

变量的默认值均为 nil。

~~~lua
a = 5               -- 全局变量
local b = 5         -- 局部变量

function joke()
    c = 5           -- 全局变量
    local d = 6     -- 局部变量
end

joke()
print(c,d)          --> 5 nil

do 
    local a = 6     -- 局部变量
    b = 6           -- 对局部变量重新赋值
    print(a,b);     --> 6 6
end

print(a,b)      --> 5 6

~~~

执行以上实例输出结果为：

~~~lu
$ lua test.lua 
5    nil
6    6
5    6

~~~

### 4.循环

while、for、break、 repeat  unitl 

~~~lua
-- while
a=10
while(a<20)
do
    
    print("一直执行",a)
    a=a+1
    if (a>15)
    then
      break
    end
    
end
~~~

### 5.流程控制
if 、if else 语句
~~~lua

if(0)
then 
    print("0 为true")
end

~~~
### 6.函数
* 1.默认全局函数
* 2.多返回值
* 3.可变参数...(三点)，select("#",...) 来获取可变参数的数量

~~~lua
function max(num1, num2)

        if (num1 > num2) then
           result = num1;
        else
           result = num2;
        end
     
        return result; 
     end
     -- 调用函数
     print("两值比较最大值为 ",max(10,8))
     print("两值比较最大值为 ",max(9,6))
~~~

### 7.字符串
字符串或串(String)是由数字、字母、下划线组成的一串字符。
### 1.定义字符串三种方式,双引号、单引号、[[]]。
~~~lua
str="lua"
print("str:",str)
str2='lua2'
print("str2:",str2)
str3=[[lua3]]
print("str3:",str3)
~~~


### 2.常用操作

~~~
--格式化字符串
print(string.format( "value : %d",10 ))

-- 反转
print(string.reverse( "lua" ))

~~~

## 8.数组
Lua 数组的索引键值可以使用整数表示，数组的大小不是固定的。

~~~
--一维数组，索引默认从1开始，也可以指定0，没有值的返回nil
array={"a","b"}
for i=0,2 do
    print("arr:",array[i])
end
~~~

## 9.迭代器
迭代器（iterator）是一种对象。
~~~
array = {"tom", "jerry"}

for key,value in ipairs(array) 
do
   print(key, value)
end
~~~

## 10.table
table 是 Lua 的一种数据结构用来帮助我们创建不同的数据类型，如：数组、字典等。

Lua table 使用关联型数组，你可以用任意类型的值来作数组的索引，但这个值不能是 nil。

Lua table 是不固定大小的，你可以根据自己需要进行扩容。

Lua也是通过table来解决模块（module）、包（package）和对象（Object）的。 例如string.format表示使用"format"来索引table string。


~~~
-- 初始化表
mytable = {}

-- 指定值
mytable[1]= "Lua"

-- 移除引用
mytable = nil
-- lua 垃圾回收会释放内存
~~~

### 11.Lua 模块与包
* 模块类似于一个封装库，从 Lua 5.1 开始，Lua 加入了标准的模块管理机制，可以把一些公用的代码放在一个文件里，以 API 接口的形式在其他地方调用，有利于代码的重用和降低代码耦合度。

* require 函数
Lua提供了一个名为require的函数用来加载模块。要加载一个模块，只需要简单地调用就可以了。例如：
~~~
require("<模块名>")
~~~
或者
~~~
require "<模块名>"
~~~
* require 用于搜索 Lua 文件的路径是存放在全局变量 package.path 中，当 Lua 启动后，会以环境变量 LUA_PATH 的值来初始这个环境变量。如果没有找到该环境变量，则使用一个编译时定义的默认路径来初始化。

[详见](https://www.runoob.com/lua/lua-modules-packages.html)


>reference
[runoob](https://www.runoob.com/lua/)
