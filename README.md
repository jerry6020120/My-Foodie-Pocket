<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Foodie Pocket</title>
    <style>
        /* 基礎風格統一 */
        body { font-family: -apple-system, sans-serif; background-color: #fff9f0; padding: 15px; margin: 0; color: #444; }
        .header { background-color: white; padding: 25px 0; text-align: center; margin-bottom: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .header h2 { margin: 0; color: #e67e22; font-size: 1.6rem; letter-spacing: 1px; }
        
        .card { background: white; padding: 18px; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.08); margin-bottom: 20px; border: none; }
        
        label { display: block; margin-top: 15px; font-weight: bold; font-size: 14px; color: #5d4037; }
        input[type="text"], select { width: 100%; padding: 12px; margin: 8px 0; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 16px; background-color: #fafafa; }
        
        button { width: 100%; padding: 14px; background-color: #e67e22; color: white; border: none; border-radius: 12px; font-weight: bold; font-size: 16px; cursor: pointer; transition: 0.2s; }
        button:active { transform: scale(0.98); opacity: 0.9; }

        /* 七顆星評分樣式 (點選變色) */
        .star-rating { display: flex; flex-direction: row-reverse; justify-content: flex-end; gap: 3px; margin: 5px 0; }
        .star-rating input { display: none; }
        .star-rating label { color: #ddd; font-size: 26px; cursor: pointer; transition: 0.2s; }
        
        /* 點選後的顏色連動 */
        .star-rating input:checked ~ label, 
        .star-rating label:hover, 
        .star-rating label:hover ~ label { color: #f1c40f; }

        .rating-box { background: #fdf2e9; padding: 12px; border-radius: 12px; margin-bottom: 12px; border: 1px solid #f9e4d4; }
        .rating-title { font-size: 13px; color: #8d6e63; font-weight: bold; margin-bottom: 5px; display: block; }

        /* 收藏清單 */
        .food-item { border-bottom: 1px solid #eee; padding: 20px 0; position: relative; }
        .food-item:last-child { border-bottom: none; }
        .tag { background: #e67e22; color: white; padding: 3px 12px; border-radius: 20px; font-size: 11px; margin-left: 5px; vertical-align: middle; }
        .star-display { color: #f1c40f; font-size: 20px; margin: 8px 0; letter-spacing: 2px; }
        .food-pic { width: 100%; height: 220px; object-fit: cover; border-radius: 15px; margin-top: 12px; box-shadow: 0 3px 8px rgba(0,0,0,0.1); }
        .delete-x { position: absolute; right: 0; top: 20px; color: #ccc; font-size: 24px; border: none; background: none; width: auto; cursor: pointer; }
    </style>
</head>
<body>

    <div class="header">
        <h2>🍱 我的美食藏寶圖</h2>
    </div>

    <div class="card" style="background-color: #eafaf1; border: 1px solid #d4efdf;">
        <button style="background-color: #27ae60; margin: 0;" onclick="pickRandom()">✨ 今天吃什麼？ (隨機抽籤)</button>
        <div id="randomDisplay" style="margin-top:12px; font-weight:bold; color: #1e8449; text-align: center; font-size: 1.1rem;"></div>
    </div>

    <div class="card">
        <strong style="font-size: 1.1rem; color: #d84315;">✍️ 紀錄新餐廳</strong>
        <input type="text" id="name" placeholder="店名">
        <select id="type">
            <option value="早午餐">🍳 早午餐</option>
            <option value="午晚餐">🍱 午晚餐</option>
            <option value="甜點點心">🍰 甜點點心</option>
        </select>

        <label>評分 (1-7 顆星)：</label>
        
        <div class="rating-box">
            <span class="rating-title">總評分</span>
            <div class="star-rating" id="rate_total">
                <input type="radio" name="rt" id="rt7" value="7"><label for="rt7">★</label>
                <input type="radio" name="rt" id="rt6" value="6"><label for="rt6">★</label>
                <input type="radio" name="rt" id="rt5" value="5"><label for="rt5">★</label>
                <input type="radio" name="rt" id="rt4" value="4" checked><label for="rt4">★</label>
                <input type="radio" name="rt" id="rt3" value="3"><label for="rt3">★</label>
                <input type="radio" name="rt" id="rt2" value="2"><label for="rt2">★</label>
                <input type="radio" name="rt" id="rt1" value="1"><label for="rt1">★</label>
            </div>
        </div>

        <div class="rating-box">
            <span class="rating-title">口味</span>
            <div class="star-rating" id="rate_taste">
                <input type="radio" name="rk" id="rk7" value="7"><label for="rk7">★</label>
                <input type="radio" name="rk" id="rk6" value="6"><label for="rk6">★</label>
                <input type="radio" name="rk" id="rk5" value="5"><label for="rk5">★</label>
                <input type="radio" name="rk" id="rk4" value="4" checked><label for="rk4">★</label>
                <input type="radio" name="rk" id="rk3" value="3"><label for="rk3">★</label>
                <input type="radio" name="rk" id="rk2" value="2"><label for="rk2">★</label>
                <input type="radio" name="rk" id="rk1" value="1"><label for="rk1">★</label>
            </div>
        </div>

        <div class="rating-box">
            <span class="rating-title">服務</span>
            <div class="star-rating" id="rate_service">
                <input type="radio" name="rs" id="rs7" value="7"><label for="rs7">★</label>
                <input type="radio" name="rs" id="rs6" value="6"><label for="rs6">★</label>
                <input type="radio" name="rs" id="rs5" value="5"><label for="rs5">★</label>
                <input type="radio" name="rs" id="rs4" value="4" checked><label for="rs4">★</label>
                <input type="radio" name="rs" id="rs3" value="3"><label for="rs3">★</label>
                <input type="radio" name="rs" id="rs2" value="2"><label for="rs2">★</label>
                <input type="radio" name="rs" id="rs1" value="1"><label for="rs1">★</label>
            </div>
        </div>

        <div class="rating-box">
            <span class="rating-title">價格</span>
            <div class="star-rating" id="rate_price">
                <input type="radio" name="rp" id="rp7" value="7"><label for="rp7">★</label>
                <input type="radio" name="rp" id="rp6" value="6"><label for="rp6">★</label>
                <input type="radio" name="rp" id="rp5" value="5"><label for="rp5">★</label>
                <input type="radio" name="rp" id="rp4" value="4" checked><label for="rp4">★</label>
                <input type="radio" name="rp" id="rp3" value="3"><label for="rp3">★</label>
                <input type="radio" name="rp" id="rp2" value="2"><label for="rp2">★</label>
                <input type="radio" name="rp" id="rp1" value="1"><label for="rp1">★</label>
            </div>
        </div>

        <input type="text" id="url" placeholder="📍 貼上 Google Maps 連結">
        <label>📸 上傳美食照片：</label>
        <input type="file" id="photoInput" accept="image/*" style="font-size: 14px; margin-top: 5px;">
        
        <button onclick="saveFood()">儲存到口袋名單</button>
    </div>

    <div class="card" id="list">
        <h3 style="border-bottom: 2px solid #e67e22; padding-bottom: 10px; margin-top: 0;">📜 我的收藏清單</h3>
    </div>

    <script>
        // 使用新的資料 Key，避免與舊版衝突
        let foods = JSON.parse(localStorage.getItem('food_v7_stars')) || [];
        render();

        function getStarVal(groupName) {
            const el = document.getElementsByName(groupName);
            for (let i of el) { if (i.checked) return parseInt(i.value); }
            return 4;
        }

        function saveFood() {
            const name = document.getElementById('name').value;
            const file = document.getElementById('photoInput').files[0];
            if(!name) return alert("店名是必填的喔！");

            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) { doSave(e.target.result); };
                reader.readAsDataURL(file);
            } else {
                doSave(null);
            }
        }

        function doSave(imgData) {
            const newItem = {
                id: Date.now(),
                name: document.getElementById('name').value,
                type: document.getElementById('type').value,
                url: document.getElementById('url').value,
                photo: imgData,
                total: getStarVal('rt'),
                taste: getStarVal('rk'),
                service: getStarVal('rs'),
                price: getStarVal('rp')
            };
            foods.unshift(newItem); // 新的排在前面
            localStorage.setItem('food_v7_stars', JSON.stringify(foods));
            render();
            // 重設
            document.getElementById('name').value = '';
            document.getElementById('url').value = '';
            document.getElementById('photoInput').value = '';
        }

        function render() {
            const container = document.getElementById('list');
            container.innerHTML = '<h3 style="border-bottom: 2px solid #e67e22; padding-bottom: 10px; margin-top: 0;">📜 我的收藏清單</h3>';
            
            foods.forEach(f => {
                const div = document.createElement('div');
                div.className = 'food-item';
                // 畫出 7 顆星
                const stars = '★'.repeat(f.total) + '☆'.repeat(7 - f.total);
                
                div.innerHTML = `
                    <button class="delete-x" onclick="remove(${f.id})">×</button>
                    <div>
                        <strong style="font-size:1.15rem;">${f.name}</strong>
                        <span class="tag">${f.type}</span>
                    </div>
                    <div class="star-display">${stars}</div>
                    ${f.url ? `<a href="${f.url}" target="_blank" style="font-size:13px; color:#3498db; text-decoration:none;">📍 在地圖中查看</a>` : ''}
                    ${f.photo ? `<img src="${f.photo}" class="food-pic">` : ''}
                `;
                container.appendChild(div);
            });
        }

        function pickRandom() {
            if(foods.length === 0) return alert("清單目前是空的喔！");
            const picked = foods[Math.floor(Math.random() * foods.length)];
            document.getElementById('randomDisplay').innerHTML = `💡 建議吃：<span style="color:#e67e22;">${picked.name}</span>`;
        }

        function remove(id) {
            if(confirm("確定要刪除這筆美食紀錄嗎？")) {
                foods = foods.filter(f => f.id !== id);
                localStorage.setItem('food_v7_stars', JSON.stringify(foods));
                render();
            }
        }
    </script>
</body>
</html>
