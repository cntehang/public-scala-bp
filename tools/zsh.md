# 使用 zsh

我们统一用 Macbook 作为开发电脑。zsh 在 2019 年中成为 macOS Catalina 的缺省 shell。建议也作为团队的标准 shell 方便共享很多 script 文件和配置。

更多的配置可以参考 [mac setup zsh](https://sourabhbajaj.com/mac-setup/iTerm/zsh.html).

## 切换到 zsh

运行 `chsh -s $(which zsh)`

## 初始化配置

拷贝 `～/.bash_profile` 的内容到 `~/.zshrc` 里面。要替换的比如 `PS1` 命令行提示符可以不要拷贝。

## 使用 Oh My Zsh

使用 `$ sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"` 按照 Oh My Zsh 管理工具。缺省的配置已经很好用了。

Oh My Zsh 有[很多主题](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes). 缺省的 `robbyrussell` 还不错。

Oh My Zsh 有[很多插件](https://github.com/ohmyzsh/ohmyzsh/wiki/Plugins)，可以根据自己的喜好配置在 `.zshrc` 的 `plugins` 变量里面。

注意问题：如果使用了 [NVM: Node Version Manager](https://github.com/nvm-sh/nvm), 请把 NVM 的设置放到 `.zshrc` 的最后，否则会报奇怪的错误。

## 极简设置

把 `.zshrc` 的 `plugins` 设为 `plugins=()` 不用任何插件。

进一步可以把提示符（PS1）简化为只显示当前目录，而不会在 github 目录显示当前的分支。一种可能的配置，改变 `~/.oh-my-zsh/themes/robbyrussell.zsh-theme` 为如下内容（请先做好备份）：

```sh
PROMPT='%{$fg[cyan]%}%c '
PROMPT+="%(?:%{$fg_bold[green]%}:%{$fg_bold[red]%})"
PROMPT+="➜ %{$reset_color%}"
```
