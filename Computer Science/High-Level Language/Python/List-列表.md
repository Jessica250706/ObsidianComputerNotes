# 常用函数

|方法|说明|
|---|---|
|lst.append(x)|将元素x**添加**至列表lst尾部|
|lst.extend(L)|将列表L中所有元素**添加**至列表lst尾部|
|lst.insert(index, x)|在列表lst指定位置index处添加元素x，该位置后面的所有元素后移一个位置|
|lst.remove(x)|在列表lst中删除首次出现的指定元素，该元素之后的所有元素前移一个位置|
|lst.pop([index])|删除并返回列表lst中下标为index（默认为-1）的元素|
|lst.clear()|删除列表lst中所有元素，但保留列表对象|
|lst.index(x)|返回列表lst中第一个值为x的元素的下标，若不存在值为x的元素则抛出异常|
|lst.count(x)|返回指定元素x在列表lst中的出现次数|
|lst.reverse()|对列表lst所有元素进行逆序|
|lst.sort(key=None, reverse=False)|对列表lst中的元素进行排序，key用来指定排序依据，reverse决定升序（False）还是降序（True）|
|lst.copy()|返回列表lst的浅复制|