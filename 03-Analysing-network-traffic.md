# Analysing Network Traffic

#### Finding Top Talkers (Hosts with Most Traffic)

- Task1: Finding top talkers(Host with Most Traffic).
- **Objective**: Identify which hosts are generating the most traffic.

![Wireshark](/assets/15.png)
![Wireshark](/assets/16.png)

#### Detect Connection issues

- Task2: Identifying Connection issue.
- **Objective**: Detect TCP retransmissions and connection issues.

```sh
1. Le client envoie un paquet TCP au serveur
   Client --------------------> Serveur
          (Paquet envoyé)

2. Le serveur doit répondre avec un ACK (acquittement)
   Client <-------------------- Serveur
          (ACK reçu)

3. Si le client n'obtient pas l'ACK à temps (timeout), il retransmet le même paquet
   Client --------------------> Serveur
          (Paquet retransmis)

4. Le serveur finit par répondre avec un ACK
   Client <-------------------- Serveur
          (ACK reçu)
```

![Wireshark](/assets/17.png)
![Wireshark](/assets/18.png)
![Wireshark](/assets/19.png)
![Wireshark](/assets/20.png)
