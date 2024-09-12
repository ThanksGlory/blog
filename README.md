# blog

#### 介绍
个人博客

#### 软件架构
软件架构说明

#### 常用命令

##### # 清除缓存

```shell
hexo clean
```

##### # 生成public发布目录

```shell
hexo generate
```

##### # 部署到服务器

```shell
hexo deploy
```

##### # 启动本地服务

```shell
hexo server
```

##### # 创建文章

```shell
hexo new page --path dir/xxx
```

hexo new page --path about/me

此时 Hexo 会创建 source/_posts/about/me.md，同时 me.md 的 Front Matter 中的 title 为 "page"。这是因为在上述命令中，hexo-cli 将 page 视为指定文章的标题、并采用默认的 layout。
