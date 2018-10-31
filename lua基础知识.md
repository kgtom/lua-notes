
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

### 3.数据类型
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

###





>reference
[runoob](https://www.runoob.com/lua/)
