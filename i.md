# 博客使用说明

## 本地测试

项目需要 Node.js 22 和 pnpm 9 或更高版本。

第一次运行时，在仓库根目录安装依赖：

```sh
pnpm install
```

启动开发服务器：

```sh
pnpm dev
```

然后在浏览器中打开：

```text
http://localhost:4321/
```

也可以明确指定监听地址和端口：

```sh
pnpm dev --host 127.0.0.1 --port 4321
```

如果端口已被占用，可以换一个端口，例如：

```sh
pnpm dev --port 4322
```

开发模式适合随写随看，保存 Markdown 文件后页面会自动刷新。需要注意的是，开发模式中的 Pagefind 搜索使用模拟结果；要测试实际的全文搜索，请构建生产版本并本地预览：

```sh
pnpm build
pnpm preview
```

预览地址通常也是：

```text
http://localhost:4321/
```

端口冲突时可运行：

```sh
pnpm preview --port 4322
```

发布前建议至少执行：

```sh
pnpm check
pnpm build
```

## 添加文章

推荐使用项目提供的命令创建文章：

```sh
pnpm new-post my-post
```

命令会生成：

```text
src/content/posts/my-post.md
```

文件名会决定文章地址。例如 `my-post.md` 对应：

```text
/posts/my-post/
```

新文章默认包含如下 frontmatter：

```yaml
---
title: my-post
published: 2026-07-28
description: ''
image: ''
tags: []
category: ''
draft: false
lang: ''
---
```

可以修改成：

```yaml
---
title: "文章标题"
published: 2026-07-28
updated: 2026-07-29
description: "显示在首页和搜索结果中的简短摘要"
image: ""
tags: ["算法", "题解"]
category: "题解"
draft: false
---
```

字段说明：

- `title`：文章标题。
- `published`：发布日期，必填。
- `updated`：最后更新日期，可选。
- `description`：首页和搜索结果中显示的摘要。
- `image`：文章封面，可留空。
- `tags`：文章标签列表。
- `category`：文章分类。
- `draft`：设为 `true` 时，文章在开发模式中可见，但不会进入生产构建。
- `lang`：仅当文章语言与全站默认的中文不同时填写。

`published` 和 `updated` 必须写成不加引号的 YAML 日期，否则 Astro 的日期校验会失败：

```yaml
published: 2026-07-28
```

不要写成：

```yaml
published: "2026-07-28"
```

frontmatter 下方直接使用标准 Markdown 编写正文。

### 公式

行内公式：

```markdown
$a^2+b^2=c^2$
```

块级公式：

```markdown
$$
\sum_{i=1}^{n} i = \frac{n(n+1)}{2}
$$
```

### 代码块

````markdown
```cpp
int main() {
    return 0;
}
```
````

### 图片

简单做法是把图片放到 `public/images/posts/`，正文中使用从站点根目录开始的路径：

```markdown
![图片说明](/images/posts/example.png)
```

如果一篇文章有多张专属图片，推荐使用独立目录：

```text
src/content/posts/my-post/index.md
src/content/posts/my-post/cover.png
src/content/posts/my-post/example.png
```

这时封面和正文图片都可以使用相对路径：

```yaml
image: "./cover.png"
```

```markdown
![图片说明](./example.png)
```

## 检查新文章

添加或修改文章后运行：

```sh
pnpm check
pnpm dev
```

在浏览器中检查文章标题、日期、分类、标签、公式、代码块和图片是否正确。发布前再执行一次生产构建：

```sh
pnpm build
pnpm preview
```

## 发布

确认本地检查通过后提交并推送：

```sh
git add .
git commit -m "Add new post"
git push
```

推送到 `main` 分支后，`.github/workflows/deploy.yml` 会通过 GitHub Actions 自动构建并部署博客。
