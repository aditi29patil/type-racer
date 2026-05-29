[typeracing.html](https://github.com/user-attachments/files/28378739/typeracing.html)
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Git-Type // Cyber Breach</title>
    <style>
        :root {
            --bg-color: #0a0e14;
            --terminal-bg: #101520;
            --primary: #00ff66;
            --accent: #00e5ff;
            --warning: #ff0055;
            --text-dim: #708090;
            --font-mono: 'Courier New', Courier, monospace;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            background-color: var(--bg-color);
            color: #ffffff;
            font-family: var(--font-mono);
            height: 100vh;
            overflow: hidden;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 20px;
        }

        /* Scanline Overlay Effect */
        body::before {
            content: " ";
            display: block;
            position: absolute;
            top: 0; left: 0; bottom: 0; right: 0;
            background: linear-gradient(rgba(18, 16, 16, 0) 50%, rgba(0, 0, 0, 0.25) 50%), linear-gradient(90deg, rgba(255, 0, 0, 0.06), rgba(0, 255, 0, 0.02), rgba(0, 0, 255, 0.06));
            z-index: 10;
            background-size: 100% 4px, 6px 100%;
            pointer-events: none;
        }

        /* Header & Stats */
        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px 20px;
            background: var(--terminal-bg);
            border: 1px solid #1f293d;
            border-bottom: 3px solid var(--primary);
            box-shadow: 0 0 15px rgba(0, 255, 102, 0.1);
        }

        .logo {
            font-weight: bold;
            font-size: 1.2rem;
            color: var(--primary);
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .stats-container {
            display: flex;
            gap: 30px;
        }

        .stat-box {
            font-size: 1rem;
        }
        .stat-box span {
            color: var(--accent);
            font-weight: bold;
        }

        /* Main Game Arena */
        #game-arena {
            position: relative;
            flex-grow: 1;
            margin: 20px 0;
            background: var(--terminal-bg);
            border: 1px solid #1f293d;
            border-radius: 4px;
            overflow: hidden;
        }

        .falling-block {
            position: absolute;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(16, 21, 32, 0.85);
            border-left: 4px solid var(--accent);
            padding: 15px;
            border-radius: 0 4px 4px 0;
            width: 80%;
            max-width: 600px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
            transition: border-color 0.2s;
        }

        .falling-block.danger {
            border-left-color: var(--warning);
            animation: pulse-border 0.5s infinite alternate;
        }

        @keyframes pulse-border {
            0% { border-left-color: var(--warning); }
            100% { border-left-color: #ff80aa; box-shadow: 0 0 10px rgba(255, 0, 85, 0.3); }
        }

        .code-display {
            font-size: 1.1rem;
            white-space: pre-wrap;
            letter-spacing: 1px;
            line-height: 1.5;
            color: var(--text-dim);
        }

        /* Code Highlighting Styles */
        .char-correct {
            color: var(--primary);
            text-shadow: 0 0 5px rgba(0, 255, 102, 0.5);
        }
        .char-current {
            background: rgba(0, 229, 255, 0.3);
            color: #fff;
            border-bottom: 2px solid var(--accent);
        }

        /* Control Bar & Input */
        .control-panel {
            display: grid;
            grid-template-columns: 1fr 300px;
            gap: 20px;
            background: var(--terminal-bg);
            padding: 15px;
            border: 1px solid #1f293d;
        }

        .input-wrapper {
            position: relative;
            display: flex;
            align-items: center;
        }

        .input-wrapper::before {
            content: ">>";
            position: absolute;
            left: 15px;
            color: var(--primary);
            font-weight: bold;
        }

        #terminal-input {
            width: 100%;
            background: #05070a;
            border: 1px solid #1f293d;
            padding: 15px 15px 15px 45px;
            color: #fff;
            font-family: var(--font-mono);
            font-size: 1.1rem;
            outline: none;
            border-radius: 4px;
        }

        #terminal-input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 10px rgba(0, 255, 102, 0.2);
        }

        /* Power-ups Display */
        .powerups-inventory {
            display: flex;
            gap: 10px;
            align-items: center;
            justify-content: space-around;
            border-left: 1px solid #1f293d;
            padding-left: 20px;
        }

        .powerup-btn {
            background: #1a2333;
            border: 1px solid var(--text-dim);
            color: var(--text-dim);
            padding: 8px 12px;
            font-family: var(--font-mono);
            font-size: 0.8rem;
            cursor: pointer;
            border-radius: 4px;
            text-transform: uppercase;
            transition: all 0.3s;
            opacity: 0.5;
        }

        .powerup-btn.ready {
            opacity: 1;
            color: #fff;
        }

        .powerup-btn.ready#btn-debugger { border-color: var(--accent); box-shadow: 0 0 8px rgba(0, 229, 255, 0.3); }
        .powerup-btn.ready#btn-overflow { border-color: var(--warning); box-shadow: 0 0 8px rgba(ff, 0, 55, 0.3); }

        .powerup-btn.ready:hover {
            transform: translateY(-2px);
        }

        /* Screen States Overlay */
        .overlay {
            position: absolute;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(10, 14, 20, 0.95);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            z-index: 100;
            text-align: center;
        }

        .overlay h1 {
            font-size: 2.5rem;
            color: var(--primary);
            margin-bottom: 20px;
            text-shadow: 0 0 10px rgba(0,255,102,0.4);
        }

        .overlay p {
            color: var(--text-dim);
            margin-bottom: 30px;
            max-width: 500px;
            line-height: 1.6;
        }

        .start-btn {
            background: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
            padding: 12px 30px;
            font-family: var(--font-mono);
            font-size: 1.2rem;
            cursor: pointer;
            text-transform: uppercase;
            transition: all 0.3s;
            box-shadow: 0 0 15px rgba(0,255,102,0.2);
        }

        .start-btn:hover {
            background: var(--primary);
            color: var(--bg-color);
            box-shadow: 0 0 25px var(--primary);
        }

        .hidden { display: none !important; }
    </style>
</head>
<body>

    <header>
        <div class="logo">System_Breach // Git-Type v1.0.4</div>
        <div class="stats-container">
            <div class="stat-box">SCORE: <span id="score-val">0000</span></div>
            <div class="stat-box">SPEED: <span id="wpm-val">0</span> WPM</div>
            <div class="stat-box">BREACH DEPTH: <span id="level-val">Lvl 1</span></div>
        </div>
    </header>

    <main id="game-arena">
        <div id="start-screen" class="overlay">
            <h1>INITIALIZE SYS_ATTACK?</h1>
            <p>Type falling system payloads to compile them and breach the core firewall. Fail to execute before impact, and security systems trace your position.</p>
            <p style="font-size: 0.9rem; color: var(--accent);">Hotkeys: Press [Space] to activate Debugger (Slow-Mo). Press [Enter] on empty input to fire Stack Overflow blast.</p>
            <button class="start-btn" onclick="startGame()">Execute Hack</button>
        </div>

        <div id="gameover-screen" class="overlay hidden">
            <h1 style="color: var(--warning); text-shadow: 0 0 10px rgba(255,0,85,0.4);">TRACE DETECTED: SYSTEM LOCKED</h1>
            <p>Your hardware signature was compromised. Firewall layers breached successfully: <span id="final-lvl" style="color:#fff;">0</span></p>
            <p>Net Final Compilation Score: <span id="final-score" style="color: var(--primary);">0</span></p>
            <button class="start-btn" style="border-color: var(--warning); color: var(--warning);" onclick="startGame()">Re-route IP & Retry</button>
        </div>

        <div id="active-block" class="falling-block hidden">
            <div class="code-display" id="code-target"></div>
        </div>
    </main>

    <div class="control-panel">
        <div class="input-wrapper">
            <input type="text" id="terminal-input" placeholder="Awaiting payload execution string..." disabled autocomplete="off">
        </div>
        <div class="powerups-inventory">
            <button class="powerup-btn" id="btn-debugger" title="Slows time down for 5 seconds">SPACE: De-bugger [0]</button>
            <button class="powerup-btn" id="btn-overflow" title="Clears current block instantly">ENTER: Stack_Overflow [0]</button>
        </div>
    </div>

    <script>
        // Database of real syntax snippets
        const snippets = [
            "const target = document.querySelector('#core');",
            "for (let i = 0; i < system.length; i++) { bypass(i); }",
            "import os\nif os.path.exists('/etc/firewall'): disable()",
            "fetch('/api/breach', { method: 'POST', body: payload });",
            "sys.stdout.write('Infiltrating structural grid...\\n')",
            "int* ptr = malloc(sizeof(firewall_data));",
            "while (!breached) { bypass_node(network->head); }",
            "std::cout << \"Access Granted. Core unlocked.\" << std::endl;",
            "useEffect(() => { runInfiltrationRoutine(); }, []);",
            "server.listen(8080, () => { console.log('Spyware online'); });"
        ];

        // Game state variables
        let score = 0;
        let level = 1;
        let currentWpm = 0;
        let isGameRunning = false;
        
        let posY = 0;
        let baseSpeed = 0.8; // Falling speed px per frame
        let currentSpeed = 0.8;
        let gameLoopId = null;

        let currentString = "";
        let typedIndex = 0;
        let totalCharsTyped = 0;
        let startTime = null;

        // Power-ups inventory metrics
        let powerups = {
            debugger: 1, // Max 1 saved
            overflow: 0  // Earned every 3 correct completions
        };
        let completionStreak = 0;
        let isSlowMo = false;

        // DOM elements cache
        const domArena = document.getElementById('game-arena');
        const domActiveBlock = document.getElementById('active-block');
        const domCodeTarget = document.getElementById('code-target');
        const domInput = document.getElementById('terminal-input');
        const domScore = document.getElementById('score-val');
        const domWpm = document.getElementById('wpm-val');
        const domLevel = document.getElementById('level-val');
        
        const btnDebugger = document.getElementById('btn-debugger');
        const btnOverflow = document.getElementById('btn-overflow');

        // Main Initiator Setup
        function startGame() {
            // Screen resets
            document.getElementById('start-screen').classList.add('hidden');
            document.getElementById('gameover-screen').classList.add('hidden');
            
            // State defaults
            score = 0;
            level = 1;
            completionStreak = 0;
            powerups.debugger = 1;
            powerups.overflow = 0;
            totalCharsTyped = 0;
            isSlowMo = false;
            baseSpeed = 0.8;
            currentSpeed = baseSpeed;
            
            updateDashboard();
            
            isGameRunning = true;
            domInput.disabled = false;
            domInput.value = "";
            domInput.focus();
            
            startTime = Date.now();
            spawnNewSnippet();
            
            // Start tick execution loop
            if (gameLoopId) cancelAnimationFrame(gameLoopId);
            gameLoopId = requestAnimationFrame(updateEngine);
        }

        function spawnNewSnippet() {
            // Select random text template
            const randomIndex = Math.floor(Math.random() * snippets.length);
            currentString = snippets[randomIndex];
            typedIndex = 0;
            domInput.value = "";
            
            // Build visual representation spans for strict comparison highlighting
            renderHighlightedCode();

            // Set geometry position to structural origin (top of arena layout container)
            posY = 0;
            domActiveBlock.style.top = posY + "px";
            domActiveBlock.classList.remove('hidden', 'danger');
        }

        function renderHighlightedCode() {
            let htmlOutput = "";
            for (let i = 0; i < currentString.length; i++) {
                if (i < typedIndex) {
                    htmlOutput += `<span class="char-correct">${escapeHtml(currentString[i])}</span>`;
                } else if (i === typedIndex) {
                    htmlOutput += `<span class="char-current">${escapeHtml(currentString[i])}</span>`;
                } else {
                    htmlOutput += `<span>${escapeHtml(currentString[i])}</span>`;
                }
            }
            domCodeTarget.innerHTML = htmlOutput;
        }

        function escapeHtml(char) {
            if(char === " ") return "&nbsp;";
            if(char === "\n") return "<br>";
            if(char === "<") return "&lt;";
            if(char === ">") return "&gt;";
            return char;
        }

        // Real-Time Engine Physics Update Loop
        function updateEngine() {
            if (!isGameRunning) return;

            // Advance positional coordinates based on frame refresh vectors
            posY += currentSpeed;
            domActiveBlock.style.top = posY + "px";

            // Threshold indicators (Warning adjustments if dropping dangerously near base floor)
            const maximumLimit = domArena.clientHeight - domActiveBlock.clientHeight;
            if (posY > maximumLimit * 0.7) {
                domActiveBlock.classList.add('danger');
            }

            // Failure Condition Intersection Check
            if (posY >= maximumLimit) {
                triggerGameOver();
                return;
            }

            // Realtime running calculation metrics
            calculateWpm();

            gameLoopId = requestAnimationFrame(updateEngine);
        }

        // Input Parser Mechanics Engine
        domInput.addEventListener('input', (e) => {
            if (!isGameRunning) return;
            
            const rawInputValue = domInput.value;
            // Determine expected sequence slice matching input length
            const expectedSegment = currentString.substring(0, rawInputValue.length);

            if (rawInputValue === expectedSegment) {
                // Splicing accurate matching state
                typedIndex = rawInputValue.length;
                totalCharsTyped++;
                renderHighlightedCode();

                // Core execution condition match validation
                if (typedIndex === currentString.length) {
                    handlePayloadCompiled();
                }
            } else {
                // Reject extra characters by trimming input field back to current structural accuracy boundary
                domInput.value = currentString.substring(0, typedIndex);
            }
        });

        // Intercept hotkey events (Space and Enter overrides)
        domInput.addEventListener('keydown', (e) => {
            if (!isGameRunning) return;

            // Trigger Space powerup conditions (Debugger Slow-Mo)
            if (e.key === ' ' && domInput.value === "" && powerups.debugger > 0) {
                e.preventDefault();
                activateDebuggerPowerup();
            }

            // Trigger Enter powerup conditions (Stack Overflow wipe)
            if (e.key === 'Enter' && domInput.value === "" && powerups.overflow > 0) {
                e.preventDefault();
                activateStackOverflowPowerup();
            }
        });

        function handlePayloadCompiled() {
            // Increments and state scaling mechanics logic
            score += currentString.length * 10 * level;
            completionStreak++;
            
            // Logic rules mechanics: earn Stack Overflow every 3 lines matching
            if (completionStreak % 3 === 0) {
                powerups.overflow = Math.min(powerups.overflow + 1, 3);
            }

            // Dynamic Level Scaling Calculations
            if (score > level * 1200) {
                level++;
                baseSpeed += 0.25; // Increase base linear speed parameters
                if (!isSlowMo) currentSpeed = baseSpeed;
            }

            updateDashboard();
            spawnNewSnippet();
        }

        // Power-up Execution Models
        function activateDebuggerPowerup() {
            powerups.debugger--;
            isSlowMo = true;
            currentSpeed = baseSpeed * 0.3; // Reduce speed vector value by 70%
            domActiveBlock.style.borderColor = "var(--accent)";
            updateDashboard();

            setTimeout(() => {
                isSlowMo = false;
                currentSpeed = baseSpeed;
                updateDashboard();
            }, 5000); // 5 second temporal mitigation limit
        }

        function activateStackOverflowPowerup() {
            powerups.overflow--;
            updateDashboard();
            // Instantly compile payload bypass operations
            handlePayloadCompiled();
        }

        function calculateWpm() {
            const timeElapsedMinutes = (Date.now() - startTime) / 60000;
            if (timeElapsedMinutes > 0 && totalCharsTyped > 0) {
                // Standard metric rule configuration: 1 word token representation = 5 characters completed
                currentWpm = Math.round((totalCharsTyped / 5) / timeElapsedMinutes);
                domWpm.innerText = currentWpm;
            }
        }

        function updateDashboard() {
            // Synchronize application tracking text layers
            domScore.innerText = String(score).padStart(4, '0');
            domLevel.innerText = `Lvl ${level}`;
            
            // Adjust powerups button styling status reflections
            btnDebugger.innerText = `SPACE: De-bugger [${powerups.debugger}]`;
            btnDebugger.className = `powerup-btn ${powerups.debugger > 0 ? 'ready' : ''}`;
            
            btnOverflow.innerText = `ENTER: Stack_Overflow [${powerups.overflow}]`;
            btnOverflow.className = `powerup-btn ${powerups.overflow > 0 ? 'ready' : ''}`;
        }

        function triggerGameOver() {
            isGameRunning = false;
            domInput.disabled = true;
            domActiveBlock.classList.add('hidden');
            cancelAnimationFrame(gameLoopId);

            document.getElementById('final-lvl').innerText = level;
            document.getElementById('final-score').innerText = score;
            document.getElementById('gameover-screen').classList.remove('hidden');
        }
    </script>
</body>
</html>
