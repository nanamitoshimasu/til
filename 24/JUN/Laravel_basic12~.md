### 12_search_board

- クエリビルダーを利用して検索条件を組み立てる方法を調べてみましょう
  クエリビルダーは、PHPのコードを使ってSQLクエリを動的に構築できる強力なツール
  クエリビルダー取得方法
  テーブルから全行: `DB::table('users')->get();`
  テーブルから単一の行/カラム: `DB::table('users')->where('name', 'John')->first();`
  (データベースから単一の行だけを取得する場合はDBファザードのfirstメソッドを使用)
  行全体が必要ない場合valueメソッドを使ってレコードから単一の値を抽出できる
  `$email = DB::table('users')->where('name', 'John')->value('email');`
  id列の値で単一の行を取得するにはfindメソッドを使用
  `$user = DB::table('users')->find(3);`
  pluckメソッドを使えば特定のカラムだけを抽出することができる
  (例えば、掲示板のタイトルだけを取得できる)
  selectメソッドを使うとクエリにカスタムの"select"句を指定できる
  distinctメソッドを使えば重複内容に取得できる
  $query = Post::query(); postモデルに基づいた新しいクエリビルダーのインスタンスを作成できる
- ビューで検索フォームを作成する方法を調べてみましょう
  form class=
  input type="search"
- コントローラーで検索条件を受け取り、結果を取得する方法を調べてみましょう
  
- タイトルと本文でLIKE検索できるようにする
- BoardsControllerに検索用のメソッドを追加

#### 手順
1. indexファイルを置き換える
   ビューの内容確認
   -  route('boards.search')
   - name="search"
2. ルーティングを追加
   Route::post('/search', [BoardsController::class, 'search'])->name('boards.search');
3. コントローラーの追加
   public function search(Request $request)
   ...
   return view('boards.index', ['boards' => $boards, 'search' => $search]);
   (検索結果をindexのビューに返す)
   (ページネーションが出るようにする)
   → `appends()`メソッド
     {{$variables->appends(request()->query())->links()}} //$variablesはその時の値
     appends()メソッド: 配列を渡す (値を追加するメソッド) https://www.php.net/manual/ja/arrayobject.append.php
     request()メソッドで、ページで送信されるリクエストを取得
     request()メソッドの中で、さらにquery()メソッドを使うことでリクエストの中のクエリストリングのみを取得できる。
　　 ※クエリストリングとは、urlの中の?名前=○○のような?以降の値のこと
