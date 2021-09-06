# PACMAN Lab Guide

![Build documentation with mkdocs](https://github.com/thu-pacman/workflows/Build%20documentation%20with%20mkdocs/badge.svg)

本文档为 PACMAN 实验室指南，采用 `mkdocs` 编写。

本站点的自动编译版本在 [这里](https://pacman.cs.tsinghua.edu.cn/guide/) 发布。

虽然本文档没有公开发布，但请不要在其中添加任何敏感信息，**尤其是任何的用户凭据**。

## 撰写

本站点内容使用 Markdown 进行编写。具体可查看 [mkdocs](https://www.mkdocs.org/) 和 [mkdocs-material](https://squidfunk.github.io/mkdocs-material/extensions/pymdown/) 文档。

如果创建了新页面，需要插入到 `mkdocs.yml` 的 `nav` 部分，否则将不会出现在编译结果中。

## 编译

首先安装依赖，而后编译即可：

```bash
python3 -m pip install --user -r requirements.txt # 安装 Python 依赖包
mkdocs serve # 直接在本地 serve，或者：
mkdocs build --clean # 生成于 site/ 文件夹中
```
