### 04 board image

- [x] baoardテーブルにimageカラムの追加
  `docker compose exec app php artisan make:migration add_image_to_boards_table --table=boards`
  string型
  down処理の記載忘れずに

- [x] boardモデルにカラム追加

- [x] boardコントローラーにimageを追加

- [x] board createビューにimageフォームを追加
  `enctype="multipart/form-data"` ->入力しないとファイルアップロードできない

- [x] board indexビューにimageを表示
  掲示板画像の表示には`Storage::url`を使用
  `<img src={{ Storage::url("images/".$board->image) }} alt="" />`

- [x] `.gitignore`に`src/public/storage`を追加

? 画像をディレクトリに保存する
  - 画像ファイルそのものはLaravelアプリケーションのディレクトリ上に保存し、その画像のパスや関連情報をデータベースに保存する

! シンボリックリンク作成
Laravelでは、デフォルトで storage/app/public ディレクトリにファイルを保存することが推奨されている。
このディレクトリは、public ディレクトリにシンボリックリンクを作成することでウェブからアクセス可能。
`php artisan storage:link`

- `$fillableプロパティ`: マスアサインメントによって設定可能な属性を制御します。これにより、セキュリティ上の理由から、意図しない属性の変更を防ぐ
  Railsでいうところのストロングパラメータのようなもので、Larvelではそれをモデルに記載している
  両者のモデルの働きの違いは他に、
  - DBスキーマの管理が自動的にされるか否か。スコープとクエリにも違いがある。

### 05_board_detail

- [x] 詳細画面の作成(ビュー)

- [x] showを追加(コントローラー)

- [x] ルーティングを追加

- [x] コメントモデルを作成
  - text comment

- [x] コメントマイグレーションを作成
  - ユーザー(1対多)とボード(1対多)のアソシエーション

- [x] ルーティングを追加
  - ボード内にネスト
    `Route::prefix('boards/{board}')->group(function ()`

- [x] コメントコントローラーを作成
  - index, new, create
