---
layout: post
title: 在Mac上使用LaTex制作中文简历
---
最近因为要发论文，所以接触到了LaTex，编辑功能非常强大，正好又要开始做简历，但是网上很多的中文模板不能够直接编译成功，最后折腾了半天，终于发现了两个可以用的模板。编辑出来的效果还不错。

####软件要求:
[MacTex](https://tug.org/mactex/)：Mac上免费的LaTex工具，安装可以参考[此内容](http://homepage.ntu.edu.tw/~ntut019/cwtex/cwmac-install.pdf)。
####编辑设置: 
TexShop->偏好设置->编码->'Unicode(UTF-8)
####中文简历模板1
此模板使用Mac上的自带的字体"Helvetica"。 下载模板之后，打开文件夹，打开"example/template\_cn\_blue.tex"文件然后使用**XeLatex**进行编译。然后就可以看到自动生成的简历。[百度云下载链接](http://pan.baidu.com/s/1qWMdTUW)
classic:
![resume1_classic](https://github.com/niuworld/niuworld.github.io/blob/master/_pic/resume1_classic.png?raw=true)
注意事项如下：

+ ```\moderncvtheme[blue]{classic}```可以更改简历的颜色和模板，颜色。模板只有**classic**和**casual**两种。
+ ```\usepackage[scale=0.8]{geometry}```这里可以更改日期列的长度
+ ```\photo[48pt]{picture.eps}```这里可以更改相片的尺寸

然后就可以根据自己的简历的内容使用不同的格式。所有的格式均已列出。

####中文简历模板2
模板1可以说是模板2的改良版，这个模板使用的字体不如第一个模板。但是可以提供的模板更多。包括: classic, casual, oldstyle, banking.请打开"example/template-zh.tex"文件。选择**LaTex**进行编译。其它注意事项同模板1.[百度云下载链接](http://pan.baidu.com/s/1i3JAxTF)
模板如下:
casual:
![](https://github.com/niuworld/niuworld.github.io/blob/master/_pic/resume2_casual.png?raw=true)
banking:
![](https://github.com/niuworld/niuworld.github.io/blob/master/_pic/resume2_banking.png?raw=true)
####中文简历模板3
模板3也是从网路上找到的，很简洁，无照片。打开"中文简历.tex"进行编辑，并且使用**XeLaTex**进行编译。[百度云下载链接](http://pan.baidu.com/s/1gdxtMa3)
模板如下：
![](https://github.com/niuworld/niuworld.github.io/blob/master/_pic/resume3.png?raw=true)
                                  
 以上的模板都是出自网络，感谢原作者的分享。