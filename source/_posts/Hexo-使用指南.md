---
title: Hexo 使用指南
date: 2026-11-12 13:07:58
categories: Blogging
tags: Hexo
---
## 官方使用说明书
```
Usage: hexo <command>

Commands:
  clean     Remove generated files and cache.
  config    Get or set configurations.
  deploy    Deploy your website.
  generate  Generate static files.
  help      Get help on a command.
  init      Create a new Hexo folder.
  list      List the information of the site
  migrate   Migrate your site from other system to Hexo.
  new       Create a new post.
  publish   Moves a draft post from _drafts to _posts folder.
  render    Render files with renderer plugins.
  server    Start the server.
  version   Display version information.

Global Options:
  --config  Specify config file instead of using _config.yml
  --cwd     Specify the CWD
  --debug   Display all verbose messages in the terminal
  --draft   Display draft posts
  --safe    Disable all plugins and scripts
  --silent  Hide output on console

For more help, you can use 'hexo help [command]' for the detailed information
or you can check the docs: https://hexo.io/docs/
```

## Hexo 常用指令
- `hexo new "<PostTitle>"`：用于在 `MyBlog/source/_posts` 下新增名为 `<PostTitle>.md` 的文件，即一篇新文章。
- `hexo clean`：用于清除所有缓存文件。之后再次编译网页时会强制将所有文件跑一遍。
- `hexo server` 或 `hexo s`：用于在本地启动一个预览服务器。之后在 `http://localhost:4000` 即可访问博客网页。修改文章时，每次保存都会实时更新到上面的网页中，可以预览。