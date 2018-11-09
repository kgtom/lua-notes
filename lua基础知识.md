
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

>reference
[runoob](https://www.runoob.com/lua/)
