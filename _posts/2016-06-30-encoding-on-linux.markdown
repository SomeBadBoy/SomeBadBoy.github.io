---
layout: post
title:  "记一次mac上的文件乱码处理"
date:   2016-06-30 14:26:32 +0800
categories: linux
---
起因是某次需求需要洗一批数据，判断国内、国际、港澳台地区订单，打上不同的标识。订单数据中存有国家以及省市字段，需要导出订单数据，在线下打上对应标识。于是乎，我写了个shell的脚本，希望能搞定这个事儿（后来的事实狠狠的打了我的脸，不过那是另一个故事了）。在意料之中的，我遇到了中文编码的问题。

别的不说，先贴代码

```bash
#!/bin/bash
ORDER_FILE=$1
RESULT="$1-result.csv"
echo "orderNum,regionType" >> $RESULT
echo "Wash $ORDER_FILE start..."
ORDER_ARRAY=($(awk -F',' '{print $1}' $ORDER_FILE))
COUNTRY_ARRAY=($(awk -F',' '{print $2}' $ORDER_FILE))
PROVINCE_ARRAY=($(awk -F',' '{print $3}' $ORDER_FILE))
FILE_LEN=`wc -l $ORDER_FILE | awk '{print $1}'`
for (( i = 1; i < $FILE_LEN; i++ ));
do
	if [ -z "${COUNTRY_ARRAY[i]}" ] ; then
		echo "${ORDER_ARRAY[i]},1" >> $RESULT
	elif [ "中国" = "${COUNTRY_ARRAY[i]}" ]; then
		if [ "香港" = "${PROVINCE_ARRAY[i]}" -o "澳门" = "${PROVINCE_ARRAY[i]}" ]; then
			echo "${ORDER_ARRAY[i]},3" >> $RESULT
		elif [ "台湾" = "${PROVINCE_ARRAY[i]}" ]; then
			echo "${ORDER_ARRAY[i]},4" >> $RESULT
		fi
	else
		echo "${ORDER_ARRAY[i]},2" >> $RESULT
	fi
done
echo "Wash $ORDER_FILE end..."
echo "Total Order Count in $ORDER_FILE is $FILE_LEN which is include the header"
exit 0;
```

可以看到，我在脚本中使用了`中国`、`台湾`这样的汉字当做条件来判断。写的时候就想过可能遇到中文编码的问题，还特意的用vim copy了一部分的case到单独的一个测试文件测试了一遍，脚本正常work，但是实际的运行结果是失败的，判断条件没有生效，最后的结果都是1。

google了linux文件乱码问题，发现可以使用`file`命令查看文件编码，发现我的文件都是`ISO8859`格式的，而linux脚本默认读取格式是`UTF-8`，编码不一致的问题导致了判断失效。但是，为什么测试文件能通过呢？于是我同样的`file`了测试文件，发现编码是`UTF-8`，于是，我希望把其余的文件转为`UTF-8`格式。我找到了`iconv`命令，`iconv -f source_encoding -t to_encoding your_file`，需要看自己系统支持哪些encoding可以使用`iconv -l`查看。很不幸的是，`ISO8859`这种编码在我的mac上没找到对应的编码格式，于是我不得不放弃批量转换的做法。换一个角度想，测试文件是我通过vim copy并保存的，那能不能从这个方便着手呢？

于是我google了`vim中文乱码`，发现是vim在打开文件时会查找vim配置项`fileencodings`来决定使用哪种编码方式读取文件。我的设置如下：
``set fileencodings=ucs-bom,utf-8,utf-16,gbk,cp936,gb18030,big5,gb18030,latin1``
优先级从左至右依次递减，也就是说vim会默认先以`ucs-bom`编码方式读取文件，如果不匹配，则使用下一个，直到最后的`latin1`。编码方式推荐从严格到宽松，这样能尽量让vim用准确的编码方式读取文件。同时vim还有两个配置`fileencoding`,`encoding`同样是表示编码的，`fileencoding`表示的是vim保存文件时采用的编码格式，`encoding`则是vim内部使用的编码格式，一般不推荐改`encoding`，默认是`UTF-8`，足以应付绝大多数情况的case了。

到这里，解决方案已经出来了，虽然比较土，我还是用vim一一打开了数据文件，`set fileencoding=utf-8`然后再save得到无乱码文件。
