# tongqi 的学习博客

博客地址：https://tong-qiiii.github.io/

基于 Hugo Extended 0.148.2 与 Stack 主题，包含 ICPC 题解、学习笔记、资料索引、搜索、文章目录、数学公式、代码高亮、代码复制与明暗切换。

## 写题解与学习笔记

- 题解：`content/solutions/<题目名>/index.md`
- 学习笔记：`content/notes/<主题名>/index.md`
- 资料导航：`content/page/links/index.md`
- 关于页：`content/page/about/index.zh-cn.md`

可复制 `static/templates/solution.md` 或 `static/templates/note.md` 到对应文章目录。填写日期、标题、slug 和标签后，将 `draft: true` 改为 `draft: false`。使用英文 slug 便于分享链接。文章旁边可以放图片、PDF 和代码附件，以相对路径引用。

也可以在安装 Hugo 后使用：

```sh
hugo new content/solutions/my-problem/index.md --kind solution
hugo new content/notes/my-note/index.md --kind note
```

在线指南：https://tong-qiiii.github.io/notes/writing-guide/

## 本地预览

```sh
git clone --recurse-submodules https://github.com/tong-qiiii/tong-qiiii.github.io.git
cd tong-qiiii.github.io
hugo server -D
```

需要 **Hugo Extended 0.148.2**。若普通克隆未带主题，请运行 `git submodule update --init --recursive`。

正式构建：

```sh
hugo --minify --cleanDestinationDir --config config.yaml
```

## 自动发布

仓库 **Settings → Pages → Source** 选择 **GitHub Actions**。提交到 `main` 后自动构建并发布，不需要个人访问令牌。Pull Request 只验证构建，不发布。

查看发布进度：https://github.com/tong-qiiii/tong-qiiii.github.io/actions

## 自定义

- 标题、简介、菜单、颜色模式：`config.yaml`
- 首页：`layouts/index.html`
- 外观：`assets/scss/custom.scss`
- 通用附件：`static/materials/`（在网页引用 `/materials/文件名`）

主题原有演示文章保留为草稿；前缀和文章明确标为示例，不代表个人比赛记录。

仓库与网站为公开访问，上传的文章和附件也会公开。
