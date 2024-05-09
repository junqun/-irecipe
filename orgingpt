<?php
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "food";

// 創建連接
$conn = new mysqli($servername, $username, $password, $dbname);

// 檢查連接是否成功
if ($conn->connect_error) {
    die("連接失敗: " . $conn->connect_error);
}

// 執行SQL查詢
$sql = "SELECT ingredient FROM food2";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    $ingredientString = "";

    // 將"ingredient"值合併到字串中
    while ($row = $result->fetch_assoc()) {
        $ingredientString .= $row["ingredient"] . ", "; 
    }
    // 移除最後的逗號和空格
    $ingredientString = rtrim($ingredientString, ", ");
        echo '<span style="font-size: 40px ; font-family: Arial ; ">' . "所有材料: " . $ingredientString . "<br>" . '</span>';
    } else {
        echo "0 個結果";
    }

$conn->close();





$apiKey="";
$url = 'https://api.openai.com/v1/chat/completions';

$headers = array(
    "Authorization: Bearer {$apiKey}",
    "OpenAI-Organization: org-EUheo1vpvr6FXRjDm6Qv0C2T",
    "Content-Type: application/json"
);

// Define messages
$messages = array();
$messages[] = array("role" => "user", "content" => $ingredientString . "，假設冰箱裡只有這些材料，身為一位家庭主婦，這些材料只能做出甚麼料理?
請回答3道料理，請回覆料理名稱以及簡易的料理步驟，
回覆格式為料理：材料：作法：，
材料請用頓號隔開，請幫我標示好料理名稱：，
請幫我在每道料理後面加上<br>，
請輪流使用每一樣材料生成料理以提升生成料理的多樣性");


// Define data
$data = array();
$data["model"] = "gpt-3.5-turbo";
$data["messages"] = $messages;
$data["temperature"] = 0.7;
$data["max_tokens"] = 1200;

// init curl
$curl = curl_init($url);
curl_setopt($curl, CURLOPT_POST, 1);
curl_setopt($curl, CURLOPT_POSTFIELDS, json_encode($data));
curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);
curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);

$result = curl_exec($curl);
if (curl_errno($curl)) {
    echo 'Error:' . curl_error($curl);
} else {
    echo $result;

    $response = json_decode($result, true);
    if (isset($response["choices"][0]["message"]["content"])) {
        $generatedText = $response["choices"][0]["message"]["content"];
        //echo "<br>生成的文本： " . $generatedText . "<br>";
        echo '<span style="font-size: 40px ; font-family: Arial ; ">' . "<br>生成的食譜：<br> " . nl2br($generatedText) . '</span>';
        //nl2br 用於分行
    } else {
        echo "無法提取生成的文本。";
    }
}

curl_close($curl);


?>
