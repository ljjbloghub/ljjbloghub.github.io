---
published: false
pubulish: true
tag: ctf
layout: post
pubulished: true
tags: ctf
categories: document
---

## 百度杯CTF比赛-YeserCMS
提示：寻找cms漏洞，flag.php在网站根目录中  
思路：应该是一个现有的CMS模版  

CMS发现：
发现cmsEasy模版中通常都会有营销网络信息
![](https://ljjbloghub.github.io/img/yesercms1.png)

在seebug中找到cmsEasy存在无限报错注入漏洞：  

访问url/celive/live/header.php，直接进行报错注入  
1.发送post数据包  

    xajax=Postdata&xajaxargs[0]=<xjxquery><q>detail=xxxxxx',(UpdateXML(1,CONCAT(0x5b,mid((SELECT/**/GROUP_CONCAT(database()) ),1,32),0x5d),1)),NULL,NULL,NULL,NULL,NULL,NULL)-- </q></xjxquery>
    
返回报错信息    
![](https://ljjbloghub.github.io/img/yesercms2.png)
2.继续查询数据库表名  
![](https://ljjbloghub.github.io/img/yesercms3.png)
![](https://ljjbloghub.github.io/img/yesercms4.png)
由于显示长度限制，通过调整1，30来显示报错  
3.查看列名    
![](https://ljjbloghub.github.io/img/yesercms5.png)
4查看用户名密码    
![](https://ljjbloghub.github.io/img/yesercms6.png)
获得密码的md5值  
![](https://ljjbloghub.github.io/img/yesercms7.png)
5.查看robots文件，发现后台登录页面/admin,登录后台，找到模版编辑获取页面文件，试着修改这些模板文件，但是发现修改后无法保存修改。但是，看这个界面，我们应该可以猜到后台是直接读取了这些文件，那么我们通过修改request里的参数通过路径遍历就可以直接读取falg.php里的信息了。 
![](https://ljjbloghub.github.io/img/yesercms8.png)


