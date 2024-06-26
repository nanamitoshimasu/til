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
- factroyを使って大量データを登録
  参考: https://qiita.com/hirotototototo/items/689947637042cf5c3284
  Laravelのバージョンでも書き方が異なる場合がある？？ 

- ルーティング
```
Route::resource('boards', BoardsController::class)
    ->only(['index', 'create', 'store']) <- 一括ルーティングして必要な文をonlyで指定
    ->middleware(['auth', 'verified']) <- ログインしている場合のみアクセスする場合こちらに記載
    ->names([
        'index' => 'boards.index',
        'create' => 'boards.create',
        'store' => 'boards.store'
    ]);
```

- ユーザーと関連付けたとき
```
    public function store(Request $request) {
        $validated = $request->validate([
            'title' => 'required|max:255',
            'description' => 'required|max:65535'
        ]);
        $board = new Board();
        $board->user_id = $request->user()->id; <- user_idを追加
        $board->title = $request->title;
        $board->description = $request->description;
        $board->save();

        return redirect (route('boards.index', $board))->with('success', '掲示板を新規登録しました。');
```
- ボードにユーザーの名前を表示する時: `{{$board->user->name}}`


- フラッシュメッセージ
  - controllerにメッセージを記載
    withメソッド: セッションにデータを一時的に保存するための簡単な方法
    `->with('success', '掲示板を作成しました。');`
    第一引数にキー名`success`, 第二引数に保存する値`掲示板を作成しました。`
    
    ビューで保存したsessionからメッセージを呼び出す↓
    ```
    @if (session('success'))
    <div class="alert alert-success">
        {{ session('success') }}
    </div>
    @endif
    ```

- デフォルトエラーメッセージ: lang/{locale}/validation.php にメッセージが含まれている
