# Crank & Combustion

物理演算でエンジン音を合成し、組んだエンジン同士でドラッグレースができるブラウザ用シミュレータ。
ファイルは 2 つだけで、ビルドもサーバー処理も要りません。

| ファイル | 役割 |
|---|---|
| `index.html` | 本体。これ 1 つで動きます |
| `garage.json` | 公開ボード。置いておくと全員の画面に同じランキングが出ます |

## GitHub Pages で公開する

1. GitHub で新しいリポジトリを作る（Public）
2. `index.html` と `garage.json` をアップロード（ドラッグ&ドロップで可）
3. リポジトリの **Settings → Pages** を開く
4. **Source** を `Deploy from a branch`、**Branch** を `main` / `/ (root)` にして Save
5. 1〜2 分待つと `https://<ユーザー名>.github.io/<リポジトリ名>/` で公開されます

コマンドで行う場合:

```bash
git init
git add index.html garage.json README.md
git commit -m "Crank & Combustion"
git branch -M main
git remote add origin https://github.com/<ユーザー名>/<リポジトリ名>.git
git push -u origin main
```

Pages の有効化だけは GitHub の画面で行ってください。

## 対戦のしかた

**リンクを送る**（推奨・アカウント不要）
対戦パネルで 2 台を選び「対戦リンクをコピー」。URL にエンジンの設定そのものが入るので、
受け取った人が開くと同じ 2 台がセットされます。サーバー処理は一切ありません。

**8 桁の番号**
「番号を発行」で 8 桁が出ます。番号は設定を指す鍵なので、`garage.json` に載っている番号か、
自分の端末で発行した番号だけが解決します。

**設定コード**
`E2-` で始まる 60 字程度の文字列。設定の実体なので、どこでも確実に動きます。

## 公開ボードを更新する

`garage.json` を書き換えてコミットすると、全員の画面に反映されます。

1. 友人にページ下部の「ガレージを書き出し」で `garage.json` を作ってもらう
2. その中の `rows` と `numbers` を、リポジトリの `garage.json` に足す
3. コミットして push

書式:

```json
{
  "kind": "crank-and-combustion-garage",
  "v": 2,
  "rows": [
    { "id": "一意な文字列", "name": "表示名", "code": "E2-…",
      "veh": "gt", "dist": 402.336, "et": 13.2, "trap": 179, "at": 0 }
  ],
  "numbers": { "10000001": { "code": "E2-…", "engine": "V8 90° · 5.00L", "at": 0 } }
}
```

`veh` は `gt` / `light` / `muscle` / `bike`、`dist` は `402.336` / `201.168` / `1000`。
同じ `veh` と `dist` の記録どうしが 1 つのランキングになります。

`et` は各自の申告値です。「挑戦」を押すと、相手のエンジンを手元で走らせ直して勝負します。

## 動作条件

- 音声には Web Audio を使います。音が出ないときは端末のマナーモードを確認してください
- 3D 表示のみ CDN から three.js を読み込むため、オフラインでは 2D 断面にフォールバックします
- 記録の保存に localStorage を使います。プライベートウィンドウでは保持されません
