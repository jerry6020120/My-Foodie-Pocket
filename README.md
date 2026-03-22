<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Foodie Pocket</title>
    <style>
        body { font-family: -apple-system, sans-serif; background-color: #fff9f0; padding: 15px; margin: 0; }
        .header { background-color: white; padding: 20px 0; text-align: center; margin-bottom: 15px; box-shadow: 0 2px 5px rgba(0,0,0,0.05); }
        .header h2 { margin: 0; color: #e67e22; font-size: 1.6rem; }
        .card { background: white; padding: 15px; border-radius: 15px; box-shadow: 0 4px 10px rgba(0,0,0,0.08); margin-bottom: 20px; }
        label { display: block; margin-top: 15px; font-weight: bold; font-size: 14px; color: #5d4037; }
        input[type="text"], select { width: 100%; padding: 12px; margin: 5px 0; border: 1px solid #ddd; border-radius: 10px; box-sizing: border-box; font-size: 16px; }
        button { width: 100%; padding: 12px; background-color: #e67e22; color: white; border: none; border-radius: 10px; font-weight: bold; margin-top: 15px; cursor: pointer; }
        
        /* 星星評分樣式 */
        .star-rating { display: flex; flex-direction: row-reverse; justify-content: flex-end; gap: 5px; margin: 5px 0; }
        .star-rating input { display: none; }
        .star-rating label { color: #ccc; font-size: 24px; padding: 0; cursor: pointer; transition: 0.2s; margin: 0; }
        .star-rating input:checked ~ label, .star-rating label:hover, .star-rating label:hover ~ label { color: #f1c40f; }
        .rating-container { background: #fdf2e9; padding: 10px; border-radius: 10px; margin-bottom: 10px; }
        .rating-label { font-size: 13px; color: #a67c52; margin-bottom: 2px; display: block; }

        /* 清單 */
        .food-item { border-bottom: 1px solid #eee; padding: 15px 0; position: relative; }
        .food-pic { width: 100%; height: 200px; object-fit: cover; border-radius: 12px; margin-top: 10px; }
        .tag { background: #e67e22; color: white; padding: 3px 10px; border-radius: 20px; font-size: 12px; }
        .star-display { color: #f1c40f; font-size: 18px; margin: 5px 0; }
        .delete-btn { position: absolute; right: 0; top: 15px; color: #bbb; background: none; width: auto; padding: 0; margin: 0; font-size: 20px; }
    </style>
</head>
<body>

    <div class="header">
        <h2>🍱 我的美食藏寶圖</h2>
    </div>

    <div class="card" style="background:#eafaf1;">
        <button style="background-color: #27ae60; margin: 0;" onclick="pickRandom()">✨ 今天吃什麼？ (隨機)</button>
        <div id="randomDisplay" style="margin-top:12px; font-weight:bold; color: #1e8449; text-align: center;"></div>
    </div>

    <div class="card">
        <strong>✍️ 紀錄新發現</strong>
        <input type="text" id="name" placeholder="店名">
        <select id="type">
            <option value="早午餐">🍳 早午餐</option>
            <option value="午晚餐">🍱 午晚餐</option>
            <option value="甜點點心">🍰 甜點點心</option>
        </select>

        <label>細項評分：</label>
        
        <div class="rating-container">
            <span class="rating-label">總評分</span>
            <div class="star-rating" id="rating_total">
                <input type="radio" name="total" id="total-5" value="5"><label for="total-5">★</label>
                <input type="radio" name="total" id="total-4" value="4"><label for="total-4">★</label>
                <input type="radio" name="total" id="total-3" value="3" checked><label for="total-3">★</label>
                <input type="radio" name="total" id="total-2" value="2"><label for="total-2">★</label>
                <input type="radio" name="total" id="total-1" value="1"><label for="total-1">★</label>
            </div>
        </div>

        <div class="rating-container">
            <span class="rating-label">口味</span>
            <div class="star-rating" id="rating_taste">
                <input type="radio" name="taste" id="taste-5" value="5"><label for="taste-5">★</label>
                <input type="radio" name="taste" id="taste-4" value="4"><label for="taste-4">★</label>
                <input type="radio" name="taste" id="taste-3" value="3" checked><label for="taste-3">★</label>
                <input type="radio" name="taste" id="taste-2" value="2"><label for="taste-2">★</label>
                <input type="radio" name="taste" id="taste-1" value="1"><label for="taste-1">★</label>
            </div>
        </div>

        <div class="rating-container">
            <span class="rating-label">服務</span>
            <div class="star-rating" id="rating_service">
                <input type="radio" name="service" id="service-5" value="5"><label for="service-5">★</label>
                <input type="radio" name="service" id="service-4" value="4"><label for="service-4">★</label>
                <input type="radio" name="service" id="service-3" value="3" checked><label for="service-3">★</label>
                <input type="radio" name="service" id="service-2" value="2"><label for="service-2">★</label>
                <input type="radio" name="service" id="service-1" value="1"><label for="service-1">★</label>
            </div>
        </div>

        <div class="rating-container">
            <span class="rating-label">價格</span>
            <div class="star-rating" id="rating_price">
                <input type="radio" name="price" id="price-5" value="5"><label for="price-5">★</label>
                <input type="radio" name="price" id="price-4" value="4"><label for="price-4">★</label>
                <input type="radio" name="price" id="price-3" value="3" checked><label for="price-3">★</label>
                <input type="radio" name="price" id="price-2" value="2"><label for="price-2">★</label>
                <input type="radio" name="price" id="price-1" value="1"><label for="price-1">★</label>
            </div>
        </div>

        <input type="text" id="url" placeholder="📍 Google Maps 連結">
        <label>📸 照片：</label>
        <input type="file" id="photoInput" accept="image/*">
        
        <button onclick="saveFood()">儲存紀錄</button>
    </div>

    <div class="card" id="list">
        <h3 style="border-bottom: 2px solid #e67e22; padding-bottom: 10px;">📜 我的收藏清單</h3>
    </div>

    <script>
        let foods = JSON.parse(localStorage.getItem('food_v3')) || [];
        render();

        function getStarValue(name) {
            const radios = document.getElementsByName(name);
            for (let r of radios) { if (r.checked) return r.value; }
            return 3;
        }

        function saveFood() {
            const name = document.getElementById('name').value;
            const photoFile = document.getElementById('photoInput').files[0];
            if(!name) return alert("請輸入店名");

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
                totalScore: getStarValue('total'),
                details: {
                    taste: getStarValue('taste'),
                    service: getStarValue('service'),
                    price: getStarValue('price')
                }
            };
            foods.push(item);
            localStorage.setItem('food_v3', JSON.stringify(foods));
            render();
            // 重設介面
            document.getElementById('name').value = '';
            document.getElementById('url').value = '';
            document.getElementById('photoInput').value = '';
        }

        function render() {
            const container = document.getElementById('list');
            container.innerHTML = '<h3 style="border-bottom: 2px solid #e67e22; padding-bottom: 10px;">📜 我的收藏清單</h3>';
            
            foods.forEach(f => {
                const div = document.createElement('div');
                div.className = 'food-item';
                div.innerHTML = `
                    <button class="delete-btn" onclick="remove(${f.id})">×</button>
                    <strong>${f.name}</strong> <span class="tag">${f.type}</span>
                    <div class="star-display">${'★'.repeat(f.totalScore)}${'☆'.repeat(5-f.totalScore)}</div>
                    ${f.url ? `<a href="${f.url}" target="_blank" style="font-size:13px; color:#3498db; text-decoration:none;">📍 查看地圖</a>` : ''}
                    ${f.photo ? `<img src="${f.photo}" class="food-pic">` : ''}
                `;
                container.appendChild(div);
            });
        }

        function pickRandom() {
            if(foods.length === 0) return alert("清單是空的");
            const picked = foods[Math.floor(Math.random() * foods.length)];
            document.getElementById('randomDisplay').innerHTML = `💡 建議吃：<span style="color:#e67e22;">${picked.name}</span>`;
        }

        function remove(id) {
            if(confirm("確定刪除？")) {
                foods = foods.filter(f => f.id !== id);
                localStorage.setItem('food_v3', JSON.stringify(foods));
                render();
            }
        }
    </script>
</body>
</html>
