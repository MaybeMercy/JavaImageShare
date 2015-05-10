### 图像加密分享

本算法基于真实像素和缺省像素的比，这个比值越大，图像越不清晰。

具体用法如下：

##### 加密

灰度图的每个像素点位1byte, 所以采用字节流读取的方法。图像A被分为10部分

1. 第1部分取A的54+0，54+10，54+20，……处像素
2. 第2部分去A的54+1，54+11，54+21，……处像素
3. ……

对于第1部分的54+1~54+9部分，使用缺省像素值（比如说0）进行填充。 这样在分享之后。图像A就被分成了同样大小的10部分

可以在每读取1byte之后使用简单的一重混淆置换。

##### 解密

对10张图片进行合并，首先将前文件头54个字节写入图像B，分别读图片1，图片2，……的一个字节，将所得值进行异或，将结果解密后写入图像B。

##### BUG

当合并的图像数量越多时，合成后的图像越来越清晰，数量为 >= 10时，会解密成源图像。但是当小于10时，会出现对缺省像素0进行的解密操作（不可避免），但是整体来讲由于真实像素与缺省像素的比值变大，所以图像清晰度变强。