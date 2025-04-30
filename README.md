<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mining Pool Simulator</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #121212;
            color: #eee;
            overflow-x: hidden;
        }
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        header {
            background-color: #1e1e1e;
            padding: 20px;
            text-align: center;
            border-bottom: 2px solid #444;
        }
        h1 {
            margin: 0;
            color: #4CAF50;
        }
        .game-area {
            display: flex;
            flex-wrap: wrap;
            margin-top: 20px;
        }
        .resources-panel {
            flex: 1;
            min-width: 300px;
            background-color: #1e1e1e;
            border-radius: 8px;
            padding: 15px;
            margin-right: 20px;
            margin-bottom: 20px;
        }
        .mining-panel {
            flex: 2;
            min-width: 400px;
            background-color: #1e1e1e;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 20px;
        }
        .upgrade-panel {
            flex: 1;
            min-width: 300px;
            background-color: #1e1e1e;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 20px;
        }
        .stats-panel {
            flex: 2;
            min-width: 400px;
            background-color: #1e1e1e;
            border-radius: 8px;
            padding: 15px;
            margin-bottom: 20px;
        }
        h2 {
            margin-top: 0;
            color: #2196F3;
            border-bottom: 1px solid #444;
            padding-bottom: 10px;
        }
        .resource {
            display: flex;
            justify-content: space-between;
            padding: 8px 0;
            border-bottom: 1px solid #333;
        }
        .resource:last-child {
            border-bottom: none;
        }
        .resource-name {
            font-weight: bold;
            color: #ddd;
        }
        .resource-value {
            color: #4CAF50;
        }
        .resource-rate {
            color: #2196F3;
            font-size: 0.9em;
        }
        button {
            background-color: #2196F3;
            color: white;
            border: none;
            padding: 10px 15px;
            margin: 5px;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        button:hover {
            background-color: #0b7dda;
        }
        button:disabled {
            background-color: #555;
            cursor: not-allowed;
        }
        .upgrade {
            background-color: #282828;
            padding: 12px;
            margin-bottom: 12px;
            border-radius: 6px;
        }
        .upgrade-title {
            font-weight: bold;
            margin-bottom: 5px;
            color: #2196F3;
        }
        .upgrade-description {
            font-size: 0.9em;
            margin-bottom: 8px;
            color: #bbb;
        }
        .upgrade-cost {
            font-size: 0.9em;
            color: #ff9800;
            margin-bottom: 8px;
        }
        progress {
            width: 100%;
            height: 20px;
            margin: 10px 0;
        }
        .mining-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
        }
        .miners-container {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-top: 15px;
        }
        .miner {
            width: 60px;
            height: 80px;
            background-color: #333;
            border-radius: 5px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }
        .miner-active {
            background-color: #1b5e20;
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0% { background-color: #1b5e20; }
            50% { background-color: #2e7d32; }
            100% { background-color: #1b5e20; }
        }
        .miner-icon {
            font-size: 24px;
            margin-bottom: 5px;
        }
        .miner-level {
            font-size: 12px;
            color: #ddd;
        }
        #notification {
            position: fixed;
            top: 20px;
            right: 20px;
            background-color: #4CAF50;
            color: white;
            padding: 15px;
            border-radius: 5px;
            display: none;
            z-index: 100;
            animation: fadeIn 0.5s;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
            margin-top: 15px;
        }
        .stat-card {
            background-color: #282828;
            padding: 15px;
            border-radius: 6px;
        }
        .stat-title {
            font-size: 0.9em;
            color: #bbb;
            margin-bottom: 5px;
        }
        .stat-value {
            font-size: 1.5em;
            color: #4CAF50;
        }
        .pools {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            margin-top: 15px;
        }
        .pool {
            background-color: #282828;
            padding: 15px;
            border-radius: 6px;
            flex: 1;
            min-width: 200px;
            cursor: pointer;
            border: 2px solid transparent;
            transition: all 0.3s;
        }
        .pool:hover {
            border-color: #2196F3;
        }
        .pool-active {
            border-color: #4CAF50;
            background-color: #1b5e20;
        }
        .pool-name {
            font-weight: bold;
            color: #2196F3;
            margin-bottom: 5px;
        }
        .pool-difficulty {
            font-size: 0.9em;
            color: #ff9800;
            margin-bottom: 5px;
        }
        .pool-reward {
            font-size: 0.9em;
            color: #4CAF50;
        }
        #mining-animation {
            height: 100px;
            margin: 10px 0;
            position: relative;
            overflow: hidden;
            background-color: #282828;
            border-radius: 5px;
        }
        .mining-particle {
            position: absolute;
            width: 10px;
            height: 10px;
            background-color: #4CAF50;
            border-radius: 50%;
        }
        #log-container {
            height: 150px;
            overflow-y: auto;
            background-color: #282828;
            padding: 10px;
            border-radius: 5px;
            margin-top: 15px;
            font-family: monospace;
            font-size: 0.9em;
        }
        .log-entry {
            margin-bottom: 5px;
            padding: 3px 0;
            border-bottom: 1px solid #333;
        }
        .timestamp {
            color: #888;
            margin-right: 5px;
        }
        .log-info {
            color: #2196F3;
        }
        .log-success {
            color: #4CAF50;
        }
        .log-warning {
            color: #ff9800;
        }
        .log-error {
            color: #f44336;
        }
        @media (max-width: 768px) {
            .game-area {
                flex-direction: column;
            }
            .resources-panel, .mining-panel, .upgrade-panel, .stats-panel {
                margin-right: 0;
                min-width: auto;
            }
        }
    </style>
</head>
<body>
    <div id="notification"></div>
    <header>
        <h1>Crypto Mining Pool Simulator</h1>
    </header>
    <div class="container">
        <div class="game-area">
            <div class="resources-panel">
                <h2>Resources</h2>
                <div class="resource">
                    <span class="resource-name">Coins</span>
                    <span class="resource-value" id="coins">0</span>
                </div>
                <div class="resource">
                    <span class="resource-name">Hashrate</span>
                    <span class="resource-value" id="hashrate">1 H/s</span>
                </div>
                <div class="resource">
                    <span class="resource-name">Energy</span>
                    <div>
                        <span class="resource-value" id="energy">100</span>
                        <span class="resource-rate" id="energy-rate">/ 100</span>
                    </div>
                </div>
                <div class="resource">
                    <span class="resource-name">Electricity Cost</span>
                    <span class="resource-value" id="electricity-cost">0.1 coins/s</span>
                </div>
            </div>

            <div class="mining-panel">
                <h2>Mining Operations</h2>
                <div class="pools">
                    <div class="pool" data-id="1" onclick="selectPool(1)">
                        <div class="pool-name">Beginner Pool</div>
                        <div class="pool-difficulty">Difficulty: Low</div>
                        <div class="pool-reward">Reward: 5 coins/block</div>
                    </div>
                    <div class="pool" data-id="2" onclick="selectPool(2)">
                        <div class="pool-name">Standard Pool</div>
                        <div class="pool-difficulty">Difficulty: Medium</div>
                        <div class="pool-reward">Reward: 20 coins/block</div>
                    </div>
                    <div class="pool" data-id="3" onclick="selectPool(3)">
                        <div class="pool-name">Pro Pool</div>
                        <div class="pool-difficulty">Difficulty: High</div>
                        <div class="pool-reward">Reward: 100 coins/block</div>
                    </div>
                </div>

                <div id="mining-animation"></div>
                
                <div class="mining-controls">
                    <div>
                        <span>Mining Progress:</span>
                        <progress id="mining-progress" value="0" max="100"></progress>
                    </div>
                    <button id="toggle-mining" onclick="toggleMining()">Start Mining</button>
                </div>

                <h3>Your Miners</h3>
                <div class="miners-container" id="miners-container">
                    <!-- Miners will be dynamically added here -->
                </div>
            </div>

            <div class="upgrade-panel">
                <h2>Upgrades</h2>
                <div class="upgrade">
                    <div class="upgrade-title">Buy New Miner</div>
                    <div class="upgrade-description">Add a new mining rig to your operation</div>
                    <div class="upgrade-cost" id="new-miner-cost">Cost: 50 coins</div>
                    <button onclick="buyMiner()">Purchase</button>
                </div>
                <div class="upgrade">
                    <div class="upgrade-title">Upgrade Hashrate</div>
                    <div class="upgrade-description">Increase your mining power</div>
                    <div class="upgrade-cost" id="hashrate-upgrade-cost">Cost: 30 coins</div>
                    <button onclick="upgradeHashrate()">Upgrade</button>
                </div>
                <div class="upgrade">
                    <div class="upgrade-title">Energy Efficiency</div>
                    <div class="upgrade-description">Reduce your electricity costs</div>
                    <div class="upgrade-cost" id="efficiency-upgrade-cost">Cost: 100 coins</div>
                    <button onclick="upgradeEfficiency()">Upgrade</button>
                </div>
                <div class="upgrade">
                    <div class="upgrade-title">Expand Energy Capacity</div>
                    <div class="upgrade-description">Increase your maximum energy</div>
                    <div class="upgrade-cost" id="energy-upgrade-cost">Cost: 75 coins</div>
                    <button onclick="upgradeEnergy()">Upgrade</button>
                </div>
            </div>

            <div class="stats-panel">
                <h2>Mining Statistics</h2>
                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-title">Total Mined</div>
                        <div class="stat-value" id="total-mined">0</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-title">Blocks Found</div>
                        <div class="stat-value" id="blocks-found">0</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-title">Electricity Used</div>
                        <div class="stat-value" id="electricity-used">0</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-title">Mining Efficiency</div>
                        <div class="stat-value" id="mining-efficiency">0%</div>
                    </div>
                </div>

                <h3>Event Log</h3>
                <div id="log-container">
                    <!-- Log entries will be added here -->
                </div>
            </div>
        </div>
    </div>

    <script>
        // Game state
        const gameState = {
            coins: 100, // Starting coins
            hashrate: 1,
            energy: 100,
            maxEnergy: 100,
            electricityCost: 0.1,
            isMining: false,
            currentPool: null,
            miners: [],
            miningProgress: 0,
            totalMined: 0,
            blocksFound: 0,
            electricityUsed: 0,
            hashrateUpgradeCost: 30,
            hashrateUpgradeLevel: 1,
            efficiencyUpgradeCost: 100,
            efficiencyUpgradeLevel: 1,
            energyUpgradeCost: 75,
            energyUpgradeLevel: 1,
            minerCost: 50,
            minerCount: 0
        };

        // Mining pools
        const pools = [
            { id: 1, name: "Beginner Pool", difficulty: 100, reward: 5 },
            { id: 2, name: "Standard Pool", difficulty: 500, reward: 20 },
            { id: 3, name: "Pro Pool", difficulty: 2000, reward: 100 }
        ];

        // DOM Elements
        const coinsElement = document.getElementById('coins');
        const hashrateElement = document.getElementById('hashrate');
        const energyElement = document.getElementById('energy');
        const energyRateElement = document.getElementById('energy-rate');
        const electricityCostElement = document.getElementById('electricity-cost');
        const miningProgressElement = document.getElementById('mining-progress');
        const toggleMiningButton = document.getElementById('toggle-mining');
        const totalMinedElement = document.getElementById('total-mined');
        const blocksFoundElement = document.getElementById('blocks-found');
        const electricityUsedElement = document.getElementById('electricity-used');
        const miningEfficiencyElement = document.getElementById('mining-efficiency');
        const minersContainer = document.getElementById('miners-container');
        const logContainer = document.getElementById('log-container');
        const miningAnimation = document.getElementById('mining-animation');
        const notification = document.getElementById('notification');

        // Upgrade cost elements
        const newMinerCostElement = document.getElementById('new-miner-cost');
        const hashrateUpgradeCostElement = document.getElementById('hashrate-upgrade-cost');
        const efficiencyUpgradeCostElement = document.getElementById('efficiency-upgrade-cost');
        const energyUpgradeCostElement = document.getElementById('energy-upgrade-cost');

        // Initialize the game
        function initGame() {
            updateResourceDisplay();
            updateUpgradeButtons();
            addLogEntry("Game initialized. Select a mining pool to begin!", "info");
            createMiner(); // Start with one miner
        }

        // Update the resource display
        function updateResourceDisplay() {
            coinsElement.textContent = Math.floor(gameState.coins);
            hashrateElement.textContent = formatHashrate(gameState.hashrate);
            energyElement.textContent = Math.floor(gameState.energy);
            energyRateElement.textContent = `/ ${gameState.maxEnergy}`;
            electricityCostElement.textContent = `${gameState.electricityCost.toFixed(2)} coins/s`;
            
            totalMinedElement.textContent = Math.floor(gameState.totalMined);
            blocksFoundElement.textContent = gameState.blocksFound;
            electricityUsedElement.textContent = Math.floor(gameState.electricityUsed);
            
            // Calculate mining efficiency
            const efficiency = gameState.totalMined > 0 ? 
                ((gameState.totalMined / gameState.electricityUsed) * 100) : 0;
            miningEfficiencyElement.textContent = `${efficiency.toFixed(1)}%`;
            
            updateUpgradeButtons();
        }

        // Format hashrate with appropriate units
        function formatHashrate(hashrate) {
            if (hashrate < 1000) {
                return `${hashrate.toFixed(1)} H/s`;
            } else if (hashrate < 1000000) {
                return `${(hashrate / 1000).toFixed(1)} KH/s`;
            } else if (hashrate < 1000000000) {
                return `${(hashrate / 1000000).toFixed(1)} MH/s`;
            } else {
                return `${(hashrate / 1000000000).toFixed(1)} GH/s`;
            }
        }

        // Update upgrade buttons
        function updateUpgradeButtons() {
            // Update costs
            newMinerCostElement.textContent = `Cost: ${gameState.minerCost} coins`;
            hashrateUpgradeCostElement.textContent = `Cost: ${gameState.hashrateUpgradeCost} coins`;
            efficiencyUpgradeCostElement.textContent = `Cost: ${gameState.efficiencyUpgradeCost} coins`;
            energyUpgradeCostElement.textContent = `Cost: ${gameState.energyUpgradeCost} coins`;
        }

        // Select a mining pool
        function selectPool(poolId) {
            const pool = pools.find(p => p.id === poolId);
            if (pool) {
                gameState.currentPool = pool;
                
                // Update UI to show active pool
                document.querySelectorAll('.pool').forEach(el => {
                    el.classList.remove('pool-active');
                });
                document.querySelector(`.pool[data-id="${poolId}"]`).classList.add('pool-active');
                
                addLogEntry(`Joined ${pool.name}. Difficulty: ${pool.difficulty}, Reward: ${pool.reward} coins`, "info");
                
                // If mining is active, reset progress
                if (gameState.isMining) {
                    gameState.miningProgress = 0;
                    miningProgressElement.value = 0;
                }
            }
        }

        // Toggle mining on/off
        function toggleMining() {
            if (!gameState.currentPool) {
                addLogEntry("Please select a mining pool first!", "warning");
                showNotification("Please select a mining pool first!");
                return;
            }
            
            gameState.isMining = !gameState.isMining;
            toggleMiningButton.textContent = gameState.isMining ? "Stop Mining" : "Start Mining";
            
            if (gameState.isMining) {
                addLogEntry("Mining started!", "success");
                // Start animation
                startMiningAnimation();
                // Update miner status
                updateMinersStatus(true);
            } else {
                addLogEntry("Mining stopped.", "info");
                // Stop animation
                stopMiningAnimation();
                // Update miner status
                updateMinersStatus(false);
            }
        }

        // Mining animation variables
        let animationInterval;
        const particles = [];

        // Start mining animation
        function startMiningAnimation() {
            // Clear existing animation
            stopMiningAnimation();
            
            // Create particles
            for (let i = 0; i < 20; i++) {
                createParticle();
            }
            
            // Animate particles
            animationInterval = setInterval(() => {
                particles.forEach(particle => {
                    const top = parseFloat(particle.style.top);
                    const left = parseFloat(particle.style.left);
                    const speed = parseFloat(particle.dataset.speed);
                    
                    // Move particle from left to right
                    particle.style.left = `${left + speed}px`;
                    
                    // If particle is out of view, reset it
                    if (left > miningAnimation.offsetWidth) {
                        resetParticle(particle);
                    }
                });
            }, 50);
        }

        // Create a particle
        function createParticle() {
            const particle = document.createElement('div');
            particle.className = 'mining-particle';
            resetParticle(particle);
            miningAnimation.appendChild(particle);
            particles.push(particle);
        }

        // Reset particle position
        function resetParticle(particle) {
            const speed = 1 + Math.random() * 5; // Random speed
            const size = 3 + Math.random() * 7; // Random size
            const hue = Math.floor(Math.random() * 60) + 120; // Random color (green to blue)
            
            particle.style.top = `${Math.random() * miningAnimation.offsetHeight}px`;
            particle.style.left = `-${size}px`;
            particle.style.width = `${size}px`;
            particle.style.height = `${size}px`;
            particle.style.backgroundColor = `hsl(${hue}, 100%, 50%)`;
            particle.dataset.speed = speed;
        }

        // Stop mining animation
        function stopMiningAnimation() {
            clearInterval(animationInterval);
            particles.forEach(particle => {
                if (particle.parentNode) {
                    particle.parentNode.removeChild(particle);
                }
            });
            particles.length = 0;
        }

        // Create a new miner
        function createMiner() {
            const minerId = gameState.miners.length + 1;
            
            // Create miner DOM element
            const minerElement = document.createElement('div');
            minerElement.className = 'miner';
            minerElement.dataset.id = minerId;
            minerElement.innerHTML = `
                <div class="miner-icon">⛏️</div>
                <div class="miner-level">Lvl 1</div>
            `;
            
            minersContainer.appendChild(minerElement);
            
            // Add miner to game state
            gameState.miners.push({
                id: minerId,
                level: 1,
                element: minerElement
            });
            
            gameState.minerCount++;
            
            addLogEntry(`New miner #${minerId} added to your operation!`, "success");
        }

        // Update miners active/inactive status
        function updateMinersStatus(active) {
            gameState.miners.forEach(miner => {
                if (active) {
                    miner.element.classList.add('miner-active');
                } else {
                    miner.element.classList.remove('miner-active');
                }
            });
        }

        // Buy a new miner
        function buyMiner() {
            if (gameState.coins >= gameState.minerCost) {
                gameState.coins -= gameState.minerCost;
                createMiner();
                
                // Increase cost for next miner
                gameState.minerCost = Math.floor(gameState.minerCost * 1.5);
                
                updateResourceDisplay();
                showNotification("New miner purchased!");
            } else {
                addLogEntry("Not enough coins to buy a new miner!", "error");
                showNotification("Not enough coins!");
            }
        }

        // Upgrade hashrate
        function upgradeHashrate() {
            if (gameState.coins >= gameState.hashrateUpgradeCost) {
                gameState.coins -= gameState.hashrateUpgradeCost;
                
                // Increase hashrate
                const prevHashrate = gameState.hashrate;
                gameState.hashrate *= 1.5;
                gameState.hashrateUpgradeLevel++;
                
                // Increase cost for next upgrade
                gameState.hashrateUpgradeCost = Math.floor(gameState.hashrateUpgradeCost * 1.8);
                
                updateResourceDisplay();
                addLogEntry(`Hashrate upgraded: ${formatHashrate(prevHashrate)} → ${formatHashrate(gameState.hashrate)}`, "success");
                showNotification("Hashrate upgraded!");
            } else {
                addLogEntry("Not enough coins to upgrade hashrate!", "error");
                showNotification("Not enough coins!");
            }
        }

        // Upgrade efficiency
        function upgradeEfficiency() {
            if (gameState.coins >= gameState.efficiencyUpgradeCost) {
                gameState.coins -= gameState.efficiencyUpgradeCost;
                
                // Improve efficiency
                const prevCost = gameState.electricityCost;
                gameState.electricityCost *= 0.85;
                gameState.efficiencyUpgradeLevel++;
                
                // Increase cost for next upgrade
                gameState.efficiencyUpgradeCost = Math.floor(gameState.efficiencyUpgradeCost * 2);
                
                updateResourceDisplay();
                addLogEntry(`Energy efficiency improved: ${prevCost.toFixed(2)} → ${gameState.electricityCost.toFixed(2)} coins/s`, "success");
                showNotification("Energy efficiency improved!");
            } else {
                addLogEntry("Not enough coins to upgrade efficiency!", "error");
                showNotification("Not enough coins!");
            }
        }

        // Upgrade energy capacity
        function upgradeEnergy() {
            if (gameState.coins >= gameState.energyUpgradeCost) {
                gameState.coins -= gameState.energyUpgradeCost;
                
                // Increase energy capacity
                const prevCapacity = gameState.maxEnergy;
                gameState.maxEnergy = Math.floor(gameState.maxEnergy * 1.5);
                gameState.energyUpgradeLevel++;
                
                // Refill energy
                gameState.energy = gameState.maxEnergy;
                
                // Increase cost for next upgrade
                gameState.energyUpgradeCost = Math.floor(gameState.energyUpgradeCost * 1.7);
                
                updateResourceDisplay();
                addLogEntry(`Energy capacity upgraded: ${prevCapacity} → ${gameState.maxEnergy}`, "success");
                showNotification("Energy capacity upgraded!");
            } else {
                addLogEntry("Not enough coins to upgrade energy capacity!", "error");
                showNotification("Not enough coins!");
            }
        }

        // Add log entry
        function addLogEntry(message, type = "info") {
            const now = new Date();
            const timestamp = `${now.getHours().toString().padStart(2, '0')}:${now.getMinutes().toString().padStart(2, '0')}:${now.getSeconds().toString().padStart(2, '0')}`;
            
            const logEntry = document.createElement('div');
            logEntry.className = 'log-entry';
            logEntry.innerHTML = `
                <span class="timestamp">[${timestamp}]</span>
                <span class="log-${type}">${message}</span>
            `;
            
            logContainer.appendChild(logEntry);
            logContainer.scrollTop = logContainer.scrollHeight;
            
            // Limit log entries
            if (logContainer.children.length > 50) {
                logContainer.removeChild(logContainer.children[0]);
            }
        }

        // Show notification
        function showNotification(message) {
            notification.textContent = message;
            notification.style.display = 'block';
            
            setTimeout(() => {
                notification.style.display = 'none';
            }, 3000);
        }

        // Game loop (runs every second)
        function gameLoop() {
            if (gameState.isMining) {
                // Check if we have energy
                if (gameState.energy <= 0) {
                    addLogEntry("Out of energy! Mining stopped.", "error");
                    toggleMining();
                    showNotification("Out of energy!");
                    return;
                }
                
                // Calculate effective hashrate based on active miners
                const effectiveHashrate = gameState.hashrate * gameState.miners.length;
                
                // Consume electricity
                const electricityCost = gameState.electricityCost * gameState.miners.length;
                gameState.coins -= electricityCost;
                gameState.electricityUsed += electricityCost;
                
                // Consume energy
                gameState.energy -= gameState.miners.length * 0.5;
                if (gameState.energy < 0) gameState.energy = 0;
                
                // If we're broke, stop mining
                if (gameState.coins < 0) {
                    gameState.coins = 0;
                    addLogEntry("You ran out of coins! Mining stopped.", "error");
                    toggleMining();
                    showNotification("You ran out of coins!");
                    return;
                }
                
                // Update mining progress
                gameState.miningProgress += effectiveHashrate / gameState.currentPool.difficulty;
                miningProgressElement.value = gameState.miningProgress;
                
                // Check if a block is found
                if (gameState.miningProgress >= 100) {
                    gameState.miningProgress = 0;
                    miningProgressElement.value = 0;
                    
                    // Reward player
                    gameState.coins += gameState.currentPool.reward;
                    gameState.totalMined += gameState.currentPool.reward;
                    gameState.blocksFound++;
                    
                    addLogEntry(`Block found! Reward: ${gameState.currentPool.reward} coins`, "success");
                    showNotification("Block found!");
                }
                
                updateResourceDisplay();
            }
        }

        // Start the game loop
        setInterval(gameLoop, 1000);

        // Initialize the game
        initGame();
    </script>
</body>
</html>
