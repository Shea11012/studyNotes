数组和变量的区别：变量是在内存中占用的空间是离散的，数组在内存中是先开辟一段连续的大内存空间，随后数组中的每个元素都放入数组内存中。数组元素使用数组 index 标识。

bash 有两种数组：普通数组和关联数组。普通数组只能使用整形数值作为数组索引，关联数组可以使用字符串作为索引。

### 普通数组

定义数组 `array=(1,2,3)`

引用数组 `${array[2]}`

打印数组所有值 `${array[*]}` 或 `${array[@]}`

查看数组索引号 `${!array[*]}`

显示数组中的元素个数（只统计值不为空的元素 ）`${#array[*]}`

### 关联数组

使用关联数组必须先使用 declare -A 声明它 `declare -A array`

除了定义操作，其余操作和普通数组没有差别

### 数组元素的截取、替换

`array0=${array[*]:2:2}` 从数组全部元素中第 2 个元素向后截取 2 个元素出来

`array1=${array[*]/5/6}` 将数组中的 5 替换为 6

`array2=${array[*]#*0}` 从左非贪婪匹配并删除所有数组变量中匹配内容

`array3=${array[*]##*0}` 从左贪婪匹配并删除所有数组变量中匹配的内容

`array4=${array[*]%0}` 从右非贪婪匹配并删除所有数组变量中匹配内容

`array5=${array[*]%%0}` 从右贪婪匹配并删除所有数组变量中匹配内容



数组 for 循环三种用法：

```
for i in ${array[*]};do
	echo $i
done
```

```
for ((i=0;i<${#array[*]};i++));do
	echo ${array[$i]}
done
```

```
for i in ${!array[*]};do
	echo ${array[$i]}
done
```





