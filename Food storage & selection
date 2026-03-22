<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>美食口袋名單</title>
    <style>
        /* 頁面美化 */
        body { font-family: -apple-system, "Microsoft JhengHei", sans-serif; background-color: #fff5e6; padding: 20px; color: #5d4037; }
        h2 { text-align: center; color: #d84315; }
        .card { background: white; padding: 15px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.1); margin-bottom: 20px; border: 1px solid #ffe0b2; }
        
        /* 輸入框與按鈕 */
        input, select, button { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ffccbc; border-radius: 10px; box-sizing: border-box; font-size: 16px; }
        button { background-color: #ff5722; color: white; border: none; font-weight: bold; cursor: pointer; transition: 0.3s; }
        button:active { background-color: #e64a19; transform: scale(0.98); }
        
        /* 推薦按鈕特別顏色 */
        .lucky-btn { background-color: #4caf50; margin-top: 10px; font-size: 1.1rem; }

        /* 清單樣式 */
        .food-item { border-bottom: 1px solid #fff3e0; padding: 15px 0; display: flex; justify-content: space-between; align-items: center; }
        .food-info { flex-grow: 1; }
        .rating { color: #fbc02d; font-weight: bold; }
        .map-link { color: #0288d1; text-decoration: none; font-size: 0.9rem; }
        .type-tag { font-size: 12px; background: #ffe0b2; padding: 2px 8px; border-radius: 20px; margin-left: 5px; }
    </style>
</head>
<body>

    <h2>🍊 美食口袋名單</h2>

    <div class="card" style="background-color: #e8f5e9; border: 1px solid #c8e6c9;">
        <p style="margin: 0; text-align: center; font-weight: bold;">不知道吃什麼嗎？</p>
        <button class="lucky-btn" onclick="pickRandomFood()">✨ 幫我選一間</button>
        <div id="randomResult" style="text-align: center; margin-top: 10px; font-weight: bold; color: #2e7d32;"></div>
    </div>

    <div class="card">
        <strong>✍️ 紀錄新發現</strong>
        <input type="text" id="storeName" placeholder="店名 (例如: 大安站拉麵)">
        <select id="storeType">
            <option value="未分類">選擇類型</option>
            <option value="早餐">早餐</option>
            <option value="午餐/晚餐">午餐/晚餐</option>
            <option value="甜點/咖啡">甜點/咖啡</option>
            <option value="宵夜">宵夜</option>
        </select>
        <select id="storeRating">
            <option value="5">⭐⭐⭐⭐⭐ (超推)</option>
            <option value="4">⭐⭐⭐⭐ (好吃)</option>
            <option value="3">⭐⭐⭐ (普通)</option>
        </select>
        <input type="text" id="mapUrl" placeholder="貼上 Google Map 分享網址">
        <button onclick="addFood()">存入我的名單</button>
    </div>

    <div id="listContainer"></div>

    <script>
        // --- 1. 初始化：從手機空間讀取資料 ---
        let foodList = JSON.parse(localStorage.getItem('my_food_list')) || [];
        renderList();

        // --- 2. 新增美食 ---
        function addFood() {
            const name = document.getElementById('storeName').value;
            const type = document.getElementById('storeType').value;
            const rating = document.getElementById('storeRating').value;
            const url = document.getElementById('mapUrl').value;

            if (!name) return alert("請輸入店名！");

            const newFood = {
                id: Date.now(),
                name: name,
                type: type,
                rating: rating,
                url: url
            };

            foodList.push(newFood);
            saveData();
            renderList();
            
            // 清空輸入框
            document.getElementById('storeName').value = '';
            document.getElementById('mapUrl').value = '';
        }

        // --- 3. 渲染清單 ---
        function renderList() {
            const container = document.getElementById('listContainer');
            container.innerHTML = '<h3>📜 收藏清單</h3>';

            foodList.forEach(item => {
                // 處理評分星星
                let stars = '⭐'.repeat(item.rating);
                
                const div = document.createElement('div');
                div.className = 'card food-item';
                div.innerHTML = `
                    <div class="food-info">
                        <strong>${item.name}</strong> <span class="type-tag">${item.type}</span><br>
                        <span class="rating">${stars}</span><br>
                        ${item.url ? `<a href="${item.url}" target="_blank" class="map-link">📍 在地圖中打開</a>` : '<span style="font-size:12px; color:#ccc;">無地圖資訊</span>'}
                    </div>
                    <button style="width:auto; background:none; color:#ff5252; border:none;" onclick="deleteFood(${item.id})">🗑️</button>
                `;
                container.appendChild(div);
            });
        }

        // --- 4. 隨機推薦邏輯 ---
        function pickRandomFood() {
            if (foodList.length === 0) {
                alert("名單空空的，快去紀錄第一間店吧！");
                return;
            }
            const resultDiv = document.getElementById('randomResult');
            resultDiv.innerText = "挑選中...";
            
            // 延遲一下模擬抽獎感
            setTimeout(() => {
                const randomIndex = Math.floor(Math.random() * foodList.length);
                const picked = foodList[randomIndex];
                resultDiv.innerHTML = `今天就吃：<br><span style="font-size:1.4rem;">🍴 ${picked.name}</span>`;
            }, 500);
        }

        // --- 5. 存檔與刪除 ---
        function saveData() {
            localStorage.setItem('my_food_list', JSON.stringify(foodList));
        }

        function deleteFood(id) {
            if (confirm("確定刪除這間店嗎？")) {
                foodList = foodList.filter(f => f.id !== id);
                saveData();
                renderList();
            }
        }
    </script>
</body>
</html>
