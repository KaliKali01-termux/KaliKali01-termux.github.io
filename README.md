# KaliKali01-termux.github.io
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Arise Clone - RPG Engine</title>
    <style>
        :root {
            --bg-color: #0f0f12;
            --card-bg: #16161a;
            --accent-color: #a370f7;
            --text-color: #f1f1f4;
            --stat-bg: #222227;
            --success-color: #00f5d4;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 16px;
            display: flex;
            justify-content: center;
        }

        .container {
            width: 100%;
            max-width: 500px;
        }

        .card {
            background-color: var(--card-bg);
            border-radius: 16px;
            padding: 20px;
            margin-bottom: 16px;
            box-shadow: 0 8px 24px rgba(0,0,0,0.5);
            border: 1px solid #232329;
        }

        h2 { 
            margin-top: 0; 
            font-size: 1.2rem;
            color: var(--accent-color); 
            letter-spacing: 0.5px;
        }

        .profile-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px;
        }

        .xp-bar-container {
            background-color: #2a2a32;
            border-radius: 8px;
            height: 14px;
            width: 100%;
            overflow: hidden;
            border: 1px solid #3a3a45;
        }

        .xp-bar {
            background: linear-gradient(90deg, #833ab4, var(--accent-color));
            height: 100%;
            width: 0%;
            transition: width 0.4s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-top: 20px;
        }

        .stat-box {
            background: var(--stat-bg);
            padding: 12px 8px;
            border-radius: 12px;
            font-size: 0.85rem;
            text-align: center;
            border: 1px solid #2d2d35;
        }

        .stat-val {
            display: block;
            font-size: 1.3rem;
            font-weight: bold;
            margin-top: 4px;
            color: #fff;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-bottom: 20px;
        }

        .input-row {
            display: flex;
            gap: 8px;
        }

        input, select, button {
            padding: 12px;
            border-radius: 10px;
            border: 1px solid #2d2d35;
            background: var(--stat-bg);
            color: var(--text-color);
            font-size: 0.95rem;
            outline: none;
        }

        input { flex: 1; }
        select { cursor: pointer; }

        button {
            background: var(--accent-color);
            color: #fff;
            font-weight: 600;
            cursor: pointer;
            border: none;
            transition: transform 0.1s ease;
        }

        button:active { transform: scale(0.97); }

        .habit-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--stat-bg);
            padding: 14px;
            border-radius: 12px;
            margin-bottom: 10px;
            border: 1px solid #2d2d35;
        }

        .habit-info p {
            margin: 0 0 4px 0;
            font-weight: 500;
        }

        .habit-info small {
            color: #8a8a93;
            font-size: 0.75rem;
        }

        .habit-btn {
            background: transparent;
            border: 2px solid var(--success-color);
            color: var(--success-color);
            padding: 8px 14px;
            font-size: 0.85rem;
            border-radius: 8px;
        }
        .habit-btn:hover {
            background: rgba(0, 245, 212, 0.1);
        }
    </style>
</head>
<body>

<div class="container">
    <!-- Character Panel -->
    <div class="card">
        <div class="profile-header">
            <div>
                <h1 style="margin:0; font-size: 1.4rem; font-weight: 700;">Level <span id="level-display">1</span></h1>
                <small style="color: #8a8a93;">Player Profile</small>
            </div>
            <div>
                <span style="color: #ffd700; font-weight: 700; font-size: 1.1rem; background: rgba(255,215,0,0.1); padding: 6px 12px; border-radius: 20px;">💰 <span id="gold-display">0</span></span>
            </div>
        </div>
        
        <div class="xp-bar-container" style="margin-top: 15px;">
            <div id="xp-bar" class="xp-bar"></div>
        </div>
        <div style="font-size: 0.75rem; color: #8a8a93; text-align: right; margin-top: 6px;" id="xp-text">0 / 100 XP</div>

        <div class="stats-grid">
            <div class="stat-box">💪 STR <span class="stat-val" id="str-display">10</span></div>
            <div class="stat-box">🧠 INT <span class="stat-val" id="int-display">10</span></div>
            <div class="stat-box">🗣️ CHA <span class="stat-val" id="cha-display">10</span></div>
        </div>
    </div>

    <!-- Habit Manager -->
    <div class="card">
        <h2>Daily Quests</h2>
        <div class="input-group">
            <input type="text" id="habit-name" placeholder="Add a real-life quest...">
            <div class="input-row">
                <select id="habit-stat" style="flex: 1;">
                    <option value="str">💪 Strength</option>
                    <option value="int">🧠 Intelligence</option>
                    <option value="cha">🗣️ Charisma</option>
                </select>
                <button onclick="addHabit()" style="padding: 12px 24px;">+ Add</button>
            </div>
        </div>
        <div id="habit-list"></div>
    </div>
</div>

<script>
    // ENGINE STATE
    let gameState = {
        level: 1,
        xp: 0,
        gold: 0,
        stats: { str: 10, int: 10, cha: 10 },
        habits: [
            { id: 1, name: "Read 10 pages", stat: "int", xpReward: 25, goldReward: 10 },
            { id: 2, name: "Workout / Cardio", stat: "str", xpReward: 30, goldReward: 15 }
        ]
    };

    // ENGINE FORMULA
    function getXpNeeded(level) {
        return level * 100; 
    }

    function awardRewards(xpGain, goldGain, statKey) {
        gameState.xp += xpGain;
        gameState.gold += goldGain;
        gameState.stats[statKey] += 1;

        // Level Up Checking System
        while (gameState.xp >= getXpNeeded(gameState.level)) {
            gameState.xp -= getXpNeeded(gameState.level);
            gameState.level++;
        }

        saveAndRender();
    }

    // UI RENDER LOOP
    function render() {
        document.getElementById('level-display').innerText = gameState.level;
        document.getElementById('gold-display').innerText = gameState.gold;
        document.getElementById('str-display').innerText = gameState.stats.str;
        document.getElementById('int-display').innerText = gameState.stats.int;
        document.getElementById('cha-display').innerText = gameState.stats.cha;

        const targetXp = getXpNeeded(gameState.level);
        const percentage = (gameState.xp / targetXp) * 100;
        document.getElementById('xp-bar').style.width = ${percentage}%;
        document.getElementById('xp-text').innerText = ${gameState.xp} / ${targetXp} XP;

        const listContainer = document.getElementById('habit-list');
        listContainer.innerHTML = '';
        
        gameState.habits.forEach(habit => {
            const item = document.createElement('div');
            item.className = 'habit-item';
            item.innerHTML = 
                <div class="habit-info">
                    <p>${habit.name}</p>
                    <small>+${habit.xpReward} XP · +${habit.goldReward} Gold · +1 ${habit.stat.toUpperCase()}</small>
                </div>
                <button class="habit-btn" onclick="completeHabit(${habit.id})">Done</button>
            ;
            listContainer.appendChild(item);
        });
    }

    // ENGINE ACTIONS
    function completeHabit(id) {
        const habit = gameState.habits.find(h => h.id === id);
        if (habit) {
            awardRewards(habit.xpReward, habit.goldReward, habit.stat);
        }
    }

    function addHabit() {
        const nameInput = document.getElementById('habit-name');
        const statSelect = document.getElementById('habit-stat');
        
        if (!nameInput.value.trim()) return;

        const newHabit = {
            id: Date.now(),
            name: nameInput.value.trim(),
            stat: statSelect.value,
            xpReward: 20,
            goldReward: 5
        };

        gameState.habits.push(newHabit);
        nameInput.value = '';
        saveAndRender();
    }

    // STATE MEMORY PERSISTENCE
    function saveAndRender() {
        localStorage.setItem('arise_clone_state', JSON.stringify(gameState));
        render();
    }

    function loadState() {
        const saved = localStorage.getItem('arise_clone_state');
        if (saved) {
            gameState = JSON.parse(saved);
        }
        render();
    }

    window.onload = loadState;
</script>

</body>
</html>
