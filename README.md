<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>美食進階口袋名單</title>
    <style>
        body { font-family: -apple-system, sans-serif; background-color: #fff9f0; padding: 15px; color: #444; }
        .card { background: white; padding: 15px; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.08); margin-bottom: 20px; }
        h2 { color: #e67e22; text-align: center; }
        label { display: block; margin-top: 10px; font-weight: bold; font-size: 14px; }
        input, select, button { width: 100%; padding: 10px; margin: 5px 0; border: 1px solid #ddd; border-radius: 8px; box-sizing: border-box; font-size: 16px; }
        button { background-color: #e67e22; color: white; border: none; font-weight: bold; margin-top: 15px; }
        
        /* 評分區塊樣式 */
        .rating-group { display: flex; align-items: center; justify-content: space-between; background: #fdf2e9; padding: 5px 10px; border-radius: 5px; margin: 5px 0; }
        .rating-group span { font-size: 14px; width: 60px; }
        .rating-group input { width: 60%; }

        /* 清單樣式 */
        .food-item { position: relative; padding-bottom: 10px; border-bottom: 1px solid #eee; margin-bottom: 15px; }
        .food-pic { width: 100%; height: 180px; object-fit: cover; border-radius: 10px; margin-top: 10px; }
        .tag { background: #e67e22; color: white; padding: 2px 8px; border-radius: 10px; font-size: 12px; }
        .score-box { font-size: 13px; color: #777; line-height: 1.6; }
        .lucky-btn { background-color: #27ae60; }
    </style>
</head>
<body>

    <h2>🍱 我的美食藏寶圖</h2>

    <div class="card" style="background:#eafaf1; text-align:center;">
        <button class="lucky-btn" onclick="pickRandom()">✨ 抽籤：今天吃什麼？</button>
        <div id="randomDisplay" style="margin-top:10px; font-weight:bold;"></div>
    </div>

    <div class="card">
        <strong>新增美食紀錄</strong>
        <input type="text" id="name" placeholder="店名">
        <select id="type">
            <option value="早午餐">早午餐</option>
            <option value="午晚餐">午晚餐</option>
            <option value="甜點點心">甜點點心</option>
        </select>

        <label>細項評分 (0.5 - 5.0)：</label>
        <div class="rating-group"><span>總評分</span><input type="number" id="r_total" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>口味</span><input type="number" id="r_taste" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>服務</span><input type="number" id="r_service" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>價格</span><input type="number" id="r_price" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>氛圍</span><input type="number" id="r_env" step="0.5" min="0.5" max="5" value="5"></div>

        <input type="text" id="url" placeholder="Google Maps 連結">
        
        <label>上傳照片：</label>
        <input type="file" id="photoInput" accept="image/*">
        
        <button onclick="saveFood()">儲存紀錄</button>
    </div>

    <div class="card" id="list">
        <h3>📜 收藏清單</h3>
    </div>

    <script>
        let foods = JSON.parse(localStorage.getItem('food_v2')) || [];
        render();

        // 處理照片轉換成文字儲存 (Base64)
        function saveFood() {
            const name = document.getElementById('name').value;
            const photoFile = document.getElementById('photoInput').files[0];
            
            if(!name) return alert("請輸入店名");

            if (photoFile) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    executeSave(e.target.result);
                };
                reader.readAsDataURL(photoFile);
            } else {
                executeSave(null);
            }
        }

        function executeSave(photoData) {
            const item = {
                id: Date.now(),
                name: document.getElementById('name').value,
                type: document.getElementById('type').value,
                url: document.getElementById('url').value,
                photo: photoData,
                scores: {
                    total: document.getElementById('r_total').value,
                    taste: document.getElementById('r_taste').value,
                    service: document.getElementById('r_service').value,
                    price: document.getElementById('r_price').value,
                    env: document.getElementById('r_env').value
                }
            };
            foods.push(item);
            localStorage.setItem('food_v2', JSON.stringify(foods));
            render();
            // 重設欄位
            document.getElementById('name').value = '';
            document.getElementById('photoInput').value = '';
        }

        function render() {
            const container = document.getElementById('list');
            container.innerHTML = '<h3>📜 收藏清單</h3>';
            foods.forEach(f => {
                const div = document.createElement('div');
                div.className = 'food-item';
                div.innerHTML = `
                    <strong>${f.name}</strong> <span class="tag">${f.type}</span>
                    <div class="score-box">
                        ⭐ 總分: ${f.scores.total} | 味: ${f.scores.taste} | 服: ${f.scores.service} | 價: ${f.scores.price} | 氛: ${f.scores.env}
                    </div>
                    ${f.url ? `<a href="${f.url}" target="_blank" style="font-size:12px; color:#3498db;">📍 地圖連結</a>` : ''}
                    ${f.photo ? `<img src="${f.photo}" class="food-pic">` : ''}
                    <button style="width:auto; background:none; color:red; font-size:12px; border:none;" onclick="remove(${f.id})">刪除</button>
                `;
                container.appendChild(div);
            });
        }

        function pickRandom() {
            if(foods.length === 0) return alert("清單是空的");
            const picked = foods[Math.floor(Math.random() * foods.length)];
            document.getElementById('randomDisplay').innerHTML = `推薦吃：${picked.name} (${picked.scores.total}分)`;
        }

        function remove(id) {
            if(confirm("確定刪除?")) {
                foods = foods.filter(f => f.id !== id);
                localStorage.setItem('food_v2', JSON.stringify(foods));
                render();
            }
        }
    </script>
</body>
</html>
