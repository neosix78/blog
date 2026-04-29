+++
title = "Il mio PC non apriva Chrome (e la diagnosi mi ha lasciato a bocca aperta)"
date = '2026-04-29T18:30:00+02:00'
draft = false
summary = "Popup minaccioso, browser bloccato, 23 processi zombie. Apro %LOCALAPPDATA%\\Chrome e trovo 12 GB di profilo. Cosa c'era dentro, e perché alla fine ha vinto il riavvio."
tags = ["chrome", "manutenzione", "performance", "windows", "sysadmin"]
categories = ["tech-tips"]

[cover]
  image = "cover.jpg"
  alt = "Infografica della diagnosi: 23 processi Chrome zombie, 12 GB di profilo, 10 GB liberati dopo pulizia"
  caption = "Riassunto della diagnosi in un colpo d'occhio"
  relative = true
+++

Stamattina ho aperto Chrome e mi è apparso un popup minaccioso: *"Stai usando una riga di comando non supportata, stabilità e sicurezza ne risentono"*. Browser bloccato. Riavviato. Stessa cosa.

Ho pensato fosse colpa di Claude Desktop. Spoiler: non lo era.

Mi sono fatto aiutare per la diagnosi e quello che è venuto fuori è imbarazzante quanto educativo. Quindi lo racconto, perché probabilmente sta succedendo anche al tuo PC.

## La scena del crimine (sul mio PC)

Task manager: **23 processi `chrome.exe`** attivi. Tutti zombie. Chrome pensava di essere aperto, ma nessuna finestra rispondeva. Killo tutto. Riapro. Stessi sintomi.

Allora vado dove non guardo mai: `%LOCALAPPDATA%\Google\Chrome\User Data`.

**12 GB.** Dodici giga di profilo Chrome. Sul mio PC. Da quanto tempo non lo guardavo? Boh, anni.

## L'autopsia (del mio profilo)

- **Service Worker cache**: 1,9 GB (siti web "agganciati" da chissà quando)
- **Modelli AI Gemini scaricati in locale da Chrome**: 4 GB (mai usati consapevolmente)
- **Profili Chrome aggiuntivi** che ho creato e dimenticato: 2,3 GB
- **Code Cache**: 710 MB
- **Crash dump** accumulati da febbraio: 100 MB
- **Cache varie** (GPU, shader, file system, navigazione): ~500 MB

E poi il colpo di scena: **OpenClaw**. Un software che avevo installato tempo fa e disinstallato. Disinstallato sì, ma i suoi flag residui erano ancora caricati in memoria. Chrome li intercettava e generava il popup di errore.

## La riparazione

1. Kill di **48 processi Chrome** in totale (zombie + residui dopo i primi tentativi)
2. Rimozione lockfile profilo
3. Pulizia mirata: cache, modelli AI Gemini, profili dormienti, crash dump
4. Risultato: **12 GB → 2 GB**. Dieci giga liberati sul mio SSD.

Popup OpenClaw? Ancora lì. Software disinstallato da mesi, ma il suo fantasma resisteva nei processi caricati in RAM.

## La soluzione finale: il banale, palloso, sottovalutato **RIAVVIO DEL PC**

Sì. Un'ora di diagnosi chirurgica, e alla fine il riavvio ha cancellato gli ultimi residui in 30 secondi. Chrome ora vola.

## Cosa ho imparato (a mie spese)

1. **Il browser non si pulisce da solo. Mai.** La voce "Cancella dati di navigazione" è la punta dell'iceberg — sotto c'è una balena.
2. **I software disinstallati lasciano tracce ovunque.** Registro, file orfani, flag in memoria. Una disinstallazione completa è una favola.
3. **Chrome scarica modelli AI di Gemini in background.** GB di roba. Se non li usi consapevolmente, sono peso morto.
4. **Il riavvio non è una resa, è uno strumento.** Pulisce la RAM da residui che nessun task manager ti mostra. La prossima volta che un tecnico ti dice "hai provato a riavviare?" non sbuffare. Funziona davvero.
5. **La manutenzione preventiva non è solo backup e antivirus.** È pulizia profonda dei profili applicativi. Lo faccio raramente. Da oggi metto promemoria ogni 6 mesi.

---

Quanti GB stai sprecando senza saperlo? Aprite `%LOCALAPPDATA%` e fate un giro. Vi auguro più fortuna di me.
