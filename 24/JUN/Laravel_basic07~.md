### 07_board_edit_delete

- 詳細画面および一覧画面に、自分が作成した掲示板に対してのみ編集リンクと削除リンクの表示
  - boards/show.blade.php index.blade.php
  - ルーティングの編集

- 編集リンクをクリックすると、編集画面に遷移し、既存のデータが表示
  - BoardsControllerに掲示板編集画面表示用のメソッドを追加
    edit($id)

- 編集画面で情報を変更し、更新ボタンを押すと、詳細画面に戻り変更内容が反映
  - BoardsControllerに掲示板更新用のメソッドが追加
    update($id)

- 削除リンクをクリックすると、確認ダイアログが表示されること
  - BoardsControllerに掲示板削除用のメソッドを追加
    destroy($id)

- 確認ダイアログで「OK」を選択すると、掲示板が削除され、一覧画面に戻ること


`@if (Auth::id() === $board->user_id)`
ログインしているユーザのみに表示させる

`$board = Board::findOrFail($id);`
idが一致するものを探しなければ404エラーを返す
このように記述することでIDが見つからなかった場合の処理を明示的に記述する必要がなくなる。コードがスッキリ。

request(): Laravelでは、request()ヘルパーを使用してリクエストデータを取得します。Railsのparamsハッシュに相当します。

```
$data = request()->all();
ビューから取得されたデータ例えば以下のように取得↓
$data = [
    'title' => '送信されたタイトル',
    'description' => '送信された説明',
    // 他のフィールドも含まれる場合があります
];
```

### 08_bookmark

- 掲示板詳細画面と一覧画面にブックマーク用のリンク追加
  - 配布ファイルを追加
  - `@include`: ビューのパーシャルは`@include`を使って参照
    パーシャル用ディレクトリを作成し、`'boards.partials.bookmark_button'`と記載すると参照できる

- bookmarksテーブルを追加するmigrationファイル作成
  - `php artisan make:model Bookmark --migration`
  - `$table->foreignId('user_id')->constrained()->onDelete('cascade');`
  - `$table->unique(['user_id', 'board_id'])` 組み合わせが一意であることを保証するための設定。
    同じ投稿に同じユーザーが複数回お気に入りに追加するのを防ぐ。データベースの整合性と一貫性が維持される。

- Bookmarkモデル作成
  - Boardモデル(1)とBookmark(多)モデルにアソシエーション
  - Userモデル(1)とBookmark(多)モデルにアソシエーション

- ルーティングの追加
  `Route::resource('boards.bookmarks', BookmarksController::class)->only(['store', 'destroy']);`

- ブックマークの作成・削除用にBookmarksController作成
  `php artisan make:controller Bookmark`
  - store
  - destroy
    return redirect()->back()->with('success', '掲示板をお気に入りに登録しました。');

- ブックマーク作成用のリンクをクリックすると、Bookmarkが作成され、リンクが削除用に変わること
- ブックマーク削除用のリンクをクリックすると、Bookmarkが削除され、リンクが作成用に変わること

#### Laravel N+1解消方法　Eagerロード
- withメソッド
  - withメソッドの引数には定義したリレーションのメソッド名を文字列で指定。
    withメソッドを使用する場合allメソッドが使用できないので、getメソッドでモデルインスタンスのコレクションを取得
```
<?php

namespace App\Http\Controllers;

use App\Models\User;

class UserController extends Controller
{
    public function index()
    {
        $users = User::with('posts')->get();

        foreach ($users as $user) {
            foreach ($user->posts as $post) {
                dump($post->title);
            }
        }
    }
}
```
  これでUsertテーブルのデータ取得とリレーション先のPostテーブル全データ取得の2回
  Eagerロードを行うと事前にリレーション先のデータを取得することでN＋1を解決している

- loadメソッド
  - loadメソッドの引数は定義したリレーションのメソッド名を文字列で指定するのはwithメソッドと同じ

  ```
  <?php

namespace App\Http\Controllers;

use App\Models\User;

class UserController extends Controller
{
    public function index()
    {
        $users = User::all()->load('posts');

        foreach ($users as $user) {
            foreach ($user->posts as $post) {
                dump($post->title);
            }
        }
    }
}
  ```

  - ふたつのメソッドの違いはEagerロードを行うタイミング
    with: モデルインスタンスのコレクションを取得する前に使用、明確に必要なリレーションシップが分かっているとき
    load: モデルインスタンスのコレクションを取得した後に使用、条件に応じてリレーションシップをロードするとき



### 09_bookmark_ajax

- axios導入
  `npm install axios`

- viewファイルの置き換えとjsファイル追加
  bookmark.jsを読み込ませるために、resources/js/app.jsに`import './bookmark';`を追加
  → ビルドされたファイルに含まれるようになる
  axiosを使ったjsファイルもthen(response =>...)(成功)、catch(error =>...)(失敗)で分岐 promiseベース

- controllerの編集
  - 参考: `return response()->json(['html' => ここにブックマークのパーシャルを入れる]);`

非同期通信の流れ
- ブックマークをクリック
- JSファイルでクリックイベントを受け取り、非同期リクエストを送信
- コントローラでリクエストを受け取り、ブックマークstoreもしくはdestoryしてJSONレスポンスを返す
- JSファイルでレスポンスを受け取り、ブックマークボタンビューへ

→ ブックマークを作成・保存後、ビューにどのボードのブックマークボタンを変更するのかわかるようにしてあげないといけない
→ 保存したブックマークの関連からボードを取得
→ どのボードの変更をすべきかビューに渡してあげる

dusk = Laravel Dusk ブラウザテスト用のツールで使用される属性

compact(): 複数の変数を一度に配列に変換する関数。変数をまとめて配列として扱いたいときに便利。


### 10_comment_ajax

- commentのパーシャル化
  - パーシャルをforeachさせるようにboard/showに記述変更
  - current userの場合は削除ボタンを用意する(borad一覧を参考に作る？)
  ```
<div class="flex text-2xl">
@if ($board->user == Auth::user())
  <a href="{{ route('boards.edit', $board->id)}}"><i class="ri-edit-box-line mr-4"></i></a>
  <form action="{{ route('boards.destroy', $board->id) }}" method="POST">
    @csrf
    @method('DELETE')
    <button type="submit" class="btn btn-danger">
      <i class="ri-delete-bin-line"></i>
    </button>
  </form>
@else
  @include('boards.partials.bookmark_button')
@endif
  ```
- commentのJSファイルを作る
  ブックマークを参考に作成
- controllerの編集
  - 参考: `return response()->json(['html' => ここにブックマークのパーシャルを入れる]);`
  - boardを取得するように記述する
  - DELETE          boards/{board}/comments/{comment} boardとcommentの両方のidが必要

ルート確認方法: `php artisan route:list`

- `document.querySelector`のセレクタ
  セレクタの取得
  `.`: クラスセレクタ(.className) クラスで要素を選択 複数要素に同じクラスを適用可能
  `#`: IDセレクタ(#idName) ID名で要素を選択 IDは一意ページ内に1つの要素にしか適用できない
