---
id: collect-261121-veille-ia/gpt-openai/mikrotik-routeros-7-24-1-stable-mirounga-publications
title: "Mikrotik RouterOS 7.24.1 Stable | Mirounga (Publications)"
domain: gpt-openai
role: reference
task: article
actors: ["OpenAI"]
dates: []
keywords: ["correction", "probleme", "lors", "stabilite", "amelioration", "introduit", "gestion", "mikrotik"]
source: https://www.mirounga.eu/blog/publications-4/mikrotik-routeros-7-24-1-stable-14
source_anchor: ""
source_lines: [1, 58]
section: "GPT_OpenAI"
---
## app

- correction de l'entrée et de la commande pour l'application opencloud-extended-collabora ;

## bridge

- correction du drapeau "E" manquant pour les commutateurs de la série CRS8xx ;
- correction du MLAG pour les commutateurs de la série CRS8xx ;
- correction d'un problème de stabilité lors de l'utilisation de VRRP sur le pont ;
- amélioration de la logique de vieillissement et de suppression des hôtes MLAG ;

## console

- correction du problème de recherche d'argument de la commande "find" (introduit dans la v7.24) ;

## container

- amélioration de l'isolation de l'hôte lorsque le conteneur est défini sur "privileged=yes" ;

## l3hw

- correction d'un problème de stabilité lors de l'activation du déchargement IPv6 ;
- correction du déchargement VRF lors de la suppression d'une interface de liaison ;

## leds

- correction du déclencheur "interface-status" ;

## snmp

- validation correcte de la longueur du mot de passe lors de l'application de la configuration ;

## ssh

- refactorisation des processus internes du service SSH ;

## switch

- correction du blocage possible du trafic Rx sur le CPU pour les appareils avec puce de commutation Marvell Prestera ;
- correction d'un problème de stabilité lors d'une boucle L2 pour les puces de commutation 98DX224S, 98DX226S, 98DX2528 et 98DX3236 ;
- correction d'un problème de stabilité sur le RB3011 (introduit dans la v7.22) ;

## system

- amélioration de la gestion des requêtes SSL/TLS invalides ;

## wifi

- correction d'un problème de stabilité pour le hAP be lite ;

## wireguard

- correction de la gestion de l'état de désactivation/activation des pairs, qui pouvait laisser le tunnel non fonctionnel (introduit dans la v7.24) ;
- correction de la gestion des clés privées, y compris les clés privées vides ;

## www

- amélioration de la réactivité du service lors de la réception de paquets malformés ;
