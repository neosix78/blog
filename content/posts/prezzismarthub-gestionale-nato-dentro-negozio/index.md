+++
title = "PrezziSmart Hub: doveva essere un Excel, ma quell'Excel è durato veramente poco"
date = '2026-10-06T10:00:00+02:00'
draft = false
slug = "prezzismarthub-gestionale-nato-dentro-negozio"
summary = "Partito da un Excel per le spedizioni, PrezziSmart Hub è diventato un gestionale che mi aiuta a vedere il negozio e decidere meglio."
description = "Da un Excel a PrezziSmart Hub: spedizioni, Danea, e-commerce, entrate e uscite, magazzino e fornitori per capire il negozio e decidere meglio."
tags = ["prezzismart", "prezzismarthub", "gestionale", "retail-tech", "danea", "woocommerce", "spedizioni", "contabilita", "magazzino", "automazione", "ai"]
categories = ["tech-tips"]

[cover]
  image = "cover.png"
  alt = "Illustrazione di un banco di negozio con pacchi, lettore di codici, POS e computer collegati a pannelli digitali di vendite, magazzino e spedizioni."
  caption = "PrezziSmartHub: dal lavoro al banco a una visione collegata del negozio."
  relative = true
+++

Quando ho iniziato a lavorare a quello che oggi chiamo PrezziSmart Hub, l'idea era decisamente più semplice di quello che poi è diventato il progetto. Tutto nasceva da un'esigenza molto concreta: eliminare una serie di operazioni ripetitive che facevo quotidianamente in negozio e che, in qualche modo, ruotavano intorno a dei file Excel.

A pensarci oggi, però, quell'Excel è durato veramente poco. Quando apro PrezziSmart Hub ormai non vedo più nemmeno lontanamente il foglio di calcolo da cui ero partito, ma qualcosa di completamente diverso e, soprattutto, molto più grande di quello che avevo immaginato.

Credo che il momento preciso in cui me ne sono reso conto sia stato lavorando sulle spedizioni. All'inizio l'Hub preparava automaticamente l'Excel che prima compilavo a mano: io dovevo semplicemente caricarlo e avevo già risparmiato parecchio tempo. Era utile, certo, ma alla fine continuavo a fare lo stesso lavoro di prima, semplicemente in maniera più veloce.

Le cose sono cambiate quando ho iniziato a integrare direttamente API, fornitori e servizi esterni, fino ad automatizzare non più la preparazione di un file, ma praticamente l'intero flusso della spedizione, comprese operazioni successive e comunicazioni con il cliente. È stato allora che ho capito che non stavo più costruendo un modo più sofisticato per creare un Excel, ma qualcosa di completamente diverso.

## La parte che avevo sottovalutato

Quello che invece avevo decisamente sottovalutato è quanto possa essere complicato sviluppare un gestionale, anche quando il gestionale deve servire praticamente una sola azienda: la mia.

All'inizio il ragionamento era abbastanza semplice: non devo creare SAP, non mi servono millemila funzioni, implemento quelle che utilizzo io e buonanotte. Il problema è che una funzione apparentemente semplice, quando inizi a utilizzarla veramente, ne porta dietro quasi sempre altre.

Implementi qualcosa, la provi, trovi un caso che non avevi previsto, fai debug e lo sistemi; poi la utilizzi realmente in negozio e ti accorgi che manca una funzione che davi quasi per scontata, la aggiungi e scopri che deve comunicare con un'altra parte del sistema. Nel frattempo ti viene in mente un modo migliore di fare la stessa cosa e il traguardo, che pensavi fosse ormai vicino, si sposta nuovamente.

Probabilmente è questa la parte più frustrante dell'intero progetto, anche perché ho fretta di utilizzarlo e continuo ad avere la sensazione che manchi sempre qualcosa. Ma è contemporaneamente anche una delle parti più interessanti, perché molte delle funzioni migliori non sono nate mentre pensavo a cosa sviluppare, ma mentre utilizzavo davvero quello che avevo sviluppato.

E le dimensioni che ha raggiunto il progetto spiegano abbastanza bene quanto abbia sbagliato le previsioni iniziali: oggi parliamo di circa 137.500 righe tra Python, JavaScript, HTML, CSS e script, con 243 revisioni Git tra il 9 giugno e il 5 ottobre 2026. Numeri abbastanza ridicoli, se penso che tutto era partito dall'idea di rendere più veloce un Excel.

## Le cose che mi danno più soddisfazione

Le spedizioni rimangono probabilmente la parte alla quale sono più affezionato, anche perché sono state tra le prime funzioni sviluppate e oggi rappresentano uno dei flussi più collaudati. Ormai il lavoro riguarda spesso modifiche che potrei definire “di fino”, salvo poi rendermi conto che, quando una determinata operazione viene ripetuta continuamente, anche risparmiare pochi passaggi diventa tutt'altro che superfluo.

Un'altra soddisfazione enorme è stata l'integrazione tra Danea e l'e-commerce. Danea è il gestionale che utilizzo da anni ed è un sistema piuttosto chiuso, quindi arrivare al punto di inserire o modificare un articolo e, con un click, ritrovarmelo correttamente sul sito mi ha dato parecchia soddisfazione, soprattutto perché so cosa c'è dietro quel singolo click.

Ed è proprio in momenti come questi che sparisce velocemente quel pensiero che, ogni tanto, inevitabilmente arriva: ma non mi conveniva continuare a pagare il canone di Danea e buonanotte?

La risposta più razionale forse sarebbe: sicuramente avresti lavorato molto meno. Il problema è che poi apro PrezziSmart Hub, vedo quello che riesce già a fare e quel pensiero mi dura più o meno due secondi.

## Il vero salto: vedere quello che prima non vedevo

Con il tempo, però, mi sono accorto che la parte più interessante del progetto non è più soltanto l'automazione. PrezziSmart Hub sta iniziando soprattutto ad aiutarmi a capire meglio la mia stessa attività.

Questo ormai riguarda anche la parte economica: utilizzo l'Hub per registrare entrate, uscite e incassi, quindi non sto più guardando soltanto cosa vendo e cosa ho in magazzino, ma riesco ad avere nello stesso sistema una visione molto più completa di quello che succede realmente nel negozio.

Danea mi ha sempre dato una quantità enorme di numeri e informazioni, quindi il problema non era l'assenza dei dati, ma il modo in cui riuscivo a vederli. In un certo senso erano informazioni contemporaneamente visibili e invisibili: esistevano, ma non erano presentate nella maniera che serviva a me per prendere determinate decisioni.

Oggi riesco quindi ad avere una visione molto più chiara delle vendite, degli incassi e delle spese, a ragionare meglio su cosa comprare e in quali quantità e, soprattutto, riesco a vedere con molta più immediatezza il magazzino immobile, cioè quei prodotti che stanno semplicemente tenendo fermi dei soldi sugli scaffali.

Lo stesso discorso vale quando un cliente mi chiede qualcosa che non ho. Prima dovevo districarmi tra diversi fornitori, cercare il prodotto, confrontare disponibilità e costi e successivamente capire a quale prezzo lo stesse vendendo il mercato. Adesso ho integrato un sistema che mi permette di cercare quasi istantaneamente lo stesso articolo presso diversi fornitori e contemporaneamente avere una panoramica dei prezzi dei competitor.

Questo non significa necessariamente essere sempre quello che costa meno, cosa che oltretutto non avrebbe senso, ma sapere immediatamente se posso essere più economico, se posso allinearmi al mercato oppure se a quelle condizioni semplicemente non mi conviene acquistare quell'articolo.

## Dal fare più velocemente al decidere meglio

Quest'ultimo aspetto è probabilmente quello che oggi trovo più interessante, perché PrezziSmart Hub sta iniziando anche ad aiutarmi a capire se abbia senso oppure no effettuare determinati acquisti.

Parte di questa evoluzione è arrivata anche da un progetto parallelo, Crescita, una serie di libri e podcast che ho creato per approfondire economia, commercio, gestione del magazzino e altri argomenti utili alla mia attività. Diverse cose che ho imparato attraverso quel percorso sono finite nel mio modo di lavorare e alcune sono state trasformate direttamente in funzioni dell'Hub; altre so già che lo saranno in futuro.

Se dovessi quindi riassumere l'evoluzione del progetto, direi che sono passato dal voler automatizzare, al voler vedere, fino al voler decidere meglio.

Non voglio ovviamente che un software decida al posto mio come gestire il negozio, ma voglio che, quando devo prendere una decisione, riesca a mettermi davanti tutte le informazioni che mi servono nella maniera più chiara possibile.

Forse è proprio questo il motivo per cui devo rassegnarmi al fatto che PrezziSmart Hub non sarà mai veramente “finito”. Posso arrivare ad avere tutte le funzioni che oggi considero necessarie, ma continuerò a lavorare, a scoprire problemi, a imparare cose nuove e inevitabilmente a pensare: questa cosa potrei farla fare all'Hub.

È frustrante? A volte parecchio. Ho sottovalutato enormemente la quantità di lavoro necessaria? Assolutamente sì.

Ma quando penso da dove sono partito e guardo quello che ho davanti oggi, continuo a pensare che ne sia valsa la pena.

Doveva essere un'evoluzione di un Excel. Il problema, o forse la cosa più bella, è che quell'Excel è durato veramente poco.
