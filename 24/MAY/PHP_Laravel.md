### PHP Laravel

- Laravel Debugger: デバック作業を行うためのパッケージツール
- Tinker: Laravel標準の対話型シェル（`rails c` みたいな感じ？)
attributeとoriginで編集前の値も確認できた

- `print_r()` 配列を表示させる時
- `str_split()` 文字列を１文字ずつ分解して配列にする ($string, 3)のようにすると3つずつに分けて配列にする

- `クロージャー`: 無名関数。関数名のない関数。通常関数には名前をつけるが、つけるまでもないような場面で使われる。
一時的に使用される場合、コールバック関数として利用されることが一般的。
```
// コールバックに使われる場合
$mapped = array_map(function($value) {
            return $value * 2;
        }, [1, 2, 3, 4]);

print_r($mapped); // 出力: [2, 4, 6, 8]
```
-`bail`: 最初のバリデーションルールが失敗した時点で残りのバリデーションルールの検証を停止できる
  - どういう時？ リソースの節約、依存関係にあるバリデーション

- `oldメソッド`: 直前に一時保持保存した入力データをsessionから取り出す
- `Hash::make()`: パスワードなどハッシュ化して保存するとき 
```
use Illuminate\Support\Facades\Hash;

public function store(UserRequest $request)
{ 
    ...
    
    $user->password = Hash::make($request->password);
```

- `php artisan make:controller コントローラ名`: コントローラ作成コマンド
