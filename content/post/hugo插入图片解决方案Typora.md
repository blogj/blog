---
date: '2025-07-24T14:12:43+08:00'
draft: false
slug: 20250724
title: 'hugo插入图片解决方案Typora '
typora-root-url: ../../static
---



## 参考链接

https://zahui.fan/posts/b4cf69c3/

1、在文章添加以下配置

![文章配置](/wp-content/uploads/2025/image-20250724154636881.png)

```
typora-root-url: ../../static
```

2、在typora设置，按照下面图片设置

![typora设置](/wp-content/uploads/2025/image-20250724154926925.png)

3、图片复制粘贴进typora 是这样的

![image-20250724155106113](/wp-content/uploads/2025/image-20250724155106113.png)

4、实现效果，在typora撰写文章时候正常显示图片、在hugo server -D命令后访问网站也能正常显示

效果如下![效果图](/wp-content/uploads/2025/image-20250724155247913.png)

5、模版创建hugo文章

编辑 hugo 目录下的 `archetypes/default.md`,这个文件是默认创建的模版。在里面添加上上述内容。
