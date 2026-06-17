# ボイスドラマ ランキングハブ

がるまに・ステラプレイヤー・ポケドラの3サイトのランキングをタブで切り替えて表示する統合サイトです。

## 仕組み（方式A：クライアント側fetch）

サーバーは持たず、ブラウザ上のJavaScriptが各サイトのデータを直接取得して表示します。

```
ユーザーがタブをクリック
  ↓
fetch() で対象サイトのJSON/APIを取得
  ↓
サイトごとに異なるデータ構造を共通フォーマットに正規化
  ↓
ランキング表・グラフを描画
```

## 各サイトのデータソース

| サイト | エンドポイント | 形式 |
|---|---|---|
| がるまに | `https://garumaninow.onrender.com/api/ranking` | Flask APIが返すJSON（要：web_server.pyにAPI追加・反映済み） |
| ステラプレイヤー | `https://jikaseiorangette.github.io/stellaplayer-ranking/data/ranking.json` | GitHub Pages上の静的JSON |
| ポケドラ | `https://jikaseiorangette.github.io/pokedora-ranking/data/latest_adt.json` | GitHub Pages上の静的JSON |

## 前提条件

- **がるまに**：`web_server.py` に `/api/ranking` を追加し、Renderに再デプロイしてください（CORSヘッダーも付与済み）。
- **ステラプレイヤー・ポケドラ**：GitHub Pagesの`data/`配下のJSONがそのまま読み込めるため追加対応は不要です。

## デプロイ方法

このフォルダの `index.html` をそのままGitHub Pagesなど任意の静的ホスティングに置くだけで動作します。

```
1. 新しいリポジトリを作成（例: ranking-hub）
2. index.html をルートに配置
3. Settings → Pages → Branch: main / Folder: / (root) で公開
```

## 注意点

- がるまにはRenderの無料プランの場合、アクセスが一定時間ない場合スリープします。タブを開いた際に初回読み込みが遅い・失敗する場合があります。
- 各サイトのデータ構造の差異（フィールド名やランキング件数など）はJavaScript内の `normalizeGaru` / `normalizeStella` / `normalizePoke` 関数で吸収しています。サイト側の仕様が変わった場合はここを更新してください。
