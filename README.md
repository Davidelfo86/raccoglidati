async function salvaDati() {
    const righe = document.querySelectorAll('#mainTable tr');
    const quotaTris = document.querySelector('.quota-tris').value;

    if (!quotaTris) {
        mostraMessaggio('⚠️ Inserisci la quota tris!', 'warning');
        return;
    }

    // Trova le righe con la tris selezionata
    const trisSelezionate = Array.from(document.querySelectorAll('.tris-select'))
        .filter(select => select.value)
        .map(select => ({
            numero: select.closest('tr').cells[0].textContent,
            posizione: select.value
        }));

    if (trisSelezionate.length !== 3) {
        mostraMessaggio('⚠️ Seleziona tutti e tre i posti della tris!', 'warning');
        return;
    }

    const corsa = {
        id: Date.now(),
        quotaTris: quotaTris,
        trisVincente: {
            primo: trisSelezionate.find(t => t.posizione === '1').numero,
            secondo: trisSelezionate.find(t => t.posizione === '2').numero,
            terzo: trisSelezionate.find(t => t.posizione === '3').numero
        },
        dati: []
    };

    // Raccoglie i dati di ogni riga
    for (let i = 1; i < 7; i++) {
        const riga = righe[i];
        const cavallo = riga.querySelector('.cavalli-input').value;
        const quota = riga.querySelector('.quota-input').value;
        const trisSelect = riga.querySelector('.tris-select');
        
        corsa.dati.push({
            numero: i,
            cavallo: cavallo,
            quota: quota,
            tris: trisSelect ? trisSelect.value : ''
        });
    }

    if (corsa.dati.every(riga => !riga.cavallo)) {
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

// Event Listeners
document.addEventListener('DOMContentLoaded', function() {
    themeToggle.addEventListener('change', toggleTheme);
    toggleTheme();
    
    // Gestione della selezione della tris
    const trisSelects = document.querySelectorAll('.tris-select');
    trisSelects.forEach(select => {
        select.addEventListener('change', function() {
            const selectedValue = this.value;
            if (selectedValue) {
                trisSelects.forEach(otherSelect => {
                    if (otherSelect !== this && otherSelect.value === selectedValue) {
                        otherSelect.value = '';
                    }
                });
            }
        });
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
