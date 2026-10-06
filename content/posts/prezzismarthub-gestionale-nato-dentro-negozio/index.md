+++
title = "PrezziSmartHub, il gestionale nato dentro al negozio"
date = '2026-10-06T10:00:00+02:00'
draft = false
slug = "prezzismarthub-gestionale-nato-dentro-negozio"
summary = "PrezziSmartHub è nato da una spedizione e oggi tiene insieme contabilità, magazzino, e-commerce, fornitori e vendite: il negozio visto dai suoi dati."
description = "Il racconto di come PrezziSmartHub unisce spedizioni, Danea, e-commerce, contabilità, fornitori e analisi vendite dentro il negozio."
tags = ["prezzismart", "prezzismarthub", "gestionale", "retail-tech", "danea", "woocommerce", "spedizioni", "contabilita", "magazzino", "automazione", "ai"]
categories = ["tech-tips"]

[cover]
  image = "cover.png"
  alt = "Illustrazione di un banco di negozio con pacchi, lettore di codici, POS e computer collegati a pannelli digitali di vendite, magazzino e spedizioni."
  caption = "PrezziSmartHub: dal lavoro al banco a una visione collegata del negozio."
  relative = true
+++

Ogni tanto qualcuno mi chiede che cos'è davvero PrezziSmartHub, e la risposta facile sarebbe dire che è il mio gestionale interno, una web app in Flask, un pannello con dentro spedizioni, contabilità, Danea, e-commerce, fornitori, clienti, statistiche e tutte quelle parole belle che sembrano fare subito ordine. Però sarebbe una risposta troppo pulita, e soprattutto sarebbe una risposta falsa, perché l'Hub non è nato come nascono i software nei racconti ordinati, quelli in cui prima fai l'analisi, poi il progetto, poi lo sviluppo, poi il rilascio.

PrezziSmartHub è nato al contrario: è nato perché in negozio c'era una cosa che mi faceva perdere tempo, poi un'altra che mi faceva perdere lucidità, poi un'altra ancora che mi faceva prendere decisioni guardando solo un pezzo del quadro, e a un certo punto mi sono accorto che non stavo costruendo un programma, stavo costruendo un modo per non farmi schiacciare dalla quantità di micro-decisioni che ogni giorno passano sopra al banco senza fare rumore.

La prima versione, quella più grezza, [l'ho già raccontata](/posts/da-script-a-gestionale-prezzismarthub/): tutto era partito dalle spedizioni, da quei minuti buttati a ricopiare nomi, indirizzi, telefoni, pesi, misure, date di ritiro, costi e codici dentro portali che non si parlavano tra loro. Sembrava un problema piccolo, quasi noioso da raccontare, ma chi lavora in negozio lo sa: quando una cosa piccola si ripete tutti i giorni, diventa struttura, e quando una struttura è storta ti mangia attenzione anche se sulla carta ti porta via solo cinque minuti.

Oggi le spedizioni dentro l'Hub non sono più soltanto un modo per generare un file o stampare una lettera di vettura. Sono diventate un flusso intero, con i servizi di spedizione e i corrieri, preventivi, storico, tracking, messaggi ai clienti, report, prezzi cliente, costi reali quando disponibili, fasce Danea, vendite collegate e perfino il pezzo fiscale che va trattato con i piedi di piombo, perché tra un bottone comodo e una scrittura sbagliata passa una linea sottile che non voglio attraversare per fretta. Il punto, però, non è dire "guarda quante integrazioni". Il punto è che adesso una spedizione non è più un evento isolato: è un pezzo di lavoro che nasce dal banco, passa dal cliente, tocca la contabilità, può diventare vendita, può generare un messaggio, può finire in un report, e soprattutto resta dentro una storia leggibile.

Questa è la differenza enorme tra un gestionale comprato e un gestionale cucito addosso: il primo ti chiede di adattarti al suo modo di ragionare, il secondo può crescere esattamente nel punto in cui il tuo lavoro ti fa attrito.

## Danea non è il nemico, è il vecchio centro di gravità

Per anni Danea è stato il centro di gravità del negozio, e sarebbe stupido raccontarlo come se fosse soltanto un ostacolo. Dentro Danea ci sono prodotti, clienti, documenti, costi, storico, abitudini, codici che uso da anni, e soprattutto c'è una parte di realtà che non si può riscrivere con leggerezza solo perché oggi ho un'interfaccia più moderna. Quando leggo certi racconti di trasformazione digitale mi sembra sempre che il software nuovo debba arrivare con la ruspa, buttare giù tutto e ricominciare. Nella vita vera di una piccola attività non funziona così, o almeno io non voglio che funzioni così.

L'Hub, in questa fase, fa una cosa più delicata: legge, collega, traduce, confronta, mette in sicurezza, e solo dove ha senso costruisce un'alternativa. Danea continua a essere la fonte ufficiale per molte cose, mentre PrezziSmartHub ci gira attorno come una cabina di regia che prende quei dati e li rende utilizzabili nel lavoro di ogni giorno. Il catalogo del sito WooCommerce, per esempio, non nasce da un caricamento manuale fatto una volta e poi dimenticato: viene alimentato dal mondo Danea, con prezzi, stock, EAN, categorie, foto, regole di arrotondamento, sincronizzazioni e controlli. L'e-commerce, a sua volta, non è un'isola appesa su Aruba: è collegato al negozio, alle spedizioni, agli ordini, alle descrizioni prodotto, al modo in cui decidiamo cosa mostrare e cosa tenere aggiornato.

Questa è una delle parti meno visibili ma più importanti. Il cliente vede un prodotto online, un prezzo, una pagina spedizioni, un modulo, magari un volantino o una categoria ordinata meglio. Dietro, però, c'è il problema vero: se il sito dice una cosa, il banco ne sa un'altra e Danea ne conserva una terza, prima o poi qualcuno paga quell'incoerenza. Di solito la pago io, con tempo, telefonate, correzioni, dubbi e quella sensazione bruttissima di non sapere quale schermata sia quella vera.

PrezziSmartHub serve anche a questo: a far sì che il negozio abbia meno verità sparse.

## La contabilità, cioè la parte in cui il negozio smette di raccontarsela

La contabilità è il punto in cui il romanticismo della tecnologia finisce abbastanza in fretta. Puoi avere il sito bello, la grafica sistemata, il gestionale su misura e tutte le automazioni del mondo, ma se a fine giornata non sai leggere con chiarezza cosa è entrato, cosa è uscito, cosa è incassato davvero e cosa invece è solo movimento apparente, stai guidando con il vetro appannato.

Per anni ho avuto un foglio Excel enorme, cresciuto dal 2017 in poi, con le sue regole, le sue colonne, le sue eccezioni, i suoi mesi importati a mano, i suoi adattamenti successivi. Non lo rinnego, perché mi ha tenuto in piedi, ma a un certo punto il foglio non bastava più. Avevo bisogno che il registro del giorno parlasse con la chiusura fiscale, con le fatture, con i movimenti di cassa, con Sella, Fineco, i finanziamenti, le spese del negozio, le spese personali da separare, le scadenze, i pagamenti fornitori e quei pacchi che sembrano sempre marginali finché non devi capire quanto ti sono costati davvero.

Dentro l'Hub la contabilità non è stata pensata come un modulo da commercialista, ma come il cruscotto mentale di chi la sera deve capire se la giornata ha senso. Entrate, uscite, incassi, contanti, POS, bonifici, finanziamenti, scadenze ricorrenti, pagamenti già saldati in Danea, export fiscale, saldi di cassa e banca: tutto questo non mi interessa perché voglio guardare numeri per passione, mi interessa perché il numero giusto al momento giusto cambia la decisione del giorno dopo. Se vedo solo il fatturato, posso illudermi; se vedo solo le uscite, posso deprimermi; se vedo incassi, fiscalità, scadenze, netto e andamento nello stesso posto, inizio almeno a ragionare.

E ragionare, per una piccola attività, non è un vezzo. È una forma di sopravvivenza.

## Vendite, magazzino immobile e quella domanda che fa male

Il tema delle vendite è più sporco di come lo raccontano i dashboard belli. Non basta sapere quanto hai venduto, perché due mesi con lo stesso fatturato possono essere due mesi completamente diversi: uno magari è fatto di prodotti con margine buono, accessori, servizi, rotazione sana e cassa che respira; l'altro può essere pieno di vendite grandi che muovono volume ma lasciano poco, oppure di merce che esce tardi, dopo essere stata ferma mesi, già pagata, già invecchiata, già diventata peso psicologico oltre che economico.

Il magazzino immobile è una delle cose più difficili da guardare con onestà, perché sulla carta sembra valore: scaffali pieni, prodotti presenti, roba comprata, merce fisica. Ma se quella merce non gira, se non porta clienti, se occupa spazio e attenzione, se mi fa sentire più tranquillo solo perché posso vederla lì, allora non è ricchezza, è cassa congelata. E in un negozio piccolo la cassa congelata non è un concetto astratto: è un pagamento fornitore che arriva, una promo che non puoi fare, un prodotto nuovo che non compri perché hai ancora il vecchio che ti guarda dallo scaffale.

Per questo l'Hub sta andando sempre più verso un'analisi delle vendite che non si fermi al totale del mese. Mi serve capire cosa si muove, cosa resta fermo, cosa porta margine, quali categorie hanno senso, quali prodotti hanno bisogno di essere spinti, liquidati, accoppiati a un servizio o semplicemente smessi di comprare. Mi serve collegare lo storico Danea, il gestionale nuovo, le vendite al banco, i resi, il magazzino pilot, i documenti e la marginalità in un modo che non cancelli il passato ma non mi costringa nemmeno a vivere per sempre dentro i limiti del passato.

Anche qui, il punto non è fare il gestionale perfetto. Il punto è smettere di lavorare a sensazione quando i dati, se messi in fila, possono dirmi una verità più utile della mia memoria.

## Fornitori e competitor: non per fare la guerra dei prezzi, ma per non combattere bendati

Una delle funzioni più recenti e più concrete dell'Hub è la ricerca fornitori. Dal banco posso cercare un prodotto e interrogare più sorgenti B2B insieme, con prezzi netti, disponibilità, link, IVA, trasporti dove noti e regole specifiche per fornitori diversi. Non è una magia: è un modo per non aprire otto portali, fare otto login, cercare otto volte la stessa cosa e poi provare a ricordarmi quale prezzo era netto, quale era lordo, quale includeva trasporto e quale invece sembrava conveniente solo perché mancava un pezzo.

Accanto ai fornitori c'è la ricerca competitor, con il confronto tra marketplace e catene di distribuzione per capire dove si trova il mercato in quel momento. Anche qui bisogna stare attenti alla tentazione più pericolosa: se guardi i competitor solo per inseguire il prezzo più basso, hai già perso. PrezziSmart non può vivere facendo la guerra dei volumi a chi compra con scale completamente diverse. Però non posso nemmeno ignorare il prezzo, perché il cliente lo vede, lo confronta, arriva al banco già con uno screenshot in mano e si aspetta che io sappia di cosa stiamo parlando.

Quindi l'Hub mi serve per una cosa più matura: sapere quando posso essere competitivo, quando devo spiegare il valore del servizio, quando ha senso ordinare da un fornitore invece che da un altro, quando un prodotto è da evitare perché il margine non regge, e quando invece posso usare il prezzo come esca intelligente senza trasformare tutto il negozio in una corsa al ribasso.

La differenza è sottile ma decisiva. Non voglio un software che mi dica solo "costa meno lì". Voglio un sistema che mi aiuti a capire se quella vendita, per il negozio, ha senso.

## Il collegamento con Crescita

Il progetto Crescita è nato da una frase molto meno tecnica e molto più personale: a fine mese mi sentivo spesso deluso, come se stessi lavorando tanto senza riuscire mai a vedere chiaramente il progresso. Poi, quando abbiamo iniziato a mettere i numeri in fila, è venuto fuori che la sensazione non raccontava tutta la storia: c'erano stagionalità fortissime, mesi fisiologicamente più deboli, confronti anno su anno più interessanti di quanto sembrassero a pelle, e soprattutto mancava il dato più importante, cioè il margine reale rispetto ai costi fissi.

Questa cosa per me è stata uno spartiacque. Perché finché guardi il mese come un voto morale, ogni calo sembra una bocciatura e ogni salita sembra una tregua. Quando invece lo guardi come un sistema, inizi a chiederti domande migliori: che scontrino medio ho avuto, quanti clienti sono entrati, quali prodotti hanno mosso margine, quali spese mi hanno assorbito, quanto devo fare per coprire la base, quali leve posso usare senza bruciarmi, dove devo smettere di buttare energia.

PrezziSmartHub è il braccio operativo di quel progetto. Crescita è la domanda, l'Hub è il posto in cui provo a costruire le risposte. Non risposte motivazionali, non "bisogna crederci", non il post LinkedIn con la foto del caffè e la frase sul mindset. Risposte fatte di dati, scadenze, vendite, incassi, magazzino, fornitori, competitor, sito, spedizioni, clienti, recensioni, servizi, stagionalità e piccoli strumenti che, messi insieme, possono cambiare il modo in cui decido.

Forse questa è la parte che mi interessa di più: non sto costruendo PrezziSmartHub perché sogno di diventare una software house. Lo sto costruendo perché il negozio è una macchina complessa, e per anni ho provato a tenerla in equilibrio con attenzione, memoria, fogli, abitudini e forza bruta. A un certo punto la forza bruta non basta più, o comunque costa troppo. Serve un sistema che osserva con me, che mi ricorda le cose, che collega i pezzi, che non mi lascia solo davanti alla sensazione del momento.

## Il gestionale perfetto non esiste, però esiste quello che cresce con te

Se oggi dovessi vendere PrezziSmartHub come prodotto, probabilmente sbaglierei tutto, perché lo riempirei di schermate e moduli e parole tecniche. In realtà la sua qualità più importante è molto meno appariscente: è nato dentro al negozio, mentre il negozio lavorava, e ogni pezzo è stato aggiunto perché c'era un problema reale che tornava a bussare.

Questo lo rende imperfetto, inevitabilmente. Ci sono parti mature e parti in test, moduli che leggono soltanto e altri che scrivono, zone dove Danea resta il padrone e zone dove il gestionale nuovo inizia a respirare da solo, flussi già quotidiani e flussi ancora da trattare con guanti spessi. Ma preferisco questa imperfezione viva a un software perfettamente ordinato che mi costringe a tradurre il mio lavoro nel linguaggio di qualcun altro.

PrezziSmartHub, per come lo vedo oggi, è questo: non un pannello di controllo per sentirmi moderno, ma un tentativo molto concreto di trasformare il caos quotidiano di una piccola attività in informazioni utilizzabili. È il punto in cui spedizioni, contabilità, e-commerce, Danea, magazzino, fornitori, competitor e crescita smettono di essere reparti separati nella mia testa e diventano una sola conversazione.

E forse il vero salto è proprio qui: quando il negozio smette di essere una somma di urgenze e comincia, finalmente, a parlare una lingua che riesco a leggere.
