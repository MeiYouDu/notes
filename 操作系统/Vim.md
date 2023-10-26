所有的 Unix Like 系统都会内建 vi 文书编辑器，其他的文书编辑器则不一定会存在。
但是目前我们使用比较多的是 vim 编辑器。
vim 具有程序编辑的能力，可以主动的以字体颜色辨别语法的正确性，方便程序设计。
# vim简介
vim是一个又vi发展来的程序开发工具，支持代码补全，编译以及错误跳转的功能。
vim 键盘图：
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649739204468-f572a7e2-ce53-422b-b19f-6c4baffe64cf.png#clientId=uc280342b-8d26-4&from=paste&id=u3de6b5a6&originHeight=724&originWidth=1024&originalType=url&ratio=1&rotation=0&showTitle=false&size=246860&status=done&style=none&taskId=u84f46868-d995-4daa-be8c-26d879aa3b2&title=)
# vim的使用
基本上vim分为三种模式，命令模式(command code)，输入模式(insert mode)，和底线命令模式(last line mode)。
## 命令模式
进入vim就是命令模式。

- i 切换到输入模式，以输入字符。
- x 删除当前光标所在处的字符。
- : 切换到底线命令模式，以在最底一行输入命令。

若想要编辑文本：启动Vim，进入了命令模式，按下i，切换到输入模式。
命令模式只有一些最基本的命令，因此仍要依靠底线命令模式输入更多命令。
## 输入模式
在命令模式中按下i可进入输入模式

- 字符按键以及Shift组合，输入字符
- ENTER，回车键，换行
- BACK SPACE，退格键，删除光标前一个字符
- DEL，删除键，删除光标后一个字符
- 方向键，在文本中移动光标
- HOME/END，移动光标到行首/行尾
- Page Up/Page Down，上/下翻页
- Insert，切换光标为输入/替换模式，光标将变成竖线/下划线
- ESC，退出输入模式，切换到命令模式
## 底线命令模式
在命令模式下按下:（英文冒号）就进入了底线命令模式。
底线命令模式可以输入单个或多个字符的命令，可用的命令非常多。
在底线命令模式中，基本的命令有（已经省略了冒号）：

- q 退出程序
- w 保存文件

按ESC键可随时退出底线命令模式。
![image.png](https://cdn.nlark.com/yuque/0/2022/png/12763837/1649739203898-44fe9b48-56b1-470a-9703-39f5af19b9ad.png#clientId=uc280342b-8d26-4&from=paste&id=ubdc5bdde&originHeight=531&originWidth=787&originalType=url&ratio=1&rotation=0&showTitle=false&size=68472&status=done&style=none&taskId=ufb213099-03d7-43ab-83e7-43389895631&title=)


