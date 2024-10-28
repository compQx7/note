# install go in Linux

1. go をダウンロードして解凍

```sh
wget https://go.dev/dl/go1.22.0.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go1.22.0.linux-amd64.tar.gz
```

2. `/etc/profile`に`export PATH=$PATH:/usr/local/go/bin`を追記。

3. ターミナル再起動
