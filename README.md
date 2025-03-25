<!DOCTYPE html>
<html data-theme="light">
<head>
    <title>Corse Cavalli</title>
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1">
    <style>
        :root[data-theme="light"] {
            --bg-color: #ffffff;
            --text-color: #333333;
            --border-color: #dddddd;
            --header-bg: #f8f9fa;
            --button-primary: #4CAF50;
            --button-secondary: #2196F3;
            --alert-warning: #f44336;
            --alert-info: #2196F3;
            --keep-color: #fbbc04;
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
            --keep-color: #fbbc04;
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
       .header-controls {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }

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

        table th:nth-child(3),
        table th:nth-child(4),
        table th:nth-child(5),
        table td:nth-child(3),
        table td:nth-child(4),
        table td:nth-child(5) {
            width: 10%;
        }

        table th:nth-child(1),
        table td:nth-child(1) {
            width: 5%;
        }

        table th:nth-child(2),
        table td:nth-child(2) {
            width: 32%;
        }

        input, select {
            width: 90%;
            padding: 8px;
            border: 1px solid var(--border-color);
            border-radius: 4px;
            background-color: var(--bg-color);
            color: var(--text-color);
        }

        input::placeholder {
            color: #999;
            font-style: italic;
        }
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
            background-color: var(--keep-color);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-weight: bold;
        }

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

        .alert-centrale {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: #ff0000;
            color: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 0 20px rgba(0,0,0,0.5);
            z-index: 1000;
            animation: blink 1s infinite;
            min-width: 300px;
            text-align: center;
        }

        .alert-centrale h2 {
            margin: 0;
            font-size: 24px;
            margin-bottom: 10px;
        }

        .alert-contenuto {
            background-color: rgba(255,255,255,0.9);
            color: #000;
            padding: 10px;
            border-radius: 5px;
            margin: 10px 0;
        }

        .alert-button {
            background-color: white;
            color: red;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            margin-top: 10px;
            cursor: pointer;
            font-weight: bold;
        }

        @keyframes blink {
            0% { opacity: 1; }
            50% { opacity: 0.7; }
            100% { opacity: 1; }
        }

        .sync-indicator {
            position: fixed;
            top: 10px;
            right: 10px;
            background-color: var(--button-secondary);
            color: white;
            padding: 5px 10px;
            border-radius: 4px;
            font-size: 12px;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .sync-indicator.visible {
            opacity: 1;
        }

        .autocomplete-items {
            position: absolute;
            border: 1px solid var(--border-color);
            border-top: none;
            z-index: 99;
            top: 100%;
            left: 0;
            right: 0;
            background-color: var(--bg-color);
        }

        .autocomplete-items div {
            padding: 10px;
            cursor: pointer;
        }

        .autocomplete-items div:hover {
            background-color: var(--header-bg);
        }

        .cavalli-input-container {
            position: relative;
            width: 100%;
        }

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
                width: 100%;
                padding: 12px;
                font-size: 14px;
                justify-content: center;
            }

            .btn-camera {
                padding: 15px;
                font-size: 16px;
            }

            .header-controls {
                flex-direction: column;
            }
        }
    </style>
</head>
<body>
    <div class="header-controls">
        <h1>Corse Cavalli</h1>
        <div class="theme-switch">
            <span>☀️</span>
            <label class="toggle-switch">
                <input type="checkbox" id="themeToggle">
                <span class="slider"></span>
            </label>
            <span>🌙</span>
        </div>
    </div>
    
    <div id="messageBox"></div>
    <div id="syncIndicator" class="sync-indicator">🔄 Sincronizzazione...</div>

    <div class="button-container">
        <button onclick="apriKeep()" class="btn-camera">
            📝 Scan con Google Keep
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
            <td><div class="cavalli-input-container">
                <input type="text" class="cavalli-input" placeholder="✂️ Incolla qui i dati della tabella...">
            </div></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>2</td>
            <td><div class="cavalli-input-container">
                <input type="text" class="cavalli-input">
            </div></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>3</td>
            <td><div class="cavalli-input-container">
                <input type="text" class="cavalli-input">
            </div></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
       <tr>
            <td>4</td>
            <td><div class="cavalli-input-container">
                <input type="text" class="cavalli-input">
            </div></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>5</td>
            <td><div class="cavalli-input-container">
                <input type="text" class="cavalli-input">
            </div></td>
            <td><input type="number" inputmode="decimal" class="primo-input"></td>
            <td><input type="number" inputmode="decimal" class="secondo-input"></td>
            <td><input type="number" inputmode="decimal" class="terzo-input"></td>
        </tr>
        <tr>
            <td>6</td>
            <td><div class="cavalli-input-container">
                <input type="text" class="cavalli-input">
            </div></td>
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
        <button onclick="cercaQuote()" class="btn-cerca">Cerca Quote</button>
        <button onclick="cercaCavallo()" class="btn-cerca">Cerca Cavallo</button>
        <button onclick="azzeraRicerca()" class="btn-cerca">Azzera Ricerca</button>
        <button onclick="salvaDati()" class="btn-salva">Salva Corsa</button>
    </div>

    <div id="risultatiRicerca" style="margin: 20px 0; display: none;">
        <div style="background-color: var(--header-bg); padding: 15px; border-radius: 5px; margin-bottom: 20px;">
            <h2 style="color: var(--text-color); margin: 0 0 15px 0;">🔍 Risultati della Ricerca</h2>
            <div id="risultatiContainer">
                <!-- Qui verranno inseriti i risultati -->
            </div>
        </div>
    </div>
<script>
let corse = [];
const SHEETS_URL = 'https://script.google.com/macros/s/AKfycbytqre6y_4j-8KJkpHtAL2b5Rl3sMxhk9Qw6e_N29cLQalqkNWDD7uW2ghS0V2EjIUj/exec';

async function leggiDaGoogleSheets() {
    try {
        document.getElementById('syncIndicator').classList.add('visible');
        const response = await fetch(SHEETS_URL + '?action=read');
        const data = await response.json();
        if (data && data.corse) {
            corse = data.corse.map(corsa => ({
                ...corsa,
                trisVincente: typeof corsa.trisVincente === 'string' ? 
                    JSON.parse(corsa.trisVincente) : corsa.trisVincente
            }));
        }
        document.getElementById('syncIndicator').classList.remove('visible');
    } catch (error) {
        console.error('Errore nella sincronizzazione:', error);
        document.getElementById('syncIndicator').classList.remove('visible');
    }
}

const themeToggle = document.getElementById('themeToggle');
themeToggle.checked = localStorage.getItem('theme') === 'dark';

function toggleTheme() {
    const html = document.documentElement;
    const isDark = themeToggle.checked;
    html.setAttribute('data-theme', isDark ? 'dark' : 'light');
    localStorage.setItem('theme', isDark ? 'dark' : 'light');
}

function apriKeep() {
    window.open('https://keep.google.com', '_blank');
    mostraMessaggio(`
        📝 Come usare Google Keep:
        1. Apri "Nuova nota con in immagine"
        2. Scatta la foto della tabella
        3. Clicca sui tre puntini (⋮)
        4. Scegli "Copia testo da immagine"
        5. Incolla nella prima casella della tabella
    `, 'info');
}

function mostraMessaggio(messaggio, tipo) {
    const messageBox = document.getElementById('messageBox');
    messageBox.innerHTML = `<div class="alert alert-${tipo}">${messaggio}</div>`;
    setTimeout(() => {
        messageBox.innerHTML = '';
    }, 8000);
}

function mostraAlertCentrale(tipo, risultati) {
    const audio = new Audio('https://www.soundjay.com/misc/sounds/bell-ringing-01.mp3');
    audio.play();

    const alertDiv = document.createElement('div');
    alertDiv.className = 'alert-centrale';
    
    let titoloMessaggio = '';
    switch(tipo) {
        case 'gara':
            titoloMessaggio = '‼️ GARE TROVATE ‼️';
            break;
        case 'cavallo':
            titoloMessaggio = '‼️ CAVALLO TROVATO ‼️';
            break;
        case 'quote':
            titoloMessaggio = '‼️ QUOTE TROVATE ‼️';
            break;
    }

    alertDiv.innerHTML = `
        <h2>${titoloMessaggio}</h2>
        <div class="alert-contenuto">
            <p>Trovati ${risultati} risultati</p>
            <p><strong>Controlla la tabella sotto</strong></p>
        </div>
        <button class="alert-button" onclick="this.parentElement.remove()">OK - HO VISTO</button>
    `;

    document.body.appendChild(alertDiv);
}

function formattaTrisVincente(tris) {
    if (typeof tris === 'string' && tris.includes('-')) {
        return tris;
    }
    if (tris && typeof tris === 'object' && 'primo' in tris && 'secondo' in tris && 'terzo' in tris) {
        return `${tris.primo}-${tris.secondo}-${tris.terzo}`;
    }
    return 'N/A';
}

function cercaGara() {
    const quoteAttuali = [];
    const righe = document.querySelectorAll('#mainTable tr');

    for (let i = 1; i < 7; i++) {
        const inputs = righe[i].querySelectorAll('input');
        if(inputs[0].value || inputs[1].value || inputs[2].value || inputs[3].value) {
            quoteAttuali.push({
                cavalli: inputs[0].value,
                primo: inputs[1].value,
                secondo: inputs[2].value,
                terzo: inputs[3].value
            });
        }
    }

    if (quoteAttuali.length === 0) {
        mostraMessaggio('⚠️ Inserisci almeno una serie di dati', 'warning');
        return;
    }

    const risultati = corse.filter(corsa => {
        return corsa.dati.some((riga, index) => {
            const quoteRiga = quoteAttuali[index];
            if (!quoteRiga) return false;
            return riga.cavalli === quoteRiga.cavalli &&
                   riga.primo === quoteRiga.primo &&
                   riga.secondo === quoteRiga.secondo &&
                   riga.terzo === quoteRiga.terzo;
        });
    });

    mostraRisultatiRicerca(risultati, 'gara');
}

function cercaCavallo() {
    const inputs = document.querySelectorAll('.cavalli-input');
    let searchInput = '';
    let corsiaRicerca = 0;

    inputs.forEach((input, index) => {
        if (input.value.trim()) {
            searchInput = input.value.trim().toLowerCase();
            corsiaRicerca = index + 1;
        }
    });

    if (!searchInput) {
        mostraMessaggio('⚠️ Inserisci il nome di un cavallo da cercare', 'warning');
        return;
    }

    const risultati = corse.filter(corsa => {
        const rigaCorrispondente = corsa.dati.find(riga => riga.numero == corsiaRicerca);
        return rigaCorrispondente && rigaCorrispondente.cavalli.toLowerCase().includes(searchInput);
    });

    mostraRisultatiRicerca(risultati, 'cavallo');
}

function cercaQuote() {
    const righe = document.querySelectorAll('#mainTable tr');
    const quoteAttuali = [];

    for (let i = 1; i < 7; i++) {
        const riga = righe[i];
        const inputs = riga.querySelectorAll('input');
        if (inputs[1].value || inputs[2].value || inputs[3].value) {
            quoteAttuali.push({
                numero: i,
                primo: inputs[1].value,
                secondo: inputs[2].value,
                terzo: inputs[3].value
            });
        }
    }

    if (quoteAttuali.length === 0) {
        mostraMessaggio('⚠️ Inserisci almeno una serie di quote prima di cercare', 'warning');
        return;
    }

    const risultatiCompleti = corse.filter(corsa => {
        return quoteAttuali.every(quoteRiga => {
            const rigaCorsa = corsa.dati[quoteRiga.numero - 1];
            return rigaCorsa &&
                   rigaCorsa.primo === quoteRiga.primo &&
                   rigaCorsa.secondo === quoteRiga.secondo &&
                   rigaCorsa.terzo === quoteRiga.terzo;
        });
    });

    mostraRisultatiRicerca(risultatiCompleti, 'quote');
}

function mostraRisultatiRicerca(risultati, tipo) {
    const risultatiDiv = document.getElementById('risultatiRicerca');
    const risultatiContainer = document.getElementById('risultatiContainer');
    
    if (!risultatiDiv || !risultatiContainer) {
        console.error('Elementi risultati non trovati');
        return;
    }
    
    risultatiContainer.innerHTML = '';
    
    if (risultati.length > 0) {
        risultati.forEach(risultato => {
            const resultDiv = document.createElement('div');
            resultDiv.style.backgroundColor = 'var(--bg-color)';
            resultDiv.style.padding = '15px';
            resultDiv.style.marginBottom = '15px';
            resultDiv.style.borderRadius = '5px';
            resultDiv.style.border = '2px solid var(--button-secondary)';
            
            const trisVincenteDiv = `
                <div style="background-color: #ff0000; color: white; padding: 15px; margin-bottom: 10px; border-radius: 5px; text-align: center; font-size: 1.2em; font-weight: bold;">
                    🎯 TRIS VINCENTE: ${formattaTrisVincente(risultato.trisVincente)} 🎯
                </div>
            `;

            resultDiv.innerHTML = `
                ${trisVincenteDiv}
                <table style="width: 100%; margin-bottom: 10px;">
                    <tr>
                        <th>Numero</th>
                        <th>Cavalli</th>
                        <th>1°posto</th>
                        <th>2°posto</th>
                        <th>3°posto</th>
                    </tr>
                    ${risultato.dati.map(riga => `
                        <tr>
                            <td>${riga.numero}</td>
                            <td>${riga.cavalli}</td>
                            <td>${riga.primo}</td>
                            <td>${riga.secondo}</td>
                            <td>${riga.terzo}</td>
                        </tr>
                    `).join('')}
                </table>
            `;
            risultatiContainer.appendChild(resultDiv);
        });
        
        risultatiDiv.style.display = 'block';
        risultatiDiv.scrollIntoView({ behavior: 'smooth' });
        mostraAlertCentrale(tipo, risultati.length);
    } else {
        risultatiDiv.style.display = 'none';
        mostraAlertCentrale(tipo, 0);
    }
}

function azzeraRicerca() {
    const risultatiDiv = document.getElementById('risultatiRicerca');
    const risultatiContainer = document.getElementById('risultatiContainer');
    risultatiContainer.innerHTML = '';
    risultatiDiv.style.display = 'none';
    mostraMessaggio('✅ Ricerca azzerata', 'info');
}

async function salvaDati() {
    const primo = document.getElementById('primoTris').value;
    const secondo = document.getElementById('secondoTris').value;
    const terzo = document.getElementById('terzoTris').value;

    if (!primo || !secondo || !terzo) {
        mostraMessaggio('⚠️ Seleziona tutti i numeri della tris!', 'warning');
        return;
    }

    const trisVincente = {
        primo: primo,
        secondo: secondo,
        terzo: terzo
    };
    
    const corsa = {
        id: Date.now(),
        trisVincente: trisVincente,
        dati: []
    };

    for (let i = 1; i < 7; i++) {
        const riga = document.querySelectorAll('#mainTable tr')[i];
        const inputs = riga.querySelectorAll('input');
        const numero = riga.cells[0].textContent;
        corsa.dati.push({
            numero: numero,
            cavalli: inputs[0].value,
            primo: inputs[1].value,
            secondo: inputs[2].value,
            terzo: inputs[3].value
        });
    }

    if (corsa.dati.every(riga => !riga.cavalli)) {
        mostraMessaggio('⚠️ Inserisci almeno un cavallo!', 'warning');
        return;
    }

    mostraMessaggio('⌛ Salvataggio in corso...', 'info');

    try {
        const salvataggioOk = await salvaInGoogleSheets(corsa);
        if (salvataggioOk) {
            await leggiDaGoogleSheets();
            pulisciForm();
            mostraMessaggio('✅ Corsa salvata con successo!', 'info');
        } else {
            mostraMessaggio('⚠️ Errore nel salvataggio', 'warning');
        }
    } catch (error) {
        console.error('Errore nel salvataggio:', error);
        mostraMessaggio('⚠️ Errore nel salvataggio', 'warning');
    }
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
                trisVincente: JSON.stringify(corsa.trisVincente)
            })
        });

        await new Promise(resolve => setTimeout(resolve, 2000));
        return true;
    } catch (error) {
        console.error('Errore nel salvataggio:', error);
        return false;
    }
}

function pulisciForm() {
    const inputs = document.querySelectorAll('#mainTable input');
    inputs.forEach(input => input.value = '');
    document.getElementById('primoTris').value = '';
    document.getElementById('secondoTris').value = '';
    document.getElementById('terzoTris').value = '';
}

// Event Listeners
document.addEventListener('DOMContentLoaded', function() {
    themeToggle.addEventListener('change', toggleTheme);
    toggleTheme();
    
    // Auto-compilazione al momento dell'incollaggio
    document.querySelector('.cavalli-input').addEventListener('paste', function(e) {
        e.preventDefault();
        
        const text = (e.clipboardData || window.clipboardData).getData('text');
        
        try {
            const pattern = /(\d)\s+([^0-9]+?)\s+([\d.]+)\s+([\d.]+)\s+([\d.]+)/g;
            let matches = [];
            let match;

            while ((match = pattern.exec(text)) !== null) {
                const numeroRiga = parseInt(match[1]);
                if (numeroRiga >= 1 && numeroRiga <= 6) {
                    matches[numeroRiga - 1] = {
                        cavallo: match[2].trim(),
                        quote: [match[3], match[4], match[5]]
                    };
                }
            }

            matches.forEach((dati, index) => {
                if (dati) {
                    const inputs = document.querySelectorAll(`#mainTable tr:nth-child(${index + 2}) input`);
                    if (inputs[0]) inputs[0].value = dati.cavallo;
                    if (inputs[1]) inputs[1].value = dati.quote[0];
                    if (inputs[2]) inputs[2].value = dati.quote[1];
                    if (inputs[3]) inputs[3].value = dati.quote[2];
                }
            });
            mostraMessaggio('✅ Dati incollati con successo!', 'info');
        } catch (error) {
            console.error('Errore nell\'elaborazione del testo:', error);
            mostraMessaggio('⚠️ Errore nell\'elaborazione del testo incollato', 'warning');
        }
    });
});

// Inizializzazione all'avvio
window.onload = async function() {
    console.log('Pagina caricata');
    await leggiDaGoogleSheets();
    toggleTheme();
    setInterval(leggiDaGoogleSheets, 60000);
};
</script>
</body>
</html>
