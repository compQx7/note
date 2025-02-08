# sql server

## 挙動メモ

- 動的 SQL で `'` をエスケープする場合は `''`。
- データが 0件の場合、結果セットは存在し、列のメタデータも含まれますが、行データが存在しない状態？何も返さない？
- `+` による文字結合は null が絡むと結果も null になることがある。null が絡む可能性がある場合は CONCAT が安全。
- CAST は ANSI SQL 標準に準拠した型変換。CONVERT は SQL Server 固有で日時フォーマット等を指定できる。

## プロシージャ

カーソル例

```sql
DECLARE @EmployeeId AS INT;
DECLARE @EmployeeName AS NVARCHAR(100);

DECLARE employee_cursor CURSOR LOCAL FOR
SELECT EmployeeId, EmployeeName FROM Employee;

OPEN employee_cursor;
FETCH NEXT FROM employee_cursor INTO
    @EmployeeId, @EmployeeName;

WHILE @@FETCH_STATUS = 0
BEGIN
    -- 処理
    FETCH NEXT FROM employee_cursor INTO
        @EmployeeId, @EmployeeName;
END

CLOSE employee_cursor;
DEALLOCATE employee_cursor;
```

## merge syntax

```Sql
MERGE INTO target_table AS target
USING source_table AS source
ON target.key_column = source.key_column
WHEN MATCHED THEN
    -- 条件が一致する場合の更新処理
    UPDATE SET target.column1 = source.column1
WHEN NOT MATCHED BY TARGET THEN
    -- ターゲットテーブルに一致する行がない場合の挿入処理
    INSERT (column1, column2)
    VALUES (source.column1, source.column2)
WHEN NOT MATCHED BY SOURCE THEN
    -- ソーステーブルに一致する行がない場合の削除処理
    DELETE
WHEN NOT MATCHED BY SOURCE AND <additional_condition> THEN
    -- ソーステーブルに一致する行がなく、追加の条件が満たされる場合の更新処理
    UPDATE SET target.column1 = value;
```
