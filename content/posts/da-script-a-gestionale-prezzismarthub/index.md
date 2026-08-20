+++
title = "Da uno script per le spedizioni al cuore del negozio: storia del mio gestionale su misura"
date = '2026-08-20T11:00:00+02:00'
draft = false
summary = "È iniziato tutto con 50 righe di Python per non impazzire tra le etichette delle Poste. Oggi PrezziSmartHub è un sistema operativo completo che fa dialogare cassa, banche, logistica, telecamere AI e magazzino. E ora si prepara a pensionare il vecchio gestionale per far posto a un motore cucito al 100% sulla nostra realtà."
tags = ["self-hosting", "python", "flask", "docker", "danea", "automazione", "piccole-imprese", "prezzismart"]
categories = ["tech-tips"]

[cover]
  image = "cover.png"
  alt = "Un bancone di negozio con pacchi ed etichette collegato da flussi di dati digitali a un'interfaccia di controllo moderna e centralizzata"
  caption = "Partito per risolvere un fastidio al banco, diventato la cabina di regia dell'attività."
  relative = true
+++

C’è un momento preciso nella giornata di chi gestisce un’attività commerciale in cui ti fermi a guardare lo schermo del computer e ti accorgi del caos totale. 🤯

Avevo dieci schede aperte nel browser: il portale delle Poste da una parte, il sito dei corrieri dall’altra, il gestionale di cassa aperto, un foglio Excel dove cercavo di far quadrare i conti della sera e le ricevute dei POS sparse sulla scrivania. 

Ogni giorno la stessa solfa: ricopiare gli stessi indirizzi a mano, fare calcoli a mente per capire quanto avevamo davvero incassato tra contanti e carte, rincorrere le scadenze delle fatture sui foglietti volanti. Chi fa commercio lo sa bene: **passi più tempo a fare da "ponte umano" tra programmi che non si parlano tra loro che a dedicarti ai clienti o alle strategie di vendita.** ⏳

Un giorno mi sono detto: *basta, questo tempo devo riprendermelo*. 🛑

---

### Com’è iniziato tutto: un semplice script 💡

PrezziSmartHub non è nato da un piano industriale disegnato a tavolino da consulenti in giacca e cravatta. È nato da un fastidio pratico, minuscolo: **non volevo più perdere un quarto d'ora per compilare tre etichette di spedizione.**

Ho aperto un file vuoto e ho scritto poche righe di codice in Python per prendere i dati di un cliente, formattarli e creare il file pronto per le Poste. Funzionava alla grande. Era grezzo, graficamente spartano, ma faceva risparmiare minuti preziosi ogni singolo giorno. ⏱️

Solo che l'appetito vien mangiando. Quando vedi che il computer può toglierti di dosso una fatica ripetitiva, ti viene naturale chiederti: *cos’altro posso fargli fare?* 🤔

---

### La valanga: quando il codice incontra la realtà del bancone 🏪⚡

Da quel piccolo script è partita una reazione a catena inarrestabile:

* 📊 **I conti serali**: Ho costruito un modulo di contabilità interna per incrociare in automatico scontrini, fatture, contanti nel cassetto, carte su Banca Sella e versamenti su Fineco.
* 📦 **La logistica unificata**: Ho integrato Paccofacile, Poste e GLS sotto un unico tetto, con calcolo automatico dei costi e tracking senza dover aprire dieci siti diversi.
* 🏷️ **I margini delle promo**: Ho creato una cabina di regia per il volantino, per calcolare al centesimo i margini reali sui prodotti eroe prima di lanciarli.
* 👁️ **L'affluenza reale**: Abbiamo persino collegato le telecamere del negozio con l'AI per capire quante persone entrano fisicamente rispetto a quanti scontrini battiamo.

Giorno dopo giorno, notte dopo notte, quel piccolo script è cresciuto fino a diventare **PrezziSmartHub**. 🧠

---

### Il prossimo grande salto: addio a Danea, benvenuto Gestionale su misura 👋💼

Ed eccoci al passo più importante, quello a cui sto lavorando proprio in questo periodo. 

Per anni ho usato **Danea Easyfatt**. Un ottimo software commerciale standard, per carità, ma pur sempre un programma nato per il mondo desktop di vent'anni fa: rigido, chiuso nel suo database, pensato per una gestione generica e slegato dal web moderno. Per farlo parlare con la mia app Android del banco, con l'e-commerce o con gli scraper dei fornitori ho dovuto costruire ponti e sincronizzazioni continue. 🌉

A un certo punto mi sono fatto una domanda: *perché devo continuare ad adattare il mio modo di lavorare ai limiti di un software esterno, quando posso cucirmi addosso il MIO gestionale ideale?* ✂️🧵

Così dentro l'Hub sta prendendo forma il nostro **nuovo modulo gestionale proprietario**. 
Un sistema progettato millimetro per millimetro sulle esigenze reali del mio negozio:
* Scansione istantanea dei codici a barre direttamente dallo smartphone o dal banco 📱🔍
* Gestione nativa delle normative che toccano da vicino il nostro settore (Reverse Charge sulla telefonia, gestione schede RAEE per il ritiro dei grandi elettrodomestici) ♻️
* Confronto immediato dei listini B2B tra i vari fornitori all'ingrosso per sapere sempre dove comprare al miglior prezzo netto 💰
* Controllo totale dei documenti e del magazzino, veloce come un fulmine e accessibile ovunque.

A breve Danea andrà definitivamente in pensione, sostituito da un motore creato esattamente su come respiriamo e vendiamo noi ogni giorno. 🎯

---

### Un cantiere sempre aperto (ed è bellissimo così) 🏗️✨

Se dovessi dire che l'Hub è un'opera finita e perfetta, mentirei. 
PrezziSmartHub è un **cantiere perennemente aperto**. 

Dietro la grafica pulita ci sono sfide tecniche continue: database da sincronizzare in sicurezza, container Docker da aggiornare, calibrazioni per non sovraccaricare il server e nuovi collegamenti all'intelligenza artificiale per automatizzare schede prodotto e risposte. 🤖

A volte qualcosa si inceppa, apro i log, capisco dove sta il problema e lo sistemo. Ma la soddisfazione di vedere l'intero ecosistema del negozio muoversi in perfetta armonia — dal banco al magazzino, dal web alle banche — non ha prezzo. 📈

Non sono un programmatore teorico chiuso in una torre d'avorio: sono un negoziante appassionato di tecnologia che ha deciso di usarla come leva per lavorare con più serenità, zero sprechi di tempo e una visione nitida del futuro della propria azienda. 🚀

E questo cantiere ha ancora tantissime novità pronte a vedere la luce! 🌟
