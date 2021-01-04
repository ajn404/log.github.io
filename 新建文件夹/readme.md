# 1.表格导入数据索引列包含回车符

![code](readme.assets/code-1609203322828.png)

*.replace(/[\r]/g,"")*去掉回车%0d

*.replace(/[ ]/g,"")*去掉空格%20

.replace(/[ ]|[\r]/g,"") 去掉空格和换行

正确的indexColumnName为%20保留，%0a换行保留

![image-20201228144721245](readme.assets/image-20201228144721245.png)

之前使用.trim（）

![image-20201228145753396](readme.assets/image-20201228145753396.png)

![image-20201228144913483](readme.assets/image-20201228144913483.png)

## **应该要去除回车**

# 2.错误信息不一致

![code](readme.assets/code-1609202721776.png)

之前

![image-20201229085419877](readme.assets/image-20201229085419877.png)

错误的message本应该为

![image-20201229085357899](readme.assets/image-20201229085357899.png)

所以改了一下错误信息