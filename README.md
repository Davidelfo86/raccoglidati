   // Funzione Google Keep
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

        const risultati = [];
        corse.forEach(corsa => {
            const rigaCorrispondente = corsa.dati[corsiaRicerca - 1];
            if (rigaCorrispondente && rigaCorrispondente.cavalli.toLowerCase().includes(searchInput)) {
                risultati.push({
                    data: corsa.data,
                    cavallo: rigaCorrispondente.cavalli,
                    quote: `${rigaCorrispondente.primo}-${rigaCorrispondente.secondo}-${rigaCorrispondente.terzo}`,
                    tris: corsa.trisVincente
                });
            }
        });

        if (risultati.length > 0) {
            mostraRisultatiRicerca(risultati, `Risultati ricerca: ${searchInput} (Corsia ${corsiaRicerca})`);
        } else {
            mostraMessaggio(`🔍 Nessun risultato trovato per "${searchInput}" nella corsia ${corsiaRicerca}`, 'warning');
        }
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

        const risultatiDiv = document.getElementById('risultatiRicerca');
        risultatiDiv.innerHTML = '';
        risultatiDiv.style.display = 'block';

        if (risultatiCompleti.length > 0) {
            risultatiCompleti.forEach(risultato => {
                const corsaDiv = document.createElement('div');
                corsaDiv.style.marginBottom = '20px';
                corsaDiv.style.borderBottom = '1px solid var(--border-color)';
                
                corsaDiv.innerHTML = `
                    <p><strong>Data: ${risultato.data}</strong> | Tris: ${risultato.trisVincente}</p>
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
                risultatiDiv.appendChild(corsaDiv);
            });
            
            mostraMessaggio(`✅ Trovate ${risultatiCompleti.length} tabelle corrispondenti`, 'info');
        } else {
            mostraMessaggio('🔍 Nessuna tabella corrispondente trovata', 'warning');
        }
    }

    function cercaGara() {
        const dataOdierna = new Date().toLocaleDateString('it-IT', {
            day: '2-digit',
            month: '2-digit',
            year: 'numeric'
        });

        const risultati = corse.filter(corsa => corsa.data === dataOdierna);

        if (risultati.length > 0) {
            mostraRisultatiRicerca(risultati.map(corsa => ({
                data: corsa.data,
                cavallo: corsa.dati.map(d => d.cavalli).join(', '),
                quote: 'Varie',
                tris: corsa.trisVincente
            })), "Risultati gare di oggi");
        } else {
            mostraMessaggio('🔍 Nessuna gara trovata per oggi', 'warning');
        }
    }

    function azzeraRicerca() {
        document.getElementById('risultatiRicerca').style.display = 'none';
        const tabellaRisultati = document.getElementById('tabellaRisultati').getElementsByTagName('tbody')[0];
        tabellaRisultati.innerHTML = '';
        mostraMessaggio('✅ Ricerca azzerata', 'info');
    }

    // Il resto delle funzioni rimane invariato
    function mostraRisultatiRicerca(risultati, titolo) {
        const tabellaRisultati = document.getElementById('tabellaRisultati').getElementsByTagName('tbody')[0];
        tabellaRisultati.innerHTML = '';

        if (risultati.length > 0) {
            risultati.forEach(r => {
                const row = tabellaRisultati.insertRow();
                row.innerHTML = `
                    <td>${r.data}</td>
                    <td>${r.cavallo}</td>
                    <td>${r.quote}</td>
                    <td>${r.tris}</td>
                `;
            });
            document.getElementById('risultatiRicerca').style.display = 'block';
            mostraMessaggio(`✅ Trovati ${risultati.length} risultati. Controlla la tabella sotto.`, 'info');
        } else {
            document.getElementById('risultatiRicerca').style.display = 'none';
            mostraMessaggio('🔍 Nessun risultato trovato', 'warning');
        }
    }

    // Le funzioni salvaDati, salvaInGoogleSheets, mostraMessaggio, aggiornaLogDati, 
    // eliminaCorsa, pulisciForm, toggleLogDati, ordinaLog rimangono invariate dal tuo codice originale

    window.onload = async function() {
        document.querySelectorAll('.cavalli-input').forEach(input => {
            setupAutoComplete(input);
        });
        await leggiDaGoogleSheets();
        toggleTheme();
        setInterval(leggiDaGoogleSheets, 60000);
    };
</script>
</body>
</html>
