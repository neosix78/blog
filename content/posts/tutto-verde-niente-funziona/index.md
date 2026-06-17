+++
title = "Quando tutto è verde ma niente funziona: 332 GB fantasma e un disco da avvitare"
date = '2026-06-17T10:00:00+02:00'
draft = false
summary = "Un assistente AI locale smette di rispondere. La caccia al colpevole tira fuori due guasti che hanno una cosa in comune: tutti gli indicatori dicevano «sano». Cronaca di una diagnosi controintuitiva e tre lezioni che mi porto dietro."
tags = ["homelab", "self-hosting", "linux", "hardware", "diagnostica"]
categories = ["tech-tips"]

[cover]
  image = "cover.jpg"
  alt = "Un hard disk da server non avvitato che vibra dentro un case aperto, con un piccolo cacciavite di fianco"
  caption = "A volte la soluzione più sofisticata è un cacciavite"
  relative = true
+++

Ho un server in negozio che fa girare un piccolo stack di modelli locali su una RTX 4090: lo uso per automazioni, ricerca e qualche esperimento. Niente di esotico — Ollama, una web UI davanti, un Ryzen a tenere il banco. Funziona, fino al giorno in cui non funziona.

Una mattina mando un banalissimo "ciao" al modello dalla web UI. Dieci minuti dopo: ancora niente. Nessuna risposta, nessun errore chiaro. Il classico guasto che non ti dice da dove cominciare.

Quello che ho trovato scavando non era *un* problema. Erano due, in fila, e tutti e due avevano la stessa beffa: ogni diagnostica diceva che era tutto a posto.

## 332 GB che non esistevano

Prima regola quando un sistema Linux si comporta in modo assurdo: guarda lo spazio su disco. `df -h` e la risposta era lì, brutale — il disco di sistema **al 100%, zero byte liberi**. Con il root pieno non risponde più niente: il modello non scrive, i log si bloccano, le cartelle temporanee non si creano. L'intero sistema va in stallo in silenzio.

Ma 332 GB di cosa? Il colpevole era un processo Chrome headless orfano — il residuo di un vecchio agente AI che avevo smesso di usare mesi prima. Il browser era morto da un pezzo, però una sua cartella di telemetria aveva continuato a scrivere in loop, gonfiandosi fino a **333 GB** senza che nessuno se ne accorgesse. Un cadavere che continuava a mangiare disco.

Cancellata quella cartella, **332 GB tornati liberi** di colpo: il root dal 100% al 26%. Il processo era già defunto e non aveva nessun servizio o autostart che lo facesse ripartire, quindi: problema chiuso. O così pensavo.

## Il disco sano che andava a 2 MB/s

Recuperato lo spazio, riparto — e il disco dati meccanico striscia. Lettura sotto i **2 MB/s**. Un test di scrittura da 1 GB con `dd` va in timeout. Per un disco che dovrebbe stare sui 100 e passa MB/s, è morte clinica.

La prima ipotesi è ovvia: il disco sta morendo. Installo `smartmontools`, lancio lo SMART e mi aspetto un cimitero di settori riallocati.

E invece: **PASSED**. Reallocated 0. Pending 0. Errori CRC 0. Nessun errore ATA. Poco più di quattro anni di vita, 41 °C. Lo SMART giurava che il disco fosse perfettamente sano. E allora perché andava a passo d'uomo?

Qui la spiegazione non è venuta da un comando, ma da un dubbio fisico. Quel disco — un WD Red da 3 TB (WD30EFRX), 5400 giri — era stato rimontato di recente durante uno spostamento hardware. E **non era stato fissato con le viti**: appoggiato, non avvitato.

Un disco meccanico non avvitato vibra. E un disco che vibra costringe la testina a micro-correzioni continue di posizionamento per restare sulla traccia. Il risultato è esattamente quello che vedevo: throughput a terra **senza un solo errore SMART**, perché i WD Red lisci non hanno sensori anti-vibrazione che lo segnalino. Il disco non è rotto. È solo *sballottato*.

SMART pulito + performance morta = non guardare il piatto, guarda le viti. Quattro viti e il problema sparisce.

## Tre lezioni che mi tengo

**1. Un processo abbandonato non è un processo innocuo.** "Ho smesso di usarlo" non vuol dire "è spento". Quel browser era morto come interfaccia ma vivo abbastanza da riempire un disco. Dismettere un tool significa rimuoverlo davvero — cartelle, telemetria, residui — non solo non aprirlo più.

**2. SMART che dice PASSED non assolve l'hardware.** Lo SMART misura la salute del *supporto magnetico*, non il modo in cui il disco è montato nel case. Performance crollate con diagnostica pulita è un sintomo da manuale di un problema fisico o meccanico, non del disco in sé. Prima di condannare un disco, controlla che sia avvitato.

**3. Monitora la cosa che si riempie davvero.** Avevo dei watchdog sullo spazio dei mount di rete, ma **nessuno sul disco di sistema** — proprio quello che si era riempito. È il buco classico: sorvegli l'ovvio e lasci scoperto il punto critico. Ho aggiunto un controllo ogni 30 minuti che mi avvisa se il root supera l'80%. Banale, e mi avrebbe risparmiato l'intera giornata.

## La morale

Tre componenti dicevano "sto bene": il browser (era morto, quindi "innocuo"), lo SMART (PASSED), il sistema operativo (nessun errore esplicito). Ed era tutto fermo lo stesso. La diagnostica ti dice cosa hai pensato di misurare; il guasto vive spesso esattamente dove non stai guardando.

A volte la soluzione più sofisticata è un cacciavite.
