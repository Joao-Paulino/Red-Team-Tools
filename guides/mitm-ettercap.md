# Man-in-the-Middle com Ettercap / Man-in-the-Middle with Ettercap

> **AVISO / WARNING**
> Executar apenas em laboratório isolado e com autorização. O uso em redes de terceiros é crime.
> Run only in an isolated lab with authorization. Using it on third-party networks is a crime.

## PORTUGUÊS

### Objetivo
Demonstrar a técnica de Man-in-the-Middle (MITM) via ARP Poisoning com a ferramenta Ettercap em ambiente controlado.

### Instalação (Linux)
- Debian/Ubuntu: `sudo apt install ettercap-graphical`
- Modo texto: `sudo apt install ettercap-text-only`
- Kali Linux: já vem pré-instalado
- Versão atual: 0.8.4.x (o pacote antigo `ettercap-gtk` está obsoleto)

### Procedimento
1. Cenário: vítima `192.168.200.10`, atacante `192.168.200.28`, gateway `192.168.200.1`.
2. Verificar tabela ARP antes: `arp -a`.
3. Ettercap: Sniff > Unified Sniffing > selecionar interface.
4. Host > Scan for hosts > Host > Host list.
5. Selecionar vítima > Add to Target 1; gateway > Add to Target 2.
6. Mitm > ARP Poisoning > marcar Sniff remote connections > OK.
7. Start > Start sniffing.
8. View > Connections para inspecionar tráfego e credenciais em texto claro.
9. Verificar tabela ARP após: `arp -a` (o MAC do gateway deve apontar para o atacante).

### Medidas preventivas
- Entradas ARP estáticas para o gateway.
- Protocolos criptografados: SSH, SFTP, HTTPS.
- Em switches: Dynamic ARP Inspection (DAI) + DHCP Snooping, rate limiting de ARP, port security e 802.1X.
- Deteção: `arpwatch`, `vamp-arp-sentinel`.

### Referências
- https://www.kali.org/tools/ettercap/
- https://github.com/Ettercap/ettercap

---

## ENGLISH

### Objective
Demonstrate a Man-in-the-Middle (MITM) attack via ARP Poisoning using Ettercap in a controlled environment.

### Installation (Linux)
- Debian/Ubuntu: `sudo apt install ettercap-graphical`
- Text-only: `sudo apt install ettercap-text-only`
- Kali Linux: pre-installed
- Current version: 0.8.4.x (the old `ettercap-gtk` package is deprecated)

### Procedure
1. Scenario: victim `192.168.200.10`, attacker `192.168.200.28`, gateway `192.168.200.1`.
2. Check ARP table before: `arp -a`.
3. Ettercap: Sniff > Unified Sniffing > select interface.
4. Host > Scan for hosts > Host > Host list.
5. Select victim > Add to Target 1; gateway > Add to Target 2.
6. Mitm > ARP Poisoning > check Sniff remote connections > OK.
7. Start > Start sniffing.
8. View > Connections to inspect traffic and cleartext credentials.
9. Check ARP table after: `arp -a` (gateway MAC should point to the attacker).

### Preventive measures
- Static ARP entries for the gateway.
- Encrypted protocols: SSH, SFTP, HTTPS.
- On switches: Dynamic ARP Inspection (DAI) + DHCP Snooping, ARP rate limiting, port security, and 802.1X.
- Detection: `arpwatch`, `vamp-arp-sentinel`.

### References
- https://www.kali.org/tools/ettercap/
- https://github.com/Ettercap/ettercap
