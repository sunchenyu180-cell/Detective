<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Pattern Recognition Detective – Self-Driving Car School-Zone</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: "Segoe UI", "Roboto", "Helvetica Neue", sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            min-height: 100vh;
            color: #e2e8f0;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 2rem 1rem;
        }
        h1 {
            font-size: 2.2rem;
            margin-bottom: 0.3rem;
            color: #fbbf24;
            text-shadow: 0 2px 10px rgba(251,191,36,0.3);
            text-align: center;
        }
        .subtitle {
            color: #94a3b8;
            margin-bottom: 2rem;
            font-size: 1.1rem;
            text-align: center;
        }
        .section-title {
            width: 100%;
            max-width: 1000px;
            color: #f1f5f9;
            font-size: 1.3rem;
            margin: 1.5rem 0 0.8rem;
            padding-left: 0.5rem;
            border-left: 4px solid #fbbf24;
        }
        .color-palette {
            display: flex;
            gap: 0.8rem;
            margin-bottom: 1rem;
            flex-wrap: wrap;
            justify-content: center;
        }
        .color-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            border: 3px solid transparent;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 2px 8px rgba(0,0,0,0.5);
        }
        .color-btn.active {
            border-color: white;
            transform: scale(1.15);
            box-shadow: 0 0 15px currentColor;
        }
        .clear-mark-btn {
            background: #475569;
            color: #e2e8f0;
            border: none;
            padding: 0.5rem 1.2rem;
            border-radius: 2rem;
            cursor: pointer;
            font-weight: bold;
            transition: 0.2s;
        }
        .clear-mark-btn:hover {
            background: #64748b;
        }
        .table-container {
            width: 100%;
            max-width: 1000px;
            max-height: 400px;
            overflow-y: auto;
            background: #1e293b;
            border-radius: 1rem;
            border: 1px solid #334155;
            margin-bottom: 1rem;
        }
        table {
            width: 100%;
            border-collapse: collapse;
        }
        th {
            background: #0f172a;
            padding: 0.8rem;
            text-align: left;
            color: #fbbf24;
            font-weight: bold;
            position: sticky;
            top: 0;
            z-index: 2;
        }
        td {
            padding: 0.7rem 0.8rem;
            border-bottom: 1px solid #334155;
            cursor: pointer;
            transition: background 0.2s;
        }
        tr:hover td {
            background: #33415555;
        }
        .action-buttons {
            display: flex;
            gap: 1rem;
            margin: 1.5rem 0;
            flex-wrap: wrap;
            justify-content: center;
        }
        .btn {
            background: #fbbf24;
            color: #0f172a;
            font-weight: bold;
            border: none;
            padding: 0.8rem 2rem;
            border-radius: 2rem;
            font-size: 1rem;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 4px 10px rgba(251,191,36,0.3);
        }
        .btn:hover {
            background: #f59e0b;
            transform: translateY(-1px);
            box-shadow: 0 6px 15px rgba(251,191,36,0.5);
        }
        .btn:disabled {
            background: #475569;
            color: #94a3b8;
            cursor: not-allowed;
            box-shadow: none;
        }
        .feedback-msg {
            background: #334155;
            border-radius: 1rem;
            padding: 1rem 1.5rem;
            color: #f1f5f9;
            max-width: 700px;
            text-align: center;
            margin: 1rem 0;
            border-left: 4px solid #fbbf24;
            display: none;
        }
        /* Pattern reveal area */
        .pattern-reveal {
            width: 100%;
            max-width: 1000px;
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            justify-content: center;
            margin: 1.5rem 0;
        }
        .pattern-card {
            background: #1e293b;
            border: 1px solid #334155;
            border-radius: 1rem;
            padding: 1rem;
            width: 220px;
            box-shadow: 0 4px 10px rgba(0,0,0,0.4);
        }
        .pattern-card h4 {
            color: #fbbf24;
            margin-bottom: 0.5rem;
        }
        /* Drag & drop matching */
        .matching-area {
            width: 100%;
            max-width: 900px;
            display: flex;
            gap: 2rem;
            margin: 1.5rem 0;
            flex-wrap: wrap;
            justify-content: center;
        }
        .name-pool, .desc-targets {
            background: #0f172a;
            border-radius: 1.2rem;
            padding: 1.2rem;
            border: 1px solid #334155;
            display: flex;
            flex-direction: column;
            gap: 0.8rem;
            min-width: 250px;
            flex: 1;
        }
        .draggable-name {
            background: #2563eb;
            color: white;
            padding: 0.7rem 1rem;
            border-radius: 0.8rem;
            cursor: grab;
            text-align: center;
            font-weight: bold;
            user-select: none;
            transition: 0.1s;
        }
        .draggable-name:active {
            cursor: grabbing;
            opacity: 0.8;
        }
        .draggable-name.dragging {
            opacity: 0.4;
        }
        .desc-drop {
            background: #1e293b;
            border: 2px dashed #475569;
            border-radius: 1rem;
            padding: 0.8rem;
            min-height: 70px;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            color: #94a3b8;
            transition: 0.2s;
        }
        .desc-drop.drag-over {
            border-color: #fbbf24;
            background: #2c3b4f;
        }
        .desc-drop.filled {
            border-style: solid;
            border-color: #22c55e;
            color: white;
            font-weight: bold;
        }
        .quiz-area {
            width: 100%;
            max-width: 600px;
            background: #0f172a;
            border-radius: 1.2rem;
            padding: 1.5rem;
            border: 1px solid #334155;
            margin-top: 1.5rem;
        }
        .quiz-option {
            background: #1e293b;
            border: 1px solid #475569;
            border-radius: 0.8rem;
            padding: 0.8rem 1rem;
            margin: 0.5rem 0;
            cursor: pointer;
            transition: 0.2s;
        }
        .quiz-option:hover {
            background: #334155;
        }
        .quiz-option.correct {
            background: #14532d;
            border-color: #22c55e;
        }
        .quiz-option.wrong {
            background: #5c1a1a;
            border-color: #ef4444;
        }
    </style>
</head>
<body>
    <h1>🕵️ Pattern Recognition Detective</h1>
    <p class="subtitle">Goal: Spot recurring danger patterns in school-zone data</p>

    <div class="section-title">📊 Morning School-Zone Event Table (12 records)</div>
    <p style="color:#94a3b8; max-width:800px; text-align:center; margin-bottom:0.5rem;">
        Select a colour and click table rows to mark patterns you discover. Then click "Reveal Patterns" to check and learn the four hidden danger templates.
    </p>

    <div class="color-palette">
        <span style="color:#e2e8f0; margin-right:0.5rem; display:flex; align-items:center;">Mark colour:</span>
        <button class="color-btn active" data-color="#ef4444" style="background:#ef4444;" title="Pattern 1"></button>
        <button class="color-btn" data-color="#3b82f6" style="background:#3b82f6;" title="Pattern 2"></button>
        <button class="color-btn" data-color="#22c55e" style="background:#22c55e;" title="Pattern 3"></button>
        <button class="color-btn" data-color="#f97316" style="background:#f97316;" title="Pattern 4"></button>
        <button class="clear-mark-btn" id="clearMarksBtn">Clear marks</button>
    </div>

    <div class="table-container">
        <table id="eventTable">
            <thead>
                <tr>
                    <th>Time</th>
                    <th>Vehicle Behaviour</th>
                    <th>Child Action</th>
                    <th>Outcome</th>
                </tr>
            </thead>
            <tbody></tbody>
        </table>
    </div>

    <div class="action-buttons">
        <button class="btn" id="revealBtn">🔍 Reveal Patterns</button>
        <button class="btn" id="resetTableBtn" style="background:#475569; color:#e2e8f0;">↺ Reset Table</button>
    </div>

    <div id="tableFeedback" class="feedback-msg"></div>

    <!-- Pattern reveal & matching (hidden initially) -->
    <div id="patternsSection" style="display:none; width:100%; display:flex; flex-direction:column; align-items:center;">
        <div class="section-title">🧩 The Four Hidden Patterns</div>
        <div class="pattern-reveal" id="patternCardsContainer"></div>

        <div class="section-title">🔗 Match the Pattern to Its Description</div>
        <p style="color:#94a3b8; margin-bottom:0.5rem;">Drag each pattern name onto the correct description card.</p>
        <div class="matching-area">
            <div class="name-pool" id="namePool">
                <h4 style="color:#cbd5e1; margin-bottom:0.5rem;">Pattern Names</h4>
            </div>
            <div class="desc-targets" id="descTargets">
                <h4 style="color:#cbd5e1; margin-bottom:0.5rem;">Descriptions</h4>
            </div>
        </div>
        <div class="action-buttons">
            <button class="btn" id="checkMatchBtn">✅ Check Matching</button>
        </div>
        <div id="matchFeedback" class="feedback-msg"></div>

        <!-- Quick quiz (hidden until matching correct) -->
        <div class="quiz-area" id="quizArea" style="display:none;">
            <h3 style="color:#fbbf24; margin-bottom:1rem;">⚡ Quick Quiz</h3>
            <p style="margin-bottom:0.8rem;">A student suddenly turns back to grab their school bag — which pattern tells the car to stay still and keep waiting?</p>
            <div class="quiz-option" data-answer="wrong">Perception + Verify</div>
            <div class="quiz-option" data-answer="wrong">Spatial Safety Redundancy</div>
            <div class="quiz-option" data-answer="wrong">Intent Externalisation</div>
            <div class="quiz-option" data-answer="correct">Dynamic Time Assessment</div>
            <div id="quizFeedback" style="margin-top:0.8rem; font-weight:bold;"></div>
        </div>
    </div>

    <script>
        (function() {
            // ---- Data ----
            const patternNames = [
                "Perception + Verify",
                "Spatial Safety Redundancy",
                "Intent Externalisation",
                "Dynamic Time Assessment"
            ];
            const patternColors = ["#ef4444", "#3b82f6", "#22c55e", "#f97316"]; // red, blue, green, orange

            const patternDescriptions = [
                "The system cross-checks a detection with another sensor before braking.",
                "The car maps out a safe buffer zone around the open door before planning a path.",
                "The vehicle flashes its hazard lights and makes a soft chime to alert nearby pedestrians.",
                "The waiting timer adjusts based on whether the student is still near the trunk or already on the sidewalk."
            ];

            const tableData = [
                { time: "07:25", car: "Lead car brakes suddenly", child: "Backpack falls out", outcome: "🚗⚠️ System slows & re-scans", pattern: 0 },
                { time: "07:26", car: "Door opens slowly", child: "Child prepares to exit", outcome: "✅ System holds, monitors", pattern: 3 },
                { time: "07:27", car: "Trunk pops open", child: "Child walks to rear", outcome: "⚠️ Hazards flash, alert sent", pattern: 2 },
                { time: "07:28", car: "Brake lights flicker", child: "Child runs back to car", outcome: "🚗⚠️ Brakes lock, tracking", pattern: 0 },
                { time: "07:29", car: "Motorcycle passes on right", child: "Child runs to sidewalk", outcome: "✅ Car calculates side gap, creeps", pattern: 1 },
                { time: "07:30", car: "Lead car hazards on", child: "Child stands at lane edge", outcome: "⚠️ Virtual no-go zone set, wait", pattern: 1 },
                { time: "07:31", car: "Lead car releases brake", child: "Child safely on sidewalk", outcome: "✅ Car resumes, ends wait", pattern: 3 },
                { time: "07:32", car: "Door fully open", child: "Child pulls out bag", outcome: "✅ Path planned around door", pattern: 1 },
                { time: "07:33", car: "Lead car accelerates suddenly", child: "Child turns and runs back", outcome: "🚗⚠️ Emergency brake", pattern: 0 },
                { time: "07:34", car: "No abnormal signal", child: "Child waves at car", outcome: "⚠️ Car honks softly", pattern: 2 },
                { time: "07:35", car: "Door closes", child: "Child leaves danger zone", outcome: "✅ Normal driving resumed", pattern: 3 },
                { time: "07:36", car: "Fog lights turn on", child: "Child crouches suddenly", outcome: "🚗⚠️ Hard stop initiated", pattern: 0 }
            ];

            // Student state
            let studentHighlights = new Array(12).fill(null); // stores color string or null
            let currentColor = patternColors[0]; // default red
            let patternsRevealed = false;

            // DnD matching
            let draggedItem = null;
            let matchAssignments = [null, null, null, null]; // for each description slot, which pattern name index?

            // DOM elements
            const tbody = document.querySelector('#eventTable tbody');
            const colorBtns = document.querySelectorAll('.color-btn');
            const clearMarksBtn = document.getElementById('clearMarksBtn');
            const revealBtn = document.getElementById('revealBtn');
            const resetTableBtn = document.getElementById('resetTableBtn');
            const tableFeedback = document.getElementById('tableFeedback');
            const patternsSection = document.getElementById('patternsSection');
            const patternCardsContainer = document.getElementById('patternCardsContainer');
            const namePool = document.getElementById('namePool');
            const descTargets = document.getElementById('descTargets');
            const checkMatchBtn = document.getElementById('checkMatchBtn');
            const matchFeedback = document.getElementById('matchFeedback');
            const quizArea = document.getElementById('quizArea');
            const quizFeedback = document.getElementById('quizFeedback');
            const quizOptions = document.querySelectorAll('.quiz-option');

            // ---- Render table ----
            function renderTable() {
                tbody.innerHTML = '';
                tableData.forEach((row, idx) => {
                    const tr = document.createElement('tr');
                    tr.dataset.index = idx;
                    tr.innerHTML = `
                        <td>${row.time}</td>
                        <td>${row.car}</td>
                        <td>${row.child}</td>
                        <td>${row.outcome}</td>
                    `;
                    if (studentHighlights[idx]) {
                        tr.style.backgroundColor = studentHighlights[idx] + '55';
                    }
                    tr.addEventListener('click', () => onRowClick(idx, tr));
                    tbody.appendChild(tr);
                });
            }

            function onRowClick(idx, tr) {
                if (patternsRevealed) {
                    // After reveal, clicking does nothing (or could clear, but we disable)
                    return;
                }
                // Toggle: if already has this current colour, clear it; else set to current colour
                if (studentHighlights[idx] === currentColor) {
                    studentHighlights[idx] = null;
                    tr.style.backgroundColor = '';
                } else {
                    studentHighlights[idx] = currentColor;
                    tr.style.backgroundColor = currentColor + '55';
                }
            }

            // Colour selection
            colorBtns.forEach(btn => {
                btn.addEventListener('click', () => {
                    colorBtns.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');
                    currentColor = btn.dataset.color;
                });
            });

            clearMarksBtn.addEventListener('click', () => {
                if (patternsRevealed) return;
                studentHighlights.fill(null);
                renderTable();
            });

            // ---- Reveal patterns ----
            revealBtn.addEventListener('click', () => {
                if (patternsRevealed) return;
                // Show correct highlights regardless of student's marks (overwrite for clarity)
                studentHighlights = tableData.map(row => patternColors[row.pattern]);
                renderTable();
                patternsRevealed = true;

                // Display pattern cards
                patternCardsContainer.innerHTML = '';
                patternNames.forEach((name, i) => {
                    const card = document.createElement('div');
                    card.className = 'pattern-card';
                    card.style.borderLeft = `4px solid ${patternColors[i]}`;
                    card.innerHTML = `<h4 style="color:${patternColors[i]}">${name}</h4><p style="font-size:0.9rem; color:#cbd5e1;">${patternDescriptions[i]}</p>`;
                    patternCardsContainer.appendChild(card);
                });

                // Show matching section
                patternsSection.style.display = 'flex';
                patternsSection.style.flexDirection = 'column';
                patternsSection.style.alignItems = 'center';
                patternsSection.style.width = '100%';
                setupMatching();
                tableFeedback.style.display = 'block';
                tableFeedback.innerHTML = '✅ Patterns revealed! The table rows are now colour-coded by pattern. Now match each pattern name to its description below.';
            });

            function setupMatching() {
                // Shuffle descriptions order for the targets
                const descIndices = [0, 1, 2, 3];
                // Simple random shuffle
                for (let i = descIndices.length - 1; i > 0; i--) {
                    const j = Math.floor(Math.random() * (i + 1));
                    [descIndices[i], descIndices[j]] = [descIndices[j], descIndices[i]];
                }
                // Render name pool (draggable)
                namePool.innerHTML = '<h4 style="color:#cbd5e1; margin-bottom:0.5rem;">Pattern Names</h4>';
                patternNames.forEach((name, idx) => {
                    const div = document.createElement('div');
                    div.className = 'draggable-name';
                    div.textContent = name;
                    div.draggable = true;
                    div.dataset.patternIndex = idx;
                    div.addEventListener('dragstart', handleDragStart);
                    div.addEventListener('dragend', handleDragEnd);
                    namePool.appendChild(div);
                });

                // Render description targets
                descTargets.innerHTML = '<h4 style="color:#cbd5e1; margin-bottom:0.5rem;">Descriptions</h4>';
                descIndices.forEach(origIdx => {
                    const drop = document.createElement('div');
                    drop.className = 'desc-drop';
                    drop.dataset.correctPattern = origIdx; // which pattern name belongs here
                    drop.textContent = patternDescriptions[origIdx];
                    drop.addEventListener('dragover', handleDragOver);
                    drop.addEventListener('dragleave', handleDragLeave);
                    drop.addEventListener('drop', handleDrop);
                    // Also allow click to remove? we'll implement double-click to clear
                    drop.addEventListener('dblclick', () => clearDrop(drop));
                    descTargets.appendChild(drop);
                });

                matchAssignments = [null, null, null, null];
                quizArea.style.display = 'none';
                matchFeedback.style.display = 'none';
            }

            // Drag and drop for matching
            function handleDragStart(e) {
                draggedItem = { patternIndex: parseInt(e.target.dataset.patternIndex), element: e.target };
                e.target.classList.add('dragging');
                e.dataTransfer.setData('text/plain', draggedItem.patternIndex);
                e.dataTransfer.effectAllowed = 'move';
            }

            function handleDragEnd(e) {
                e.target.classList.remove('dragging');
                draggedItem = null;
                document.querySelectorAll('.desc-drop').forEach(d => d.classList.remove('drag-over'));
            }

            function handleDragOver(e) {
                e.preventDefault();
                e.dataTransfer.dropEffect = 'move';
                e.currentTarget.classList.add('drag-over');
            }

            function handleDragLeave(e) {
                e.currentTarget.classList.remove('drag-over');
            }

            function handleDrop(e) {
                e.preventDefault();
                e.currentTarget.classList.remove('drag-over');
                const dropZone = e.currentTarget;
                const patternIdx = parseInt(e.dataTransfer.getData('text/plain'));
                if (isNaN(patternIdx)) return;

                // Remove previous assignment if this drop zone already had a name
                const prevAssigned = dropZone.dataset.assignedPattern;
                if (prevAssigned !== undefined) {
                    // Return that name to pool (it will be re-rendered)
                    // We'll just update matchAssignments and re-render
                    const prevIdx = parseInt(prevAssigned);
                    // No need to physically move, we rebuild pool
                }

                // Assign new pattern to this drop zone
                dropZone.dataset.assignedPattern = patternIdx;
                dropZone.textContent = patternNames[patternIdx];
                dropZone.classList.add('filled');
                // Remove the name from pool (by rebuilding)
                rebuildNamePool();
            }

            function clearDrop(dropZone) {
                if (dropZone.dataset.assignedPattern !== undefined) {
                    dropZone.removeAttribute('data-assigned-pattern');
                    dropZone.textContent = patternDescriptions[parseInt(dropZone.dataset.correctPattern)];
                    dropZone.classList.remove('filled');
                    rebuildNamePool();
                }
            }

            function rebuildNamePool() {
                // Collect assigned pattern indices
                const assignedSet = new Set();
                document.querySelectorAll('.desc-drop').forEach(d => {
                    if (d.dataset.assignedPattern !== undefined) {
                        assignedSet.add(parseInt(d.dataset.assignedPattern));
                    }
                });
                // Rebuild pool with unassigned names
                namePool.innerHTML = '<h4 style="color:#cbd5e1; margin-bottom:0.5rem;">Pattern Names</h4>';
                patternNames.forEach((name, idx) => {
                    if (!assignedSet.has(idx)) {
                        const div = document.createElement('div');
                        div.className = 'draggable-name';
                        div.textContent = name;
                        div.draggable = true;
                        div.dataset.patternIndex = idx;
                        div.addEventListener('dragstart', handleDragStart);
                        div.addEventListener('dragend', handleDragEnd);
                        namePool.appendChild(div);
                    }
                });
            }

            // Check matching
            checkMatchBtn.addEventListener('click', () => {
                const drops = document.querySelectorAll('.desc-drop');
                let allCorrect = true;
                drops.forEach(drop => {
                    const correct = parseInt(drop.dataset.correctPattern);
                    const assigned = drop.dataset.assignedPattern ? parseInt(drop.dataset.assignedPattern) : null;
                    if (assigned !== correct) {
                        allCorrect = false;
                        drop.style.borderColor = '#ef4444';
                    } else {
                        drop.style.borderColor = '#22c55e';
                    }
                });
                matchFeedback.style.display = 'block';
                if (allCorrect) {
                    matchFeedback.innerHTML = '🎉 Perfect! All patterns matched correctly. You have a sharp detective eye!';
                    matchFeedback.style.borderLeftColor = '#22c55e';
                    // Show quiz
                    quizArea.style.display = 'block';
                } else {
                    matchFeedback.innerHTML = '⚠️ Some matches are not right. Check the ones with red borders and try again.';
                    matchFeedback.style.borderLeftColor = '#ef4444';
                }
            });

            // Quiz interaction
            quizOptions.forEach(opt => {
                opt.addEventListener('click', () => {
                    if (quizFeedback.textContent !== '') return; // already answered
                    const isCorrect = opt.dataset.answer === 'correct';
                    quizOptions.forEach(o => o.classList.remove('correct', 'wrong'));
                    if (isCorrect) {
                        opt.classList.add('correct');
                        quizFeedback.textContent = '✅ Correct! Dynamic Time Assessment adjusts waiting based on the child\'s behaviour.';
                        quizFeedback.style.color = '#22c55e';
                    } else {
                        opt.classList.add('wrong');
                        quizFeedback.textContent = '❌ Not quite. Think about which pattern cares about how long the car waits depending on what the child does.';
                        quizFeedback.style.color = '#ef4444';
                        // Allow retry
                        setTimeout(() => {
                            quizFeedback.textContent = '';
                            quizOptions.forEach(o => o.classList.remove('wrong'));
                        }, 1500);
                    }
                });
            });

            // Reset table (clear highlights, reset all)
            resetTableBtn.addEventListener('click', () => {
                studentHighlights.fill(null);
                patternsRevealed = false;
                renderTable();
                patternsSection.style.display = 'none';
                tableFeedback.style.display = 'none';
                matchFeedback.style.display = 'none';
                quizArea.style.display = 'none';
                quizFeedback.textContent = '';
                quizOptions.forEach(o => o.classList.remove('correct', 'wrong'));
            });

            // Initial render
            renderTable();
        })();
    </script>
</body>
</html>
