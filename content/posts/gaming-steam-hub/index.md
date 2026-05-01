+++
title = "Il mio Gaming Steam Hub: dalle chiavi a basso prezzo a una dashboard personale"
date = '2026-05-01T09:00:00+02:00'
draft = false
summary = "Volevo solo trovare le chiavi Steam al prezzo più basso. Tre mesi dopo ho una webapp che mi dice cosa sto giocando, quanto, quando, cosa è in sconto sulla mia wishlist e cosa è gratis su Game Pass."
tags = ["python", "flask", "steam", "gaming", "self-hosted", "automazione"]
categories = ["tech-tips"]

[cover]
  image = "cover.png"
  alt = "Screenshot della dashboard Gaming Steam Hub: card con stats della libreria, top giochi per ore, wishlist in offerta e feed Telegram"
  caption = "La dashboard finale: tutto quello che voglio sapere sul mio gaming, in un colpo d'occhio."
  relative = true
+++

## Apertura

Oggi un post diverso. Non un problema del negozio, nessuna automazione che mi fa risparmiare ore di lavoro. Una cosa più semplice e più personale: mi piacciono le statistiche.

Mi piace sapere a cosa gioco, quanto ci gioco, quando ci gioco. E già che ci sono, mi piace anche sapere se un gioco che mi interessa è sceso di prezzo, e se per caso è già incluso nel Game Pass che pago tutti i mesi.

Ho costruito tutto questo a pezzi, nei ritagli di tempo. Il risultato finale è una webapp che gira sul mio server di casa e che ho aperto solo a me stesso. Ma la storia di come ci sono arrivato è il classico esempio di un'idea piccola che cresce.

## Il contesto

Tutto è iniziato con un'idea singola: voglio una skill che mi dica dove comprare le chiavi dei giochi al prezzo più basso possibile. Stop.

Uso [Claude Code](https://claude.com/product/claude-code) tutti i giorni, e ho iniziato a scrivere skill personali per qualunque cosa ripetessi più di tre volte. La ricerca chiavi era una di quelle: ogni volta che un gioco mi piaceva, finivo a controllare a mano Fanatical, GreenManGaming, Humble, GamesPlanet, Instant Gaming. Una skill che chiamasse [IsThereAnyDeal](https://isthereanydeal.com) mi avrebbe risolto la vita.

Detto, fatto. Una skill `steam-keys` che, alla domanda *"miglior prezzo Cyberpunk 2077"*, mi rispondeva con il negozio più conveniente, il prezzo, lo sconto e — bonus — se era un minimo storico oppure no.

Poi però mi sono chiesto: *"perché non aggiungere anche una mini-recensione dalle review Steam?"*. Quando un gioco è in sconto la prima cosa che faccio è cercare di capire se vale la pena. Aggiunto: la skill ora prendeva anche le recensioni utenti più recenti in italiano, e se non bastavano le auto-traduceva da altre lingue.

E perché non insegnarle i miei gusti? Sono un super fan degli ARPG stile Diablo (Path of Exile, Grim Dawn, Last Epoch). Aggiunto: ogni mattina la skill mi suggerisce 2-3 giochi del genere che potrebbero piacermi, sia in offerta che inclusi nel Game Pass.

A questo punto avevo:

- una skill che sapeva tutto sui prezzi delle chiavi
- una skill che leggeva le recensioni Steam
- un cron giornaliero che monitorava la mia wishlist e mi notificava su Telegram quando un gioco raggiungeva un nuovo minimo storico

Bene. Ma per consultarla dovevo scrivere a Claude. E le statistiche storiche del mio gaming non c'erano. *Quante ore ho fatto questa settimana? Quante della libreria non ho mai aperto? A che ora mi piace giocare?*

Servivano da qualche parte. Possibilmente in una pagina che potessi aprire al volo dal telefono.

## Cosa ho fatto

Ho tirato su un piccolo Flask sul mio server casalingo, sulla porta 8772. Una pagina sola, niente login (è raggiungibile solo dalla rete locale), refresh automatico a intervalli diversi a seconda di cosa stai guardando.

I dati arrivano da quattro fonti:

1. **API ufficiali Steam** (`GetPlayerSummaries`, `GetOwnedGames`, `GetRecentlyPlayedGames`) per libreria, ore giocate e cosa sto facendo *adesso*.
2. **API XboxLive** (xbl.io) per la presenza Xbox, perché ogni tanto gioco anche da console.
3. **SQLite locale**: ogni 10 minuti uno snapshot della libreria viene salvato in un database. Da lì calcolo il delta giornaliero (quanto ho giocato oggi), settimanale (grafico ultimi 7 giorni), e il picco orario degli ultimi 30 giorni.
4. **Database della skill `steam-keys`**: la wishlist con i prezzi storici già la stavo collezionando per le notifiche Telegram, mi bastava leggerla.

La dashboard è organizzata in card, ognuna risponde a una domanda specifica:

- **🎮 Now playing**: cosa sto giocando *adesso* (Steam o Xbox), con header del gioco
- **📊 Stats libreria**: totale giochi, mai aperti, ore totali, picco orario, copertura Game Pass
- **📅 Oggi**: minuti per gioco da mezzanotte
- **📈 Ultimi 7 giorni**: grafico a barre delle ore
- **⏰ Picco orario**: a che ora mi piace giocare (spoiler: dopo le 22)
- **🏆 Top 10 by ore**: classifica all-time
- **🔥 Ultimi 14 giorni**: cosa ho giocato di recente
- **🛒 Ultimi acquisti**: cosa ho aggiunto alla libreria (rilevato dal diff degli snapshot)
- **⭐ Consigliati per te**: ARPG mai aperti in libreria + simili a quelli che sto giocando + ARPG gratis su Game Pass
- **🔥 Trending Steam**: cosa sta giocando il mondo, con marker se l'ho già giocato
- **💰 Più venduti Steam**: top globale, con prezzo e sconto
- **🏷️ Wishlist in offerta**: i miei giochi in sconto, con stella ★ se è il minimo storico
- **📰 Feed Telegram**: ultimi report mandati dalla skill `steam-keys`

Ogni gioco è cliccabile. All'inizio il click apriva la pagina Steam in una tab nuova, ma da poche ore ho aggiunto una funzione che mi piaceva: cliccando si apre un popup con il riassunto delle recensioni Steam (positive %, descrizione tipo *Very Positive*) e tre snippet di recensioni reali, in italiano se ce ne sono. Se invece il gioco ha recensioni *Mixed* o peggio, il popup mostra solo titolo e bottone "Apri su Steam" — la sezione recensioni viene nascosta automaticamente.

```python
# Soglia per decidere se mostrare le recensioni nel popup
_REVIEWS_MIN_TOTAL = 10
_REVIEWS_MIN_POSITIVE_PCT = 70

# Cache 24h per appid (le recensioni cambiano lentamente)
show = (d["total_reviews"] >= _REVIEWS_MIN_TOTAL
        and d["percent_positive"] >= _REVIEWS_MIN_POSITIVE_PCT)
```

Tutta la cache è in memoria, semplice dict con TTL. Steam summary ogni 90 secondi, libreria ogni 20 minuti, recensioni 24 ore. Niente Redis, niente complicazioni — sono l'unico utente.

Il servizio gira come `systemd --user service`, riparte da solo se crasha. Uso totale di RAM: 20 MB. CPU: invisibile.

## Cosa ho imparato

**Le passioni si possono coltivare con l'AI tanto quanto il lavoro.** Non è solo per il negozio o per gli script che mi fanno risparmiare tempo. Quando ho un'ora libera la sera, mettere insieme una cosa così con Claude Code è un piacere puro: scrivo l'idea in italiano, lui produce il codice, io lo correggo dove serve. Tre serate per arrivare alla versione che uso ogni giorno.

**Le idee piccole sono le migliori da cui partire.** Se all'inizio mi fossi messo a progettare *"una dashboard completa per il mio gaming"* probabilmente non l'avrei mai finita. Sono partito da una skill di una funzione (cerca chiavi al prezzo minimo) e ho aggiunto un pezzo per volta solo quando il problema mi si presentava davvero. Ogni pezzo nasce da una frase tipo *"però sarebbe bello se anche..."*.

**Self-hosting per cose così è banale e gratis.** Nessun cloud, nessun abbonamento, nessuna preoccupazione di privacy: la mia libreria Steam, la mia wishlist, le mie ore di gioco restano sul mio server. Una porta, un servizio systemd, finito. Lo dico perché spesso si pensa che servano competenze devops sofisticate, e invece per uso personale basta un Raspberry o un mini PC sotto la TV.

## Risorse

- [IsThereAnyDeal API](https://docs.isthereanydeal.com) — il backend dei prezzi delle chiavi
- [Steam Web API](https://steamcommunity.com/dev) — libreria, ore, recensioni
- [xbl.io](https://xbl.io) — wrapper API XboxLive (versione free per uso personale)
- [Claude Code](https://claude.com/product/claude-code) — ne ho già parlato in [un altro post](/posts/cron-sentinel/), per me è diventato il modo standard di scrivere codice
