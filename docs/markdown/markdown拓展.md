# 1. 插入图像
图像的创建方式跟超链接类似,并且同样有两种写法,行内式和参考式

语法中图片Alt表示如果图片加载不出来就显示图片Alt文字,图片Title表示鼠标悬停时显示的文字,两者都是非必须的

## 1.1行内式 {docsify-ignore}

语法: `![图片Alt](图片地址 "图片Title")`

代码

    十元:
        ![十元](../img/19年3月27日-21时38分.png "这是十元")

效果:

十元:
![十元](../img/19年3月27日-21时38分.png)

## 1.2参考式 {docsify-ignore}

语法:
在要插入图片的地方:'![图片Alt][标记]',在文档最后写上'[标记]:图片地址 "图片Title"'

代码:

    十元:
    ![十元][十元link]

    [十元link]: ../img/19年3月27日-21时38分.png "这是十元"

效果:

十元:
![十元][十元link]

[十元link]: ../img/19年3月27日-21时38分.png "这是十元"

# ~~2.内容目录~~(markdown2不支持)

~~在段落中填写'[TOC]'以显示目录结构~~

~~效果:~~

~~[toc]~~

# 3.注脚

语法:
在需要添加注脚的文字后面加上脚注名字[^注脚名字],这一过程称为加注.
脚注,脚注前必须有对应的脚注名字

注意：
经测试注脚与注脚之间必须空一行，不然会失效。成功后会发现，即使你没有把注脚写在文末，经Markdown转换后，也会自动归类到文章的最后

代码:

    使用Markdown[^1]可以效率的书写文档

    [^1]:Markdown是一种纯文本标记语言

效果:

使用Markdown[^1]可以效率的书写文档

[^1]:Markdown是一种纯文本标记语言

# 4.LaTex公式(数学公式)

## 4.1 使用$标记表示行内公式 {docsify-ignore}

代码:

    $E=mc^2$

效果:

$E=mc^2$

## 4.2 使用$$表示整行都是公式 {docsify-ignore}
代码:

    $$\sum_{i=1}^n a_i=0$$

效果:

$$\sum_{i=1}^n a_i=0$$

# 5.流程图 (flowchart.js)
代码:

    flow
    st=>start: Start:>https://www.zybuluo.com
    io=>inputoutput: verification
    op=>operation: Your Operation
    cond=>condition: Yes or No?
    sub=>subroutine: Your Subroutine
    e=>end
    st->io->op->cond
    cond(yes)->e
    cond(no)->sub->io

效果:

flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io

