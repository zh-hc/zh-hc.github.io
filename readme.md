## 介绍

创建第一篇文章
```bash
hugo new posts/first_post.md
````

本地启动
```bash
hugo serve
hugo serve --disableFastRender # 实时预览正在编辑的页面
```

- 以下是一些方便你清晰管理和生成文章的目录结构建议:

    - 保持博客文章存放在 content/posts 目录, 例如: content/posts/我的第一篇文章.md
    - 保持简单的静态页面存放在 content 目录, 例如: content/about.md
    - 本地资源组织
        有三种方法来引用图片和音乐等本地资源:

            使用页面包中的页面资源. 你可以使用适用于 Resources.GetMatch 的值或者直接使用相对于当前页面目录的文件路径来引用页面资源.
            将本地资源放在 assets 目录中, 默认路径是 /assets. 引用资源的文件路径是相对于 assets 目录的.
            将本地资源放在 static 目录中, 默认路径是 /static. 引用资源的文件路径是相对于 static 目录的.
            引用的优先级符合以上的顺序.

        在这个主题中的很多地方可以使用上面的本地资源引用, 例如 链接, 图片, image shortcode, music shortcode 和前置参数中的部分参数.

        页面资源或者 assets 目录中的图片处理会在未来的版本中得到支持. 非常酷的功能! 

## 主题

https://hugoloveit.com/zh-cn