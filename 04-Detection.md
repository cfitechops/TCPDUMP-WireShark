# Suricata

- IDS/IPS (http/https, ftp, smd)
- Automatic Protocol detection
- Lua Scripting
- Multi-threading

#### How does Suricata work ?

- Network Traffic acquisition
- Packet Persing and Analysis
- Signature Matching
- Deep Packet Inspection (Optional)
- Action and Logging

```sh
Rules --> Signature --> Alert --> log (Suricata.log)
```

#### What Protocols are used in Suricata ?

- Basic Protocols:

  - TCP (Transmission Control Protocol)
  - UDP (User Datagram Protocol)
  - ICMP (Internet Control Message Protocol)
  - IP (Internet Protocol)

- Application Layer ProtocolS(Layer 7):

  - HTTP(Hypertext Transfer Protocol)
  - HTTP/2
  - FTP (Filr Transfer Protocol)
  - TLS/SSL (Transport Layer Security/Secure Sockets Layer)
  - SMB (Server Message Block)
  - DNS (Domain Name System)

#### Suricata Rules

```sh
alert http $HOME_NET any -> $EXTERNAL_NET any (msg: "HTTP GET Request Containing Rule in URI"; flow:established, to_server; http.method; content: "GET"; http.uri; content: "rule"; fast_pattern; classtype:bad-unknown; sid:123; rev:1;)
```

- A rule/signature consists of the following:

  - The **action**, determining what happens when the rule matches.
  - The **header**, defining the protocol, IP addresses, ports and direction of the rule.
  - The **rule options**, defining the specifics of the rule.

**Red** is the action, **green** is the header and **blue** are the options.

- Valid actions ared:

  - **alert** - generate an alert (IDS)
  - **pass** - stop further inspection of the packet
  - **drop** - drop packet and generate alert
  - **reject** - send RST/ICMP unreach error to the sender of the macthing packet (IPS)
  - **rejectsrc** - same as just reject.
  - **rejectdst** - send RST/ICMP error packet to receiver of the macthing packet
  - **rejectboth** - send RST/ICMP error packets to both sides of the conversation

```sh
HOME_NET = <UBUNTU IP>
EXTERNAL_NET = "any"
http = http://google.com/image/rule
```

#### Installing Suricata IDS on Ubuntu Server

```sh
uname -a
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
sudo apt-get install suricata -y
sudo systemctl status suricata
cd /etc/suricata
ls
mkdir rules

cd /tmp/ && curl -LO https://rules.emergingthreats.net/open/suricata-6.0.8/emerging.rules.tar.gz
sudo tar -xvzf emerging.rules.tar.gz && sudo mv rules/*.rules /etc/suricata/rules/
sudo chmod 640 /etc/suricata/rules/*.rules

ls
cd /etc/suricata/rules
```

#### Setting up Suricata IDS

root@forwarder:~# sudo nano /etc/suricata/suricata.yaml

```sh
vars:
  address-groups:
    HOME_NET: "[192.168.129.206]"

    #EXTERNAL_NET: "!$HOME_NET"
    EXTERNAL_NET: "any"

default-rule-path: /etc/suricata/rules

rule-files:
  - "*.rules"

# Global stats configuration
stats:
  enabled: yes

# Linux high speed capture support
af-packet:
  - interface: enp0s3
```

```sh
sudo systemctl restart suricata
sudo systemctl status suricata
```

#### Suricata Logs

```sh
cd /var/log/suricata/
ls
tail -f fast.log
```

#### Labo: Nmap Scan detection using Suricata IDS

```sh
tail -f /var/log/suricata/fast.log
```

#### Attack

```sh
nmap -sS <IP>
```
