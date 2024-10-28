# vim基本
* オペレーターとモーション

- バッファ、ウィンドウ、タブ、折り畳みを活用する。

## オペレータ
編集動作
d
y
c
r
< >
v （ビジュアルモード）

D
c C
s S

## モーション
任意の場所へ移動
f

## テキストオブジェクト
i.aを始めに置く
w ( " t p


# mode
## insert mode
i I a A o O
c C
s S
* 手前文字を削除
Ch
* 手前単語を削除
Cw
* 改行
Ci
* インデント
Ct Cd
* １コマンド
Co[command]
* レジスタ挿入
Cr[char]
* 行を消して挿入
cc S
* 行末まで削除して挿入
c$ C
* 単語を消して挿入
ciw

## visual mode
* ビジュアルモード、行単位選択、矩形選択
v V Cv
* 選択範囲の反対側へ移動
o
* 矩形選択中に反対側へ移動
O
* ()範囲選択
[ai][b(]
* {}範囲選択
[ai][B{]
* タグ範囲選択
[ai][t]

# move
* 左下上右
h j k l
* 単語単位
w e b 
* ファイルの最上、最下
gg G
* １画面
C-f C-b
* 半画面
C-d C-u
* 行頭（空欄無視、空欄考慮）、行末
0 ^ $
* 
{ } ( ) [ ]
* 対応カッコ
%
* 行内文字検索（移動）
f F ; ,
* ファイル内検索（移動）
/ ? n N
* 次行先頭、前行先頭
+ -
* ジャンプリスト内の新しい位置へ、ジャンプリスト内の古い位置へ
C-i C-o
* 画面内の上、真ん中、下
H M L
* 定義へ移動（ローカル、グローバル）
gd gD
* 直近編集箇所へ移動
g; g,
* 直近編集箇所へ移動してインサートモード
gi

## jump
* マークリスト
:marks
* 現在地をcharにマーク
m[char]
* マークした場所へ移動（’の代わりに’を使うと、マーク位置の行頭に移動する）
`[char]
* vimが以前終了した場所に移動
`0
* ファイル内で最後に編集して閉じた場所へ移動
`"
* ファイル内で最後に変更した場所へ移動
`.
* ジャンプリスト
:ju
* ジャンプリスト内の新しい位置
Ci
* ジャンプリスト内の古い位置
Co
* 変更リスト
:changes
* 変更リスト内の新しい位置
g,
* 変更リスト内の古い位置
g;
* カーソル下の単語のタグに移動
C]

# window scroll
* 画面のみ移動
{Cy Ce}
* カーソル位置を画面中央へ、画面最上へ、画面最下へ
zz  zt  zb

# 折り畳み
インデント形式が使いやすい印象
set foldmethod=indent
* ファイル全体を１段階折りたたむ、全段階折りたたむ
zm  zM
* ファイル全体を１段階展開、全段階展開
zr  zR
* カーソル行を折りたたむ、展開する
zc  zo
* 折り畳み単位でジャンプ
zj  zk

# search
* 検索
{/?}{char} n N
[fFtT][char] ; ,
* カーソル位置wordの一致検索
 * #
* カーソル位置wordを含む検索
g* g#

# 複数ファイル
* 複数ファイルでpatternを検索
:vim[grep] [pattern] [path(*,**,etc...)]
* 次の一致箇所、前の一致箇所へ移動
:cn[ext]  :cp[revious]
* 最初の一致、最後の一致へ移動
:cfir[st]  :cla[st]
* 一致箇所一覧ウィンドウ
:cw[indow]  :cope[n]
* クイック修正ウィンドウを開く？
:ccl[ose]
* QuickFixに対して置換を実行
:cdo %s/before/after/gc | update
* システムgrep
:grep pattern [TargetFile]  検索結果表示後に :cw でQuickFixList表示
* 検索対象について
 **: カレントディレクトリ以下を再帰的に検索
 * : カレントディレクトリ内を検索

# copy
y yy(Y)
:5,7yank(y)
:10put(pu)
:5,7t10
* 単語ヤンク
yiw
# cut
x
dd
diw daw
:5,7delete
# paste
p P
:10put
:5,7m10
# レジスタ
* レジスタ内容表示
:reg
* 指定したcharレジスタにヤンク、ペースト
"[char]y "[char]p
* システムクリップボードにヤンク、ペースト
"+y "+p
* 特殊レジスタ
% 現在のファイル名
* カーソル位置の単語
CrCw


# replace
:%s/before/after/g
:%s/before/after/gc
:3,5s/b/a/g
v :s/b/a/

# 大文字小文字
viewモード [u/U]
guモーション gUモーション

# indent
<< >>
* カッコ上にカーソルがある時にブロック単位でインデント
>%
* ブロックの内側を字下げ
>ib
* <>タグのブロックを字下げ
>at
* ペーストして現在の行に揃える
]p

# auto complement
{Cn Cp}
Cn Cp
Cx Ci
Cx Cl


# macro
* マクロ記録開始
q[a-z0-9]
* マクロ記録終了
q
* マクロ利用
@[a-z0-9]
* 直前に利用したマクロ利用
@@


# buffer
* 一覧
:ls
* 次のバッファ、前のバッファ
:bn(ext) :bp(rev)
* 最初のバッファ、最後のバッファ
:bf(irst) :bl(ast)
* バッファ番号指定
:b[n]
* バッファ名指定
:b [BufName]
* バッファ削除
:bd(elete)

# window
* window分割
Cw {sv}
{:sp :vs} filename
* window切り替え
Cw {w h j k l}
Cw {c o}
{:clo :on}
* すべてのwindowを同じ幅にする
Cw=
* windowを隣と入れ替え
Cw x
* windowのcresize
:res [+-][n]
:vert [+-][n]
Cw [n][+-<>]

# tab
* 新規でタブを開く
:tabnew
* tabを開く
:tabe [FileName]
* 次のタブ、前のタブ
gt :taben[ext]  gT :tabp[revious]  3gt
* #番目のタブに移動
 #gt
* タブを#番目に移動
:tabm[ove] #
* 現在のタブを閉じる、現在以外のタブを閉じる
:tabc[lose]  :tabo[nly]
* すべてのタブでcommandを実行
:tabdo [command]  ex: qですべてのタブを閉じる
* すべてのバッファをタブにする
:tab ba[ll]
* 次のウィンドウと入れ替え
Cwx

# explorer
vim .
Enter:開く、下の階層へ行く
v:画面分割で開く
o:新規ウィンドウで開く
-:上の階層へ行く

# ?
:ar[gs]

# 終了
* 保存して終了
:wq :x ZZ
* 強制終了
:q! ZQ

# terminal
:term[inal]

# vimdiff

# others
コマンドライン上にカーソル位置のwordを挿入
CrCw
選択箇所を標準入力としてコマンドに渡して実行し、結果の出力を受け取って書き込み。コマンドを並べたファイルでコマンドランチャのようにも使える
V:![command]
:r![command]
* undo, redo
u Cr

インサートモード中にひとつだけコマンドを実行する
Co

## コマンドランチャー
v,Vで選択後に :![Command] とすることで、選択行を標準入力でCommandに渡し、その結果を標準出力として受け取って、選択行に反映させることができる。
例：選択範囲!sort 選択範囲!perl [FileName]

:!shに渡すことで、選択行に記述したコマンドを標準入力として渡して実行できる。コマンドランチャとして利用できる。

グローバルコマンド（検索して行削除など）


# vim以外との連携
* move
Ca Ce Cf Cb Af Ab
* delete
Cu Ck Cw

# help
:h vimtuor
:h
