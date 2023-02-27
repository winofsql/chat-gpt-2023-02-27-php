# chat-gpt-2023-02-27-php

### PHP で、テキストファイルにデータを保存する掲示板プログラムを作成してください
```php
<?php
// 投稿内容を保存するファイル名を指定
$filename = "board.txt";

// 投稿内容が送信された場合の処理
if ($_SERVER["REQUEST_METHOD"] == "POST" && isset($_POST["name"]) && isset($_POST["message"])) {
    // 投稿内容を変数に格納
    $name = $_POST["name"];
    $message = $_POST["message"];

    // 投稿内容を書き込むテキストファイルを開く
    $fp = fopen($filename, "a");

    // 投稿内容を書き込む
    fwrite($fp, $name . "|" . $message . "\n");

    // ファイルを閉じる
    fclose($fp);
}

// テキストファイルから投稿内容を取得する
$posts = [];
if (file_exists($filename)) {
    // テキストファイルを開く
    $fp = fopen($filename, "r");

    // ファイルが存在する場合、1行ずつ読み込み、配列に格納する
    while (!feof($fp)) {
        $line = fgets($fp);
        $post = explode("|", $line);
        if (count($post) === 2) {
            $posts[] = $post;
        }
    }

    // ファイルを閉じる
    fclose($fp);
}

?>

<!-- 投稿フォーム -->
<form method="POST">
    <div>
        <label for="name">名前：</label>
        <input type="text" name="name" id="name">
    </div>
    <div>
        <label for="message">投稿内容：</label>
        <textarea name="message" id="message"></textarea>
    </div>
    <button type="submit">投稿する</button>
</form>

<!-- 投稿一覧 -->
<?php if (!empty($posts)): ?>
    <ul>
    <?php foreach ($posts as $post): ?>
        <li>
            <strong><?php echo htmlspecialchars($post[0], ENT_QUOTES); ?></strong>
            <?php echo htmlspecialchars($post[1], ENT_QUOTES); ?>
        </li>
    <?php endforeach; ?>
    </ul>
<?php endif; ?>
```
<div>
このプログラムでは、投稿内容を保存するためのテキストファイル名を指定し、ファイルが存在する場合は投稿一覧を表示するようになっています。投稿内容が送信された場合は、ファイルを開いて投稿内容を書き込みます。また、投稿内容を取得する場合は、ファイルを開いて1行ずつ読み込み、配列に格納しています。テキストファイルには、投稿者の名前と投稿内容をパイプ記号（|）で区切って保存しています。投稿内容の表示には、htmlspecialchars関数を使用してエスケープ処理を
</div>

# ▲ 途中で説明が切れる

### PHP で、テキストファイルにデータを保存する掲示板プログラムを MVC にのっとり、3つのファイルで作成してください
<div>
以下は、PHPを用いてMVCパターンに沿った掲示板プログラムを3つのファイルで作成する例です。

まず、モデルファイル board_model.php を作成します。このファイルでは、テキストファイルに投稿内容を書き込む write_post メソッドと、テキストファイルから投稿内容を取得する read_posts メソッドを定義します。
</div>
```php
<?php
class BoardModel {
    private $filename = "board.txt";

    public function write_post($name, $message) {
        $fp = fopen($this->filename, "a");
        fwrite($fp, $name . "|" . $message . "\n");
        fclose($fp);
    }

    public function read_posts() {
        $posts = [];
        if (file_exists($this->filename)) {
            $fp = fopen($this->filename, "r");
            while (!feof($fp)) {
                $line = fgets($fp);
                $post = explode("|", $line);
                if (count($post) === 2) {
                    $posts[] = $post;
                }
            }
            fclose($fp);
        }
        return $posts;
    }
}
?>
```
<div>
次に、ビューファイル board_view.php を作成します。このファイルでは、投稿フォームと投稿一覧を表示します。
</div>
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>掲示板</title>
</head>
<body>
    <h1>掲示板</h1>

    <form method="POST" action="board.php">
        <div>
            <label for="name">名前：</label>
            <input type="text" name="name" id="name">
        </div>
        <div>
            <label for="message">投稿内容：</label>
            <textarea name="message" id="message"></textarea>
        </div>
        <button type="submit">投稿する</button>
    </form>

    <?php if (!empty($posts)): ?>
        <ul>
        <?php foreach ($posts as $post): ?>
            <li>
                <strong><?php echo htmlspecialchars($post[0], ENT_QUOTES); ?></strong>
                <?php echo htmlspecialchars($post[1], ENT_QUOTES); ?>
            </li>
        <?php endforeach; ?>
        </ul>
    <?php endif; ?>
</body>
</html>
```
<div>
最後に、コントローラファイル board.php を作成します。このファイルでは、モデルとビューを読み込んで、投稿フォームが送信された場合はモデルの write_post メソッドを呼び出して投稿内容をテキストファイルに書き込み、投稿一覧を取得してビューに渡します。
</div>
```php
<?php
require_once "board_model.php";

$model = new BoardModel();
$view = "board_view.php";

if ($_SERVER["REQUEST_METHOD"] == "POST" && isset($_POST["name"]) && isset($_POST["message"])) {
    $model->write_post($_POST["name"], $_POST["
```

# ▲ 途中でコードが切れる
