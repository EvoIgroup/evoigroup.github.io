---
date: 2023-02-08
title: "使用 Docsy 轻松记录"
linkTitle: "宣布文件"
description: "Docsy Hugo 主题让项目维护者和贡献者专注于内容，而不是从头开始重塑网站基础设施"
author: Riona MacNamara ([@rionam](https://twitter.com/bepsays))
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
  params:
    byline: "Photo: Riona MacNamara / CC-BY-CA"
---

**这是包含图片的典型博客文章。**

前面的内容指定博客文章的日期、标题、将显示在博客登录页面上的简短描述及其作者。

## 包括图像

这是一张包含署名和标题的图片 (`featured-sunset-get.png`)。

{{< imgproc sunset Fill "600x300" >}}
在即将推出的 Hugo 0.43 中获取并缩放图像。
{{< /imgproc >}}

这篇文章的前言指定了要分配给所有图像资源的属性：

```
resources:
- src: "**.{png,jpg}"
  title: "Image #:counter"
  params:
    byline: "Photo: Riona MacNamara / CC-BY-CA"
```

要在页面中包含图像，请像这样指定其详细信息：

```
{{< imgproc sunset Fill "600x300" >}}
在即将推出的 Hugo 0.43 中获取并缩放图像。
{{< /imgproc >}}
```

图像将以前面内容中指定的大小和署名呈现。


