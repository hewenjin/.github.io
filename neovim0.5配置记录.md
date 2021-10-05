neovim 0.5 于7月2日发布，正式带来treesitter和LSP client的支持，我也在第一时间升级到了 0.5，我的neovim/vim配置之前在Arch的环境下一直用的.vimrc，两者的配置混在一起，而 neovim 0.5 带来的新特性和新插件已经完全不能和vim兼容了，想要尝试新特性的同学，是时候把neovim和vim的配置完全分离开了。

写 neovim 的配置免不了使用一些lua，到今天花了一个半周末的时间，把有必要用lua重写的全都重写了，特别记录一下

先贴上个人仓库：

<https://github.com/HeWenJin/config>

dotfiles目录有老版的vimrc配置，其实也不算老，这一版的代码自动完成基于[coc.nvim](https://github.com/neoclide/coc.nvim)，现阶段比neovim内置的LSP更好用，还有基于[oh-my-zsh](https://github.com/ohmyzsh/ohmyzsh)的zshrc和基于[oh-my-tmux](https://github.com/gpakosz/.tmux)的tmux.conf文件，也欢迎看官拿去用

nvim目录就是3天努力的成果了，大部分插件都使用了neovim基于lua写的插件，少量目前还没有很好替代品的插件用了vim的插件，最终成果截图：
![最终成果](https://pic1.zhimg.com/v2-09ccc82fdac94a17b78955fe18f7cd48_b.png)

跟vim相比肉眼可见最大的不同就是出色的代码高亮效果了，跟VSCode这类编辑器的效果可以媲美，之前接触到的大佬的配置对代码高亮这一块都不怎么感冒，但我个人还是对代码高亮的效果有刚需，因为这个之前想要完全迁移到vim开发总是缺点动力，现在可以说neovim把这一块短板补足了。

nvim-lspinstall这个插件也让LSP(language server protocol，不是老色批)的安装变的简单，但是因为nvim大部分东西都是刚起步，特定语言的支持还不是很成熟，比如我日常使用最多的java，对应的nvim-jdtls（因为jdtls对于LSP的实现不标准，由大佬对nvim的LSP客户端专门又做了一次封装）到现在还差了一些功能，比如查看依赖树，其他语言的LSP倒是基本都安装即可用。

我这套配置实现的特性做一个总结：
1. LSP支持下的代码自动完成，代码高亮，编辑增强，集成翻译插件
2. 文件树，缓冲区，状态栏美化
3. 基于nvim-telescope的文件/文件内容/tags搜索，与neovim深度集成
4. 优化自带终端操作，并集成rest client插件方便接口测试
5. 基本debug支持，简单数据库管理支持
6. git增强且易用，基本实现与VSCode集成git一样的效果，lazygit简化git操作重点推荐

以上只是列举了我认为最重要的特性，完全的配置请看上面贴的仓库，所有使用的插件在[nvim/lua/plugins/init.lua](https://github.com/HeWenJin/config/blob/main/nvim/lua/plugins/init.lua) 里，也不用担心插件多了影响启动速度，基于lua的插件比vimscript快得多

依赖的CLI工具：

<https://github.com/kdheepak/lazygit.nvim>

<https://github.com/BurntSushi/ripgrep>

<https://github.com/tmux/tmux>

Arch Linux用户可以用`yay` `pacman`安装
如果想用我这套配置，拉下仓库后设置软链接即可，示例：
```
ln -s ~/project/my/config/dotfiles/.tmux.conf.local ~/.tmux.conf.local
ln -s ~/project/my/config/dotfiles/.zshrc ~/.zshrc
ln -s ~/project/my/config/nvim ~/.config/nvim
```
neovim插件管理器应该会在第一次进入时自动下载，视网络情况可能需要你懂得的东西，下载好后，执行`:PackerInstall`和`:PackerCompile`
参考的是tjdevires neovim大佬的配置：

<https://github.com/tjdevries/config_manager/tree/master/xdg_config/nvim>

插件来源：

<https://github.com/rockerBOO/awesome-neovim>

<https://github.com/akrawchyk/awesome-vim>

总的来说除了java的开发体验还缺点东西，大部分语言已经可以获得很好的开发体验了，作为把java当主力语言的开发者，希望接下来自己也能出一份力完善neovim的生态，毕竟生命在于折腾。
