# NeoVim Encoding

```sh
# Vim がファイルを開くときのエンコードを確認
:set enc?
# ファイルのエンコードを確認
:set fenc?
# ファイルの改行コードを確認
:set ff?

# SJISで開き直す
:e ++enc=sjis
# 保存時のエンコード指定（SJISで保存する）
:set fenc=sjis
```

