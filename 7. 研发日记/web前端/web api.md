# canvas
canvas可以转变成图片

1. 通过toDataURL转变成base64格式的字符串，如果需要变成文件的话还需要将base64的字符串转成文件（也可以转成blob，但是都需要操作buffer，目前还不太理解字符集编码格式之类的知识，等有时间好好研究一下编码）。
2. 通过toBlob转变成blob，blob可以转成二进制的图片。

