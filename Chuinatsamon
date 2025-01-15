<?php
session_start();


date_default_timezone_set('Asia/Bangkok');

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $threadTitle = isset($_POST['title']) ? trim($_POST['title']) : '';
    $threadContent = isset($_POST['content']) ? trim($_POST['content']) : '';
    $threadAuthor = isset($_POST['author']) ? trim($_POST['author']) : '';
    $errors = [];

    if (strlen($threadTitle) < 4 || strlen($threadTitle) > 140) {
        $errors['title'] = "ชื่อกระทู้ต้องยาว 4–140 ตัวอักษร";
    }

    if ($threadTitle !== strip_tags($threadTitle)) {
        $errors['title'] = "ชื่อกระทู้จะไม่อนุญาตให้ใส่ HTML";
    }

    if (strlen($threadContent) < 6 || strlen($threadContent) > 2000) {
        $errors['content'] = "เนื้อหากระทู้ต้องยาว 6–2000 ตัวอักษร";
    }

    if (empty($threadAuthor)) {
        $errors['author'] = "กรุณาใส่ชื่อผู้เขียน";
    }

    if (empty($errors)) {
        
        if (!isset($_SESSION['threads'])) {
            $_SESSION['threads'] = [];
        }
        
        $newThread = [
            'title' => htmlspecialchars($threadTitle),
            'content' => htmlspecialchars($threadContent),
            'author' => htmlspecialchars($threadAuthor),
            'date' => date("Y-m-d H:i:s")  
        ];

        $_SESSION['threads'][] = $newThread; 

        header("Location: " . $_SERVER['PHP_SELF']);
        exit;
    }
}

$threads = isset($_SESSION['threads']) ? $_SESSION['threads'] : [];
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Back-endDeveloperIntern:Chutinatsamon</title>
    <style>
        body {
            font-family: 'Helvetica Neue', sans-serif;
            line-height: 1.6;
            background-color: #f5f5f5;
            margin: 0;
            padding: 0;
        }

        .container {
            max-width: 600px;
            margin: 50px auto;
            background: #ffffff;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }

        h1 {
            color: #333;
            text-align: center;
            font-size: 28px;
            margin-bottom: 30px;
        }

        form {
            display: grid;
            gap: 20px;
        }

        label {
            font-weight: bold;
            color: #444;
        }

        input[type="text"], textarea {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input[type="text"]:focus, textarea:focus {
            border-color: #ff9800;
            outline: none;
        }

        button {
            background: #ff9800;
            color: white;
            padding: 12px 18px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s;
        }

        button:hover {
            background-color: #e68900;
        }

        .error {
            color: red;
            font-size: 0.9em;
            margin-top: -5px;
        }

        .thread-list {
            margin-top: 40px;
        }

        .thread-item {
            margin-bottom: 20px;
            padding: 15px;
            background: #fafafa;
            border-left: 5px solid #ff9800;
        }

        .thread-item h3 {
            font-size: 20px;
            margin: 0;
            color: #444;
        }

        .thread-item p {
            font-size: 16px;
            color: #666;
        }

        .thread-item .content {
            margin-top: 10px;
            font-size: 14px;
            color: #444;
            line-height: 1.5;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>ตั้งกระทู้ใหม่</h1>

        <form method="post" action="">
            <div>
                <label for="title">ชื่อกระทู้:</label>
                <input type="text" id="title" name="title" value="<?php echo isset($threadTitle) ? htmlspecialchars($threadTitle) : ''; ?>" required>
                <?php if (!empty($errors['title'])): ?>
                    <div class="error"><?php echo $errors['title']; ?></div>
                <?php endif; ?>
            </div>
            <div>
                <label for="author">ชื่อผู้เขียน:</label>
                <input type="text" id="author" name="author" value="<?php echo isset($threadAuthor) ? htmlspecialchars($threadAuthor) : ''; ?>" required>
                <?php if (!empty($errors['author'])): ?>
                    <div class="error"><?php echo $errors['author']; ?></div>
                <?php endif; ?>
            </div>
            <div>
                <label for="content">เนื้อหากระทู้:</label>
                <textarea id="content" name="content" rows="6" required><?php echo isset($threadContent) ? htmlspecialchars($threadContent) : ''; ?></textarea>
                <?php if (!empty($errors['content'])): ?>
                    <div class="error"><?php echo $errors['content']; ?></div>
                <?php endif; ?>
            </div>
            <button type="submit">ตั้งกระทู้</button>
        </form>

        <div class="thread-list">
            <h2>กระทู้ทั้งหมด</h2>
            <?php if (!empty($threads)): ?>
                <?php foreach ($threads as $thread): ?>
                    <div class="thread-item">
                        <h3><?php echo htmlspecialchars($thread['title']); ?></h3>
                        <p><strong>ผู้เขียน:</strong> <?php echo htmlspecialchars($thread['author']); ?></p>
                        <p><strong>วันที่:</strong> <?php echo date("d-m-Y H:i:s", strtotime($thread['date'])); ?></p>
                        <div class="content"><?php echo nl2br(htmlspecialchars($thread['content'])); ?></div>
                    </div>
                <?php endforeach; ?>
            <?php else: ?>
                <p>ยังไม่มีการตั้งกระทู้</p>
            <?php endif; ?>
        </div>
    </div>
</body>
</html>
