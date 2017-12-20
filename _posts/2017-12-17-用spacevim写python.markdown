---
layout: post
author: 万仕诚
title: '用spacevim写python'
subtitle: '用终极编辑器之神写代码'
date: 2017-12-17
cover: 'http://file.wanshicheng.org/assets/img/postcover.jpg'
categories: linux
tags: vim spacevim python
---
默认的 vim：

![vim](http://file.wanshicheng.org/assets/img/vim.png)

优化后的 SpaceVim：

![SpaceVim](http://file.wanshicheng.org/assets/img/spacevim.png)

在这个看脸的时代里，连编辑器也难逃此劫。

## 安装

如果安装了 vim 或者 neovim，只需一行代码就可以得到靓丽的 SpaceVim：

```bash
curl -sLf https://spacevim.org/install.sh | bash
```

或者只为 vim 或 neovim 安装 SpaceVim：
```bash
curl -sLf https://spacevim.org/install.sh | bash -s -- --install vim
curl -sLf https://spacevim.org/install.sh | bash -s -- --install neovim
```

如果你感到不满意可以直接卸载SpaceVim：

```bash
curl -sLf https://spacevim.org/install.sh | bash -s -- --uninstall
```

## 配置
### 主题配置
SpaceVim 默认的颜色主题采用的是 gruvbox。这个棕色的主题是在不符合我的审美观。可以在 ~/.SpaceVim.d/init.vim 中修改 SpaceVim 的主题。在适当的地方添加两行，就会变成本文开头的那种主题。

```vim
let g:spacevim_colorscheme = 'one'
let g:spacevim_colorscheme_bg = 'dark'
```
### Python 配置
只需安装下面几个包，就可以使用 SpaceVim 愉快地编辑 Python 了。

 ```bash
pip install flake8
pip install yapf
pip install autoflake
pip install isort
 ```

然后创建一个 python 文件：

```bash
vim test.py
```
输入以下内容：
```python
import platform

os = platform.system()
print(platform.platform())
print(platform.version())
```
最后自己动手体验一下。接着按下 Esc 输入 SPC l r，Python 就会自动运行。这里的 SPC 指的是“空格”，官方文档就是这么写的。我在这个地方卡了好久好久～～嗯，这是它跟别的 vim 大不一样的地方。几乎所有的操作都需要使用 SPC，这就是叫做 SpaceVim 的原因吧！

### JavaScript 配置
对于写 Python Web 的童鞋，还需要做以下配置。
安装 nodejs，在 mac 下可以使用 brew：

```bash
brew install nodejs
```
然后安装 tern：

```bash
cd ~/.cache/vimfiles/repos/github.com/ternjs/tern_for_vim
npm install tern
```

最后进入 vim 输入：
```vim
:SPUpdate tern_for_vim
```
SpaceVim 的插件管理系统就会将其自动装好。

---

参考资料：

[colorscheme](http://spacevim.org/layers/colorscheme/)

[SpaceVim Layers: lang#python](https://spacevim.org/layers/lang/python/)
