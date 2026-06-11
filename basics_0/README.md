# Networking Basics #0

## Description

Ce projet introduit les concepts fondamentaux des réseaux informatiques : le modèle OSI, les types de réseaux, les adresses MAC et IP, les protocoles TCP/UDP, les ports et le protocole ICMP.

---

## Learning Objectives

À la fin de ce projet, vous devez être capable d'expliquer sans l'aide de Google :

### Modèle OSI
- Ce qu'est le modèle OSI
- Combien de couches il possède (7)
- Comment il est organisé (de la couche la plus basse — physique — à la plus haute — application)

### LAN / WAN / Internet
- LAN : réseau local (maison, bureau), portée géographique réduite
- WAN : réseau étendu reliant plusieurs LANs, portée nationale ou internationale
- Internet : réseau mondial interconnectant des WANs

### Adresses IP
- Une adresse IP identifie un appareil sur un réseau (comme une adresse postale)
- 2 types : adresses **publiques** (accessibles depuis Internet) et **privées** (réseau local)
- `localhost` = `127.0.0.1`, l'appareil lui-même
- Un **subnet** (sous-réseau) est une subdivision logique d'un réseau IP
- IPv6 a été créé car les adresses IPv4 (32 bits, ~4 milliards) sont épuisées

### TCP / UDP
- **TCP** : fiable, vérifie la réception des données, plus lent
- **UDP** : rapide, sans vérification, peut perdre des paquets
- Un **port** est une porte d'entrée logique sur un appareil (65535 ports disponibles)
- Ports à retenir : SSH = **22**, HTTP = **80**, HTTPS = **443**
- **ping** (ICMP) permet de vérifier si un appareil est accessible sur le réseau

---

## Requirements

- Ubuntu 22.04
- Scripts Bash interprétés avec `#!/usr/bin/env bash`
- Tous les scripts doivent passer `shellcheck` sans erreur
- Tous les fichiers doivent se terminer par une nouvelle ligne
- Les scripts doivent être exécutables

---

## Files

| Fichier | Type | Description |
|---|---|---|
| `0-OSI_model` | Réponses | Questions sur le modèle OSI |
| `1-types_of_network` | Réponses | LAN, WAN, Internet |
| `2-MAC_and_IP_address` | Réponses | Adresses MAC et IP |
| `3-UDP_and_TCP` | Réponses | Différences TCP/UDP |
| `4-TCP_and_UDP_ports` | Script Bash | Affiche les ports en écoute avec PID/programme |
| `5-is_the_host_on_the_network` | Script Bash | Ping une adresse IP 5 fois |

---

## Usage

```bash
# Ports en écoute (nécessite sudo pour voir tous les PIDs)
sudo ./4-TCP_and_UDP_ports

# Vérifier si un hôte est accessible
./5-is_the_host_on_the_network 8.8.8.8

# Sans argument → affiche l'usage
./5-is_the_host_on_the_network
# Usage: 5-is_the_host_on_the_network {IP_ADDRESS}
```

---

## Author

Holberton School — Networking Basics project
