# パッケージ選定
react-window
- divテーブルにも対応、逐次ロード対応（拡張パッケージ）
- コミット頻度は高くないがreact-virtualizedほどではない。

react-virtulized
- react-windowと作者は同じ。
- 多くの機能を取り込みすぎて動作が重いとのことで、作者自身react-windowの使用を推奨

tanstack/table
- Headless UI
- 検証ハードルと実装難易度が高かったため未検証

react-virtuoso
- tr,th,tdなどを前提としている実装と判断して未検証
- ドキュメントを見た感じでは逐次ロード機能がない。

# react-windowとその拡張パッケージ
[react-window](https://github.com/bvaughn/react-window)
- 大きなテーブルデータなどのレンダリングをウィンドウで最適化する。

[react-window-infinite-loader](https://github.com/bvaughn/react-window-infinite-loader)
- ウィンドウスクロール内で逐次読み込みを行う。

[react-virtualized-auto-sizer](https://github.com/bvaughn/react-virtualized-auto-sizer)
- 親コンポーネントのサイズに応じてウィンドウサイズを調整する。

# 主なコンポーネント
- FixedSizeList: 単方向、固定幅
- FixedSizeGrid: 縦横両方向、固定幅
- VariableSizeList: 単方向、変動幅（セルごとに指定可）
- VariableSizeGrid: 縦横両方向、変動幅（セルごとに指定可）
※「単方向」については、ソース内に水平方向は廃止する旨のコメントを見かけたため、垂直方向のみの想定です。

# 使用サンプル
- [CodeSandBox](https://react-window.vercel.app/#/examples/grid/variable-size)
- [react-windowでこんなことできる？のまとめ](https://zenn.dev/zaki_yama/articles/react-window-tips)
- [react-window で巨大なリストを低コストに表示する](https://qiita.com/karszawa/items/4fbad2649af830f0f310)
- [repo](https://github.com/forbole/big-dipper-2.0-cosmos/blob/main/packages/ui/src/screens/blocks/components/desktop/index.tsx#L30)
- [repo](https://github.com/forbole/big-dipper-2.0-cosmos/blob/main/packages/ui/src/screens/blocks/components/desktop/index.tsx)
- [repo:複数VariableSizeGrid](https://github.com/dangtv/BIRDS/blob/master/webui/client/src/common/QueryResultDataTable.js#L196)
- [repo:スクロールバー非表示](https://github.com/bvaughn/react-virtualized/blob/v9.22.3/source/MultiGrid/MultiGrid.js#L646)


# 無限スクロールと両方向の仮想スクロール
react-windowにおいて、無限スクロールを利用したうえで縦横両方向のウィンドウィングを実装することは、下記理由により難しそう。
・react-window-infinite-loaderはreact-windowのList形式しか対応していない。（テストコードにもGrid形式の利用例はない）
・無限スクロールを使えるList形式では、両方向のウィンドウィングに対応する実装にはなっていない。

追記
[issue](https://github.com/bvaughn/react-window/issues/613)のコードで動いた。

```javascript
import { VariableSizeGrid } from 'react-window';
import InfiniteLoader from 'react-window-infinite-loader';
import AutoSizer from 'react-virtualized-auto-sizer';

export const GridTable = ({ props }) => {

	~

	const loadMoreItems = useCallback(async (startIndex, stopIndex) => {
		setMoreItemsLoading(true);

		const data = await fetchData('http://localhost:30000', startIndex === 0 ? startIndex : startIndex + 1, 10);

		setMoreItemsLoading(false);
		setPage((prev) => prev + 1);
		setRows((prev) => [...prev, ...data.rows]);
	}, []);

	const itemCount = rows.length + 1;

	const isItemLoaded = (index) => index < rows.length - 1;

	// 仮想ロードを行う閾値
	const virtualLoadThreshold = 2;
	// 無限ロード（データ取得）を行う閾値
	// 仮想ロード以上の数字を指定する必要がある。
	const infiniteLoadThreshold = virtualLoadThreshold + 3;

	~

	return (
		<AutoSizer style={{ minHeight: '750px' }}>
			{({ height, width }) => (
				<InfiniteLoader
					isItemLoaded={isItemLoaded}
					itemCount={itemCount}
					loadMoreItems={loadMoreItems}
					ref={infiniteLoaderRef}
					// minimumBatchSize={1}
					threshold={infiniteLoadThreshold}
				>
					{({ onItemsRendered, ref }) => (
						<VariableSizeGrid
							className="grid"
							height={height}
							width={width}
							ref={ref}
							columnCount={columnDefs.length}
							columnWidth={(index) => columnDefs[index].width}
							rowCount={itemCount}
							rowHeight={(index) => rowStyle.height}
							itemData={rows}
							onItemsRendered={({
								overscanRowStartIndex,
								overscanRowStopIndex,
								visibleRowStartIndex,
								visibleRowStopIndex,
							}) => {
								onItemsRendered({
									overscanStartIndex: overscanRowStartIndex + virtualLoadThreshold,
									overscanStopIndex: overscanRowStopIndex + virtualLoadThreshold,
									visibleStartIndex: visibleRowStartIndex + virtualLoadThreshold,
									visibleStopIndex: visibleRowStopIndex + virtualLoadThreshold,
								});
							}}
							>
							{({
								columnIndex,
								rowIndex,
								style,
								data,
							}) => (
								<Cell
									key={`r${rowIndex}c${columnIndex}`}
									style={style}
									columnDef={columnDefs[columnIndex]}
									row={data[rowIndex]}
								/>
							)}
						</VariableSizeGrid> 
					)}
				</InfiniteLoader>
			)}
		</AutoSizer>
	)
}
```

# スクロールバーを非表示にしつつスクロールを行う
- react-windowのグリッドをdiv（サイズ指定）で囲い、スクロールバーをdivからはみ出させることで、スクロールバーを隠しつつ横スクロールを可能にすることはできました。（スクロールバーは存在しているが見せない、という方向性です。）
[複数グリッド間で同期したい](https://zenn.dev/zaki_yama/articles/react-window-tips#%E8%A4%87%E6%95%B0-grid-%E9%96%93%E3%81%A7%E3%82%B9%E3%82%AF%E3%83%AD%E3%83%BC%E3%83%AB%E3%82%92%E5%90%8C%E6%9C%9F%E3%81%97%E3%81%9F%E3%81%84)
- 別にMac対応が必要

# セルの高さや幅を自動で設定したい
・VariableSizeGrid（縦横 サイズ可変 + 仮想スクロール）の場合、サイズを`(index) => { }`形式の関数で指定する必要がある。
・上記関数は数字を返す必要がある。（文字列で'3rem'などを渡しても正しくレンダリングされない）
・Reactのstyleプロップでは、単位の指定がない場合はデフォルトで"px"となる。
 
このため、高さ（幅）を数字で渡すことになり、どうしてもpx指定になってしまうと思われます。
