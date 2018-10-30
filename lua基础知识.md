
## 一、概况

## 二、基础知识

### 1.变量格式
* 建议最好不要使用下划线加大写字母的标示符，因为Lua的保留字也是这样的。
* Lua 区分大小写语言
* 不允许使用特殊字符，如 @, $, 和 % 
* 默认情况下，变量认为是全局的

### 2.数据类型
Lua中有8个基本类型分别为：nil、boolean、number、string、userdata、function、thread和table。

~~~lua
print('hello world lua')

--[[
print('dd1')
print('dd2')
print('dd3')
--]]

--默认索引从1开始

local tbl = {"apple", "pear", "orange", "grape"}
for key, val in pairs(tbl) do
    print("Key", key)
end
 
-- 一个空的table
a={}
print("a:",a)

a["key"]="val"
-- key
key=10
a[key]=15
for k,v in pairs(a) do
    print(k..":"..v)
end
print(".......")
a3 = {}
for i = 1, 10 do
    a3[i] = i
end

print(a3["key"])

a3["key"] = "val"
print(a3["key"])

-- 没有key 
print(a3["none"])


~~~
