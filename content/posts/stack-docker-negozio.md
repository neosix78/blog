+++
date = '2026-04-29T17:38:58+02:00'
draft = true
title = 'Lo stack Docker di un negozio di elettrodomestici'
summary = "Cosa gira sotto il banco di Prezzismart: containers, micro-servizi e perché Ubuntu nudo invece di un PaaS."
tags = ["docker", "self-hosting", "stack", "retail"]
categories = ["infrastruttura"]
ShowToc = true
+++

> *Bozza — outline da espandere*

## Perché Docker, e perché su un Ubuntu nudo

- contesto: negozio reale, banda consumer, niente cloud-native
- vincoli: deve reggere se internet salta, deve girare se torno tardi
- alternative scartate (PaaS, k8s, "lasciamo tutto su Windows")

## Il pizzo di servizi

- Odoo
- n8n
- gestionale Danea (sync XML-RPC)
- altri micro-servizi (radio, social, scraper fornitori)

## Reverse proxy + DNS interno

## Backup e recovery

## Cosa rifarei e cosa no

---

*[da scrivere]*
