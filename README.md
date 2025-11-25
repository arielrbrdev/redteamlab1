# 🔴 Red Strike Force: Pentest Full-Scope e Análise de Vulnerabilidades

## 🛡️ Destaques

* **Serviço:** Teste de Intrusão "Black Box" (Máxima Profundidade).
* **Metodologia:** Abrangência em todas as **Camadas do Modelo TCP/IP** (Link, Rede, Transporte, Aplicação), incluindo **Engenharia Social** e **Análise de Firmware**.
* **Ferramentas:** **GVM/OpenVAS**, **Nmap**, **SQLmap**, **Aircrack-ng**, **Ettercap**, **Hping3**.
* **Habilidades Demonstradas:** Pentesting Web (SQL Injection, XSS), Comprometimento Wireless (WEP/WPA2), Evasão de Firewall (Fragmentação IP), Análise de Sistemas EOL, Exploração de Zero-Day/RCE (OpenSSH).

---

## 🚀 Escopo e Metodologia

O Pentest foi realizado para a NutriServ Sistemas S.A. com uma abordagem **"Black Box"**, onde a equipe não recebeu informações prévias sobre a infraestrutura (sem IPs, diagramas, ou credenciais).

### Abordagem Multicamadas (TCP/IP)

| Camada | Vetores de Teste | Achados de Alto Impacto |
| :--- | :--- | :--- |
| **Link (L2) / Wireless** | **ARP Poisoning (Ettercap)**, Descoberta e teste de redes wireless (WEP/WPA2). | Quebra trivial de WEP (chave "senha") e de WPA2, permitindo MITM e roubo de tráfego. |
| **Rede (L3) / Transporte (L4)** | Port Scanning (Nmap), Evasão de Firewall, Varreduras de vulnerabilidade (GVM). | **Evasão de Filtro/ACLs por Fragmentação IP (Hping3)**. Múltiplos sistemas operacionais em **End-of-Life (EOL)**. |
| **Aplicação (L7)** | **OWASP Top 10**, credenciais padrão, análise de serviços legados (Telnet, FTP, RMI). | **SQL Injection** com extração de hashes MD5, **XSS** em jQuery desatualizado, **Exposição de `phpinfo()`**. |
| **Pós-Exploração** | Movimentação Lateral, Enumeração de AD, Persistência. | Identificação de um **Bind Shell (porta 1524)** no host Metasploitable, um caminho direto para acesso root. |

---

## 🛑 Achados Críticos (Exemplos de Prova de Conceito)

### 1. RCE Crítico em OpenSSH (CVSS 9.8 - 10.0)

A varredura de vulnerabilidades detectou falhas de **RCE (Execução de Código Remota)** de alto perfil em hosts internos:

* **Vulnerabilidade:** Versão **OpenSSH 9.3p2** associada às CVEs **CVE-2024-6387** (RegreSShion, RCE não autenticada) e **CVE-2023-38408**.
* **Impacto:** Permite o **comprometimento completo** (acesso de nível *root*) dos hosts afetados sem a necessidade de credenciais.
* **Recomendação:** Aplicação de *patch* imediata para uma versão corrigida.

### 2. Comprometimento de Aplicação Web (SQL Injection)

O host `dvwa.vm` (10.6.6.13) foi totalmente comprometido através de uma falha de injeção SQL:

* **Vetor:** O campo "User ID" permitiu a injeção do *payload* `' OR '1'='1`, listando todos os usuários e explorando a vulnerabilidade.
* **Exfiltração:** Uso de `UNION SELECT` para extrair todas as credenciais de usuário, incluindo **hashes de senha MD5**.
* **Lição:** Demonstra a falha na sanitização de *input*, levando à **violação completa da confidencialidade** e **integridade** do sistema.

### 3. Falha de Segurança Wireless (WEP)

A rede **`IoT Network`** foi invadida em minutos:

* **Vulnerabilidade:** Uso do protocolo de criptografia **WEP** (obsoleto e fraco).
* **Exploração:** A ferramenta **Aircrack-ng** quebrou a chave em ASCII (**"senha"**) em um curto período, com a coleta de aproximadamente 20.7k IVs.
* **Impacto:** Permite a **intercepção** (*eavesdropping*) e **injeção** de tráfego, expondo credenciais e permitindo *pivot* lateral para a rede interna.

---

## 🗺️ Topologia de Rede Identificada

Durante a fase de reconhecimento *Black Box*, a seguinte topologia foi mapeada no Cisco Packet Tracer:



O diagrama ilustra as redes **Virtual LAN 10.5.5.0/24**, **Virtual LAN 10.6.6.0/24**, a **DMZ 172.17.0.0/24** e o **Corporate Network 192.168.0.0/24**, mostrando a complexidade e a extensão do escopo avaliado.
