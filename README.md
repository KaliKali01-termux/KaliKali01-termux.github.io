<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Arise - Life RPG</title>
    <style>
        :root {
            --bg-dark: #09090b;
            --panel-bg: #141419;
            --accent-neon: #a855f7;
            --accent-green: #22c55e;
            --text-main: #f4f4f5;
            --text-muted: #a1a1aa;
            --border-color: #27272a;
            --gold-color: #eab308;
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
            background-color: var(--bg-dark);
            color: var(--text-main);
            margin: 0;
            padding: 0;
            padding-bottom: 80px; /* Space for bottom nav */
        }

        .header-bar {
            background-color: var(--panel-bg);
            padding: 16px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border-color);
            position: sticky;
            top: 0;
            z-index: 100;
        }

        .app-title {
            font-size: 1.3rem;
            font-weight: 800;
            letter-spacing: 1px;
            background: linear-gradient(to right, #c084fc, #6366f1);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .gold-badge {
            background: rgba(234, 179, 8, 0.1);
            border: 1px solid rgba(234, 179, 8, 0.3);
            color: var(--gold-color);
            padding: 6px 14px;
            border-radius: 20px;
            font-weight: 700;
            font-size: 0.95rem;
        }

        .main-container {
            padding: 16px;
            max-width: 500px;
            margin: 0 auto;
        }

        /* Screen Management */
        .app-screen {
            display: none;
        }
        .app-screen.active {
            display: block;
        }

        .card {
            background-color: var(--panel-bg);
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 16px;
            border: 1px solid var(--border-color);
        }

        /* Character Profile UI */
        .avatar-circle {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, var(--accent-neon), #6366f1);
            border-radius: 50%;
            margin: 0 auto 12px auto;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            box-shadow: 0 0 15px rgba(168, 85, 247, 0.4);
        }

        .level-text {
            text-align: center;
            font-size: 1.5rem;
            font-weight: 800;
            margin: 0 0 4px 0;
        }

        .xp-container {
            background: #202025;
            border-radius: 20px;
            height: 10px;
            overflow: hidden;
            margin: 12px 0 6px 0;
        }
        .xp-fill {
            height: 100%;
            background: linear-gradient(90deg, var(--accent-neon), #c084fc);
            width: 0%;
            transition: width 0.4s ease;
        }

        .stats-row {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 10px;
            margin-top: 20px;
        }
        .stat-card {
            background: #1a1a22;
            border: 1px solid #272730;
            padding: 12px;
            border-radius: 12px;
            text-align: center;
        }
        .stat-num {
            display: block;
            font-size: 1.4rem;
            font-weight: 700;
            color: #fff;
            margin-top: 4px;
        }
        /* Inputs & Buttons */
        .input-box {
            width: 100%;
            padding: 14px;
            background: #1a1a22;
            border: 1px solid var(--border-color);
            border-radius: 12px;
            color: white;
            font-size: 1rem;
            margin-bottom: 10px;
            outline: none;
        }
        .input-box:focus {
            border-color: var(--accent-neon);
        }

        .btn-primary {
            width: 100%;
            padding: 14px;
            background: var(--accent-neon);
            border: none;
            border-radius: 12px;
            color: white;
            font-weight: 700;
            font-size: 1rem;
            cursor: pointer;
        }

        /* Items List (Quests / Food / Shop) */
        .list-item {
            background: #1a1a22;
            border: 1px solid var(--border-color);
            padding: 14px;
            border-radius: 12px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 10px;
        }
        .item-details p { margin: 0 0 4px 0; font-weight: 600; }
        .item-details small { color: var(--text-muted); font-size: 0.8rem; }

        .action-btn {
            background: transparent;
            border: 2px solid var(--accent-green);
            color: var(--accent-green);
            padding: 8px 16px;
            border-radius: 8px;
            font-weight: 700;
            cursor: pointer;
        }
        .action-btn.shop {
            border-color: var(--gold-color);
            color: var(--gold-color);
        }

        /* Bottom Navigation Bar */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background-color: var(--panel-bg);
            border-top: 1px solid var(--border-color);
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            z-index: 100;
        }
        .nav-item {
            background: none;
            border: none;
            color: var(--text-muted);
            display: flex;
            flex-direction: column;
            align-items: center;
            font-size: 0.75rem;
            cursor: pointer;
            gap: 4px;
        }
        .nav-item.active {
            color: var(--accent-neon);
        }
        .nav-icon {
            font-size: 1.4rem;
        }
    </style>
</head>
<body>

    <div class="header-bar">
        <div class="app-title">ARISE // LIFE</div>
        <div class="gold-badge">💰 <span id="gold-val">0</span></div>
    </div>

    <div class="main-container">
        
        <div id="screen-profile" class="app-screen active">
            <div class="card" style="text-align: center;">
                <div class="avatar-circle">👤</div>
                <h2 class="level-text">Level <span id="level-val">1</span></h2>
                <div class="xp-container">
                    <div id="xp-fill" class="xp-fill"></div>
                </div>
                <div style="font-size: 0.8rem; color: var(--text-muted);" id="xp-text">0 / 100 XP</div>

                <div class="stats-row">
                    <div class="stat-card">💪 STR <span class="stat-num" id="str-val">10</span></div>
                    <div class="stat-card">🧠 INT <span class="stat-num" id="int-val">10</span></div>
                    <div class="stat-card">🗣️ CHA <span class="stat-num" id="cha-val">10</span></div>
                </div>
            </div>
        </div>

        <div id="screen-quests" class="app-screen">
            <div class="card">
                <h2 style="margin-bottom: 12px;">Create Custom Quest</h2>
                <input type="text" id="quest-input" class="input-box" placeholder="Quest name (e.g., Hit the gym)...">
                <select id="quest-stat" class="input-box">
                    <option value="str">💪 Strength</option>
                    <option value="int">🧠 Intelligence</option>
                    <option value="cha">🗣️ Charisma</option>
                </select>
                <button class="btn-primary" onclick="addCustomQuest()">Add Quest</button>
            </div>
            
            <h2>Active Quests</h2>
            <div id="quests-list"></div>
        </div>

        <div id="screen-calorie" class="app-screen">
            <div class="card" style="display: flex; justify-content: space-around; text-align: center;">
                <div>
                    <span style="font-size: 1.5rem; font-weight: bold;" id="cal-count">0</span>
                    <small style="display: block; color: var(--text-muted);">Calories Logged</small>
                </div>
                <div style="border-left: 1px solid var(--border-color); padding-left: 20px;">
                    <span style="font-size: 1.5rem; font-weight: bold;" id="water-count">0</span>
                    <small style="display: block; color: var(--text-muted);">Water (Glasses)</small>
                </div>
            </div>

            <div class="card">
                <h2>Log Food / Drink</h2>
                <input type="text" id="food-name" class="input-box" placeholder="Food name (e.g., Apple, Oats)...">
                <input type="number" id="food-cals" class="input-box" placeholder="Calories amount (e.g., 150)...">
                <button class="btn-primary" onclick="logFood()" style="background-color: var(--accent-green);">Log Diet (+CON)</button>
            </div>
            <div style="display: flex; gap: 10px;">
                <button class="btn-primary" onclick="logWater()" style="background: #3b82f6;">💧 Log 1 Glass Water</button>
                <button class="btn-primary" onclick="resetCalorie()" style="background: #ef4444;">Reset Today</button>
            </div>
        </div>

        <div id="screen-shop" class="app-screen">
            <div class="card">
                <h2>Custom Rewards Store</h2>
                <small style="color: var(--text-muted); display: block; margin-bottom: 12px;">Spend your hard-earned gold on real life breaks!</small>
            </div>
            <div id="shop-list"></div>
        </div>

    </div>

    <div class="bottom-nav">
        <button class="nav-item active" onclick="switchScreen('profile', this)">
            <span class="nav-icon">👤</span>Profile
        </button>
        <button class="nav-item" onclick="switchScreen('quests', this)">
            <span class="nav-icon">⚔️</span>Quests
        </button>
        <button class="nav-item" onclick="switchScreen('calorie', this)">
            <span class="nav-icon">🍎</span>Diet
        </button>
        <button class="nav-item" onclick="switchScreen('shop', this)">
            <span class="nav-icon">🛒</span>Shop
        </button>
    </div>

<script>
    // CORE SYSTEM MEMORY
    let appState = {
        level: 1, xp: 0, gold: 100,
        stats: { str: 10, int: 10, cha: 10 },
        calories: 0, water: 0,
        quests: [
            { id: 1, name: "Drink 3 Liters of Water", stat: "str", xp: 20, gold: 10 },
            { id: 2, name: "Read Study Material", stat: "int", xp: 30, gold: 15 }
        ],
        shop: [
            { id: 101, name: "Watch 1 Hour Netflix", cost: 40 },
            { id: 102, name: "Eat a Cheat Snack", cost: 80 }
        ]
    };

    // TAB SWITCHER
    function switchScreen(screenId, element) {
        document.querySelectorAll('.app-screen').forEach(s => s.classList.remove('active'));
        document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
        
        document.getElementById(screen-${screenId}).classList.add('active');
        element.classList.add('active');
    }

    // GAME MATHEMATICS
    function getXpLimit(lvl) { return lvl * 100; }
    function earn(xp, gold, stat) {
        appState.xp += xp;
        appState.gold += gold;
        if(stat && appState.stats[stat] !== undefined) appState.stats[stat] += 1;

        while(appState.xp >= getXpLimit(appState.level)) {
            appState.xp -= getXpLimit(appState.level);
            appState.level++;
        }
        saveAndUI();
    }

    // CORE INTERACTIONS
    function addCustomQuest() {
        const name = document.getElementById('quest-input').value.trim();
        const stat = document.getElementById('quest-stat').value;
        if(!name) return;

        appState.quests.push({ id: Date.now(), name, stat, xp: 25, gold: 10 });
        document.getElementById('quest-input').value = '';
        saveAndUI();
    }

    function completeQuest(id) {
        const q = appState.quests.find(item => item.id === id);
        if(q) earn(q.xp, q.gold, q.stat);
    }

    function logFood() {
        const name = document.getElementById('food-name').value.trim();
        const cals = parseInt(document.getElementById('food-cals').value);
        if(!name || isNaN(cals)) return;

        appState.calories += cals;
        document.getElementById('food-name').value = '';
        document.getElementById('food-cals').value = '';
        earn(10, 5, 'str'); // Eating logs improve physical knowledge/constitution
    }

    function logWater() {
        appState.water += 1;
        earn(5, 2, 'str');
    }

    function resetCalorie() {
        appState.calories = 0;
        appState.water = 0;
        saveAndUI();
    }

    function buyItem(id) {
        const item = appState.shop.find(s => s.id === id);
        if(item && appState.gold >= item.cost) {
            appState.gold -= item.cost;
            alert(Purchased: ${item.name}! Enjoy your reward.);
            saveAndUI();
        } else {
            alert("Not enough gold! Finish more daily quests.");
        }
    }

    // RE-RENDERING ENGINE
    function renderEngine() {
        document.getElementById('level-val').innerText = appState.level;
        document.getElementById('gold-val').innerText = appState.gold;
        document.getElementById('str-val').innerText = appState.stats.str;
        document.getElementById('int-val').innerText = appState.stats.int;
        document.getElementById('cha-val').innerText = appState.stats.cha;
        document.getElementById('cal-count').innerText = appState.calories;
        document.getElementById('water-count').innerText = appState.water;

        const max = getXpLimit(appState.level);
        document.getElementById('xp-fill').style.width = ${(appState.xp / max) * 100}%;
        document.getElementById('xp-text').innerText = ${appState.xp} / ${max} XP;

        // Load Quests UI
        const qList = document.getElementById('quests-list');
        qList.innerHTML = '';
        appState.quests.forEach(q => {
            qList.innerHTML += 
                <div class="list-item">
                    <div class="item-details">
                        <p>${q.name}</p>
                        <small>+${q.xp} XP · +${q.gold} Gold · +1 ${q.stat.toUpperCase()}</small>
                    </div>
                    <button class="action-btn" onclick="completeQuest(${q.id})">Done</button>
                </div>;
        });

        // Load Shop UI
        const sList = document.getElementById('shop-list');
        sList.innerHTML = '';
        appState.shop.forEach(s => {
            sList.innerHTML += 
                <div class="list-item">
                    <div class="item-details">
                        <p>${s.name}</p>
                        <small>Cost: ${s.cost} Gold</small>
                    </div>
                    <button class="action-btn shop" onclick="buyItem(${s.id})">Claim</button>
                </div>;
        });
    }

    function saveAndUI() {
        localStorage.setItem('arise_premium_state', JSON.stringify(appState));
        renderEngine();
    }
    window.onload = function() {
        const localData = localStorage.getItem('arise_premium_state');
        if(localData) appState = JSON.parse(localData);
        renderEngine();
    }
</script>
</body>
</html>
