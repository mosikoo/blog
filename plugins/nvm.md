## nvm使用简介

####安装

使用curl方式来安装
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.2/install.sh | bash
```
装完之后可以在 `~/.nvm`中看到

####配置环境变量
`zsh`是在`~/.zshrc`配置

```
vim ~/.zshrc //打开配置文件

//最后一行加上
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" 
```

为了每次打开bash都能将nvm自动加到环境变量中

重启

```
source ~/.zshrc
```

### 使用




