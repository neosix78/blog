+++
title = "Prevedere la domanda con l'AI: la sorpresa non è il volume, è il ritmo"
date = '2026-06-18T09:00:00+02:00'
draft = false
summary = "Ho ripreso un esperimento fermo da mesi: usare un foundation model di Google per prevedere la domanda del negozio. Pensavo che la regola fosse 'più dati vendi, meglio funziona'. Sbagliato. La cosa che conta non è quanto vendi, è se quello che vendi ha un ritmo. E lungo la strada ho capito che lo strumento serve a tutt'altro da quello per cui l'avevo tirato fuori."
tags = ["ai", "forecasting", "time-series", "timesfm", "retail", "automazione", "claude-code"]
categories = ["tech-tips"]
ShowToc = true
TocOpen = false

[cover]
  image = "cover.png"
  alt = "Un'onda blu regolare e una linea ambra frastagliata convergono in un cervello metà organico e metà circuito; oltre una linea che segna il presente, prosegue verso il futuro solo l'onda regolare."
  caption = "A sinistra il ritmo e il rumore. Oltre il presente, il modello porta avanti solo ciò che ha una struttura da riconoscere."
  relative = true
+++

C'è un esperimento che tenevo in un cassetto da mesi. L'idea era semplice da enunciare e fastidiosa da verificare: **può un modello di intelligenza artificiale prevedere quanto venderò la settimana prossima, meglio di quanto faccio io a occhio?**

La tentazione, in un negozio, è di rispondere "tanto lo so già io cosa gira". E in parte è vero. Ma "lo so io" non si trasforma in un ordine dimensionato bene, non finisce in un report che leggo col caffè la mattina, e soprattutto non scala: vale per le tre o quattro cose che ho in testa, non per tutto il resto. Volevo vedere se una macchina poteva fare quel lavoro noioso al posto mio — e farlo abbastanza bene da fidarmi.

Quello che è uscito dall'esperimento non è la risposta che mi aspettavo. È migliore, ed è più sottile.

## Cos'è un "foundation model" per le serie temporali

Tutti hanno presente i modelli linguistici tipo ChatGPT: li hanno addestrati su quantità sterminate di testo, e adesso "capiscono" il linguaggio senza che tu debba insegnargli la tua lingua da zero. Negli ultimi anni è successa la stessa cosa con le **serie temporali** — cioè con i numeri che si muovono nel tempo: vendite giorno per giorno, consumi, traffico, temperature.

Google Research ha pubblicato un modello che si chiama **TimesFM** (Time-series Foundation Model). L'hanno allenato su una montagna di serie storiche di ogni tipo, e il risultato è che sa fare previsioni **"zero-shot"**: gli dai lo storico di una serie qualsiasi — le tue vendite, per dire — e lui ti sputa fuori la previsione dei prossimi giorni o settimane, **senza che tu debba addestrare niente**. Niente data scientist, niente mesi di tuning. Lo scarichi, gli passi i tuoi numeri, ti risponde.

Per chi ha una piccola impresa questo è potenzialmente enorme. La previsione della domanda è sempre stata roba da aziende strutturate, con software costosi e qualcuno pagato per farli girare. Un modello pre-addestrato che gira gratis su una scheda grafica di casa ribalta l'equazione: lo strumento di fascia alta diventa accessibile a chi fattura un millesimo.

Il punto, come sempre, è che "potenzialmente enorme" e "utile sul serio per me" sono due cose diverse. E in mezzo c'è la verifica.

## La domanda giusta non è "funziona?", è "dove funziona?"

L'errore da principianti, davanti a uno strumento così, è chiedersi "funziona o no?" e cercare un sì/no. La realtà è che funziona benissimo su alcune cose e malissimo su altre, e tutto il valore sta nel capire **quali** sono le une e le altre — prima di costruirci sopra qualcosa.

Quindi non mi sono fidato del README né di una prova al volo. Ho messo su un banco di prova vero: ho preso il modello, gli ho fatto prevedere periodi del **passato** che già conoscevo, e ho confrontato le sue previsioni con quello che era successo davvero. Questo si chiama *backtest*, ed è l'unico modo onesto di valutare un previsore: lo fai indovinare "al buio" su date di cui tu sai già la risposta, e misuri quanto ci ha preso.

Come metro di paragone ho usato il metodo più banale del mondo, quello che usano tutti senza chiamarlo così: la **media mobile**. "Quanto ho venduto in media nelle ultime settimane? Ecco, prevedo quello." Se un modello da centinaia di milioni di parametri non riesce a battere una media, non vale la complessità che ti porti in casa. È un confronto spietato apposta.

## La sorpresa: non conta quanto vendi, conta se c'è un ritmo

La mia ipotesi di partenza era ragionevole e sbagliata: **"più una cosa vende, più dati ci sono, meglio il modello la prevede."** Sembra ovvio. È falso.

Ho scoperto che potevo avere due categorie di prodotti con un volume di vendite simile, e il modello stracciava la media mobile su una e pareggiava (o peggiorava) sull'altra. La differenza non era nella quantità. Era nella **forma** delle vendite nel tempo.

Faccio l'esempio più chiaro che ho trovato, senza numeri perché i numeri non sono il punto:

- Ci sono prodotti che vendi **a strappi irregolari**. Un cliente entra, ne compra in blocco, poi per un po' silenzio, poi un altro blocco. Il volume totale magari è alto, ma la sequenza è essenzialmente **rumore**: non c'è un domani che assomiglia all'ieri. Su questi il modello non ha niente da "capire", e infatti non batte la media.
- Ci sono prodotti che vendi con un **ritmo**. Un certo giorno della settimana sempre di più, un certo periodo dell'anno sempre di più. C'è una **struttura** — settimanale, stagionale — che si ripete. Qui il modello brilla: la cattura, la proietta in avanti, e ti dice "guarda che sta arrivando il picco" prima che la media mobile se ne accorga.

Il caso più bello è stato un prodotto stagionale: la media mobile, che guarda solo all'indietro, arriva sempre in ritardo sul cambio di stagione. Il foundation model invece *sa* che certe cose succedono in certi mesi, perché lo ha visto succedere in migliaia di altre serie durante l'addestramento. Su quella categoria ha **dimezzato** l'errore della media mobile — non per una questione di quantità, ma perché c'era un pattern stagionale forte da intercettare.

**La regola che mi porto a casa: un foundation model per le serie temporali rende dove c'è volume *e* struttura insieme. Il volume da solo non basta. Il ritmo è quello che conta.**

È un'intuizione che vale ben oltre il mio negozio. Se stai valutando uno strumento del genere per la tua attività, non chiederti "ho abbastanza dati?". Chiediti "le cose che vendo hanno una ciclicità riconoscibile?". Se sì, c'è terreno. Se vendi a sorpresa, nessun modello del mondo inventerà un ordine che nei dati non c'è.

## Due strumenti diversi travestiti da uno solo

C'è un secondo equivoco in cui sono caduto, e che vale la pena smontare perché è quello che mi ha sbloccato.

All'inizio cercavo un **ottimizzatore di magazzino**: volevo che il modello mi dicesse *quanto stock tenere* per non restare mai scoperto. È un problema legittimo, ma è difficile, perché lì non conta solo "quanto venderò in media", conta lo **scenario brutto**: il rischio di restare a zero proprio nella settimana in cui arriva il cliente. Su quel fronte il modello aiuta, ma solo in casi specifici e con cautele.

Poi ho cambiato domanda. Invece di "quanto stock?", ho chiesto semplicemente: **"quanto venderò / quanto lavoro avrò la prossima settimana?"** — una previsione di *domanda*, non di scorta. E lì il quadro è cambiato completamente: il modello è risultato un ottimo **previsore di domanda** su un sacco di categorie diverse, molte più di quante reggessero il discorso magazzino.

Sono due usi distinti, con due metri di giudizio diversi:

- **Ottimizzare lo stock** → la metrica è "quante volte resto scoperto e quanto magazzino immobilizzo". Caso stretto: pochi prodotti, quelli con la combinazione giusta.
- **Prevedere la domanda** → la metrica è semplicemente "quanto ci azzecca". Caso largo: tantissime categorie, anche quelle dove l'ottimizzazione di stock non aveva senso.

Capito questo, ho smesso di forzare lo strumento dentro il problema sbagliato. Il valore vero, per una realtà piccola, **non è il magazzino: è il report.**

## Il pezzo che uso davvero: i report previsionali automatici

Ecco la parte concreta, quella che è finita in produzione e che gira da sola.

Ho costruito due report automatici che si generano senza che io faccia nulla:

1. **Un report settimanale**, ogni lunedì mattina. Per le categorie ad alta rotazione mi dice la domanda attesa della settimana, con un secondo numero accanto — lo **scenario prudente** — che risponde alla domanda "e se va forte?". Per i servizi a ritmo settimanale netto mi dà anche la distribuzione per giorno, utile per organizzare il lavoro e i ritiri.

2. **Un report mensile stagionale**, il primo del mese. Qui ci finiscono le categorie che si muovono sui tempi lunghi, dove ha senso ragionare a mese e non a settimana — quelle dove la stagionalità è l'informazione principale.

Entrambi mi arrivano dritti sul mio canale interno, formattati e leggibili in dieci secondi. Sotto il cofano: il modello gira di notte su una scheda grafica di casa (costo per previsione: praticamente zero), legge lo storico, calcola, impacchetta il messaggio e lo manda. Io la mattina ho già davanti "questa settimana ti aspetti più o meno questo, occhio che questa categoria è in salita".

Non è fantascienza e non è nemmeno difficile, una volta che hai capito **cosa** chiedere allo strumento. È esattamente il tipo di lavoro noioso e ripetitivo — *guardare i numeri e proiettarli in avanti, ogni settimana, per ogni categoria* — che una macchina fa meglio e senza stancarsi, liberando la mia testa per le decisioni che contano.

## Un paio di trappole tecniche (per chi ci si mette)

Due cose che mi hanno quasi fregato, nel caso vogliate provarci:

- **Il confronto deve essere onesto.** È facilissimo far sembrare il modello un genio confrontandolo con una media mobile mal tarata. Bisogna metterli sullo stesso piano, con le stesse regole del gioco, altrimenti il "vince l'AI" è solo un artefatto del test. Metà del mio lavoro è stato costruire un confronto equo, non far girare il modello.
- **Lo scenario prudente non si somma.** Quando vuoi una previsione "cautelativa" su più settimane di fila, la tentazione è sommare lo scenario brutto di ogni settimana. È un errore: significa assumere che vadano male *tutte insieme*, cosa statisticamente improbabile. Il rischio si diversifica nel tempo, e se non lo tieni in conto finisci per ordinare il doppio del necessario. È una sottigliezza matematica che, su un ordine vero, sono soldi.

## Cosa mi porto a casa

La lezione grossa non è "l'AI prevede le vendite". È più precisa, e più utile:

> Un foundation model per le serie temporali è uno strumento potente e ormai **alla portata di una piccola impresa**, ma rende solo se gli chiedi la cosa giusta sulla categoria giusta. Cerca il **ritmo**, non il volume. Usalo come **previsore di domanda** prima ancora che come ottimizzatore di magazzino. E mettilo a produrre **report automatici**, perché è lì — nel lavoro ripetitivo tolto dalle spalle — che il valore si sente davvero.

Per anni la previsione della domanda è stata un lusso da grande distribuzione. Oggi gira gratis su una scheda grafica e ti manda un messaggio il lunedì mattina. Vale la pena capirla, anche se hai un negozio solo.

## Per ora è tutto sul campo

Una precisazione doverosa: quello che ho raccontato è appena entrato in funzione. I report girano, le previsioni arrivano — ma siamo nella **fase di test vero**, quella che conta più di qualsiasi backtest: confrontare settimana dopo settimana ciò che il modello aveva previsto con quello che è successo davvero, sul campo.

Quindi prendete questo post per quello che è — il punto di partenza, non il verdetto. Tra qualche mese conto di scrivere un seguito onesto: **cosa ha funzionato, cosa no, e soprattutto se il modello ci ha azzeccato davvero** — statisticamente, numeri alla mano, non a impressione. Quella è l'unica prova che vale. Ci si rilegge allora.
