# VRC Photo Gallery

VRChatワールド内の写真展示を、GitHub Pages経由で外部更新するためのデータ置き場です。

写真や展示情報をGitHub側で更新することで、VRChatワールドを再Build / 再Uploadせずに展示内容を変更できる構成を想定しています。

## 構成

```text
VRC-PhotoGallery/
├─ gallery.csv
├─ index.html
└─ images/
   ├─ 001.jpg
   ├─ 002.jpg
   ├─ 003.jpg
   └─ ...
```

## GitHub Pages

公開URLは以下の形式です。

```text
https://USERNAME.github.io/VRC-PhotoGallery/
```

画像：

```text
https://USERNAME.github.io/VRC-PhotoGallery/images/001.jpg
```

展示データ：

```text
https://USERNAME.github.io/VRC-PhotoGallery/gallery.csv
```

## 画像ファイル

画像は `images` フォルダ内に配置します。

ファイル名は3桁の連番を使用します。

```text
001.jpg
002.jpg
003.jpg
...
```

推奨仕様：

- JPEG形式を基本とする
- 最大解像度 2048 × 2048以内
- 写真は事前に適切なサイズへ縮小する
- ファイル名と展示IDを一致させる

例：

```text
ID 1 → images/001.jpg
ID 12 → images/012.jpg
```

## gallery.csv

展示情報は `gallery.csv` で管理します。

```csv
id,enabled,title,author,date,description
1,1,夏の海,AuthorA,2026-08-15,海辺で撮影した写真
2,1,夕暮れ,AuthorB,2026-08-20,夕方の浜辺
3,0,,,,
```

### 項目

| 項目 | 内容 |
|---|---|
| `id` | 展示番号 |
| `enabled` | `1` = 表示、`0` = 非表示 |
| `title` | 作品名 |
| `author` | 作者名 |
| `date` | 撮影日など |
| `description` | 作品説明 |

文字コードはUTF-8を使用します。

説明文などにカンマを含める場合は、CSVのフィールドをダブルクォートで囲みます。

```csv
1,1,最後の夏,AuthorA,2026-08-15,"海、夕暮れ、夏の終わり"
```

## 写真を追加する

例として12番目の作品を追加する場合：

1. 写真を2048px以内へリサイズ
2. `012.jpg` にリネーム
3. `images/012.jpg` として追加
4. `gallery.csv` に展示情報を追加

```csv
12,1,作品名,作者名,2026-09-16,作品説明
```

5. Commit / Push
6. GitHub Pagesへの反映後、VRChat側でReload

通常の写真追加ではVRChatワールドの再Uploadは不要です。

## 写真を非表示にする

画像を削除せず、`enabled` を `0` に変更します。

```csv
12,0,作品名,作者名,2026-09-16,作品説明
```

再表示する場合は `1` に戻します。

## 写真を差し替える

同じ展示番号を維持したまま写真だけ変更する場合は、同名ファイルを置き換えます。

```text
images/012.jpg
```

URLは変わらないため、Unity側の設定変更は不要です。

## VRChat側

VRChatワールドでは主に以下を使用します。

- `VRCStringDownloader`
  - `gallery.csv` の取得

- `VRCImageDownloader`
  - 展示画像の取得

- `VRCUrl`
  - GitHub Pages上のURL登録

画像URLはUnity側へ事前登録し、実行時のURL生成には依存しない構成を想定しています。

## 更新フロー

```text
写真を追加・変更
↓
gallery.csvを編集
↓
Commit / Push
↓
GitHub Pages更新
↓
VRChat側でReload
```

## 注意

- Repository名を変更するとGitHub PagesのURLが変わる可能性があります。
- GitHubユーザー名の変更でもURLが変わる可能性があります。
- `images` フォルダ名や画像命名規則は、運用開始後はなるべく変更しないでください。
- GitHub Pagesへ置いた画像やCSVは公開情報として扱ってください。
- 画像差し替え直後はキャッシュの影響で古い画像が表示される場合があります。

## 用途

主用途はVRChatワールド内の写真展示ですが、

- イベント写真
- スクリーンショット展示
- イラスト展示
- 制作記録
- イベント告知画像
- スタッフ・クレジット紹介

など、外部更新可能な画像ギャラリーとして利用できます。
