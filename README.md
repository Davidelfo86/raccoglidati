!DOCTYPE html>
<html data-theme="light">
<head>
    <title>Corse Cavalli</title>
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <!-- Aggiungiamo Tesseract.js per OCR -->
    <script src='https://unpkg.com/tesseract.js@v2.1.0/dist/tesseract.min.js'></script>
    <style>
        /* Variabili tema chiaro/scuro */
        :root[data-theme="light"] {
            --bg-color: #ffffff;
            --text-color: #333333;
            --border-color: #dddddd;
            --header-bg: #f8f9fa;
            --button-primary: #4CAF50;
            --button-secondary: #2196F3;
            --alert-warning: #f44336;
            --alert-info: #2196F3;
        }

        :root[data-theme="dark"] {
            --bg-color: #1a1a1a;
            --text-color: #ffffff;
            --border-color: #444444;
            --header-bg: #2d2d2d;
            --button-primary: #45a049;
            --button-secondary: #1976d2;
            --alert-warning: #d32f2f;
            --alert-info: #1976d2;
        }

        body {
            font-family: Arial, sans-serif;
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
            background-color: var(--bg-color);
            color: var(--text-color);
            transition: all 0.3s ease;
        }

        /* Header e controlli */
        .header-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .controls-container {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        /* Loader per OCR */
        .loading {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0a(0,0,0,0.8);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            font-size: 20px;
            z-index: 1000;
        }

        .loading-spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid #3498db;
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
            margin-bottom: 20px;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* Theme switch */
        .theme-switch {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .toggle-switch {
            position: relative;
            display: inline-block;
            width: 60px;
            height: 34px;
        }

        .toggle-switch input {
            opacity: 0;
            width: 0;
            height: 0;
        }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background-color: #ccc;
            transition: .4s;
            border-radius: 34px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 26px;
            width: 26px;
            left: 4px;
            bottom: 4px;
            background-color: white;
            transition: .4s;
            border-radius: 50%;
        }

        input:checked + .slider {
            background-color: var(--button-secondary);
        }

        input:checked + .slider:before {
            transform: translateX(26px);
        }

        /* Tabella principale */
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
            border: 1px solid var(--border-color);
        }

        th, td {
            padding: 12px;
            text-align: left;
            border: 1px solid var(--border-color);
        }

        th {
            background-color: var(--header-bg);
        }

        /* Bottoni */
        .button-container {
            display: flex;
            gap: 10px;
            margin: 20px 0;
            flex-wrap: wrap;
        }

        button {
            padding: 10px 20px;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            transition: opacity 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        button:hover {
            opacity: 0.9;
        }

        .btn-salva {
            background-color: var(--button-primary);
        }

        .btn-cerca {
            background-color: var(--button-secondary);
        }

        .btn-camera {
            background-color: var(--button-secondary);
        }

        /* Input fields */
        input, select {
            width: 90%;
            padding: 8px;
            border: 1px solid var(--border-color);
            border-radius: 4px;
            background-color: var(--bg-color);
            color: var(--text-color);
        }

        /* Alert messages */
        .alert {
            padding: 15px;
            margin: 10px 0;
            border-radius: 4px;
            color: white;
        }

        .alert-warning {
            background-color: var(--alert-warning);
        }

        .alert-info {
            background-color: var(--alert-info);
        }

        /* Responsive design */
        @media screen and (max-width: 768px) {
            body {
                padding: 10px;
                font-size: 14px;
            }

            table {
                font-size: 12px;
            }

            th, td {
                padding: 6px;
            }

            input, select {
                padding: 4px;
                font-size: 12px;
            }

            button {
                padding: 8px 16px;
                font-size: 14px;
                width: 100%;
                justify-content: center;
            }

            .header-controls {
                flex-direction: column;
            }

            .controls-container {
                width: 100%;
            }
        }
    </style>
</head>
<body>
    <div class="header-controls">
        <h1>Corse Cavalli</h1>
        <div class="controls-container">
            <div class="theme-switch">
                <span>☀️</span>
                <label class="toggle-switch">
                    <input type="checkbox" id="themeToggle">
                    <span class="slider"></span>
                </label>
                <span>🌙</span>
            </div>
        </div>
    </div>
    
    <div id="messageBox"></div>

    <!-- Loader per OCR -->
    <div id="loading" class="loading" style="display: none;">
        <div class="loading-spinner"></div>
        <div id="loadingText">Elaborazione immagine...</div>
    </div>

    <div class="button-container">
        <button onclick="apriCamera()" class="btn-camera">
            📸 Scan Tabella
        </button>
    </div>

    <table id="mainTable">
        <tr>
            <th></th>
            <th>Cavalli</th>
            <th>1°posto</th>
            <th>2°posto</th>
            <th>3°posto</th>
        </tr>
        <tr>
            <td>1</td>
            <td><input type="text" class="cavalli-input"></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>2</td>
            <td><input type="text" class="cavalli-input"></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>3</td>
            <td><input type="text" class="cavalli-input"></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>4</td>
            <td><input type="text" class="cavalli-input"></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>5</td>
            <td><input type="text" class="cavalli-input"></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>6</td>
            <td><input type="text" class="cavalli-input"></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr class="tris-row">
            <td></td>
            <td>Tris vincente</td>
            <td>
                <select id="primoTris" style="width: 60%">
                    <option value="">-</option>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                    <option value="5">5</option>
                    <option value="6">6</option>
                </select>
            </td>
            <td>
                <select id="secondoTris" style="width: 60%">
                    <option value="">-</option>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                    <option value="5">5</option>
                    <option value="6">6</option>
                </select>
            </td>
            <td>
                <select id="terzoTris" style="width: 60%">
                    <option value="">-</option>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                    <option value="5">5</option>
                    <option value="6">6</option>
                </select>
            </td>
        </tr>
    </table>

    <div class="button-container">
        <button onclick="cercaGara()" class="btn-cerca">Cerca Gara</button>
        <button onclick="salvaDati()" class="btn-salva">Salva Corsa</button>
        <button onclick="toggleLogDati()" class="btn-toggle" id="toggleLogBtn">
            Nascondi Log
        </button>
    </div>

    <div class="log-container" id="logContainer">
        <div class="log-controls">
            <label>Ordina per: </label>
            <select id="sortOrder" onchange="ordinaLog()">
                <option value="newest">Più recenti</option>
                <option value="oldest">Più vecchie</option>
            </select>
        </div>
        <h3>Log Dati</h3>
        <div id="logDati"></div>
    </div>
    <script>
        let corse = JSON.parse(localStorage.getItem('corse')) || [];
        const SHEETS_URL = 'https://script.google.com/macros/s/AKfycbxLFkI2fU329vYO1Q73rsorW3SLIt8lKO4PpvxyuHZZuZOnHm5BsY2Jg92lRxhxXsf5/exec';

        // Gestione tema
        const themeToggle = document.getElementById('themeToggle');
        themeToggle.checked = localStorage.getItem('theme') === 'dark';
        
        function toggleTheme() {
            const html = document.documentElement;
            const isDark = themeToggle.checked;
            html.setAttribute('data-theme', isDark ? 'dark' : 'light');
            localStorage.setItem('theme', isDark ? 'dark' : 'light');
        }

        themeToggle.addEventListener('change', toggleTheme);
        toggleTheme();

        // Funzione per la fotocamera e OCR
        async function apriCamera() {
            const input = document.createElement('input');
            input.type = 'file';
            input.accept = 'image/*';
            input.capture = 'environment';
            
            input.onchange = async function(e) {
                const file = e.target.files[0];
                if (file) {
                    // Mostra loader
                    document.getElementById('loading').style.display = 'flex';
                    document.getElementById('loadingText').textContent = 'Elaborazione immagine...';
                    
                    try {
                        // Inizializza Tesseract
                        const worker = await Tesseract.createWorker();
                        await worker.loadLanguage('ita');
                        await worker.initialize('ita');
                        
                        // Riconosci il testo
                        document.getElementById('loadingText').textContent = 'Riconoscimento testo...';
                        const { data: { text } } = await worker.recognize(file);
                        await worker.terminate();

                        // Analizza e compila i campi
                        compilaCampi(text);
                        
                        // Nascondi loader e mostra successo
                        document.getElementById('loading').style.display = 'none';
                        mostraMessaggio('✅ Dati inseriti automaticamente!', 'info');
                    } catch (error) {
                        console.error('Errore OCR:', error);
                        document.getElementById('loading').style.display = 'none';
                        mostraMessaggio('⚠️ Errore nel riconoscimento del testo', 'warning');
                    }
                }
            };
            
            input.click();
        }

        function compilaCampi(text) {
            // Dividi il testo in righe
            const righe = text.split('\n');
            let indiceRiga = 0;
            
            righe.forEach(riga => {
                if (indiceRiga < 6) {
                    // Cerca pattern di numeri e testo
                    const matches = riga.match(/([A-Za-z\s]+)\s+(\d+\.?\d*)\s+(\d+\.?\d*)\s+(\d+\.?\d*)/);
                    if (matches) {
                        const [, cavallo, quota1, quota2, quota3] = matches;
                        
                        // Trova gli input della riga corrente
                        const inputs = document.querySelectorAll(`#mainTable tr:nth-child(${indiceRiga + 2}) input`);
                        inputs[0].value = cavallo.trim();
                        inputs[1].value = quota1;
                        inputs[2].value = quota2;
                        inputs[3].value = quota3;
                        
                        indiceRiga++;
                    }
                }
            });
        }

        // Gestione visibilità log
        function toggleLogDati() {
            const logContainer = document.getElementById('logContainer');
            const toggleBtn = document.getElementById('toggleLogBtn');
            const isVisible = !logContainer.classList.contains('hidden');
            
            logContainer.classList.toggle('hidden');
            toggleBtn.textContent = isVisible ? 'Mostra Log' : 'Nascondi Log';
        }

        // Ordinamento log
        function ordinaLog() {
            const sortOrder = document.getElementById('sortOrder').value;
            const corsaOrdinate = [...corse].sort((a, b) => {
                const dateA = new Date(a.data.split('/').reverse().join('-'));
                const dateB = new Date(b.data.split('/').reverse().join('-'));
                return sortOrder === 'newest' ? dateB - dateA : dateA - dateB;
            });
            
            aggiornaLogDati(corsaOrdinate);
        }

        async function salvaInGoogleSheets(corsa) {
            try {
                const response = await fetch(SHEETS_URL, {
                    method: 'POST',
                    mode: 'no-cors',
                    headers: {
                        'Content-Type': 'application/json',
                    },
                    body: JSON.stringify({
                        ...corsa,
                        trisVincente: "'" + corsa.trisVincente
                    })
                });
                return true;
            } catch (error) {
                console.error('Errore nel salvataggio:', error);
                return false;
            }
        }

        function mostraMessaggio(messaggio, tipo) {
            const messageBox = document.getElementById('messageBox');
            messageBox.innerHTML = `<div class="alert alert-${tipo}">${messaggio}</div>`;
            setTimeout(() => {
                messageBox.innerHTML = '';
            }, 5000);
        }

        function cercaGara() {
            const righe = document.querySelectorAll('#mainTable tr');
            const cavalliAttuali = [];

            for (let i = 1; i < 7; i++) {
                const riga = righe[i];
                const cavalloInput = riga.querySelectorAll('input')[0];
                if (cavalloInput.value.trim()) {
                    cavalliAttuali.push(cavalloInput.value.trim());
                }
            }

            if (cavalliAttuali.length === 0) {
                mostraMessaggio('⚠️ Inserisci almeno un cavallo prima di cercare', 'warning');
                return;
            }

            let trovata = false;
            for (let corsa of corse) {
                let cavalliCorsa = corsa.dati
                    .filter(d => d.cavalli.trim())
                    .map(d => d.cavalli.trim());

                if (JSON.stringify(cavalliCorsa) === JSON.stringify(cavalliAttuali)) {
                    mostraMessaggio(`🎯 Corsa trovata! La tris vincente era: ${corsa.trisVincente}`, 'info');
                    trovata = true;
                    break;
                }
            }

            if (!trovata) {
                mostraMessaggio('🔍 Nessuna corsa simile trovata nel database', 'warning');
            }
        }

        // ... resto del codice JavaScript (salvaDati, aggiornaLogDati, eliminaCorsa, pulisciForm) rimane uguale ...

        window.onload = function() {
            aggiornaLogDati();
            toggleTheme();
        };
    </script>
</body>
</html>
