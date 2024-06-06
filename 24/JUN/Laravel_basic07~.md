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
