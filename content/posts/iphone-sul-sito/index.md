+++
title = "Ho messo gli iPhone sul sito. Poi il database ha deciso di andare in pensione."
date = '2026-07-25T10:00:00+02:00'
draft = false
summary = "Per anni gli iPhone sono stati un prodotto da negozio fisico. Poi ho provato a metterli online e ho scoperto che il problema non è il cliente: sono io che devo inseguire prezzi, stock e un database che muore nel momento peggiore."
tags = ["iphone", "woocommerce", "ecommerce", "retail", "automazione", "sync"]
categories = ["tech-tips"]

[cover]
  image = "cover.png"
  alt = "Uno smartphone che emerge da un flusso di dati e linee di codice, stile tecnologico minimale"
  caption = "Quando il prodotto fisico incontra la logica digitale"
  relative = true
+++

Per anni da PrezziSmart gli iPhone se li comprava solo venendo in negozio. Non che volessimo nasconderli: è che tenere aggiornati prezzo e disponibilità di un prodotto così volatile sul sito era un incubo ricorrente. Ogni mattina il prezzo del fornitore poteva essere diverso, lo stock poteva sparire in un'ora, e noi rischiavamo di vendere qualcosa che poi non avevamo.

Quindi il sito restava senza iPhone. I clienti li cercavano, ci chiamavano, venivano in negozio. Funzionava, ma era un limite.

Questa settimana ho deciso di chiudere il buco.

## Perché gli iPhone sono diversi dagli altri prodotti

Nel negozio fisico la regola è semplice: il cliente chiede, io controllo il listino del giorno, ordino se serve, consegno quando arriva. Sul sito invece il cliente si aspetta tutto e subito: prezzo visibile, disponibilità immediata, spedizione gratuita, possibilità di pagare a rate.

Se uno di questi elementi manca o è storto, l'utente abbandona. E se il prezzo è troppo alto rispetto alla grande distribuzione, ti marchia come caro. Se è troppo basso, tu ci perdi. Non c'è margine per errori grossolani.

Gli iPhone inoltre hanno una complicazione in più: il fornitore B2B non ti dice quanti ne ha in magazzino. L'API risponde solo "disponibile" o "non disponibile". Per un e-commerce abituato a gestire giacenze numeriche, è come guidare con il contagiri rotto.

## Il primo approccio: sincronizzare tutto in automatico

Ho progettato e messo in funzione, attraverso lo sviluppo assistito dall'IA, uno script che ogni mattina e ogni pomeriggio:

1. preleva i modelli iPhone dal catalogo del distributore;
2. confronta i prezzi con quelli della grande distribuzione online;
3. sceglie il prezzo di vendita più equilibrato possibile;
4. verifica la disponibilità reale aprendo la pagina prodotto con un browser headless;
5. pubblica (o nasconde) gli articoli su WooCommerce.

La parte più delicata è stato il punto tre. Non potevo semplicemente applicare un ricarico fisso: su alcuni modelli sarei stato troppo caro, su altri avrei bruciato margini inutilmente. Ho dovuto costruire una logica ibrida che tiene conto del costo effettivo e del prezzo di mercato, sempre arretrando sul più conservativo dei due.

Per lo stock, ho accettato il compromesso: niente quantità, solo sì/no. Se il modello è disponibile, resta visibile. Se finisce, scompare automaticamente. Due sincronizzazioni al giorno non sono il tempo reale, ma sono sufficienti per non vendere aria.

## Le aspettative del cliente online

Un prodotto "solo online" deve comportarsi da prodotto online. Ho aggiunto quindi la spedizione gratuita per tutti gli iPhone e la possibilità di pagare a rate. Quest'ultima è stata la più rognosa: i plugin ufficiali dei servizi di rate richiedono un contratto e-commerce diretto che al momento non abbiamo. La soluzione è un gateway custom che genera l'ordine in stato "in attesa" e poi invia al cliente il link di pagamento a rate generato manualmente.

Non è elegante, ma funziona. E nel nostro caso funzionare conta più dell'eleganza.

Ho anche dovuto correggere la homepage del sito. Dopo la pubblicazione dei primi cinquanta modelli, la pagina iniziale rischiava di diventare un muro di iPhone. Per fortuna WooCommerce permette di limitare quanti prodotti per categoria mostrare: adesso la home è di nuovo un negozio di elettrodomestici, non un corner Apple.

## Il colpo di scena: il database muore

Tutto sembrava funzionare. Poi, il giorno dopo, lo script ha smesso di scrivere.

Il database di WordPress — quello su cui gira prezzismart.it — era diventato read-only. Scaduto. Non aveva più spazio per scrivere ordini, prodotti, aggiornamenti. Immaginate di svegliarvi e scoprire che il vostro registratore di cassa registra solo a memoria: i dati entrano, ma non si salvano.

La causa era banale: il piano hosting del database aveva una scadenza che non avevo segnato. Quando è scaduto, il provider ha bloccato le scritture invece di chiudere tutto. Sintomo classico: le pagine si vedono, ma nessuna modifica resta.

## La migrazione di emergenza

Ho dovuto:

1. individuare il database attivo e quello scaduto;
2. esportare le tabelle di WordPress dal database bloccato;
3. importarle nel nuovo database attivo;
4. aggiornare `wp-config.php` e tutti gli script che puntavano al vecchio;
5. escludere gli iPhone dalla sincronizzazione con il gestionale desktop, perché altrimenti il gestionale li avrebbe continuamente segnati come esauriti.

L'ultimo punto è fondamentale. Il gestionale del negozio (Danea) gestisce il magazzino fisico. Gli iPhone "solo online" invece hanno una logica a sé: il loro stock dipende dal fornitore, non da quello che c'è in negozio. Se avessi lasciato che Danea li sincronizzasse, il sito avrebbe continuato a mostrare e nascondere prodotti a caso.

## Cosa ho imparato

1. **Il prezzo online non è il prezzo del negozio.** Non puoi prendere il listino e aggiungere una percentuale fissa. Serve una logica che guardi sia il costo sia il mercato.
2. **Lo stock binario è meglio dello stock finto.** Se non hai la quantità numerica, è più sicuro dire "sì/no" che inventare giacenze.
3. **Le scadenze del database contano come backup.** Una scadenza dimenticata può bloccare il negozio più di un attacco informatico. Ora le ho tutte in calendario.
4. **I prodotti "online only" devono essere isolati dal gestionale fisico.** Altrimenti due mondi che dovrebbero collaborare finiscono per combattere.
5. **La homepage è un bene comune.** Pubblicare tanti prodotti di una categoria sola rischia di deformare la percezione dell'intero negozio. Il cap per categoria è un'arma potente.

---

Gli iPhone sono finalmente online. Non tutto è perfetto, ma tutto è misurabile. E in un negozio, misurabile è molto meglio di perfetto.

Se gestite un e-commerce con prodotti a stock volatile, controllate le scadenze del database. E non fidatevi mai di un "sembra funzionare" il giorno prima di un weekend.
