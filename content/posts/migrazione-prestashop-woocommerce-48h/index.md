+++
title = "Ho rifatto l'e-commerce del negozio in 48 ore, e il sito era la parte facile"
date = '2026-06-27T09:00:00+02:00'
draft = false
summary = "Il vecchio sito girava su PrestaShop da dieci anni, con un catalogo fermo al 2015. L'ho rifatto da zero in un weekend. Ma la migrazione vera non erano i prodotti: era insegnare a due sistemi — il gestionale e il sito — a dirsi la stessa verità. E lì sono cominciati i guai veri, quasi tutti dove non stavo guardando."
tags = ["woocommerce", "prestashop", "ecommerce", "migrazione", "self-hosting", "piccole-imprese"]
categories = ["tech-tips"]

[cover]
  image = "cover.png"
  alt = "Due schermi affiancati: a sinistra un vecchio e-commerce rosso e datato, a destra uno shop moderno blu e bianco, collegati al centro da un flusso di dati luminoso"
  caption = "La migrazione facile è quella che si vede. Quella vera è il filo in mezzo."
  relative = true
+++

Il sito del negozio girava su PrestaShop da una decina d'anni. Era online, accettava ordini, mostrava prodotti — quindi sulla carta "funzionava". Ma il catalogo era fermo a un'epoca geologica fa: roba del 2015-2017, prezzi vecchi, articoli che non vendo più da anni. Un e-commerce che era diventato un cartellone pubblicitario scolorito: c'era, ma non rappresentava più niente di quello che succede davvero in negozio.

Il problema vero, però, non era PrestaShop. Era che il sito e il magazzino vivevano in due mondi separati. Il catalogo reale — quello con i prezzi giusti, le giacenze vere, le offerte della settimana — sta nel gestionale desktop con cui faccio fatture e magazzino. Il sito era un'isola. Ogni allineamento andava fatto a mano, e quindi non si faceva mai.

Mi sono dato un weekend per rifarlo. Quello che ho imparato è che spostare la vetrina è la parte facile e veloce. Il lavoro vero, quello che ti mangia le ore, è costruire il filo che tiene insieme gestionale e sito — e fare in modo che si dicano la verità anche quando uno dei due mente.

## Prima decisione: buttare, non migrare

La tentazione del migrante è salvare tutto. "Sono dieci anni di prodotti, esportiamoli e reimportiamoli, non si sa mai." Sbagliato.

Sono andato a guardare il vecchio catalogo con freddezza: la stragrande maggioranza degli articoli era morta. Roba fuori produzione, prezzi del 2016, schede senza foto. Tenerli avrebbe solo diluito tutto: un negozio con quattrocento fantasmi sembra un negozio che non sa cosa vende. Quasi quattrocento prodotti non sono finiti nel sito nuovo. Spariti.

Il valore SEO di un sito vecchio, mi sono convinto, **non sta nei prodotti**: sta nel dominio anziano e nelle categorie evergreen. "Lavatrici", "frigoriferi", "condizionatori" valgono — gli articoli specifici del 2016 no. Quindi: redirect 301 puntati sulle categorie, non sui singoli prodotti morti. Quasi cinquecento regole di redirect per dire a Google "quel prodotto non c'è più, ma la categoria sì, vai lì". I prodotti morti puntano alla loro categoria genitore, non uno-a-uno. Tenere in vita un redirect per ogni cadavere sarebbe stato lavoro inutile per preservare traffico che non esisteva.

Il catalogo nuovo non l'ho esportato da PrestaShop. L'ho generato dal gestionale, prendendo **solo gli articoli con giacenza positiva**: cinquecentoquaranta prodotti vivi, con prezzo e categoria reali. Non una migrazione del vecchio, una ricostruzione dal sistema che dice la verità sul magazzino.

## La sync facile e la sync di fino

La prima notte ho buttato dentro i cinquecentoquaranta prodotti e ho guardato la vetrina popolarsi. Sembrava fatto. **Non era fatto per niente.** Era fatto il dieci per cento. Il novanta per cento del lavoro è quello che chiamo la *sync di fino*: far sì che ogni singolo dettaglio — il prezzo al centesimo, la foto giusta, l'offerta, l'esclusione — sia corretto e si aggiorni da solo. È lì che un e-commerce sembra serio o sembra finto.

E la sync di fino è una caccia a problemi subdoli, quasi tutti con la stessa beffa: la causa non era mai dove la cercavo.

**I prezzi arrotondati come un macellaio.** Il gestionale tiene i prezzi netti, il sito li vuole ivati. Facile. Solo che la prima notte li avevo arrotondati all'euro intero, e in vetrina venivano fuori cifre da discount finto. La regola giusta è arrotondare per eccesso al decimo di euro — quei `,90` e `,50` che fanno la differenza tra "prezzo pensato" e "prezzo a caso". Una riga di codice, ma è il tipo di dettaglio che un cliente nota in mezzo secondo anche senza saperlo.

**Le foto che non si caricavano mai.** Questa mi ha fatto perdere il pomeriggio. Mettevo i file immagine al posto giusto, con il nome giusto, validi — e sul sito zero. Nessuna foto. Nessun errore. Il file c'era, era buono, e non arrivava. Ho controllato i permessi, il formato, il percorso, il plugin. Tutto a posto. Tutto fermo.

La causa era di una stupidità che fa male: nello script, la funzione che prepara le immagini girava *dopo* quella che salvava i dati da spedire al sito. Tradotto: il pacchetto che arrivava al sito veniva sigillato prima che le foto ci venissero infilate dentro. Il controllo "questa foto esiste?" rispondeva sempre no — non perché la foto mancasse, ma perché veniva aggiunta un istante troppo tardi. Analizzando il problema con l'assistente IA abbiamo individuato l'ordine errato e fatto invertire i due blocchi: di colpo si sono caricate tutte. Mezza giornata per l'ordine sbagliato di due operazioni.

**I tredici frigoriferi gemelli.** In vetrina, metà dei frigoriferi mostrava la stessa identica foto: un combinato bianco generico, tredici volte. Il primo istinto è dare la colpa al sito o alla sync. Sbagliato di nuovo. La sync faceva il suo dovere alla perfezione: copiava fedelmente quello che trovava. Il problema era alla *sorgente* — sul disco di rete c'erano tredici file con tredici nomi diversi che però erano fisicamente la stessa immagine placeholder. La sync è onesta e stupida: se le dai spazzatura tredici volte, te la mostra tredici volte, ordinatamente. Ho dovuto recuperare le foto vere altrove e sostituirle a monte. La lezione me la sono segnata: quando l'output è sbagliato, il sospetto non va quasi mai sul tubo, va sulla fonte.

**Le marche che non esistevano.** Volevo filtrare i prodotti per marca — Samsung, Bosch, LG. Peccato che nel gestionale la marca non sia un campo: è annegata dentro il nome del prodotto, scritta in venti modi diversi. Niente colonna pulita da leggere. Ho dovuto scrivere un estrattore che pesca il brand dal testo della descrizione con una sessantina di pattern. Non elegante, ma è la realtà di lavorare con dati nati per fare fatture, non per popolare un e-commerce.

**I due sync che si pestavano i piedi.** A un certo punto la sincronizzazione cominciava bene e poi crollava a metà con un errore secco. Causa: partivano due sincronizzazioni in parallelo — una mia a mano, una automatica dal pannello — e si contendevano lo stesso file di lavoro. La prima a finire lo cancellava, la seconda lo cercava e non lo trovava più. Risolto con un lucchetto: se una sync è già in corso, la seconda si fa da parte invece di schiantarsi. È lo stesso identico errore che avevo già fatto mesi prima su un altro pezzo del sistema. Alcune lezioni le impari due volte.

**Il pannello che si piantava su "Caricamento…".** Dopo decine di test, il cruscotto da cui lancio la sync restava a fissare il vuoto. Anche in incognito. La sync in realtà girava — era il pannello a non riuscire più a leggerne lo stato, perché lo interrogava ogni cinque secondi e dopo un po' di giri il server lo zittiva con un "troppe richieste". Un limite anti-abuso che avevo messo io, che si ritorceva contro un componente legittimo. Esentato quell'endpoint, tutto è tornato a respirare.

**Il sync che leggeva ieri.** L'ultima, sottile: il pulsante "sincronizza" lavorava su una copia vecchia dei dati del gestionale. Premevo, ed esportava prezzi di stamattina mentre al banco ne avevano già cambiato uno. Ho dovuto aggiungere un passo zero: prima di sincronizzare, va' a prendere l'ultima versione fresca dei dati del negozio. Sembra ovvio detto così, ma è esattamente il tipo di anello mancante che fa sembrare un sistema "quasi giusto" — la cosa peggiore che un sistema possa sembrare.

## La cosa controintuitiva che ho dovuto accettare

Le offerte mi hanno costretto a un ragionamento che all'inizio sembrava al contrario. Volevo il prezzo barrato: vecchio prezzo sbarrato, prezzo scontato in evidenza.

L'istinto dice: metto lo sconto in un campo "offerta". E invece la logica giusta è l'inverso, dettata da un vincolo fisico del negozio: **la cassa batte sempre il prezzo dal listino principale.** Quindi il prezzo *scontato* deve stare lì, nel listino che usa la cassa, altrimenti allo scontrino il cliente paga la cifra sbagliata. È un secondo listino, normalmente vuoto, a tenere il prezzo *pieno* — quello che sul sito diventa il numero sbarrato.

Suona alla rovescia finché non lo guardi dal punto di vista giusto: non è il sito a comandare, è la cassa. Il sito si adatta al banco, non viceversa. Una volta accettato quello, tutto è andato a posto da solo, e adesso le offerte si accendono e si spengono direttamente dal gestionale, senza che io tocchi mai il sito.

## Lo switch: l'anticlimax

Dopo tutto questo, il momento che temevo — mandare online il sito nuovo al posto del vecchio — è durato meno di un minuto. Vecchio sito in manutenzione, redirect attivi, cambio del puntamento, fatto. Nessun downtime percepibile. La sitemap a Google, confermata in pochi giorni.

È sempre così: la parte che fa paura sulla carta — il cutover — è quella più controllata, perché ci arrivi solo quando tutto il resto è già verificato. La parte che ti distrugge è quella che sottovaluti, l'allineamento dei dettagli, che non fa scena ma è dove vive il novanta per cento del lavoro.

## Cosa porto a casa

**Migrare un sito vecchio è prima di tutto decidere cosa è morto.** Il riflesso di "salviamo tutto per sicurezza" è il nemico: avrei trascinato quattrocento fantasmi nel sito nuovo e diluito esattamente le categorie che invece volevo far contare. A volte la migrazione migliore è una potatura.

**La sync che "vedo i prodotti" è il dieci per cento. La sync di fino è il resto.** Il prezzo arrotondato bene, la foto giusta, l'offerta che si accende dal gestionale, le due sincronizzazioni che non si ammazzano a vicenda: è quella roba lì che separa un e-commerce credibile da una vetrina finta. E non si vede finché non manca.

**Quando l'output è sbagliato, sospetta la sorgente, non il tubo.** I tredici frigoriferi gemelli, il sync che leggeva dati vecchi, le marche che non esistevano come campo: nessuno di questi era un problema del sito. Erano tutti problemi *a monte*, nel sistema che alimenta il sito. La sync è onesta e stupida — riflette fedelmente la verità che le dai, comprese le bugie. Se vuoi un sito che dice il vero, devi prima sistemare la fonte.

Il sito nuovo adesso si aggiorna da solo da quello che succede al banco. Ed è quella, non la grafica, la differenza vera tra prima e dopo: non un e-commerce più bello, ma un e-commerce *collegato alla realtà*. La vetrina era la parte facile. Il filo in mezzo era tutto il lavoro.
