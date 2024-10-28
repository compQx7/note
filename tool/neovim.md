# install
## windows
```powershell
winget install Neovim.Neovim
```

## linux
### Ubuntu
この方法では最新版はインストールできない。
```sh
sudo apt install neovim
```

最新版インストール方法
/etc/wsl.confに追記しWSL環境を再起動する。
```conf
[boot]
systemd=true
```
```powershell
wsl --shutdown
```
```sh
sudo snap install nvim --classic
```
