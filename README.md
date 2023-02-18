# THU 移动应用开发 Flutter 教程

<https://thu-mobile-dev.github.io/book>

- 生成 [MkDocs](https://www.mkdocs.org) 
- 主题 [Material](https://github.com/squidfunk/mkdocs-material)

## 添加文章

在 `docs/` 文件夹下添加文章，在 `mkdocs.yml` 的 `nav` 部分添加文章路径（从 `docs/` 往后开始）。

## 本地调试

（单次）首先安装 `mkdocs`：

```sh
pip install mkdocs-material
```

clone 本仓库，进入 `book/` 之后执行：

```sh
mkdocs serve
```

等待构建和部署完成，浏览器打开 <http://127.0.0.1:8000/book/> 即可看到本网站。这种方式支持热加载，Markdown 文件在修改之后保存网页会重新构建并自动刷新。

## 云端部署

### nginx

使用 `mkdocs build` 得到 `site/` 打包上传至服务器解压，在 nginx 的配置文件中设置端口和路径即可。

### 使用 GitHub Pages

本仓库已经添加 `.github/workflows/issueMySite.yml`，将仓库设置为 public，并在仓库页的 `Settings > Pages` 选择 `gh-pages` 等待片刻即可在 <https://thu-mobile-dev.github.io/book> 看到本网站。当修改提交到 main 分支，网页会自动构建部署。
