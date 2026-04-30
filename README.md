<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Equilibrium Clash: Valley of the End</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&family=Inter:wght@400;600&display=swap');
        
        body {
            font-family: 'Inter', sans-serif;
            background-color: #020617;
            color: #f8fafc;
            margin: 0;
            overflow-x: hidden;
        }

        .font-header { font-family: 'Orbitron', sans-serif; }

        /* THE ARENA */
        .arena-bg {
            background-image: url('微信圖片_20260421155512_1_2.jpg');
            background-size: cover;
            background-position: center bottom;
            background-repeat: no-repeat;
            position: relative;
            border: 4px solid #1e293b;
            /* Removed fixed height to make it responsive to laptop screens */
            overflow: hidden;
            border-radius: 1rem;
        }

        .arena-shake {
            animation: shake 0.4s infinite;
        }
        @keyframes shake {
            0% { transform: translate(1px, 1px) rotate(0deg); }
            20% { transform: translate(-3px, 0px) rotate(1deg); }
            40% { transform: translate(1px, -1px) rotate(1deg); }
            60% { transform: translate(-3px, 1px) rotate(0deg); }
            80% { transform: translate(-1px, -1px) rotate(1deg); }
            100% { transform: translate(1px, -2px) rotate(-1deg); }
        }

        .arena-overlay {
            position: absolute;
            inset: 0;
            background: linear-gradient(to bottom, rgba(0,0,0,0.1) 0%, transparent 40%, rgba(0,0,0,0.6) 100%);
            z-index: 1;
        }

        /* Intro Animation Text */
        .intro-text-anim {
            animation: pop-in 1s ease-out forwards;
        }
        @keyframes pop-in {
            0% { transform: scale(0.3); opacity: 0; }
            30% { transform: scale(1.2); opacity: 1; text-shadow: 0 0 40px #fff; }
            80% { transform: scale(1); opacity: 1; }
            100% { transform: scale(1.5); opacity: 0; }
        }

        /* Laser Energy Trails */
        .laser-f {
            background: linear-gradient(90deg, #4f46e5, #818cf8);
            box-shadow: 0 0 30px #4338ca, 0 0 60px #4338ca;
            z-index: 10;
        }
        .laser-b {
            background: linear-gradient(270deg, #d97706, #fbbf24);
            box-shadow: 0 0 30px #d97706, 0 0 60px #d97706;
            z-index: 10;
        }

        /* The Clash Orb */
        .clash-orb {
            position: absolute;
            width: 70px;
            height: 70px;
            border-radius: 50%;
            background: radial-gradient(circle, #ffffff 10%, #818cf8 50%, #fbbf24 90%);
            box-shadow: 0 0 40px #fff, 0 0 80px #6366f1, 0 0 120px #f59e0b;
            z-index: 25;
            display: flex;
            justify-content: center;
            align-items: center;
            top: 50%;
            transform: translate(-50%, -50%);
            animation: orb-pulse 0.1s infinite alternate;
        }
        .clash-ring {
            position: absolute;
            inset: -15px;
            border-radius: 50%;
            border: 6px solid transparent;
            border-top-color: #818cf8;
            border-bottom-color: #fbbf24;
            animation: spin 0.3s linear infinite;
        }
        @keyframes orb-pulse {
            from { transform: translate(-50%, -50%) scale(0.9); }
            to { transform: translate(-50%, -50%) scale(1.15); }
        }
        @keyframes spin { 100% { transform: rotate(360deg); } }

        /* Chakra Auras */
        .aura-f {
            position: absolute;
            inset: -30px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(99,102,241,0.8) 0%, transparent 60%);
            z-index: -1;
            opacity: 0;
            transition: opacity 0.2s, transform 0.2s;
            animation: aura-flicker 0.1s infinite alternate;
        }
        .aura-b {
            position: absolute;
            inset: -30px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(245,158,11,0.8) 0%, transparent 60%);
            z-index: -1;
            opacity: 0;
            transition: opacity 0.2s, transform 0.2s;
            animation: aura-flicker 0.1s infinite alternate;
        }
        @keyframes aura-flicker {
            from { transform: scale(0.9); }
            to { transform: scale(1.1); }
        }

        /* Floating Action Feedback */
        .float-text {
            position: absolute;
            font-family: 'Orbitron', sans-serif;
            font-weight: bold;
            font-size: 16px;
            pointer-events: none;
            z-index: 40;
            animation: float-up 0.5s ease-out forwards;
        }
        @keyframes float-up {
            0% { transform: translateY(0) scale(1); opacity: 1; }
            100% { transform: translateY(-60px) scale(1.5); opacity: 0; }
        }

        /* Falling Leaves Effect */
        .leaf {
            position: absolute;
            width: 14px;
            height: 14px;
            background: #4ade80;
            border-radius: 2px 10px;
            opacity: 0;
            pointer-events: none;
            z-index: 2;
            animation: drift linear forwards;
        }
        @keyframes drift {
            0% { transform: translate(0, -20px) rotate(0deg); opacity: 0; }
            10% { opacity: 0.8; }
            90% { opacity: 0.8; }
            100% { transform: translate(-150px, 450px) rotate(360deg); opacity: 0; }
        }

        .char-label {
            background: rgba(0, 0, 0, 0.9);
            padding: 4px 12px;
            border-radius: 8px;
            font-size: 11px;
            margin-top: -10px; /* pull it up slightly closer to char */
            border: 1px solid rgba(255, 255, 255, 0.3);
            white-space: nowrap;
        }

        .particle { position: absolute; border-radius: 50%; width: 12px; height: 12px; }
        .tab-active { border-bottom: 4px solid #3b82f6; color: #3b82f6; }

        /* NEW: Start Screen Styling */
        .start-screen-bg {
            background-image: url('開頭圖片.jpg');
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
        }
        .select-btn {
            background: rgba(0,0,0,0.7);
            border: 2px solid rgba(255,255,255,0.4);
            font-family: 'Orbitron', sans-serif;
            transition: all 0.3s ease-in-out;
        }
        .select-btn:hover {
            background: rgba(0,0,0,0.9);
            transform: scale(1.1);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col">

    <!-- START SCREEN (CHARACTER SELECT) -->
    <div id="start-screen" class="fixed inset-0 z-[100] start-screen-bg flex items-center justify-center transition-opacity duration-700">
        <div class="absolute inset-0 bg-black/20"></div> <!-- Slight dark overlay for readability -->
        <h1 class="absolute top-10 font-header text-5xl md:text-6xl text-white drop-shadow-[0_0_20px_rgba(0,0,0,1)] z-10 tracking-widest text-center w-full">SELECT YOUR CHARACTER</h1>
        
        <!-- Naruto Select (Top Left) -->
        <button onclick="startGameFromSelect('naruto')" class="select-btn absolute top-32 left-8 md:left-24 px-8 py-4 rounded-xl text-2xl md:text-3xl font-bold border-yellow-400 text-yellow-400 shadow-[0_0_15px_#fbbf24] hover:shadow-[0_0_30px_#fbbf24] z-10">
            CHOOSE NARUTO
        </button>
        
        <!-- Sasuke Select (Bottom Right) -->
        <button onclick="startGameFromSelect('sasuke')" class="select-btn absolute bottom-32 right-8 md:right-24 px-8 py-4 rounded-xl text-2xl md:text-3xl font-bold border-indigo-400 text-indigo-400 shadow-[0_0_15px_#818cf8] hover:shadow-[0_0_30px_#818cf8] z-10">
            CHOOSE SASUKE
        </button>
    </div>

    <!-- Navigation -->
    <nav class="bg-slate-900 border-b border-slate-800 p-4 shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto flex justify-between items-center">
            <h1 class="font-header text-xl font-bold tracking-tighter">EQUILIBRIUM<span class="text-blue-500">CLASH</span></h1>
            <div class="flex space-x-6">
                <button onclick="switchView('game')" id="btn-game" class="font-bold pb-1 transition tab-active">SHINOBI DUEL</button>
                <button onclick="switchView('sim')" id="btn-sim" class="font-bold pb-1 transition text-slate-400 border-transparent border-b-4 hover:text-slate-300">SCIENTIFIC SIM</button>
            </div>
        </div>
    </nav>

    <main class="flex-grow flex flex-col items-center justify-center p-4 w-full h-[calc(100vh-80px)]">
        
        <!-- =================== PART 1: GAME VIEW =================== -->
        <section id="view-game" class="w-full max-w-7xl h-full flex flex-col relative">
            
            <div class="text-center mb-2 flex-shrink-0">
                <h2 class="font-header text-2xl md:text-3xl text-white italic tracking-widest uppercase">Valley of the End</h2>
                <p class="text-slate-400 text-xs md:text-sm mt-1">Mash your buttons to push the equilibrium position!</p>
            </div>

            <!-- The Game Arena -->
            <div id="arena" class="arena-bg flex-grow shadow-[0_0_50px_rgba(0,0,0,0.5)] transition-all min-h-[300px]">
                <div class="arena-overlay"></div>
                <!-- Leaves Container -->
                <div id="leaves-container" class="absolute inset-0 overflow-hidden pointer-events-none"></div>

                <!-- Lasers acting as Energy Trails -->
                <div id="laser-f" class="absolute left-0 h-6 laser-f top-1/2 -translate-y-1/2 opacity-70" style="width: 50%;"></div>
                <div id="laser-b" class="absolute right-0 h-6 laser-b top-1/2 -translate-y-1/2 opacity-70" style="width: 50%;"></div>
                
                <!-- Epic Clash Orb -->
                <div id="clash" class="clash-orb" style="left: 50%;">
                    <div class="clash-ring"></div>
                </div>

                <!-- Sasuke (Left Side of Orb) -->
                <!-- Wrapper handles the dynamic absolute positioning based on the orb's location -->
                <div id="pos-sasuke" class="absolute z-30" style="top: 50%; left: 50%; transform: translate(calc(-100% - 35px), -50%); transition: left 0.1s linear;">
                    <!-- Inner handles the "lunging" animation when clicking -->
                    <div id="char-sasuke" class="flex flex-col items-center transition-transform duration-75">
                        <div class="relative w-28 h-28 bg-indigo-950/80 backdrop-blur-md rounded-full border-4 border-indigo-400 flex items-center justify-center shadow-[0_0_30px_#4338ca]">
                            <!-- Chakra Aura -->
                            <div id="aura-f" class="aura-f"></div>
                            <!-- Sasuke Avatar -->
                            <div class="w-full h-full rounded-full overflow-hidden relative z-10">
                                <img src="佐助.jpg" alt="Sasuke" class="w-full h-full object-cover" />
                            </div>
                        </div>
                        <span class="char-label text-indigo-300 font-header font-bold tracking-wider z-20 shadow-lg">SASUKE (A → B)</span>
                    </div>
                </div>

                <!-- Naruto (Right Side of Orb) -->
                <div id="pos-naruto" class="absolute z-30" style="top: 50%; left: 50%; transform: translate(35px, -50%); transition: left 0.1s linear;">
                    <div id="char-naruto" class="flex flex-col items-center transition-transform duration-75">
                        <div class="relative w-28 h-28 bg-yellow-950/80 backdrop-blur-md rounded-full border-4 border-yellow-400 flex items-center justify-center shadow-[0_0_30px_#d97706]">
                            <!-- Chakra Aura -->
                            <div id="aura-b" class="aura-b"></div>
                            <!-- Naruto Avatar -->
                            <div class="w-full h-full rounded-full overflow-hidden relative z-10">
                                <img src="銘人.jpg" alt="Naruto" class="w-full h-full object-cover" />
                            </div>
                        </div>
                        <span class="char-label text-yellow-300 font-header font-bold tracking-wider z-20 shadow-lg">NARUTO (B → A)</span>
                    </div>
                </div>

            </div>

            <!-- Rate Meters -->
            <div class="grid grid-cols-2 gap-8 md:gap-12 mt-4 flex-shrink-0">
                <div class="bg-slate-900/50 p-3 md:p-4 rounded-xl border border-slate-800 shadow-lg">
                    <div class="flex justify-between mb-2">
                        <p class="text-[10px] md:text-xs font-header text-indigo-400 uppercase tracking-widest">Forward Rate (kf)</p>
                        <p class="text-[10px] md:text-xs font-header text-indigo-400 font-bold" id="val-meter-f">0</p>
                    </div>
                    <div class="h-3 md:h-4 bg-slate-950 rounded-full overflow-hidden border border-slate-700 p-0.5">
                        <div id="meter-f" class="h-full bg-indigo-500 rounded-full w-0 transition-all shadow-[0_0_10px_#6366f1]"></div>
                    </div>
                </div>
                <div class="bg-slate-900/50 p-3 md:p-4 rounded-xl border border-slate-800 shadow-lg">
                    <div class="flex justify-between mb-2">
                        <p class="text-[10px] md:text-xs font-header text-yellow-500 uppercase tracking-widest">Backward Rate (kb)</p>
                        <p class="text-[10px] md:text-xs font-header text-yellow-500 font-bold" id="val-meter-b">0</p>
                    </div>
                    <div class="h-3 md:h-4 bg-slate-950 rounded-full overflow-hidden border border-slate-700 p-0.5">
                        <div id="meter-b" class="h-full bg-yellow-500 rounded-full w-0 transition-all shadow-[0_0_10px_#fbbf24]"></div>
                    </div>
                </div>
            </div>

            <!-- Controls -->
            <div class="flex justify-center space-x-16 md:space-x-24 mt-4 pb-2 md:pb-4 flex-shrink-0 relative">
                <div id="sparks-left" class="absolute left-[calc(50%-6rem)] -top-4 w-24 h-24 pointer-events-none z-50"></div>
                <div id="sparks-right" class="absolute right-[calc(50%-6rem)] -top-4 w-24 h-24 pointer-events-none z-50"></div>

                <div class="text-center flex flex-col items-center">
                    <button id="btn-a" class="w-20 h-20 md:w-24 md:h-24 bg-indigo-700 hover:bg-indigo-600 rounded-full border-4 border-indigo-400 font-header text-3xl md:text-4xl font-bold shadow-[0_10px_0_#312e81] active:shadow-[0_0px_0_#312e81] active:translate-y-2 transition-all text-white">A</button>
                    <p class="text-xs md:text-sm font-bold text-slate-400 mt-2 md:mt-4 tracking-widest uppercase">Press 'A'</p>
                </div>
                <div class="text-center flex flex-col items-center">
                    <button id="btn-l" class="w-20 h-20 md:w-24 md:h-24 bg-yellow-600 hover:bg-yellow-500 rounded-full border-4 border-yellow-400 font-header text-3xl md:text-4xl font-bold shadow-[0_10px_0_#78350f] active:shadow-[0_0px_0_#78350f] active:translate-y-2 transition-all text-white">L</button>
                    <p class="text-xs md:text-sm font-bold text-slate-400 mt-2 md:mt-4 tracking-widest uppercase">Press 'L'</p>
                </div>
            </div>

            <!-- INTRO ANIMATION MODAL -->
            <div id="intro-modal" class="hidden absolute inset-0 z-[60] bg-slate-950/90 backdrop-blur-md flex flex-col items-center justify-center rounded-xl pointer-events-none transition-opacity duration-500">
                <h1 id="intro-text" class="font-header text-8xl text-white italic drop-shadow-[0_0_30px_#fff] opacity-0">READY</h1>
            </div>

            <!-- FLASH OVERLAY (For Endings) -->
            <div id="flash-overlay" class="hidden absolute inset-0 z-[55] rounded-xl pointer-events-none transition-opacity duration-700 bg-white opacity-0"></div>

            <!-- GAME OVER SCREEN -->
            <div id="game-over-modal" class="hidden absolute inset-0 z-[56] bg-slate-950/95 backdrop-blur-lg flex flex-col items-center justify-center rounded-xl border border-slate-800">
                <h3 id="winner-title" class="font-header text-6xl mb-6 italic tracking-wider uppercase text-center"></h3>
                <p id="winner-desc" class="text-slate-300 text-xl max-w-2xl text-center mb-10 leading-relaxed font-semibold"></p>
                <button onclick="playIntroSequence()" class="bg-white text-black hover:bg-slate-200 px-12 py-5 rounded-full font-header font-bold text-2xl transition-all transform hover:scale-105 shadow-[0_0_30px_rgba(255,255,255,0.4)]">
                    REMATCH
                </button>
            </div>
        </section>

        <!-- =================== PART 2: SIMULATION VIEW =================== -->
        <section id="view-sim" class="hidden w-full max-w-7xl h-full grid grid-cols-1 md:grid-cols-4 gap-6">
            <div class="bg-slate-900 p-6 rounded-2xl border border-slate-800 space-y-6 shadow-2xl flex flex-col">
                <h3 class="font-header text-blue-400 text-xl tracking-widest uppercase border-b border-slate-800 pb-4">Rate Controls</h3>
                <div>
                    <label class="block text-xs font-bold text-indigo-400 uppercase tracking-widest mb-3">Forward Rate (kf)</label>
                    <input type="range" id="range-f" min="0.05" max="5" step="0.05" value="1" class="w-full h-2 rounded-lg appearance-none cursor-pointer accent-indigo-500 bg-slate-800">
                    <p class="text-[10px] text-slate-500 mt-2">Speed of A converting to B</p>
                </div>
                <div>
                    <label class="block text-xs font-bold text-yellow-500 uppercase tracking-widest mb-3">Backward Rate (kb)</label>
                    <input type="range" id="range-b" min="0.05" max="5" step="0.05" value="1" class="w-full h-2 rounded-lg appearance-none cursor-pointer accent-yellow-500 bg-slate-800">
                    <p class="text-[10px] text-slate-500 mt-2">Speed of B converting back to A</p>
                </div>
                <div class="bg-slate-950 p-4 rounded-xl border border-slate-800 mt-auto">
                    <p class="text-[10px] text-slate-500 uppercase font-bold tracking-widest mb-2">System Status</p>
                    <div class="flex items-center text-emerald-400 font-header text-sm">
                        <span class="relative flex h-3 w-3 mr-3"><span class="animate-ping absolute inline-flex h-full w-full rounded-full bg-emerald-400 opacity-75"></span><span class="relative inline-flex rounded-full h-3 w-3 bg-emerald-500"></span></span>
                        DYNAMIC EQUILIBRIUM
                    </div>
                </div>
            </div>

            <div class="md:col-span-3 bg-slate-900 border border-slate-800 rounded-2xl p-6 flex flex-col shadow-2xl">
                <div id="sim-container" class="bg-[#0b1121] border-2 border-slate-800 rounded-xl flex-grow relative overflow-hidden shadow-inner mb-6 min-h-[300px]">
                    <!-- Dynamic Counters overlay injected by JS -->
                </div>
                <div class="px-8 bg-slate-950 py-4 rounded-xl border border-slate-800 flex-shrink-0">
                    <div class="flex justify-between text-xs font-header text-slate-400 mb-3 tracking-widest">
                        <span class="text-indigo-400">REACTANTS DOMINATE</span>
                        <span class="text-white">EQUILIBRIUM POSITION</span>
                        <span class="text-yellow-500">PRODUCTS DOMINATE</span>
                    </div>
                    <div class="h-3 bg-slate-800 rounded-full relative">
                        <div id="equil-pointer" class="absolute top-1/2 -translate-y-1/2 w-6 h-6 bg-white rounded-full transition-all duration-700 shadow-[0_0_15px_rgba(255,255,255,0.8)] border-4 border-slate-900" style="left: 50%;"></div>
                    </div>
                </div>
            </div>
        </section>
    </main>

    <script>
        // ================= GAME LOGIC =================
        let pos = 50; 
        let rF = 0; 
        let rB = 0; 
        let gameActive = false;
        const DECAY_RATE = 0.5;
        const PUSH_POWER = 5.5;

        // Environmental FX: Falling Leaves
        const leavesContainer = document.getElementById('leaves-container');
        function spawnLeaf() {
            if(!gameActive) return;
            const leaf = document.createElement('div');
            leaf.className = 'leaf';
            leaf.style.left = Math.random() * 100 + '%';
            leaf.style.animationDuration = (Math.random() * 3 + 3) + 's';
            leavesContainer.appendChild(leaf);
            setTimeout(() => { if(leaf.parentNode) leaf.parentNode.removeChild(leaf); }, 6000);
        }
        setInterval(spawnLeaf, 500);

        // Action Feedback FX
        function spawnFloatingText(side) {
            const container = side === 'A' ? document.getElementById('sparks-left') : document.getElementById('sparks-right');
            const spark = document.createElement('div');
            spark.className = 'float-text ' + (side === 'A' ? 'text-indigo-400' : 'text-yellow-400');
            spark.innerText = "+RATE";
            spark.style.left = (Math.random() * 60 + 20) + '%';
            spark.style.top = '50%';
            container.appendChild(spark);
            setTimeout(() => { if(spark.parentNode) spark.parentNode.removeChild(spark); }, 600);
        }

        function updateGameUI() {
            // Update center orb and energy trails behind characters
            document.getElementById('clash').style.left = pos + '%';
            document.getElementById('laser-f').style.width = pos + '%';
            document.getElementById('laser-b').style.width = (100 - pos) + '%';
            
            // Move characters dynamically with the orb
            document.getElementById('pos-sasuke').style.left = pos + '%';
            document.getElementById('pos-naruto').style.left = pos + '%';

            // Update Meters
            const renderRF = Math.min(100, rF * 3);
            const renderRB = Math.min(100, rB * 3);
            document.getElementById('meter-f').style.width = renderRF + '%';
            document.getElementById('meter-b').style.width = renderRB + '%';
            document.getElementById('val-meter-f').innerText = Math.floor(renderRF);
            document.getElementById('val-meter-b').innerText = Math.floor(renderRB);

            // Update Chakra Auras intensity based on rate
            const auraF = document.getElementById('aura-f');
            const auraB = document.getElementById('aura-b');
            auraF.style.opacity = Math.min(1, rF / 20);
            auraF.style.transform = `scale(${1 + (rF / 50)})`;
            auraB.style.opacity = Math.min(1, rB / 20);
            auraB.style.transform = `scale(${1 + (rB / 50)})`;

            // Screen Shake trigger if reaction is close to completion (edges)
            const arena = document.getElementById('arena');
            if(pos > 85 || pos < 15) {
                if(!arena.classList.contains('arena-shake')) arena.classList.add('arena-shake');
            } else {
                arena.classList.remove('arena-shake');
            }
        }

        function gameLoop() {
            if(!gameActive) return;

            rF = Math.max(0, rF - DECAY_RATE);
            rB = Math.max(0, rB - DECAY_RATE);
            
            // Equilibrium shifts toward whichever rate is stronger
            pos += (rF - rB) * 0.08;
            pos = Math.max(0, Math.min(100, pos));

            updateGameUI();

            // Win Condition check (Pushing opponent completely out)
            if(pos >= 96) triggerGameOver('sasuke');
            else if(pos <= 4) triggerGameOver('naruto');
            else requestAnimationFrame(gameLoop);
        }

        function handleInput(side) {
            if(!gameActive) return;
            if(side === 'A') {
                rF += PUSH_POWER;
                spawnFloatingText('A');
                // Lunging physical push animation
                const char = document.getElementById('char-sasuke');
                char.style.transform = 'scale(1.1) translateX(15px)';
                setTimeout(() => char.style.transform = 'scale(1) translateX(0)', 100);
            }
            if(side === 'L') {
                rB += PUSH_POWER;
                spawnFloatingText('L');
                // Lunging physical push animation
                const char = document.getElementById('char-naruto');
                char.style.transform = 'scale(1.1) translateX(-15px)';
                setTimeout(() => char.style.transform = 'scale(1) translateX(0)', 100);
            }
        }

        // ================= INTRO & OUTRO SEQUENCES =================
        async function playIntroSequence() {
            // Reset and hide everything else
            gameActive = false;
            document.getElementById('game-over-modal').classList.add('hidden');
            document.getElementById('flash-overlay').classList.add('hidden');
            
            pos = 50; rF = 0; rB = 0;
            updateGameUI(); // Reset characters to center

            const introModal = document.getElementById('intro-modal');
            const introText = document.getElementById('intro-text');
            introModal.classList.remove('hidden');
            introModal.classList.remove('opacity-0');

            const textSequence = ["3", "2", "1", "CLASH!"];
            
            for (let text of textSequence) {
                introText.innerText = text;
                introText.style.color = text === "CLASH!" ? "#ef4444" : "#ffffff";
                
                // Restart animation
                introText.classList.remove('intro-text-anim');
                void introText.offsetWidth; // Trigger reflow
                introText.classList.add('intro-text-anim');
                
                await new Promise(r => setTimeout(r, 1000));
            }

            // Fade out intro modal
            introModal.classList.add('opacity-0');
            setTimeout(() => {
                introModal.classList.add('hidden');
                gameActive = true;
                gameLoop();
            }, 500);
        }

        function triggerGameOver(winner) {
            gameActive = false;
            document.getElementById('arena').classList.remove('arena-shake');
            
            // Giant Screen Flash
            const flash = document.getElementById('flash-overlay');
            flash.classList.remove('hidden');
            flash.style.backgroundColor = winner === 'sasuke' ? '#6366f1' : '#f59e0b';
            flash.style.opacity = '0.9';

            setTimeout(() => {
                // Fade flash and show modal
                flash.style.opacity = '0';
                setTimeout(() => flash.classList.add('hidden'), 700);

                const modal = document.getElementById('game-over-modal');
                const title = document.getElementById('winner-title');
                const desc = document.getElementById('winner-desc');

                modal.classList.remove('hidden');
                if(winner === 'sasuke') {
                    title.innerText = "REACTION COMPLETE!";
                    title.className = "font-header text-5xl mb-6 italic tracking-wider text-indigo-400 drop-shadow-[0_0_15px_#4338ca]";
                    desc.innerText = "Sasuke's Forward Reaction (A → B) was unstoppable! The reaction reached completion, and all Reactants have been converted into Products.";
                } else {
                    title.innerText = "SYSTEM REVERTED!";
                    title.className = "font-header text-5xl mb-6 italic tracking-wider text-yellow-500 drop-shadow-[0_0_15px_#d97706]";
                    desc.innerText = "Naruto's Backward Reaction (B → A) dominated! The system shifted entirely back to the starting state, demonstrating full reversibility.";
                }
            }, 800);
        }

        // Key Listeners
        document.getElementById('btn-a').addEventListener('mousedown', () => handleInput('A'));
        document.getElementById('btn-l').addEventListener('mousedown', () => handleInput('L'));
        document.getElementById('btn-a').addEventListener('touchstart', (e) => { e.preventDefault(); handleInput('A'); });
        document.getElementById('btn-l').addEventListener('touchstart', (e) => { e.preventDefault(); handleInput('L'); });

        window.addEventListener('keydown', (e) => {
            if(e.key.toLowerCase() === 'a') {
                handleInput('A');
                document.getElementById('btn-a').classList.add('translate-y-2', 'shadow-[0_0px_0_#312e81]');
                setTimeout(() => document.getElementById('btn-a').classList.remove('translate-y-2', 'shadow-[0_0px_0_#312e81]'), 100);
            }
            if(e.key.toLowerCase() === 'l') {
                handleInput('L');
                document.getElementById('btn-l').classList.add('translate-y-2', 'shadow-[0_0px_0_#78350f]');
                setTimeout(() => document.getElementById('btn-l').classList.remove('translate-y-2', 'shadow-[0_0px_0_#78350f]'), 100);
            }
        });


        // ================= SIMULATION LOGIC =================
        let particles = [];
        const NUM_PARTICLES = 50;
        let simRunning = false;

        function initSim() {
            const container = document.getElementById('sim-container');
            container.innerHTML = `
                <div class="absolute bottom-6 left-6 bg-indigo-950/80 border border-indigo-500/30 px-6 py-3 rounded-xl backdrop-blur-md z-10">
                    <span class="text-xs font-bold text-indigo-300 block mb-1">Reactant [A]</span>
                    <span id="count-a" class="text-3xl font-header text-white">25</span>
                </div>
                <div class="absolute bottom-6 right-6 bg-yellow-950/80 border border-yellow-500/30 px-6 py-3 rounded-xl backdrop-blur-md text-right z-10">
                    <span class="text-xs font-bold text-yellow-300 block mb-1">Product [B]</span>
                    <span id="count-b" class="text-3xl font-header text-white">25</span>
                </div>
            `;
            particles = [];
            for(let i=0; i < NUM_PARTICLES; i++) {
                let p = {
                    type: i < (NUM_PARTICLES / 2) ? 'A' : 'B',
                    x: Math.random() * 95, 
                    y: Math.random() * 95,
                    vx: (Math.random() - 0.5) * 3,
                    vy: (Math.random() - 0.5) * 3,
                    el: document.createElement('div')
                };
                p.el.className = 'particle ' + (p.type === 'A' ? 'bg-indigo-500 shadow-[0_0_10px_#6366f1]' : 'bg-yellow-500 shadow-[0_0_10px_#fbbf24]');
                container.appendChild(p.el);
                particles.push(p);
            }
        }

        function runSim() {
            if(!simRunning) return;

            const kf = parseFloat(document.getElementById('range-f').value) * 0.01;
            const kb = parseFloat(document.getElementById('range-b').value) * 0.01;
            let bCount = 0;

            particles.forEach(p => {
                p.x += p.vx; p.y += p.vy;
                if(p.x < 1 || p.x > 97) p.vx *= -1;
                if(p.y < 1 || p.y > 96) p.vy *= -1;

                if(p.type === 'A' && Math.random() < kf) { 
                    p.type = 'B'; 
                    p.el.className = 'particle bg-yellow-500 shadow-[0_0_10px_#fbbf24]';
                } else if(p.type === 'B' && Math.random() < kb) { 
                    p.type = 'A'; 
                    p.el.className = 'particle bg-indigo-500 shadow-[0_0_10px_#6366f1]';
                }

                if(p.type === 'B') bCount++;
                p.el.style.left = p.x + '%'; 
                p.el.style.top = p.y + '%';
            });

            document.getElementById('count-a').innerText = NUM_PARTICLES - bCount;
            document.getElementById('count-b').innerText = bCount;
            document.getElementById('equil-pointer').style.left = ((bCount / NUM_PARTICLES) * 100) + '%';

            requestAnimationFrame(runSim);
        }

        // ================= NAVIGATION =================
        function switchView(view) {
            const btnGame = document.getElementById('btn-game'), btnSim = document.getElementById('btn-sim');
            const viewGame = document.getElementById('view-game'), viewSim = document.getElementById('view-sim');

            if(view === 'game') {
                viewGame.classList.remove('hidden'); viewSim.classList.add('hidden');
                btnGame.classList.add('tab-active', 'border-blue-500'); btnGame.classList.remove('text-slate-400', 'border-transparent');
                btnSim.classList.remove('tab-active', 'border-blue-500'); btnSim.classList.add('text-slate-400', 'border-transparent');
                simRunning = false;
                if(!gameActive) playIntroSequence(); 
            } else {
                viewGame.classList.add('hidden'); viewSim.classList.remove('hidden');
                btnSim.classList.add('tab-active', 'border-blue-500'); btnSim.classList.remove('text-slate-400', 'border-transparent');
                btnGame.classList.remove('tab-active', 'border-blue-500'); btnGame.classList.add('text-slate-400', 'border-transparent');
                gameActive = false; simRunning = true;
                initSim(); runSim();
            }
        }

        // ================= CHARACTER SELECTION =================
        let selectedCharacter = null;
        
        function startGameFromSelect(char) {
            selectedCharacter = char;
            const startScreen = document.getElementById('start-screen');
            
            // Fade out the start screen smoothly
            startScreen.classList.add('opacity-0');
            
            // Wait for CSS transition, then hide and begin intro sequence
            setTimeout(() => {
                startScreen.classList.add('hidden');
                playIntroSequence();
            }, 700);
        }

        // Start game with intro on load (Changed to wait for character select)
        window.onload = () => {
            // The game will wait for startGameFromSelect() to be called.
        };
    </script>
</body>
</html>
