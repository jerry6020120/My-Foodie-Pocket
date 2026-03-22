<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Foodie Pocket</title>
    <style>
        /* 這裡稍微優化了視覺，讓標題更乾淨 */
        body { font-family: -apple-system, sans-serif; background-color: #fff9f0; padding: 15px; margin: 0; }
        .header { background-color: white; padding: 20px 0; text-align: center; margin-bottom: 15px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .header h2 { margin: 0; color: #e67e22; font-size: 1.5rem; }
        .card { background: white; padding: 15px; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.08); margin-bottom: 20px; }
        label { display: block; margin-top: 10px; font-weight: bold; font-size: 14px; color: #5d4037; }
        input, select, button { width: 100%; padding: 12px; margin: 5px 0; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 16px; }
        button { background-color: #e67e22; color: white; border: none; font-weight: bold; margin-top: 10px; cursor: pointer; }
        .rating-group { display: flex; align-items: center; justify-content: space-between; background: #fdf2e9; padding: 8px 12px; border-radius: 8px; margin: 5px 0; }
        .rating-group span { font-size: 14px; width: 60px; }
        .rating-group input { width: 40%; text-align: center; }
        .food-item { border-bottom: 1px solid #eee; padding: 15px 0; }
        .food-pic { width: 100%; height: 200px; object-fit: cover; border-radius: 12px; margin-top: 10px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        .tag { background: #e67e22; color: white; padding: 3px 10px; border-radius: 20px; font-size: 12px; }
    </style>
</head>
<body>

    <div class="header">
        <h2>🍱 我的美食藏寶圖</h2>
    </div>

    <div class="card" style="background:#eafaf1; border: 1px solid #d4efdf;">
        <button style="background-color: #27ae60; margin: 0;" onclick="pickRandom()">✨ 抽籤：今天吃什麼？</button>
        <div id="randomDisplay" style="margin-top:12px; font-weight:bold; color: #1e8449; text-align: center;"></div>
    </div>

    <div class="card">
        <strong>✍️ 新增美食紀錄</strong>
        <input type="text" id="name" placeholder="店名 (例如：阿霞飯店)">
        <select id="type">
            <option value="早午餐">🍳 早午餐</option>
            <option value="午晚餐">🍱 午晚餐</option>
            <option value="甜點點心">🍰 甜點點心</option>
        </select>

        <label>細項評分 (0.5 - 5.0)：</label>
        <div class="rating-group"><span>總評分</span><input type="number" id="r_total" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>口味</span><input type="number" id="r_taste" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>服務</span><input type="number" id="r_service" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>價格</span><input type="number" id="r_price" step="0.5" min="0.5" max="5" value="5"></div>
        <div class="rating-group"><span>氛圍</span><input type="number" id="r_env" step="0.5" min="0.5" max="5" value="5"></div>

        <input type="text" id="url" placeholder="📍 貼上 Google Maps 連結">
        <label>📸 上傳美食照片：</label>
        <input type="file" id="photoInput" accept="image/*">
        
        <button onclick="saveFood()">儲存到口袋名單</button>
    </div>

    <div class="card" id="list">
        <h3 style="border-bottom: 2px solid #e67e22; padding-bottom: 10px;">📜 我的收藏清單</h3>
    </div>

    <script>
        let foods = JSON.parse(localStorage.getItem('food_v2')) || [];
        render();

        function saveFood() {
            const name = document.getElementById('name').value;
            const photoFile = document.getElementById('photoInput').files[0];
            if(!name) return alert("店名是必填的喔！");

            if (photoFile) {
                const reader = new FileReader();
                reader.onload = function(e) { executeSave(e.target.result); };
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
            document.getElementById('name').value = '';
            document.getElementById('photoInput').value = '';
            document.getElementById('url').value = '';
        }

        function render() {
            const container = document.getElementById('list');
            container.innerHTML = '<h3 style="border-bottom: 2px solid #e67e22; padding-bottom: 10px;">📜 我的收藏清單</h3>';
            if(foods.length === 0) container.innerHTML += '<p style="color:gray; text-align:center;">目前還沒有紀錄喔！</p>';
            
            foods.forEach(f => {
                const div = document.createElement('div');
                div.className = 'food-item';
                div.innerHTML = `
                    <div style="display:flex; justify-content:space-between; align-items:flex-start;">
                        <div>
                            <strong style="font-size:1.1rem;">${f.name}</strong> <span class="tag">${f.type}</span>
                        </div>
                        <button style="width:auto; background:none; color:#bbb; font-size:18px; border:none; margin:0;" onclick="remove(${f.id})">×</button>
                    </div>
                    <div style="font-size: 13px; color: #777; margin: 5px 0;">
                        ⭐ 總分: ${f.scores.total} | 味: ${f.scores.taste} | 服: ${f.scores.service} | 價: ${f.scores.price} | 氛: ${f.scores.env}
                    </div>
                    ${f.url ? `<a href="${f.url}" target="_blank" style="font-size:13px; color:#3498db; text-decoration:none;">📍 查看地圖</a>` : ''}
                    ${f.photo ? `<img src="${f.photo}" class="food-pic">` : ''}
                `;
                container.appendChild(div);
            });
        }

        function pickRandom() {
            if(foods.length === 0) return alert("清單是空的，快去吃好料來紀錄！");
            const picked = foods[Math.floor(Math.random() * foods.length)];
            document.getElementById('randomDisplay').innerHTML = `💡 建議吃：<span style="font-size:1.2rem; color:#e67e22;">${picked.name}</span><br>(評分：${picked.scores.total})`;
        }

        function remove(id) {
            if(confirm("確定要刪除這筆紀錄嗎？")) {
                foods = foods.filter(f => f.id !== id);
                localStorage.setItem('food_v2', JSON.stringify(foods));
                render();
            }
        }
    </script>
</body>
</html>
