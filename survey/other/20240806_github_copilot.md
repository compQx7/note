# GitHub Copilot

## Copilot を最大限活かすためのポイント

- 3種類のチャットを使い分けること
- copilot の立場に立って考えて、いかに良いインプットを渡すかに限る。
- チャットで聞きたいことに応じて、chat participants, slash command, context (@, /, #) を利用する。

## view

Chat Panel
Ctrl + Alt + I

Quick Chat
Ctrl + Shift + I

Inline Chat
ターミナルでも可能
Ctrl + I

## Chat participants and Slash commands

@workspace: 現在のワークスペースをコンテキストに含める
@vscode: VSCode 自体に関する質問に含める
@terminal

/help: GitHub Copilot に関する質問
/clear: セッションクリア
@workspace /explain (or /explain)
@workspace /fix (or /fix)
@vscode /api (or /api): 拡張機能開発関連
@vscode /search (or /search): 検索クエリ提案

以下、生成系
/doc
@workspace /tests (or /fix)
@workspace /new (or /new)

## Chat context

特定の情報をコンテキストに含める。
attach context button でも同様のことができる。

#codebase
#editor: アクティブエディター
#git: 現在のリポジトリの Git 情報
#selection: エディター内の見えているコード
#file: 特定ファイル（ファイルの完全な内容を含める）
#terminalLastCommand
#terminalSelection

## provide context

- open files
- top level comment
- appropriate includes and references
- meaningful function name
- specific and well-scoped function comments
- prime copilot with sample code
- Be consistent and keep the quality bar high （コンテキストにゴミがあると出力もゴミ）
- Use chat variables for context
- be specific in your ask and break down a large task into separate, smalller tasks.
- Iterate on your solution （会話を記憶しているため、繰り返し会話する）

## prompt

- Be specific and detailed in your question, avoiding vague or ambiguous terms like "what does this do" (where "this" could be interpreted as the last answer, current file, or whole project, etc.).
- Incorporate terms and concepts in your prompt that are likely to appear in your code or its documentation.
- Review the used references in the response to ensure that the files are relevant. Iterate on your question if necessary.
- Explicitly include relevant context by selecting code or mentioning chat variables such as #editor, #selection, or #file.
- Responses can draw from multiple references, such as "find exceptions without a catch block" or "provide examples of how handleError is called". However, don't anticipate a comprehensive code analysis across your codebase, such as "how many times is this function invoked?" or "rectify all bugs in this project".
- Avoid assuming information beyond the code (for now), such as "who contributed to this file?" or "summarize review comments for this folder".

## improve the quality of input provided to copilot

・参考情報を開いておく。
　例：定義元ファイル、似た作りの関連ファイル、テストファイル
・参考情報をファイル内に埋め込む。
　例：変数や関数の説明をコメントで追記する。定義を貼り付ける。
・VSCode に準備されているコンテキスト構文（？）を利用する。
　例：@workspace, #editor, #file <path>, /test etc...
　https://code.visualstudio.com/docs/copilot/workspace-context
・質問を具体的にする。
　例：選択範囲に対して、「/test テストコードを生成」ではなく「/test 出力される HTML を検証するテストコードを生成」
・セッション内の会話もコンテキストに含まれるので繰り返し質問する。
　例：「/test 出力される HTML を検証するテストコードを生成」->「xxx prop が null の時のケースも追加」
