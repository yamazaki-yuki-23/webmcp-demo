# WebMCP デモ: 商品検索

開いている Web ページの検索関数を、WebMCP のツールとしてブラウザに登録するサンプルです。
`index.html` 1 枚だけで動きます。商品は 3 件（マウス 2,000 円、キーボード 4,500 円、モニター 18,000 円）です。

## 動かし方

1. Chrome 156 以降で `chrome://flags/#enable-webmcp-testing` を Enabled にして再起動する
2. `index.html` を Chrome で開く（ファイルを直接開いても、`python3 -m http.server 8000` で配信してもよい）
3. 画面に「WebMCPツールを登録しました。」と出ることを確かめる
4. DevTools を開き、Application → WebMCP → Available Tools で `search_products` を選ぶ
5. Run Tool の `maxPrice` に `5000` を入れて Run Tool を押す
6. 画面に「マウス: 2000円」「キーボード: 4500円」が出て、呼び出し履歴の Output にも同じ一覧が返る

## ページがしていること

- `document.modelContext.registerTool()` に、ツール名・説明・入力の条件（JSON Schema）・実行する関数を渡して登録する
- 実行する関数は、検索ボタンを押したときと同じ `searchProducts()` を呼ぶ。画面を更新し、同じ一覧を呼び出し元にも返す
- `document.modelContext` が無いブラウザでは、フォームからの検索だけが動く

## スクリプトから呼ぶとき

Chrome 157 では、`executeTool` にツール名の文字列ではなく `getTools()` が返すツールのオブジェクトを渡します。

```js
const tools = await document.modelContext.getTools();
const tool = tools.find((t) => t.name === "search_products");
await document.modelContext.executeTool(tool, { maxPrice: 5000 });
```

## 参考

- 解説記事: https://qiita.com/tamakiiii/items/b0502acefe741305bfab
- 仕様（草案）: https://webmachinelearning.github.io/webmcp/
- Chrome のドキュメント: https://developer.chrome.com/docs/ai/webmcp

WebMCP は W3C Web Machine Learning Community Group で検討中の草案です。API は今後変わることがあります。
