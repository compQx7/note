# grep

| option | description | example |
| --- | --- | --- |
| -r | 再帰的に検索 |  |
| --include |  | `--include=\*.{js,jsx}` |
| -e | or条件を指定 |  |
| -v | 除外条件を指定 |  |
| -n | 行番号も取得 |  |
| -H | pathも取得 |  |
|  |  |  |

grep -r --include=\*.{js,jsx} "export " /src/lib > temp.txt


# find
```bash
find [path] [options]
```
- `[path]`：検索を開始するディレクトリのパス。
- `[options]`：検索条件やアクションを指定するオプション。

| option | description | example |
| --- | --- | --- |
| -type | f: file, d: directory |  |
| -path | 抽出対象のパス（*でワイルドカード） | `'*/src/*` |
| -exec | 各ファイルに対してコマンドを実行 | -exec grep "export " |
| -name | ファイル名の検索条件（*でワイルドカード） |  |
| -size | ファイルサイズで検索 c（バイト）、k（キロバイト）、M（メガバイト）、G（ギガバイト | -size +500k |
| -mtime, -atime, -ctime | 最終変更日、アクセス日、状態変更日 | -mtime -7（7日以内） |
| -or, -and, -not（または !） | 複数条件の組合せ | -name "*.txt" -or -name "*.log" |
|  |  |  |

find /src/ui -type f -path '*/src/lib/*' \( -name "*.js" -o -name "*.jsx" \) -exec grep -H "export " {} \; | grep -v "eslint-disable" > temp.txt

- **複数のディレクトリを検索**：
  ```bash
  find /path1 /path2 -name "*.log"
  ```

- **深さを制限する**（`-maxdepth`、`-mindepth` オプション）：
  ```bash
  find /path/to/search -maxdepth 2 -name "*.txt"
  ```

- **ファイルの内容で検索**（`find` と `grep` の組み合わせ）：
  ```bash
  find /path/to/search -type f -exec grep "search_string" {} \;
  ```

- **空のファイルやディレクトリを検索**：
  ```bash
  find /path/to/search -type f -empty
  ```

- **特定のパターンを除外する**：
  ```bash
  find /path/to/search -name "*.txt" ! -name "exclude_pattern.txt"
  ```
`find` コマンドの応用的な使い方を理解する上で役立ちます。続けて、いくつかの応用例を紹介します。

9. **特定の時間範囲のファイルを検索**：
   - 特定の日付以降に変更されたファイルを検索：
     ```bash
     find /path/to/search -type f -newermt "2023-01-01"
     ```

10. **特定のサイズ範囲のファイルを検索**：
    - 500KB 以上 1MB 以下のファイルを検索：
      ```bash
      find /path/to/search -size +500k -size -1M
      ```

11. **ファイル名ではなく、パスに基づいて検索**：
    - 特定のディレクトリパスを含むファイルを検索：
      ```bash
      find /path/to/search -path "*/config/*"
      ```

12. **複数の条件を組み合わせる**：
    - `.log` または `.txt` 拡張子を持ち、最終変更から7日以内のファイルを検索：
      ```bash
      find /path/to/search \( -name "*.log" -or -name "*.txt" \) -mtime -7
      ```

13. **特定の条件に一致しないファイルを検索**：
    - 644 以外のパーミッションを持つファイルを検索：
      ```bash
      find /path/to/search ! -perm 644
      ```

14. **ディレクトリの深さに基づいて検索**：
    - 特定の深さのディレクトリ内のファイルを検索：
      ```bash
      find /path/to/search -mindepth 2 -maxdepth 3
      ```

- `find` コマンドは非常に強力であり、特に `-exec` オプションを使用する場合には注意が必要です。`-exec` オプションで実行されるコマンドは、検索結果の各ファイルに対して実行されます。誤ったコマンドを実行すると、予期せぬファイルの変更や削除が発生する可能性があります。
- 複雑な条件を指定する際は、括弧 `()` を使用してグルーピングすることが重要です。これにより、条件の組み合わせが期待通りに動作することを保証できます。ただし、シェルによっては括弧をエスケープする必要があります（例：`\(` と `\)`）。

# sed
| option | description | example |
| --- | --- | --- |
|  |  |  |
|  |  |  |

sed -i 's/old_string/\t/g' filename.txt
sed 's/abc\|def/xyz/g' filename.txt
sed 's/foo.*/bar/' file.txt：file.txt 内の "foo" で始まる任意の文字列を "bar" に置換します。
