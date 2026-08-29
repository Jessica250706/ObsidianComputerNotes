# 知识

  

## 序列类型

![[image.png]]

### 比较

||List/列表|Tuple/元组|Dict/字典|Set/集合|
|---|---|---|---|---|
|类型名称|list|tuple|dict|set|
|定界符|方括号[]|圆括号()|大括号{}|大括号{}|
|是否可变(ID可变)|是|否|是|是|
|是否有序|是|是|否|否|
|是否支持下标|是（使用序号作为下标）|是（使用序号作为下标）|是（使用“键”作为下标）|否|
|元素分隔符|逗号|逗号|逗号|逗号|
|对元素形式的要求|无|无|键:值|必须可哈希|
|对元素值的要求|无|无|“键”必须可哈希|必须可哈希|
|元素是否可重复|是|是|“键”不允许重复，“值”可以重复|否|
|元素查找速度|非常慢|很慢|非常快|非常快|
|新增和删除元素速度|尾部操作快，其他位置慢|不允许|快|快|

### 详细

[[Computer Science/High-Level Language/Python/List-列表]]

[[Tuple-元组]]

[[Computer Science/High-Level Language/Python/Set-集合]]

[[Dict-字典]]

[[Computer Science/High-Level Language/Python/String-字符串|String-字符串]]