# TCPDUMP

#### Qu'est-ce que TCPDUMP ?

- TCPDUMP est un outil CLI d'analyse de paquets réseau, disponible sur les systèmes UNIX.

- Fonctionnalités clés pour :

  - 🕵️‍♂️ Capturer et analyser le trafic réseau en temps réel
  - 🔍 Diagnostiquer des problèmes de connectivité
  - 🛡️ Détecter des activités suspectes (scans, attaques DDoS)

- Compatibilité : Linux, macOS, BSD (nécessite les droits root)

#### Principaux avantages de TCPDUMP

```sh
|------------------------------------------|-----------------------------------------------|
| Surveillance du réseau en temps réel     | Filtrer le trafic pour une analyse spécifique |
|------------------------------------------|-----------------------------------------------|
| Enquête de sécurité et criminalistique   | Identifier les problèmes de trafic            |
|------------------------------------------|-----------------------------------------------|
```

#### Installation

- Sur Debian/Ubuntu

```sh
sudo apt update && sudo apt install tcpdump -y
```

- Vérification

```sh
tcpdump --version
```

#### Syntaxe de Base

```sh
tcpdump [options] [filtres]
```

- **Options Principales**

```sh
-------------|-------------------------------------------|-------------------------|
Option	           Description	                                  Exemple
-------------|-------------------------------------------|-------------------------|
-i eth0	         Interface réseau (any pour toutes)	        -i wlan0
-------------|-------------------------------------------|-------------------------|
-c 50            Limite le nombre de paquets	              -c 100
-------------|-------------------------------------------|-------------------------|
-w file.pcap	   Sauvegarde dans un fichier .pcap	          -w capture.pcap
-------------|-------------------------------------------|-------------------------|
-r file.pcap	   Lit une capture existante	                -r traffic.pcap
-------------|-------------------------------------------|-------------------------|
-n	             Désactive la résolution DNS	              -nn (ports aussi)
-------------|-------------------------------------------|-------------------------|
-v	             Verbosité (-vv pour plus de détails)	      -vvv
-------------|-------------------------------------------|-------------------------|
-s 0          	 Capture complète des paquets	               -s 1500
-------------|-------------------------------------------|-------------------------|
```

- **[Expression]**

```sh
host [IP]       : Capture le trafic provenant ou destiné à une adresse IP spécifique.
port [number]   : Capture le trafic provenant ou destiné à un port spécifique.
src [IP]        : Filtre les paquets provenant d'une adresse IP source.
dst [IP]        : Filtre les paquets destinés à une adresse IP cible.
tcp, udp, icmp  : Capture uniquement un protocole spécifique.
```

#### Commandes de base

```sh
sudo tcpdump -v
sudo tcpdump -vv -c 10
sudo tcpdump -D
sudo tcpdump -i any
sudo tcpdump -i any > file.out
sudo tcpdump -i wlp3s0 | tee file1.out
```

#### Capturer des paquets par protocoles

```sh
ping <IP>
sudo tcpdump -i wlp3s0 icmp
sudo tcpdump -i wlp3s0 tcp

# Attack
sudo nmap -sS <IP>
sudo nmap -sU <IP>
||
sudo tcpdump -i wlp3s0 udp
```

#### Capturer des paquets à l'aide de ports

```sh
sudo tcpdump -i wlp3s0 host <IP>

# Attack
sudo nmap -sS <IP>

||
sudo tcpdump -i wlp3s0 host <IP> -vv

# Attack
sudo nmap -sS <IP>
```

#### Capturer des paquets par source et destination spécifiques

```sh
sudo tcpdump -i wlp3s0 src host <IP> and dst host <IP>

nslookup cfitech.be

sudo tcpdump -i wlp3s0 src host <IP> and dst port 443
sudo tcpdump -i wlp3s0 src host <IP> and dst port 443 -vvv
sudo tcpdump -i wlp3s0 src host <IP> and dst net 192.168.1.0/24
```

#### Capturer les sondes d'analyse du réseau

```sh
# Une attaque par scan SYN

          SYN (443, 80)
Hacker  --------------------->  Serveur
          SYN + ACK
----------------------------------------
        tcpdump capture active
```

```sh
#tcpdump 'tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack = 0'

tcpdump -i eth0 'tcp[tcpflags] & (tcp-syn) != 0 and tcp[tcpflags] & (tcp-ack) == 0 and (port 80 or port 443)'

tcpdump -i eth0 'tcp[tcpflags] & (tcp-syn) != 0 and port (443 or 80)' -w scan_probes.pcap

# Attack
sudo nmap -sS <IP>
```
