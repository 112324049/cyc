<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>我的隨身筆記</title>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
            background-color: #f2f2f7;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        h2 { margin-bottom: 20px; color: #333; }
        .container {
            width: 100%;
            max-width: 500px;
            background: white;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.1);
        }
        .input-group { display: flex; gap: 10px; margin-bottom: 20px; }
        input {
            flex: 1;
            padding: 12px;
            font-size: 16px;
            border: 1px solid #ddd;
            border-radius: 8px;
            outline: none;
        }
        button#addBtn {
            padding: 12px 20px;
            background-color: #007aff;
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
        }
        button#addBtn:active { background-color: #005bb5; }
        ul { list-style: none; padding: 0; margin: 0; }
        li {
            background: #fff;
            border-bottom: 1px solid #eee;
            padding: 15px 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-size: 18px;
        }
        li:last-child { border-bottom: none; }
        .delete-btn {
            background-color: #ff3b30;
            color: white;
            border: none;
            padding: 8px 12px;
            border-radius: 6px;
            font-size: 14px;
            margin-left: 10px;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <h2>📝 記帳與行程</h2>

    <div class="container">
        <div class="input-group">
            <input type="text" id="inputBox" placeholder="輸入事項 (例如: 午餐 100元)">
            <button id="addBtn" onclick="addItem()">新增</button>
        </div>

        <ul id="list">
            </ul>
    </div>

    <script>
        // 1. 初始化：打開網頁時，從手機記憶體讀取資料
        let myData = JSON.parse(localStorage.getItem('myAppData')) || [];
        renderList();

        // 2. 新增功能的程式
        function addItem() {
            let input = document.getElementById('inputBox');
            let text = input.value.trim();
            
            if (text === "") {
                alert("請輸入內容喔！");
                return;
            }

            myData.push(text); // 把資料加入清單
            saveData();        // 存入手機記憶體
            renderList();      // 更新畫面
            input.value = "";  // 清空輸入框
        }

        // 3. 刪除功能的程式
        function deleteItem(index) {
            if (confirm("確定要刪除這筆資料嗎？")) {
                myData.splice(index, 1); // 刪除指定的那一筆
                saveData();
                renderList();
            }
        }

        // 4. 儲存到手機 (LocalStorage)
        function saveData() {
            localStorage.setItem('myAppData', JSON.stringify(myData));
        }

        // 5. 顯示畫面
        function renderList() {
            let listElement = document.getElementById('list');
            listElement.innerHTML = ""; // 先清空舊畫面

            myData.forEach((item, index) => {
                let li = document.createElement('li');
                li.innerHTML = `
                    <span>${item}</span>
                    <button class="delete-btn" onclick="deleteItem(${index})">刪除</button>
                `;
                listElement.appendChild(li);
            });
        }
    </script>

</body>
</html>
