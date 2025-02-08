# NeoVim environment

## Windows

下記から .zip 取得
path を通して終わり

[NeoVim_Release](https://github.com/neovim/neovim/releases)

Windows環境で設定は `$env:LOCALAPPDATA\nvim` で管理する。

### LazyVim

[公式](http://www.lazyvim.org/installation)の通りにバックアップを取る

```sh
# required
Move-Item $env:LOCALAPPDATA\nvim $env:LOCALAPPDATA\nvim.bak

# optional but recommended
Move-Item $env:LOCALAPPDATA\nvim-data $env:LOCALAPPDATA\nvim-data.bak

# clone
git clone https://github.com/LazyVim/starter $env:LOCALAPPDATA\nvim
# remove .git
Remove-Item $env:LOCALAPPDATA\nvim\.git -Recurse -Force
```

### NvChad

```sh
git clone https://github.com/NvChad/starter $ENV:USERPROFILE\AppData\Local\nvim
nvim
# remove .git
Remove-Item $env:LOCALAPPDATA\nvim\.git -Recurse -Force

# uninstall
rm -Force ~\AppData\Local\nvim
rm -Force ~\AppData\Local\nvim-data
```
