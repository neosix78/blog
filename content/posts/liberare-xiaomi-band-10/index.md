+++
title = "Sei ore per riprendermi i miei dati: storia di una Xiaomi Band 10 sbrandizzata"
date = '2026-05-26T09:00:00+02:00'
draft = false
summary = "Una smart band da 36 euro che misura battito, sonno, stress, ossigeno. Dati intimi, dati miei. Per portarli sul mio server ho passato una sera a smontare un sistema costruito apposta per non darmeli. Te la racconto perché credo che valga la pena sapere quanto sia difficile fare una cosa che dovrebbe essere normale."
tags = ["privacy", "self-hosted", "homelab", "xiaomi", "gadgetbridge", "grafana", "influxdb", "iot", "sovranita-digitale"]
categories = ["tech-tips"]

[cover]
  image = "cover.jpg"
  alt = "Xiaomi Smart Band 10 al polso che mostra ore 17:54, 5.847 passi del giorno e batteria al 78%"
  caption = "La Band 10 al polso, un martedì qualunque. Da ieri sera parla solo col mio server."
  relative = true
+++

Ieri sera mi sono comprato una Xiaomi Smart Band 10. Trentasei euro, batteria una settimana, misura tutto: battito, passi, sonno, stress, ossigeno, e una cosa che chiamano "Vitality Score" che è praticamente il PAI.

Sono tornato a casa, me la sono messa al polso, e ho deciso che quei dati non sarebbero finiti su un server in Germania.

Detto così sembra una scelta semplice. Lo era, in teoria. In pratica mi ha portato via tutta la serata. Te la racconto perché credo che valga la pena sapere quanto sia diventato faticoso fare una cosa che dovrebbe essere normale: tenere a casa propria i numeri che il proprio corpo produce.

## Quello che ti raccontano (e quello che non ti dicono)

Per usarla devi installare **Mi Fitness**, accettare quaranta pagine di condizioni, e da quel momento ogni battito viaggia verso un server Xiaomi. Non c'è un toggle "tienili offline". Non c'è un export decente. Non esiste l'opzione "sono miei, te li lascio vedere ma non te li regalo".

E ti accorgi che è diventato così ovunque. La bilancia spedisce il peso al cloud. L'auto manda chilometraggio e abitudini di guida. Il frigorifero "smart" segnala quante volte hai aperto lo sportello. Le impostazioni per spegnere queste cose sono sepolte sotto cinque sottomenu, scritte in piccolo, e ogni aggiornamento le sposta.

La privacy non ce l'hanno tolta tutta in un giorno. Ce l'hanno tolta **un click alla volta**. Ed è sempre stata presentata come un favore: *"ci servono i tuoi dati per offrirti l'esperienza migliore."* Tradotto onestamente: ci servono perché valgono di più di quello che hai pagato per il prodotto.

Volevo provare a uscire dal giro. Almeno per questa cosa qui. Tenere quei numeri **dentro casa mia**, su un piccolo Proxmox in salotto, dentro un Grafana che gira solo per me. Niente di eroico. Una piccola dignità tecnologica.

## Due vicoli ciechi prima di trovare la porta giusta

Le guide su Reddit dicono di usare **Gadgetbridge** — l'app open-source che parla con la band offline — e per farlo serve l'**auth key** del device. La via storica per estrarla è uno script Python, `huami-token`. Lo lanci, fai login, dovrebbe darti la chiave.

Solo che con la Band 10 entra in un loop infinito di verifiche email. Ci ho perso due ore prima di capire il perché: Xiaomi associa il "device trusted" al fingerprint del browser, non al `device_id` che lo script genera. Browser e script sono entità diverse. Lo script chiede una nuova verifica all'infinito.

Piano B: facciamo passare i dati da **Mi Fitness → Google Fit → API**. Apro Mi Fitness, cerco l'integrazione Google Fit. Non c'è più. Vado a guardare lo stato di Google Fit API: deprecata, end-of-service fine 2026, niente più signup nuovi. La via che fino a due anni fa era ufficialmente raccomandata è stata smurata e via.

A questo punto mi sono fermato a guardare cosa stava succedendo, davvero.

Avevo davanti tre porte. La prima — l'app ufficiale — è aperta, illuminata, c'è un cartello "entra, basta firmare qui". La seconda — Google Fit — l'hanno murata. La terza — il tool community — è dietro una recinzione, col filo spinato, e devi sapere già che esiste per trovarla.

Non è un caso. È **il design**. Le crepe da cui potresti recuperare i tuoi dati senza passare dal cloud le chiudono a una a una: rimuovono opzioni dalle app, deprecano API, cambiano l'auth schema, e nessuno annuncia mai esplicitamente "abbiamo chiuso". È erosione lenta. La rana nella pentola.

E funziona. Se non avessi avuto un homelab e tre ore libere mi sarei fermato. Avrei riacceso Mi Fitness, fatto spallucce, e sarei andato a letto regalando i miei dati come tutti gli altri.

## La terza porta

Cercando un'ultima volta sono finito su [PiotrMachowski/Xiaomi-cloud-tokens-extractor](https://github.com/PiotrMachowski/Xiaomi-cloud-tokens-extractor). Nato per le luci Yeelight, gestisce il login Xiaomi in modo serio: captcha via server HTTP locale, 2FA via email, prova tutte le regioni cloud.

Lo lanci, apri una pagina dal telefono sulla `:31415`, fai il captcha, conferma email. In venti secondi ti stampa **tutti** i device Xiaomi del tuo account con MAC, modello e token in chiaro. Sessantadue device, scopro guardando la lista — avevo dimenticato di averne tanti. La band era in fondo.

Token salvato in un `.env` con `chmod 600`, fuori da Git e dal vault Obsidian. Da lì in poi è andata in discesa.

Gadgetbridge nightly da F-Droid (NON quella del Play Store, che è "sponsor only" e non ha il supporto Band 10). Pair Bluetooth, incollo MAC e token, forzo Bluetooth Classic. Connessione al primo tentativo. Batteria 86%, passi già sincronizzati, HR live a 72 bpm. Mi Fitness disinstallato — la chiave resta valida perché è legata al binding account, non al telefono.

## Dalla band al mio server

Gadgetbridge esporta in automatico ogni ora il suo SQLite. Una folder Syncthing sul Pixel, condivisa col container Syncthing sul LXC. Il file atterra in `/opt/band-stack/syncthing-data/gb-export/` da solo, in pochi secondi.

Un cron `*/10 * * * *` legge il database, fa la diff rispetto allo stato precedente, e fa ingest in InfluxDB. Heart rate, stress, vitality, sonno per stadi, batteria. Granularità da minuto a giornaliera.

Quando alle 00:06 ho visto i primi punti dati comparire in InfluxDB è scattata quella piccola scarica di felicità che è il motivo per cui faccio queste cose. Una sciocchezza, lo so. Ma è la sciocchezza che mi fa stare bene.

## Quarantasette pannelli (perché poi mi ci sono appassionato)

Mi ero detto: sei pannelli e basta. Dopo dodici ore erano quarantasette, divisi in nove sezioni a fisarmonica.

Tutto parte dal **Today Hero** in alto: HR live, Vitality, una cosa che ho chiamato **Body Battery** (formula casalinga con vitality + stress invertito + sleep score), passi/calorie/batteria del giorno, ultimo sonno con uno score calcolato. Poi sezioni per cuore (HR con zone colorate, heatmap distribuzione), stress, sonno (state-timeline degli stadi), movimento (passi con target settabile), SpO2, batteria.

Tre annotation sovrapposte ai grafici: una regione blu per le notti di sonno, un marker arancio per i picchi cardiaci, un marker verde quando la band si ricarica. Cose piccole, che messe insieme trasformano un grafico in una piccola narrazione visiva.

Sopra tutto, due automatismi che girano da soli.

**Ogni mattina alle 7** un cron prende i KPI delle ultime 24h, li confronta con la media a 7gg, e li manda a Groq (llama-3.3-70b) con un prompt italiano di sessanta parole. Mi torna un riassunto del tipo *"Ieri 8.200 passi (+12% sulla media), notte di 6h45m con poco REM, stress sotto la media: parti con batteria piena ma il sonno corto si paga. Tieni la mattina leggera."* Atterra in cima alla dashboard e mi arriva su Telegram.

**Ogni domenica alle 20** un recap settimanale: cosa è andato bene, cosa migliorare, una micro-azione per la settimana che inizia. Più un audio di trenta secondi sintetizzato in italiano dal mio Kokoro locale, voce Sara, che mi arriva come messaggio vocale. Lo ascolto il lunedì mentre apro il negozio.

Quattro alert silenziosi fanno la guardia: batteria bassa, HR a riposo alto per più giorni, crollo di vitalità, sonno medio sotto le cinque ore per due notti. Niente di clinico. Solo sentinelle che mi avvisano se qualcosa si muove in una direzione che vale la pena guardare.

## Conto della serata

- Band 10: **36 €**
- Tempo: **6 ore** in una sera
- Cloud Xiaomi consultato dopo il pair: **zero volte**
- LXC consumato: **0,4% CPU**, 180MB RAM

In due giorni di dati ho già notato che il mio HR a riposo cala di otto battiti il giorno dopo una buona nottata, e che lo stress sale puntuale fra le 17:30 e le 19:30 — l'ora in cui chiudo il negozio. Con sei mesi di storico potrò chiedere a un'AI di guardarsi il mio sonno e dirmi cose vere. `holtWinters` è già configurato, aspetta solo che ci siano dati abbastanza.

## Non è solo una band

Se sei arrivato fin qui forse pensi "vabbè, ma è una band, è una sciocchezza". Hai ragione, presa da sola lo è.

Ma il battito, le ore di sonno, i livelli di stress, l'ossigenazione, il pattern delle giornate sono dati medici. Più sensibili di un estratto conto. Pensa a quanto valgono per un'assicurazione, per un datore di lavoro, per un advertiser farmaceutico. E pensa a quanti se ne accumulano in dieci anni di band sempre al polso, e in quante mani passano nel frattempo senza che ti venga mai più chiesto un consenso aggiornato.

Sei ore per riprendermi il controllo di **un** sensore al polso sono troppe. Lo so. Dovrebbe esserci una spunta "tieni i dati a casa mia", e via. Invece è una battaglia ogni volta, ed è progettata per farti sentire stupido nel combatterla — *"ma chi te lo fa fare", "che hai da nascondere", "tanto i dati li hanno già tutti"*.

Io l'ho fatto perché posso. Perché ogni volta che qualcuno costruisce la sua piccola infrastruttura indipendente, anche imperfetta, dimostra che si può fare. E magari domani lo dimostra a un altro.

La Band 10 ora vive nel mio polso e parla solo col mio server. È una vittoria minuscola, ma vale. Non perché ho stupito qualcuno. Vale perché in un mondo che ti chiede continuamente di firmare per cedere un altro pezzettino di te, ogni tanto si può rispondere di no.

Sei ore di una sera, e mi sono ripreso qualcosa che era già mio.
