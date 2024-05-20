**Laravel基礎**

- `Breeze`: Laravelアプリケーションを構築するための必要な初期設定や基本機能を一括で提供。最小限でシンプル。
- `Breezejp`: laravel Breezeの日本語化をサポートするパッケージ。

- Breezeログイン処理の流れを詳細に説明してくれている記事
https://zenn.dev/bs_kansai/articles/5ac107b0f1cd7d


- 02_board_index_create

(課題内容)
- 掲示板一覧閲覧機能を作成。
- 掲示板作成機能を実装。
- 掲示板の作成に成功したら、掲示板一覧画面に遷移。
- ログインしているユーザーのみ掲示板を作成出来る。
- 開発環境での確認用にBoardとUserのデータを作成するseedファイルを追加。

(手順)
- [x] boardモデルを作る
```
$  php artisan make:model Board
```
- [x] Boardモデルファイルを編集
  - Userモデルとのアソシエーション定義
  ```
      public function user() {
        return $this->belongsTo(User::class);
    }
  ```

- [x] Userモデルファイルの編集
  - Boardモデルとのアソシエーション定義
  ```
      public function board() {
        return $this->hasMany(Boad::class);
    }
  ```

- [x] Boardマイグレーションファイルを編集
  -  `$table->foreignId('user_id')->constrained()->onDelete('cascade');`
    constrained()メソッド: デフォルトで関連付けられるテーブル名とカラム名を推測、自動的に外部キー制約を追加
    onDelete('cascade'): railsでいうところの`dependent: :destroy`


```
title(タイトル): string
description(本文): text
user_id(ユーザーID): foreignId
```

- [x] `docker compose exec app php artisan migrate`
- [x] `docker compose exec app php artisan migrate --env=testing`]}

- [x] ルーティング設定 

- Boardコントローラーファイル作成
```
docker-compose exec app php artisan make:controller BoardsController
```
  - [x] indexメソッドが作成
  - [x] 掲示板作成ページ表示用のcreateメソッド
  - [x] ログインユーザーのみの作成
  - [x] 掲示板作成用のstoreメソッド
  - [x] バリデーションの設定
    → タイトル(title)は入力必須かつ255文字を最大文字数とする
    → 本文(description)は入力必須かつ65535文字を最大文字数とする
  - [x] 掲示板作成後に掲示板一覧画面に遷移

- [x] 各種ビューファイルをダウンロード

- [x] Seedファイルの作成
```
docker-compose exec app php artisan make:seeder BoardsTableSeeder
docker-compose exec app php artisan make:seeder UsersTableSeeder
```
LaravelのSeed？
