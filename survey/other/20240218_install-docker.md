# docker environment

## how to install docker in Ubuntu of WSL2

1. 簡単な手順であるスクリプトによるインストール

    ```sh
    sudo apt update
    sudo apt upgrade
    curl -fsSL https://get.docker.com -o get-docker.sh
    sudo sh get-docker.sh
    ```

    インストール後、Dockerサービスが自動で起動するように設定される。

2. ルート権限無しでDockerを実行できるようユーザーをDockerグループに追加

    ```sh
    sudo usermod -aG docker $USER
    ```

3. WSL2の再起動

## how to uninstall docker perfectly

1. dockerパッケージのアンインストール

    ```sh
    sudo apt purge docker-ce docker-ce-cli containerd.io
    ```

2. dockerの設定とデータの削除

    ```sh
    sudo rm -rf /var/lib/docker
    sudo rm -rf /var/lib/containerd
    ```

3. dockerグループの削除

    ```sh
    sudo groupdel docker
    ```

4. システムの再起動

    ```sh
    sudo reboot
    ```
