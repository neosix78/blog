+++
title = "Ho tradotto DragonSword in italiano con un’AI locale. Perché dobbiamo ancora arrangiarci?"
date = '2026-10-23T09:00:00+02:00'
draft = false
summary = "Una traduzione amatoriale di DragonSword: Awakening, una RTX 4090 e un modello locale. Come si abbatte il costo di una prima localizzazione, cosa resta da revisionare e una domanda alle aziende che continuano a escludere l’italiano."
tags = ["intelligenza-artificiale", "gaming", "traduzione", "inferenza-locale", "self-hosting"]
categories = ["esperimenti"]

[cover]
image = "cover.png"
alt = "DragonSword: Awakening, finalmente in italiano: un mondo fantasy e una GPU collegati da parole nei colori italiani"
caption = "Traduzione amatoriale · AI locale"
relative = true
+++

«Italiano non disponibile». Chi gioca su PC conosce bene questa scritta. La leggi sulla pagina Steam, magari dopo aver visto un trailer che ti aveva convinto, e devi decidere se passare oltre o accettare di giocare in un’altra lingua. Con *DragonSword : Awakening* ho deciso di provare una terza strada: tradurlo da solo.

Mi sono fatto aiutare dall’AI e ho messo al lavoro un modello sul mio server. In una manciata di ore ho costruito la procedura necessaria, poi ho lasciato lavorare la macchina. Ne è uscita una traduzione italiana amatoriale completa, che ho già provato nel gioco. È ancora da revisionare linguisticamente, ma permette di leggere menu, missioni e dialoghi in italiano.

Per chi mastica bene l’inglese cambia poco. Per chi invece perde pezzi della storia o deve fermarsi davanti alla descrizione di un’abilità, cambia parecchio. E qualcuno, senza la propria lingua, quel gioco non lo avrebbe nemmeno comprato. È questo che mi interessa: oggi chi sa smanettare può rendere accessibile a più persone qualcosa che prima avrebbe richiesto un lavoro difficilmente affrontabile da solo. Lo stesso ragionamento vale per tante applicazioni che usiamo ogni giorno.

## Una GPU al lavoro, senza consumare l’abbonamento

La prima difficoltà è stata raggiungere i testi. DragonSword usa Unreal Engine 5 e li conserva in tabelle dentro archivi `.pak`: ho dovuto capire come estrarli e come reinserirli in un formato che il gioco accettasse. Qui servono ancora pazienza e un po’ di competenza tecnica. Ogni titolo ha le sue particolarità, quindi non esiste un pulsante universale «traduci il gioco».

Una volta aperta quella porta, però, gran parte del lavoro si presta all’automazione. Gli script leggono le tabelle, passano piccoli gruppi di frasi al modello e salvano progressivamente le risposte. Se qualcosa manca, si riparte da lì. Alla fine i testi vengono ricomposti in un pacchetto aggiuntivo, che il gioco carica senza dover sostituire gli archivi originali.

Naturalmente bisogna spiegare al modello che cosa sta traducendo. Un pulsante ha poco spazio, mentre un dialogo deve avere un tono credibile. Un nome deve restare lo stesso per tutta l’avventura e una variabile dentro una frase deve sopravvivere alla traduzione. Ho impostato queste indicazioni insieme alle scelte terminologiche, mantenendo anche parole familiari ai giocatori, come Buff e Cooldown.

A tradurre è stato **Liquid 24B, eseguito attraverso Ollama sulla mia RTX 4090**. Questa è l’inferenza locale: il modello gira sul mio hardware e riceve le richieste dagli script, senza affidare la traduzione massiva a un servizio esterno. L’assistente AI mi ha aiutato a costruire il sistema; il lavoro ripetitivo l’ho fatto fare alla macchina.

Su decine di migliaia di testi, la differenza si sente. Ogni richiesta comprende anche istruzioni e regole, oltre al testo da tradurre; poi ci sono le risposte e gli eventuali tentativi da ripetere. È un volume che può arrivare a milioni di token. Non ho un conteggio complessivo per dare una cifra esatta, ma la traduzione massiva **non ha consumato le quote dei miei abbonamenti AI né richiesto pagamenti per API a token**.

Ho lasciato lavorare la GPU mentre facevo altro, potendo riprovare senza preoccuparmi del credito. Hardware ed elettricità hanno ovviamente un costo: nel mio caso la scheda era già lì, usata anche per altri progetti.

Il risultato ha i limiti di una prima versione. Qualche frase può suonare rigida, un termine può richiedere una correzione e una battuta può perdere il senso senza il giusto contesto. La revisione resta necessaria. Ma poter partire da una traduzione già funzionante cambia completamente la quantità di lavoro da affrontare.

## La traduzione è disponibile

Potete scaricare la traduzione italiana di DragonSword : Awakening qui: **[SCARICA LA TRADUZIONE — LINK DA INSERIRE]**.

È un progetto amatoriale non ufficiale, realizzato con AI e ancora aperto alla revisione linguistica. Traduce i testi, senza aggiungere un doppiaggio italiano, e richiede il gioco. Per installarlo basta copiare, a gioco chiuso, `pakchunk999-WindowsClient_P.pak` nella cartella `DS\Content\Paks\~mods\`, creando quest’ultima se manca. Rimuovendo il file si torna alla situazione originale. Eventuali aggiornamenti del gioco potrebbero richiedere un pacchetto aggiornato.

## E adesso la domanda alle aziende

Al momento in cui scrivo, la [pagina Steam di DragonSword](https://store.steampowered.com/app/4570720/DragonSword__Awakening/?l=italian) elenca undici lingue supportate. L’italiano è escluso. Non conosco le ragioni interne di Hound13, ma dopo questo esperimento una domanda me la faccio.

**Se io sono riuscito a impostare in una manciata di ore una traduzione amatoriale e farla funzionare nel gioco, cosa potrebbe ottenere un team di due o tre persone in una settimana o quindici giorni?**

Dipende dal titolo, certamente. Ma chi sviluppa dispone dei sorgenti e conosce il contesto; può parlare con chi ha scritto i dialoghi e usare gli strumenti interni. Io sono partito dagli archivi del gioco già pubblicato.

Una localizzazione ufficiale comporta costi e responsabilità maggiori. Va revisionata, provata e mantenuta dopo gli aggiornamenti. Eppure, proprio perché la prima stesura è diventata così accessibile, mi sembra ragionevole chiedere se quei conti vengano rifatti alla luce degli strumenti disponibili. Anche una versione sperimentale, dichiarata e migliorata nel tempo, potrebbe essere una possibilità da valutare.

Non penso che questo renda superflui i traduttori. Vorrei piuttosto vederli lavorare su quella base, dedicando tempo al tono dei personaggi e agli errori che una macchina non sa riconoscere. Il valore di quel lavoro rimane; è cambiato il punto da cui si può partire.

Un’azienda può comunque decidere che il pubblico italiano non giustifichi l’investimento. Ma allora parliamo di quella scelta commerciale. Perché, dopo aver visto cosa riesce a fare una persona con gli strumenti di oggi, l’assenza totale di una lingua diventa più difficile da accettare.

**Quanto deve ancora abbassarsi il costo della prima localizzazione perché l’italiano smetta di essere lasciato alla buona volontà dei giocatori?**

Io una prima versione imperfetta, dichiarata e migliorabile la posso accettare. Che la risposta continui a essere «arrangiatevi», molto meno.
