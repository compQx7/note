# environment variable
bashの場合、~/.bashrcに下記を追加してシェル再起動
export PATH=$PATH:[NewPath]

# 解凍
tar.gz
tar --zcvf [xxx.tar.gz] [DirectoryName]
tar --zxvf [xxx.tar.gz]

tar.bz2
tar --jcvf [xxx.tar.bz2] [DirectoryName]
tar --jxvf [xxx.tar.bz2]

tar.xz
tar --Jcvf [xxx.tar.xz] [DirectoryName]
tar --Jxvf [xxx.tar.xz]

tar
tar --cvf [xxx.tar] [DirectoryName]
tar --xvf [xxx.tar]
