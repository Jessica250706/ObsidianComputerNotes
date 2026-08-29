# 常用函数

|方法|说明|
|---|---|
|dict.clear()|删除所有元素|
|dict.copy()|复制字典|
|dict.get(key)|获取键为key对应的值,没有返回None|
|dict.get(key,value)|获取键为key对应的值,没有返回value|
|dict.pop(key)|如果键为key存在,返回对应的值,并删除该键值对,否则会产生KeyError|
|dict.pop(key,value)|如果键为key存在,返回对应的值,并删除该键值对,否则返回value|
|dict.popitem()|弹出最后一个键值对,结果是一个元组|
|dict.setdefaut(key,value=None)|如果键值key存在,返回对应的值,否则增加键值对(key,value)(注意这里只是表示一个对,不是元组),value可以有默认值None|
|dict.update([other])|使用字典或键值对,来更新或添加键值对到字典|
|dict.items()|返回所有元素|
|dict.keys()|返回所有键|
|dict.values()|返回所有值|