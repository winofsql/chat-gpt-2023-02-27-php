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
```
このプログラムでは、投稿内容を保存するためのテキストファイル名を指定し、ファイルが存在する場合は投稿一覧を表示するようになっています。投稿内容が送信された場合は、ファイルを開いて投稿内容を書き込みます。また、投稿内容を取得する場合は、ファイルを開いて1行ずつ読み込み、配列に格納しています。テキストファイルには、投稿者の名前と投稿内容をパイプ記号（|）で区切って保存しています。投稿内容の表示には、htmlspecialchars関数を使用してエスケープ処理を
```
