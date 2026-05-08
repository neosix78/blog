+++
title = "Sei agent AI sullo stesso server, e i quattro strati di memoria che non ti aspetti"
date = '2026-05-08T22:00:00+02:00'
draft = false
summary = "Mi è arrivato un link a un repo. Ne è uscita una sessione di sei ore di lavoro architetturale. Alla fine ho un sistema in cui cinque agent diversi parlano la stessa lingua, un sesto è isolato per scelta, e ho capito che la 'memoria' di un AI non è una sola cosa — sono almeno quattro strati distinti, e tre dei miei li conoscevo solo a metà."
tags = ["ai-agent", "claude-code", "codex", "gemini", "context-engineering", "self-hosted", "memory"]
categories = ["tech-tips"]

[cover]
  image = "cover.png"
  alt = "Sei sagome di assistenti AI affiancati su un terminale notturno, ognuno con una piccola etichetta per il tipo di memoria che gestisce; una linea sottile li collega a un'unica directory centrale."
  caption = "Sei agent diversi che leggono dallo stesso magazzino. La parte difficile è arrivarci."
  relative = true
+++

Mi è arrivato un link a un repo GitHub. Era una di quelle giornate in cui chiedi un parere su un tool e ti aspetti dieci minuti di triage, niente di più. Ne sono uscite sei ore di lavoro, un'intera documentazione architetturale rifatta da zero, e l'ammissione — sgradevole — che il sistema su cui lavoro tutti i giorni era frammentato in modi che non avevo mai mappato.

Il punto di partenza era apparentemente innocuo. Il repo proponeva una "memoria persistente unificata per agent AI": l'idea era che ogni assistente — Claude Code, Codex, Cursor, e così via — invece di tenersi le cose nel suo cassetto privato, le scrivesse in un servizio centrale comune al quale tutti gli altri potevano attingere. Belle promesse, benchmark interessanti, lavoro fatto in maniera seria. Il primo istinto è stato di scartarlo come over-engineering — il classico "non per me, io ho già il mio sistema di note curate". Sbagliato. Quando ho iniziato a guardare meglio cosa avevo davvero sul server, mi sono reso conto che il problema che il repo voleva risolvere era *esattamente* il mio. Solo che non lo avevo mai formulato in quei termini.

Quello che racconto qui non è un benchmark né una recensione di quel tool. È la cronaca di cosa ho scoperto sotto al cofano del mio setup multi-agent, perché alcune scelte di prima oggi sembrano sbagliate, e qual è il modello mentale a cui sono arrivato dopo aver acceso le luci. Ho anche pubblicato il piano operativo nel mio repository personale; questo post è la parte teorica, quella che credo possa essere utile a chi si trova nella stessa situazione e si chiede da dove cominciare.

## Sei agent, e ognuno ha il suo cassetto

Sul mio server convivono — o tentano di convivere — sei diversi assistenti basati su LLM. C'è Claude Code, che è il mio cavallo da battaglia per il lavoro al terminale. C'è Codex CLI di OpenAI, che apro più raramente ma c'è. C'è Gemini CLI di Google, idem. C'è OpenCode, un esperimento minimale open-source. C'è Hermes, un client Codex custom che uso come backend per il mio bot Telegram interno. E c'è Paperclip, un'interfaccia desktop multi-workspace che internamente fa girare Claude Code dentro a delle "celle" separate.

Ognuno di questi assistenti ha il proprio file di "regole" da leggere all'avvio — `CLAUDE.md` per uno, `GEMINI.md` per un altro, `AGENTS.md` per Codex, e così via. Ognuno ha il proprio path dove cercare le "skill" (le mie istruzioni ricamate per task ricorrenti). Ognuno ha un proprio formato per la memoria persistente. Ognuno ha le proprie credenziali. Sono nati in momenti diversi, supportano standard diversi, e la community non è ancora arrivata a una convenzione condivisa che li metta d'accordo.

Il setup, finché lo guardi dal punto di vista *di un singolo agent in una singola cartella*, sembra ragionevole. Apro Claude Code dalla root del mio home, lui legge il `CLAUDE.md` che ho lì, importa la mia memoria personale (un centinaio di file curati a mano nel tempo), vede tutte le skill che ho scritto. Tutto fila.

Il problema arriva nel momento in cui esci dalla situazione ideale. Apro Claude da una sotto-directory perché sto lavorando a un progetto specifico, e di colpo la memoria personale non viene caricata: è agganciata alla cwd esatta, e lì siamo in un'altra. Apro Codex per provare una cosa, e Codex non ha la minima idea di cosa Claude sapeva ieri. Apro Paperclip in un workspace nuovo, e siccome ogni workspace ha un identificatore casuale come cwd, la memoria parte sempre vuota. Hermes, che gira come servizio in background per rispondere ai messaggi che mi arrivano sul Telegram, ha la cwd impostata su una directory di sistema che non corrisponde al mio home, quindi del mio context personale non legge quasi nulla.

La cosa imbarazzante è che il mio "sistema di memoria" — di cui andavo abbastanza fiero — funzionava davvero solo nello scenario A: un singolo agent, in una singola directory, con la luna in fase favorevole. Tutto il resto era zoppo, e io non lo avevo notato perché, quando incrociavo gli scenari, il prezzo lo pagavo silenziosamente.

## I quattro strati di memoria

La discussione cross-AI che ho fatto durante la sessione — io che ragionavo a voce alta con Claude Code, e poi incrociavo le sue risposte con quelle di ChatGPT — ha portato fuori un modello mentale che mi è risultato chiarificatore: la "memoria" di un agent non è una cosa sola. Sono almeno quattro strati distinti, ognuno con regole, costi e finalità diverse. E quando uno di questi strati silenziosamente prevale sugli altri, succedono incidenti.

{{< figure src="strati-memoria.png" alt="I quattro strati di memoria di un agent AI: semantica/dichiarativa, configurazionale, procedurale, episodica" caption="Sopra leggera e curata a mano, sotto pesante e silente. Quasi tutti i problemi pratici nascono dal confondere uno strato con un altro." >}}

Il primo strato è quello che chiamo **memoria semantica**, o dichiarativa. È il "wiki interno": fatti stabili, regole, decisioni prese in passato, lesson learned dalle volte in cui ho sbagliato. Cose che cambiano poco e che metto per iscritto deliberatamente, una alla volta, perché so che le voglio ritrovare. Sono file markdown, una manciata di centinaia di kilobyte in totale, e sono curati a mano: niente di automatico, ogni volta che imparo qualcosa di rilevante apro il file e lo scrivo. Il costo di scrittura è quindi alto — ci vuole disciplina, tempo, attenzione — ma il costo di lettura è bassissimo, perché entrano sempre nel contesto iniziale dell'agent senza dover fare query particolari. È lo strato che uso quando voglio insegnare all'AI una regola che dovrà valere ovunque e per sempre, finché non la cambio io stesso.

Il secondo strato è la **memoria configurazionale**. Spiega chi è l'agent, che ruolo gioca, con che tono deve parlare, quali azioni può fare in autonomia e quali invece richiedono sempre una mia conferma. Sono i file `CLAUDE.md`, `GEMINI.md`, `AGENTS.md`, e per Hermes addirittura un file chiamato `SOUL.md` — sì, "anima", non sto scherzando, è proprio così che si chiama. È piccolo come strato, una manciata di kilobyte in tutto, ma è cruciale perché viene letto eagerly da ogni nuova sessione e plasma il comportamento dell'assistente prima ancora che l'utente abbia scritto il primo prompt. Quando voglio che un agent risponda sempre in italiano, o che non mandi mai email senza il mio OK esplicito, è qui che metto la regola.

Il terzo strato è la **memoria procedurale**: il "come si fa una cosa". Sono le skill — file markdown che dicono "quando l'utente chiede X, esegui questi passaggi in quest'ordine". Ne ho una sessantina nel sistema, alcune semplicissime (un file solo con istruzioni tipo "guarda qui, poi qui, scrivi così") e altre più complesse, con script Python che le accompagnano, template, asset. Il loro grosso vantaggio è che sono lazy: il corpo della skill viene caricato solo quando la invochi, non all'avvio dell'agent. Quindi puoi averne tante senza preoccuparti che gonfino il context iniziale e rendano l'AI lenta o ciarliera.

Il quarto strato è quello che non avevo davvero mappato, ed è quello dove ho avuto l'epifania. È la **memoria episodica**: ogni sessione che fai con un agent — *ogni* sessione, senza eccezioni — viene registrata su disco. Claude scrive un file JSONL per ogni conversazione. Codex scrive un altro JSONL. OpenCode usa un database SQLite. Paperclip usa file ndjson. Tutti questi archivi crescono ad ogni interazione, e contengono *ogni* prompt che hai scritto, *ogni* risposta che ti è stata data, *ogni* tool call che è stata fatta, *ogni* output prodotto, *ogni* errore incontrato, *ogni* decisione presa in linguaggio naturale. La dimensione è megabyte, gigabyte: tre, quattro ordini di grandezza più grande di tutti gli altri strati messi insieme. È il vero schedario del lavoro fatto.

Ed è silente. Nessun agent legge automaticamente dalle sessioni passate. Il dato c'è, è completo, è tuo, ma è muto finché tu non vai a cercarlo a mano. Stasera ho dimostrato che si può consultare: ho aperto il database SQLite di OpenCode e con tre query SQL ho ricostruito una conversazione di un'ora prima — ogni tool call, ogni query SQL eseguita, ogni testo generato, perfino i ragionamenti interni dell'agent. In pratica un audit retrospettivo, una cosa che fino a quel momento non sapevo nemmeno di poter fare. Era lì da sempre.

Il repo da cui era partita tutta la sessione — quello sulla memoria persistente unificata — faceva esattamente questo: prometteva di trasformare lo strato 4 da "schedario muto" a "indice ricercabile", con embedding semantici e tutto il corredo. Risolveva un problema reale. Solo che non era *il mio* problema reale, almeno non oggi: nel mio uso quotidiano, la cosa che mi mancava davvero era prima sistemare gli strati uno-due-tre, fare in modo che fossero coerenti tra agent diversi e tra cartelle diverse. Una volta sistemati quelli, l'episodica può aspettare. Se mai la voglio, è dietro l'angolo.

## Perché ho scelto i file invece di un servizio

Tra le due strade — installare un servizio centrale che gestisse tutta la memoria, oppure restare nel mondo dei file di testo aggiungendo qualche convenzione in più — ho scelto la seconda. Le ragioni sono noiose ma robuste, e vale la pena dirle perché mi sono accorto che spesso, quando ti trovi davanti a un repo "fico", la tentazione di installarlo è più forte della domanda di se ti serva davvero.

La prima ragione è che il problema vero non era il retrieval. Il vero problema era il *bootstrap*: il context buono esisteva già, scritto da me, in file che non si erano persi da nessuna parte. Quello che non funzionava era farlo arrivare in modo coerente nei vari agent, in tutte le situazioni, senza che si parlassero alle spalle. Aggiungere un servizio di retrieval semantico sopra una base che era frammentata significava risolvere il problema sbagliato. Avrei avuto un AI che sapeva cercare benissimo, dentro un context che continuava a essere disallineato.

La seconda ragione è che file più git battono database più servizio quando sei un singolo utente che lavora da solo. Non c'è bisogno di locking, non c'è concorrenza, non c'è versioning custom da gestire. Modifico, faccio commit, push, e il file è su GitHub al sicuro. Il diff è leggibile da qualunque editor. Il debug è "leggi il file e capisci cosa c'è scritto". Aggiungere un servizio in più voleva dire aggiungere un'altra cosa che può rompersi alle tre di notte, un altro health check, un'altra dipendenza al boot del sistema, e tutto questo per qualcosa che, oggi, non mi serve. Forse mi servirà domani — vedremo.

La terza ragione è che Hermes non parla MCP, e MCP era il protocollo del servizio in questione. Avrei comunque avuto bisogno di un fallback file-based per Hermes. Allora tanto valeva farlo bene fin da subito, far diventare il fallback la strada principale, e considerare il servizio centrale solo come un'ottimizzazione futura quando e se il volume di traffico lo giustifichi.

La quarta ragione, la più onesta, è che il valore aggiunto di un servizio dedicato si vede a scale che io non ho. Quando hai cento sessioni al giorno, decine di agent, multipli utenti che scrivono in parallelo sulla stessa memoria, lifecycle complesso con priorità e decadimento, allora un servizio è la scelta giusta. Io faccio una decina di sessioni al giorno, sono da solo sul mio server, ho un budget tempo limitato. Il servizio sarebbe stata una Lamborghini parcheggiata in garage: bellissima, sprecata.

Resta sempre l'opzione di valutarlo dopo qualche settimana di uso reale, se mi accorgo che il file-first non basta. Ho messo nel piano un timer di trenta giorni: a quel punto mi farò la domanda esplicita "ho avuto il fastidio di non trovare una sessione vecchia? sto rifacendo gli stessi workflow per la decima volta?". Se la risposta è sì, il servizio diventa giustificato. Se la risposta è no, file-first basta e si rivaluta il prossimo giro.

## Una sola fonte, tanti puntatori magri

Sui dettagli dell'implementazione potrei perdermi per pagine intere. Salto al modello, che è la parte che mi sembra trasferibile.

Il pattern che ho adottato si riassume in una frase semplice: per ogni cosa, una sola fonte di verità, e dei puntatori magri — qualche centinaio di byte ciascuno — negli altri posti dove gli agent vanno a guardare. Niente copie, niente duplicazione di contenuto, niente di quel pattern terribile in cui finisci con quattro versioni dello stesso file in quattro directory diverse, e quando ne modifichi una le altre tre ti sorridono di traverso e continuano a fare quello che facevano prima.

Per le skill, in concreto: scrivo il file completo in una directory canonica (la chiamo, in maniera neutrale, una directory di "agent-skills"). Negli altri path che gli agent vanno a guardare ci sono solo due possibilità. O un symlink di directory che punta al canonical, oppure un wrapper-stub di poche righe in markdown che dice "questa skill esiste, ma il file vero sta lì, leggilo come fonte autorevole". Modifico la skill in un solo posto e funziona ovunque, automaticamente, perché tutti i puntatori risolvono allo stesso file fisico.

Per il context globale, lo stesso ragionamento. Ho un set di file canonical dentro al mio vault Obsidian: regole stabili del dominio, architettura del sistema, lesson learned, stato dei progetti, integrazioni esterne, quirk specifici di ogni agent, indici. Sono organizzati così:

```
~/obsidian-vault/SecondBrain/context/
├── business-rules.md          regole stabili del dominio
├── agent-ecosystem.md         architettura del sistema
├── memory/
│   ├── lessons-learned.md     errori già fatti, fix testati
│   ├── project-status.md      stato progetti
│   └── integrations.md        API, webhook, secrets
├── agents/
│   ├── claude.md              quirks specifici Claude Code
│   ├── codex.md               quirks Codex
│   └── ...
└── indexes/
    ├── skills.md              indice skill canoniche
    └── memory-map.md          dove sta cosa
```

Ogni agent ha un piccolo file di bootstrap — `CLAUDE.md`, `GEMINI.md`, `SOUL.md`, e così via — che dice "all'inizio di ogni sessione leggi questi file canonical via path assoluto". Niente di più, perché le regole vere stanno nei file vault, una sola volta. Il bootstrap è solo un dito che indica.

Il bello dei path assoluti è che sono immuni alla cwd. Apri l'agent da qualsiasi directory ti pare: il file vault sta sempre allo stesso posto, lo trova. Anche Paperclip da un workspace con UUID random lo trova. È la cosa più stupida e più robusta del mondo, e ha risolto la metà dei problemi che mi affliggevano.

## Le tre martellate in faccia

Niente di tutto questo è andato liscio, e mi sembra giusto raccontare anche le brutte. Tre cose mi hanno preso a martellate durante la giornata, e ognuna ha lasciato una lezione utile.

La prima martellata è stata la memoria persistente di Gemini che batte il file globale. Avevo fatto tutto il lavoro per bene: avevo scritto nel `GEMINI.md` globale la regola "all'inizio di ogni sessione leggi il file canonical X, poi Y, poi Z", e mi sembrava elegantissimo. Il primo test mi è esploso in faccia: gli ho chiesto di mostrarmi un dato di vendita, e lui è andato a eseguire uno script bash legacy che non avevo aggiornato. Ho indagato per dieci minuti convinto di aver scritto male il `GEMINI.md`, di aver sbagliato la sintassi, di non so cos'altro. Poi ho capito: Gemini ha una memoria persistente sua, in un'altra directory ancora, che si auto-popola quando aggiungi roba con il tool "Add to memory" durante una sessione. E quella memoria locale *vince* sul file globale. Sempre. Il fix immediato sarebbe stato sovrascrivere la memoria locale con quello che volevo io. L'ho fatto, il test è passato, e poi mi sono fermato a pensare. Il fix era cosmetico. La verità più scomoda è che il pattern "memoria persistente locale che si auto-popola" è strutturalmente in tensione con il pattern "vault canonico condiviso". Per Gemini specificamente, il sistema file-first non funziona benissimo, e probabilmente non funzionerà mai bene senza un lavoro più invasivo. Ho deciso quindi di tenerlo fuori dal sistema, lasciandolo nella sua bolla isolata con la sua memoria legacy. Lo apro così raramente che non vale la pena di insistere. Una buona soluzione tecnica include riconoscere quando *non* applicarla.

La seconda martellata è stata la scoperta che Hermes e Codex CLI condividono lo stesso refresh token OAuth. Entrambi usano le credenziali di ChatGPT/Codex, e la rotazione del refresh token è zero-sum: chi parte per ultimo invalida l'altro. Ho aperto Codex CLI per testare una cosa, e dieci minuti dopo Hermes ha smesso di rispondere ai messaggi sul Telegram. L'errore era abbastanza esplicito — diceva proprio "il token è stato consumato da un altro client" — ma il fix che il messaggio raccomandava non era quello giusto. Mi suggeriva di rifare l'OAuth interattivo dal browser, mentre in realtà la prima volta che è successo bastava un comando di reset dello stato di "exhaustion" che Hermes mantiene internamente per evitare di fare retry infiniti. Quando però il token era *davvero* invalido — quando avevo aperto Codex *dopo* il reset — è servito davvero il flusso device-code dal browser. Ho documentato il quirk perché la prossima volta che capita, e capiterà, non voglio doverci ripensare per dieci minuti come stasera. La lezione qui è doppia: gli errori dei tool a volte ti raccontano una storia un po' più drammatica di quella vera, e i sistemi di autenticazione condivisi sono fragili in modi non ovvi.

La terza martellata è stata lo script di sync che non vedeva niente. Avevo riscritto il programma che propaga le skill tra i diversi path, l'avevo testato a tavolino, sembrava perfetto. Prima esecuzione vera: "0 file processati". Ho passato cinque minuti a leggere il mio stesso codice, convinto di aver introdotto un bug in qualche regex o di aver sbagliato un controllo. La causa era molto più stupida: una delle directory di partenza era un symlink — cosa che ho scoperto leggendo la documentazione del mio stesso repository, *che avevo scritto io mesi prima*, e prontamente dimenticato — e il comando `find` di default non segue i symlink a meno che tu non glielo dica esplicitamente. Aggiunta una `-L`, e tutto si è acceso. La lezione qui è tristemente personale: la documentazione che scrivi a te stesso è importante anche quando pensi di ricordarti tutto. Anzi, soprattutto allora, perché è proprio quando sei convinto di sapere come funziona il tuo sistema che è più probabile che ti perda un dettaglio.

## Il drift detector: come accorgersi che le cose stanno divergendo

L'ultimo pezzo dell'architettura che mi è sembrato di valore — e che nei post precedenti non avevo mai usato — è qualcosa che è uscito durante la discussione cross-AI con ChatGPT. Una volta che hai un sistema con file canonical, puntatori, copie e symlink sparpagliati in più posti, hai bisogno di un modo per accorgerti se qualcosa sta divergendo nel tempo. Senza quel meccanismo, il drift accade in silenzio: una skill che funziona oggi smette di funzionare fra due settimane perché un puntatore è rimasto a indicare un file che non esiste più, e tu te ne accorgi solo nel momento sbagliato, magari mentre stai cercando di fare un'altra cosa di fretta.

Ho scritto allora un piccolo script bash che lanciato a mano o via cron settimanale fa il giro di tutti i path interessati e controlla se le cose tornano. Verifica che ogni skill legacy abbia tutti i suoi puntatori in tutti i target previsti, segnala i puntatori che indicano un file canonical che non esiste più, trova le skill che hanno lo stesso nome ma contenuto divergente in path diversi, controlla che i file di bootstrap citino path assoluti che esistono davvero, verifica che ogni skill nativa abbia la sua replica nel target di Claude Code. L'output è un report markdown su disco: verde se tutto a posto, lista di problemi specifici altrimenti.

L'ho impostato come cron settimanale, e se trova drift mi arriva un alert sul Telegram interno. Niente di sofisticato, ma in un sistema dove ho file in cinque o sei posti diversi è la differenza tra "scopro a caso fra tre mesi che metà delle skill non funzionano in Codex" e "lunedì mattina ricevo l'elenco esatto delle cose da sistemare". Stasera, alla prima esecuzione, ha trovato cinquanta problemi reali. Ho riscritto lo script di sync per gestire meglio i casi che mi ero dimenticato, ho rilanciato, e il drift è sceso a zero. Cinquanta problemi sistemati in trenta minuti, dopo che per mesi erano andati avanti senza che me ne accorgessi nemmeno.

## Cosa porto a casa

Una cosa che non avrei capito senza fare l'esercizio è che la frase "il mio AI ha memoria" è troppo poco specifica per essere utile. Ci sono almeno quattro categorie diverse di memoria, ognuna con il suo costo e la sua finalità, e quasi tutti i problemi che incontri nella pratica nascono dal fatto che ne stai usando una per fare il lavoro di un'altra. La memoria semantica curata a mano non sostituisce la memoria episodica delle sessioni: una è un wiki, l'altra è un archivio. Il file di configurazione non è la stessa cosa di una skill procedurale: uno dice chi sei, l'altra dice come si fa una cosa specifica. Una memoria persistente che si auto-popola da sola non è equivalente a un context globale che leggi all'avvio: la prima accumula liberamente e a volte mente, il secondo è esplicito e controllato.

Quando uno di questi strati silenziosamente prevale sugli altri, succedono cose strane: l'AI ti risponde con knowledge che non sai più da dove arriva, esegue script vecchi mentre tu pensavi di averlo aggiornato, "dimentica" cose che gli avevi detto cinque sessioni prima ma che non avevi mai messo per iscritto da nessuna parte. Mappare i quattro strati significa potersi chiedere — concretamente, su un caso preciso — *dove sta questa cosa, e dove dovrebbe stare?*. Senza quella mappa, ogni risposta sbagliata è un mistero. Con la mappa, è una traccia da seguire e quasi sempre porta da qualche parte.

L'altra cosa che porto a casa è meno tecnica ed è più sgradevole da ammettere. I sistemi che usi tutti i giorni meritano lo stesso tipo di manutenzione che dai agli altri sistemi che hai costruito. Mi ero abituato a considerare il setup AI come "qualcosa che funziona, non toccarlo, hai paura di romperlo". In realtà era frammentato in modi che pagavo silenziosamente — token sprecati a ricaricare contesto, errori ripetuti agent dopo agent, lesson che andavano riapprese ogni volta da capo perché non erano scritte nel posto giusto. Sei ore di lavoro architetturale stasera valgono probabilmente un mese di frizione recuperata. Visto a posteriori, è il tipo di manutenzione che avrei dovuto fare due mesi fa, e che probabilmente dovrò rifare fra tre o quattro mesi quando i tool saranno cambiati di nuovo. È un lavoro che non finisce mai del tutto, ma rinviarlo costa più che farlo.

Il piano operativo dettagliato — i passi esatti, i backup, lo script di sync, il drift detector, i test pratici eseguiti sui cinque agent — l'ho lasciato in un repository personale, anonimizzato dai dati specifici del mio dominio. Se ti serve un punto di partenza per fare un esercizio simile sul tuo setup, scrivimi e te lo passo.
