---
title: "Network Enumeration with Nmap"
subtitle: "Anotações completas do módulo — HTB Academy"
date: "31/03/2026"
lang: "pt-BR"
toc: true
toc-depth: 3
fontsize: 11pt
geometry: "margin=2.2cm"
---

# Visão geral do módulo

**Título original:** `Network Enumeration with Nmap`  
**Tradução:** **Enumeração de Rede com Nmap**

**Categoria:** Regular / Offensive  
**Dificuldade:** Easy  
**Tier:** 1  
**Tempo estimado:** 7 horas  
**Total de seções:** 12  
**Exercícios interativos:** 7  
**Criador:** Cry0l1t3
**Status:** **Concluído** · `+10 Cubes`

## Descrição do módulo

O **Nmap** é uma das ferramentas de **mapeamento e descoberta de rede** mais utilizadas por causa de seus **resultados precisos** e de sua **eficiência**. A ferramenta é amplamente usada tanto por profissionais de segurança **ofensiva** quanto **defensiva**. Este módulo cobre os **fundamentos necessários** para utilizar o Nmap na realização de uma **enumeração de rede eficaz**.

## Resumo do módulo

O **Nmap** é usado para **identificar e escanear sistemas** em uma rede. Ele é uma parte importante do **diagnóstico de rede** e da **avaliação de sistemas conectados à rede**. Neste módulo, aprendemos o básico da ferramenta e como ela pode ser usada de forma eficiente para:

- mapear redes;
- identificar hosts ativos;
- escanear portas;
- enumerar serviços;
- detectar sistemas operacionais;
- usar scripts do NSE;
- entender impacto de firewall, IDS e IPS na enumeração.

## Estrutura do módulo

1. **Enumeration**
2. **Introduction to Nmap**
3. **Host Discovery**
4. **Host and Port Scanning**
5. **Saving the Results**
6. **Service Enumeration**
7. **Nmap Scripting Engine**
8. **Performance**
9. **Firewall and IDS/IPS Evasion**
10. **Firewall and IDS/IPS Evasion - Easy Lab**
11. **Firewall and IDS/IPS Evasion - Medium Lab**
12. **Firewall and IDS/IPS Evasion - Hard Lab**

---

# O que fizemos juntos neste chat

Ao longo deste módulo, o trabalho feito em conjunto neste chat foi:

- tradução fiel de todo o overview do módulo para **pt-BR**;
- tradução progressiva das **12 sessões**;
- consolidação de **anotações extremamente completas** por sessão;
- explicações conceituais sobre:
  - enumeração;
  - host discovery;
  - port scanning TCP e UDP;
  - version detection;
  - banner grabbing;
  - NSE;
  - tuning de performance;
  - leitura de comportamento de firewall/IDS/IPS;
- organização do conteúdo já pensando em uma versão final **GitHub-ready** e em **backup em PDF**.
- integração do **cheat sheet oficial do módulo** como apêndice de consulta rápida.

> **Nota importante sobre os labs das seções 10, 11 e 12:**  
> Nesta conversa foram fornecidos apenas os **enunciados introdutórios** desses laboratórios. Portanto, as notas finais registram o **contexto, a intenção do exercício e os takeaways estratégicos**, mas **não** um walkthrough completo da resolução prática desses labs, porque esse material específico não foi enviado.

## Hands-on do módulo — resolução integrada

Além das anotações teóricas deste chat, a resolução prática do módulo foi integrada a partir do export fiel do outro chat de hands-on. Com isso, esta versão final passa a registrar **o que foi feito na prática**, **como cada exercício foi destravado** e **qual foi o resultado obtido**.

### Conquista do módulo

- **Módulo concluído com sucesso**
- **Recompensa obtida:** `+10 Cubes`

## Hands-on 1 — Banner grabbing manual em serviço suspeito

### Contexto

Após uma enumeração ampla, o alvo mais suspeito apareceu como:

- `31337/tcp open ftp ProFTPD`

A porta `31337` chamou atenção por ser altamente “CTF-like”, e a hipótese foi que o Nmap não estava mostrando tudo no banner por padrão.

### Abordagem usada

Foco em **banner grabbing manual**, priorizando a porta 31337.

```bash
nc -nv 10.129.25.235 31337
```

### Resultado

A resposta do serviço entregou a flag diretamente no banner:

```text
220 HTB{pr0F7pDv3r510nb4nn3r}
```

### Resposta final do exercício

```text
HTB{pr0F7pDv3r510nb4nn3r}
```

### Takeaway prático

Quando um serviço aparece em uma porta não convencional e o hint sugere que o Nmap pode não estar mostrando tudo, **banner grabbing manual** pode ser mais eficaz do que confiar só na detecção automática de versão.

---

## Hands-on 2 — Enumeração web com NSE e descoberta via `robots.txt`

### Contexto

O alvo web apresentava apenas a página padrão do Apache, então a estratégia foi usar scripts HTTP do NSE para procurar artefatos escondidos.

### Abordagem usada

```bash
nmap -Pn -p80 --script http-title,http-headers,http-server-header,http-enum 10.129.25.241
```

Esse scan mostrou:

- `Apache2 Ubuntu Default Page: It works`
- `Apache/2.4.29 (Ubuntu)`
- descoberta de ` /robots.txt ` via `http-enum`

Depois, a enumeração foi aprofundada com acesso direto ao arquivo:

```bash
curl http://10.129.25.241/robots.txt
```

### Resultado

O conteúdo do `robots.txt` revelou a flag:

```text
User-agent: *
Allow: /
HTB{873nniuc71bu6usbs1i96as6dsv26}
```

### Resposta final do exercício

```text
HTB{873nniuc71bu6usbs1i96as6dsv26}
```

### Takeaway prático

Mesmo quando o site parece genérico, **scripts NSE voltados para HTTP** podem apontar rapidamente para arquivos relevantes. Nesse caso, `http-enum` foi suficiente para revelar o caminho crítico e o `curl` confirmou a resposta.

---

## Hands-on 3 — Easy Lab: detecção silenciosa de sistema operacional

### Contexto

No laboratório fácil de evasão, o objetivo era descobrir o sistema operacional do host **com o mínimo de ruído possível**, levando em conta risco de alerta e banimento.

### Alvo

- `10.129.26.209`

### Estratégia usada

Primeiro, foi feito um scan leve nas portas mais comuns:

```bash
nmap -Pn -sS --top-ports 10 --max-retries 1 -T2 -vvv 10.129.26.209
```

Esse scan encontrou principalmente:

- `22/tcp open ssh`
- `80/tcp open http`

Em seguida, foi feita uma confirmação silenciosa por banner:

```bash
nmap -Pn -sV --version-light -p22,80 --max-retries 1 -T2 10.129.26.209
```

### Evidências observadas

- `OpenSSH 7.6p1 Ubuntu 4ubuntu0.7`
- `Apache httpd 2.4.29 ((Ubuntu))`
- `Service Info: OS: Linux`

### Resposta final do exercício

```text
Ubuntu
```

### Takeaway prático

Quando o lab pede o nome do sistema operacional, a resposta correta pode ser a versão **mais específica revelada pelos banners**. Aqui, embora “Linux” fosse plausível, a resposta exata esperada era **Ubuntu**.

---

## Hands-on 4 — Medium Lab: enumeração DNS via UDP com `dns-nsid`

### Contexto

O laboratório médio explicitava a necessidade de prestar atenção ao **UDP na VPN**. A linha de raciocínio adotada foi focar primeiro em DNS, pela porta 53/udp.

### Alvo

- `10.129.26.211`

### Estratégia usada

Confirmação inicial da porta DNS:

```bash
sudo nmap -Pn -sU -p53 --max-retries 1 -T2 10.129.26.211
```

Depois, enumeração com script NSE específico:

```bash
sudo nmap -Pn -sU -p53 -sV --script dns-nsid 10.129.26.211
```

### Resultado

O script `dns-nsid` consultou o `bind.version` e revelou diretamente a flag:

```text
HTB{GoTtgUnyze9Psw4vGjcuMpHRp}
```

### Resposta final do exercício

```text
HTB{GoTtgUnyze9Psw4vGjcuMpHRp}
```

### Takeaway prático

Em serviços DNS, scripts NSE especializados podem entregar muito mais valor do que um scan UDP genérico. Neste caso, a combinação entre **UDP 53** e **`dns-nsid`** foi suficiente para resolver o exercício.

---

## Hands-on 5 — Hard Lab: porta escondida, evasão por source port e banner manual

### Contexto

Este foi o exercício mais trabalhoso do módulo. O alvo já apresentava apenas banners normais em `22/tcp` e `80/tcp`, então a dificuldade real estava em descobrir **qual serviço adicional estava sendo protegido pelo filtro** e **qual forma de comunicação fazia esse serviço responder**.

### Alvo

- `10.129.26.213`

### Primeira fase — enumeração com `--source-port 53`

O primeiro passo foi usar scans com **porta de origem 53**, partindo da hipótese de que o firewall estava mais permissivo com tráfego que parecesse DNS.

```bash
sudo nmap -Pn -n -sS -p- --open --max-retries 1 -T2 --source-port 53 -vvv 10.129.26.213
```

Esse scan encontrou inicialmente apenas:

- `22/tcp open ssh`
- `80/tcp open http`

Depois, foi feita enumeração de versão nas portas já visíveis:

```bash
sudo nmap -Pn -n -sV --version-all -p22,80 --source-port 53 --max-retries 1 -T3 10.129.26.213
sudo nmap -Pn -n -sV --version-all --script=banner -p22,80 --source-port 53 --max-retries 1 -T3 10.129.26.213
sudo nmap -Pn -n -p80 -sV --version-all --script http-title,http-headers,banner --source-port 53 --max-retries 1 -T3 10.129.26.213
```

As respostas mostraram apenas banners normais:

- `OpenSSH 7.6p1 Ubuntu 4ubuntu0.7`
- `Apache httpd 2.4.29 ((Ubuntu))`
- `Apache2 Ubuntu Default Page: It works`

### Segunda fase — hipóteses descartadas

Como a resposta ainda não aparecia, a investigação foi avançando por hipóteses sucessivas:

#### Hipótese A — Banco de dados

Foram testadas portas típicas de serviços de banco:

```bash
sudo nmap -Pn -n -sS -p3306,5432,1433,1521,27017,6379 --open --max-retries 1 -T3 --source-port 53 -f 10.129.26.213
```

#### Hipótese B — Serviços de armazenamento/compartilhamento

Foram testadas portas ligadas a NFS, SMB, rsync e similares:

```bash
sudo nmap -Pn -n -sS -p21,111,139,445,873,2049,3260 --open --max-retries 1 -T3 --source-port 53 -f -vvv 10.129.26.213
```

Também foram testadas variações usando portas de origem `20`, `21`, `2049` e `111`, além de scans UDP em `111` e `2049`.

Apesar de toda essa investigação, o resultado continuava praticamente limitado a `22/tcp` e `80/tcp`, o que indicava que o serviço escondido estava fora do caminho mais óbvio.

### Terceira fase — descoberta da porta escondida

A virada do exercício veio com o foco explícito na **porta 50000**, ainda usando a origem 53:

```bash
sudo nmap -Pn -n -p50000 --source-port 53 10.129.26.213
```

### Resultado intermediário

```text
50000/tcp open ibm-db2
```

Ou seja, a porta relevante finalmente apareceu como aberta.

### Quarta fase — conexão manual para forçar resposta do serviço

Conectar com `nc` ou `ncat` na porta 50000 apenas abria a sessão, mas o serviço não devolvia banner automaticamente. O passo final foi enviar um input mínimo:

```bash
printf '
' | sudo ncat -nv --source-port 53 10.129.26.213 50000
```

### Resultado final

A resposta do serviço trouxe a flag diretamente:

```text
220 HTB{kjnsdf2n982n1827eh76238s98di1w6}
500 Invalid command: try being more creative
```

### Resposta final do exercício

```text
HTB{kjnsdf2n982n1827eh76238s98di1w6}
```

### Takeaways práticos do hard lab

- encontrar a porta certa nem sempre basta; às vezes é preciso **fazer o serviço falar** manualmente;
- `-sV` pode marcar algo como `tcpwrapped` e ainda assim não mostrar o que uma conexão manual simples revelaria;
- o raciocínio do exercício exigiu **ajuste de hipótese**, **troca de source port**, **descoberta de porta escondida** e **interação manual com banner**.

---

## Resumo consolidado do hands-on

### Exercícios resolvidos e respostas finais

1. **Banner grabbing manual em ProFTPD na porta 31337**  
   Resposta: `HTB{pr0F7pDv3r510nb4nn3r}`

2. **Enumeração HTTP até `robots.txt`**  
   Resposta: `HTB{873nniuc71bu6usbs1i96as6dsv26}`

3. **Easy Lab — sistema operacional do alvo**  
   Resposta: `Ubuntu`

4. **Medium Lab — enumeração DNS via UDP 53 / `dns-nsid`**  
   Resposta: `HTB{GoTtgUnyze9Psw4vGjcuMpHRp}`

5. **Hard Lab — serviço escondido na porta 50000 com source port 53**  
   Resposta: `HTB{kjnsdf2n982n1827eh76238s98di1w6}`

### O que o hands-on reforçou na prática

- banner grabbing manual continua essencial;
- scripts NSE certos economizam muito tempo;
- em ambiente com filtro e IDS/IPS, a escolha do **tipo de scan**, **timing**, **source port** e **nível de ruído** muda completamente o resultado;
- serviços “inofensivos” na saída padrão do Nmap podem esconder a pista real;
- conexão manual com `nc`/`ncat` ainda é decisiva quando a enumeração automática não fecha o raciocínio.

---


# Objetivos de aprendizagem

Ao finalizar este módulo, os principais objetivos dominados foram:

- entender a **mentalidade correta de enumeração**;
- usar o **Nmap** para descobrir hosts vivos;
- interpretar estados de portas com confiança;
- diferenciar **SYN scan**, **Connect scan** e **UDP scan**;
- detectar serviços e versões com `-sV`;
- salvar resultados de forma adequada com `-oN`, `-oG`, `-oX` e `-oA`;
- usar o **NSE** para ampliar enumeração e triagem inicial;
- ajustar desempenho de scans sem perder muita visibilidade;
- entender, em nível conceitual, como firewalls, IDS e IPS afetam a enumeração.

---

# Fundamentos centrais do módulo

## 1. Enumeração é a chave

A sessão inicial do módulo deixa uma mensagem muito clara:

**o objetivo não é “invadir logo” o alvo; o objetivo é entender o alvo o máximo possível.**

Enumeração é o processo de coletar o maior volume de informação útil sobre:

- hosts;
- portas;
- serviços;
- versões;
- sistema operacional;
- banners;
- comportamento da rede;
- controles defensivos;
- possíveis superfícies de ataque.

Quanto mais informação correta existe, mais fácil fica:

- montar hipóteses;
- encontrar vetores de ataque;
- validar o que é real e o que é ruído;
- evitar perda de tempo com caminhos errados.

## 2. Ferramenta não substitui entendimento

Outro ponto muito importante do módulo:

- ferramentas ajudam;
- mas elas **não substituem** conhecimento sobre protocolos e serviços.

O diferencial está em saber:

- como um serviço normalmente responde;
- o que significa uma ausência de resposta;
- o que pode ser banner real e o que pode ser inferência do Nmap;
- quando o scanner está certo;
- quando o scanner provavelmente está omitindo contexto.

## 3. Enumeração manual continua crítica

Scanners aceleram muito o trabalho, mas nem sempre revelam tudo.

O módulo mostra isso em vários pontos:

- host discovery com ARP versus ICMP;
- UDP sem resposta;
- banner de serviço mais rico do que a saída final do Nmap;
- estados `filtered` que exigem interpretação;
- ajustes de performance que podem introduzir falso negativo.

---

# Cheatsheet principal do módulo

## Sintaxe base

```bash
nmap <scan types> <options> <target>
```

## Descoberta de hosts

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet
sudo nmap -sn -oA tnet -iL hosts.lst
sudo nmap 10.129.2.18 -sn -oA host
sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping
```

## Port scanning TCP

```bash
sudo nmap -sS localhost
sudo nmap 10.129.2.28 --top-ports=10
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT
```

## Port scanning UDP

```bash
sudo nmap 10.129.2.28 -F -sU
sudo nmap 10.129.2.28 -sU -Pn -n --disable-arp-ping --packet-trace -p 137 --reason
```

## Salvar resultados

```bash
sudo nmap 10.129.2.28 -p- -oA target
xsltproc target.xml -o target.html
```

## Service enumeration

```bash
sudo nmap 10.129.2.28 -p- -sV
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
sudo nmap 10.129.2.28 -p- -sV -v
```

## Banner grabbing complementar

```bash
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
nc -nv 10.129.2.28 25
```

## NSE

```bash
sudo nmap <target> -sC
sudo nmap <target> --script <category>
sudo nmap <target> --script <script1>,<script2>
sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
sudo nmap 10.129.2.28 -p 80 -A
sudo nmap 10.129.2.28 -p 80 -sV --script vuln
```

## Performance

```bash
sudo nmap 10.129.2.0/24 -F
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
sudo nmap 10.129.2.0/24 -F --max-retries 0
sudo nmap 10.129.2.0/24 -F --min-rate 300
sudo nmap 10.129.2.0/24 -F -T 5
```

---

# Anotações completas por sessão

# Sessão 1 — Enumeration

## Ideia principal

A **enumeração** é a parte mais crítica do processo. O alvo não é “obter acesso rápido”, mas sim **identificar todas as formas possíveis de ataque**, a partir da maior quantidade possível de informação confiável.

## Pontos principais

- Ferramentas só são úteis se soubermos o que fazer com os dados que elas retornam.
- É essencial entender:
  - como os serviços funcionam;
  - qual sintaxe usam;
  - como interagir corretamente com eles.
- Enumeração é coleta máxima de informação.
- Quanto melhor a enumeração, mais claros ficam:
  - vetores de ataque;
  - misconfigurations;
  - comportamentos anômalos;
  - caminhos reais de exploração.

## Dois objetivos principais da enumeração

1. Encontrar **funções e recursos** que permitam interação com o alvo.
2. Encontrar **informações adicionais** que facilitem o acesso ao sistema.

## Takeaways

- **Enumeração é a chave.**
- Ferramenta sem interpretação técnica gera resultado pobre.
- Enumeração manual é componente crítico.
- Misconfigurations e negligência defensiva frequentemente entregam pistas valiosas.

---

# Sessão 2 — Introduction to Nmap

## O que é o Nmap

O **Nmap** é uma ferramenta open-source de:

- análise de rede;
- auditoria de segurança;
- descoberta de hosts;
- enumeração de portas;
- detecção de serviços;
- fingerprint de sistema operacional.

## Casos de uso

- auditoria de segurança;
- suporte a testes de intrusão;
- validação de firewall e IDS;
- mapeamento de rede;
- análise de respostas;
- identificação de portas abertas;
- apoio à avaliação de vulnerabilidades.

## Arquitetura lógica do uso do Nmap

1. **Host discovery**
2. **Port scanning**
3. **Service enumeration and detection**
4. **OS detection**
5. **Scriptable interaction via NSE**

## Tipos de scan apresentados

- `-sS` → SYN scan
- `-sT` → TCP Connect scan
- `-sA` → ACK scan
- `-sU` → UDP scan
- `-sN`, `-sF`, `-sX` → Null, FIN e Xmas
- `-sO` → IP protocol scan
- `-sI` → Idle scan
- `--scanflags` → customização de flags

## SYN scan (`-sS`)

O módulo introduziu o SYN scan como um dos métodos mais importantes.

### Comportamento

- envia `SYN`;
- se recebe `SYN-ACK`, interpreta **open**;
- se recebe `RST`, interpreta **closed**;
- se não recebe resposta, tende a interpretar **filtered**.

### Vantagem

- rápido;
- muito usado;
- não completa o three-way handshake.

## Exemplo visto

```bash
sudo nmap -sS localhost
```

## O que a saída mostrou

- host local ativo;
- quatro portas TCP abertas;
- associação entre porta, estado e serviço.

## Takeaways

- O Nmap não é só “scanner de porta”.
- Ele é uma plataforma de enumeração.
- Entender a lógica do scan importa tanto quanto executar o comando.

---

# Sessão 3 — Host Discovery

## Ideia principal

Antes de escanear serviços e portas, precisamos saber:

**quais hosts estão vivos?**

## Comando-base da sessão

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet
```

## Flags principais

- `-sn` → desabilita port scanning e foca em host discovery
- `-oA` → salva em todos os formatos
- `-iL` → lê alvos de arquivo

## Formas mostradas no módulo

### Escanear uma subnet

```bash
sudo nmap 10.129.2.0/24 -sn -oA tnet
```

### Escanear uma lista de IPs

```bash
sudo nmap -sn -oA tnet -iL hosts.lst
```

### Escanear múltiplos IPs

```bash
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20
```

### Escanear faixa do octeto

```bash
sudo nmap -sn -oA tnet 10.129.2.18-20
```

## ARP versus ICMP

Essa foi uma das partes mais valiosas da sessão.

### O que a prática mostrou

Mesmo usando `-PE`, em rede local o Nmap pode identificar um host como ativo **via ARP**, antes mesmo de usar ICMP.

### Ferramentas de validação

- `--packet-trace`
- `--reason`
- `--disable-arp-ping`

### Lição

Nem sempre “host is up” quer dizer a mesma coisa. Pode ser:
- resposta ARP;
- resposta ICMP;
- outra forma de evidência.

## Takeaways

- Ausência de resposta não significa automaticamente host inexistente.
- Firewall e filtros podem esconder hosts vivos.
- Validar o *porquê* do “host up” é parte da enumeração madura.

---

# Sessão 4 — Host and Port Scanning

## Estados de porta no Nmap

O módulo apresentou seis estados:

- `open`
- `closed`
- `filtered`
- `unfiltered`
- `open|filtered`
- `closed|filtered`

## Interpretação resumida

- **open** → serviço aceitando interação
- **closed** → porta acessível, mas sem serviço escutando
- **filtered** → o Nmap não consegue concluir por causa de filtro/firewall
- **unfiltered** → acessível, mas sem definição exata entre aberta e fechada
- **open|filtered** → comum em scans UDP ou cenários com pouca resposta
- **closed|filtered** → aparece em contextos mais específicos como idle scan

## Descobrindo TCP aberto

### Exemplos apresentados

```bash
sudo nmap 10.129.2.28 --top-ports=10
sudo nmap 10.129.2.28 -p 21 --packet-trace -Pn -n --disable-arp-ping
sudo nmap 10.129.2.28 -p 443 --packet-trace --disable-arp-ping -Pn -n --reason -sT
```

## SYN scan versus Connect scan

### `-sS`
- mais furtivo;
- não completa o handshake;
- muito comum em enumeração.

### `-sT`
- completa o handshake;
- costuma ser mais barulhento;
- porém é bastante preciso.

## Portas filtradas

A sessão mostrou a diferença entre:

### Drop
- sem resposta;
- retransmissões;
- maior demora;
- resultado típico: `filtered`.

### Reject
- resposta explícita;
- ajuda a inferir o comportamento defensivo.

## UDP scanning

### Exemplo

```bash
sudo nmap 10.129.2.28 -F -sU
```

## Regras práticas importantes para UDP

- resposta UDP do serviço → **open**
- ICMP type 3 code 3 → **closed**
- ausência de resposta → frequentemente **open|filtered**

## Takeaways

- Resultado de porta precisa ser interpretado com contexto.
- TCP e UDP têm lógicas muito diferentes.
- `filtered` não é “sem utilidade”; é um sinal sobre a defesa.

---

# Sessão 5 — Saving the Results

## Objetivo

Salvar scan não é detalhe: é parte do processo profissional.

## Formatos de saída

### `-oN`
Saída normal  
Extensão: `.nmap`

### `-oG`
Saída grepable  
Extensão: `.gnmap`

### `-oX`
Saída XML  
Extensão: `.xml`

### `-oA`
Salva todos os formatos de uma vez.

## Comando principal da sessão

```bash
sudo nmap 10.129.2.28 -p- -oA target
```

## Arquivos gerados

- `target.nmap`
- `target.gnmap`
- `target.xml`

## Uso prático de cada formato

### `.nmap`
Melhor para leitura humana.

### `.gnmap`
Melhor para parsing rápido com:
- `grep`
- `cut`
- `awk`
- `sed`

### `.xml`
Melhor para:
- automação;
- parsing estruturado;
- integração;
- relatório.

## Converter XML em HTML

```bash
xsltproc target.xml -o target.html
```

## Takeaways

- Scan salvo pode ser revisitado, comparado e documentado.
- `-oA` é uma excelente prática padrão.
- XML é o formato mais reaproveitável para automação e reporting.

---

# Sessão 6 — Service Enumeration

## Objetivo

Descobrir **qual serviço** está rodando e **qual versão** ele expõe.

## Comando-base

```bash
sudo nmap 10.129.2.28 -p- -sV
```

## O que `-sV` faz

Tenta identificar:

- nome do serviço;
- versão;
- às vezes hostname;
- às vezes SO;
- às vezes CPE.

## Monitorar scans longos

### Barra de espaço

Durante o scan, a barra de espaço exibe status parcial.

### `--stats-every`

```bash
sudo nmap 10.129.2.28 -p- -sV --stats-every=5s
```

## Verbosidade

```bash
sudo nmap 10.129.2.28 -p- -sV -v
```

Permite ver portas abertas à medida que forem sendo encontradas.

## Banner grabbing

O módulo mostrou que o Nmap muitas vezes usa banners diretamente e, quando isso não basta, tenta casar com assinaturas conhecidas.

### Ponto muito importante

Nem sempre a saída final do Nmap mostra tudo o que o serviço realmente entregou.

## Exemplo SMTP

O banner manual mostrou mais contexto do que a saída resumida do Nmap:

- hostname;
- software;
- pista da distribuição Linux.

## Ferramentas complementares

### `nc`

```bash
nc -nv 10.129.2.28 25
```

### `tcpdump`

```bash
sudo tcpdump -i eth0 host 10.10.14.2 and 10.129.2.28
```

## Fluxo TCP observado

1. `SYN`
2. `SYN-ACK`
3. `ACK`
4. `PSH-ACK` (banner/dados)
5. `ACK`

## Takeaways

- Porta aberta sem versão é só metade da enumeração.
- Banner grabbing manual pode revelar mais do que o scanner resume.
- `nc` e `tcpdump` complementam o Nmap muito bem.

---

# Sessão 7 — Nmap Scripting Engine (NSE)

## O que é o NSE

O **Nmap Scripting Engine** permite usar scripts em **Lua** para interação com serviços específicos.

## Categorias relevantes do módulo

- `default`
- `discovery`
- `safe`
- `version`
- `auth`
- `brute`
- `intrusive`
- `exploit`
- `dos`
- `vuln`

## Formas de uso

### Scripts padrão

```bash
sudo nmap <target> -sC
```

### Categoria inteira

```bash
sudo nmap <target> --script <category>
```

### Scripts específicos

```bash
sudo nmap <target> --script <script1>,<script2>
```

## Exemplo SMTP

```bash
sudo nmap 10.129.2.28 -p 25 --script banner,smtp-commands
```

### O que isso entregou

- banner SMTP;
- identificação do Postfix;
- pista de Ubuntu;
- comandos suportados pelo servidor.

## `-A`

O scan agressivo agrega:

- `-sV`
- `-O`
- `--traceroute`
- scripts padrão (`-sC`)

### Exemplo

```bash
sudo nmap 10.129.2.28 -p 80 -A
```

## `--script vuln`

Usado para triagem inicial de vulnerabilidades conhecidas.

### Exemplo

```bash
sudo nmap 10.129.2.28 -p 80 -sV --script vuln
```

### O que apareceu no material

- indícios de WordPress;
- páginas administrativas;
- username `admin`;
- referências a CVEs associadas ao Apache 2.4.29.

## Takeaways

- O NSE transforma o Nmap em ferramenta de enumeração ativa.
- `-sC` é um ótimo baseline.
- `--script vuln` ajuda na triagem, mas não substitui validação manual.
- `-A` é conveniente, porém mais ruidoso e menos controlado.

---

# Sessão 8 — Performance

## Ideia principal

Ajustar performance melhora velocidade, mas pode reduzir visibilidade.

## RTT

### Exemplo

```bash
sudo nmap 10.129.2.0/24 -F --initial-rtt-timeout 50ms --max-rtt-timeout 100ms
```

### Lição

Reduzir RTT demais pode fazer o scan:
- perder hosts;
- interpretar atraso como ausência;
- introduzir falso negativo.

## Retries

### Exemplo

```bash
sudo nmap 10.129.2.0/24 -F --max-retries 0
```

### Lição

Menos retries = mais velocidade, mas também:
- menos insistência;
- maior chance de perder portas reais.

## Min-rate

### Exemplo

```bash
sudo nmap 10.129.2.0/24 -F --min-rate 300
```

### Lição

Em ambiente controlado, aumentar taxa mínima pode acelerar muito sem perda relevante.  
Em ambiente sensível, pode:
- gerar ruído;
- acionar monitoramento;
- causar bloqueio.

## Templates `-T`

- `-T0` → paranoid
- `-T1` → sneaky
- `-T2` → polite
- `-T3` → normal
- `-T4` → aggressive
- `-T5` → insane

## Takeaways

- Performance tuning é sempre equilíbrio entre velocidade e confiança do resultado.
- `-T4` costuma ser útil em laboratório e rede boa.
- `-T5` pode funcionar, mas não deve virar padrão automático.

---

# Sessão 9 — Firewall and IDS/IPS Evasion

## Escopo das anotações

Nesta conversa, esta sessão foi consolidada em **nível conceitual**, sem detalhamento operacional.

## Conceitos centrais

### Firewall
Decide se o tráfego:
- entra;
- é descartado;
- é rejeitado;
- ou é permitido.

### IDS
Observa e alerta.

### IPS
Observa e reage automaticamente.

## Drop versus Reject

### Drop
- silêncio;
- timeout;
- mais incerteza.

### Reject
- resposta explícita;
- mais contexto;
- melhor inferência de regra.

## Ponto central da sessão

O comportamento de uma porta não depende só da porta em si, mas também de:

- tipo de pacote;
- origem do tráfego;
- política do firewall;
- monitoramento do IDS/IPS;
- contexto da comunicação.

## Takeaways

- `filtered` precisa de interpretação contextual.
- Controles defensivos podem induzir falso negativo.
- O comportamento da defesa muda a leitura do scan.

---

# Sessão 10 — Firewall and IDS/IPS Evasion - Easy Lab

## O que foi enviado nesta conversa

Apenas o **enunciado introdutório** do laboratório.

## O que o lab ensina

- existe IDS/IPS ativo;
- existe contagem de alertas;
- existe risco de banimento;
- o alvo deve ser testado o mais silenciosamente possível.

## Ponto-chave

A página `status.php` funciona como feedback do comportamento defensivo, permitindo correlacionar:

- ação executada;
- aumento de alertas;
- impacto operacional.

## Takeaways

- o ambiente reage ao comportamento do atacante;
- cada ação tem custo;
- o foco do lab é **controle de ruído**.

---

# Sessão 11 — Firewall and IDS/IPS Evasion - Medium Lab

## O que foi enviado nesta conversa

Apenas o **enunciado introdutório** do laboratório.

## O que o lab ensina

Após o primeiro teste:
- administradores alteraram IDS/IPS;
- firewall ficou mais rígido;
- o tráfego passou a ser filtrado com mais severidade.

## Pista crítica do enunciado

O exercício destaca a importância de **UDP na VPN**.

## Takeaways

- o ambiente defensivo evolui;
- o que funcionava antes pode deixar de funcionar;
- é necessário adaptar a leitura do cenário;
- transporte e contexto de comunicação importam.

---

# Sessão 12 — Firewall and IDS/IPS Evasion - Hard Lab

## O que foi enviado nesta conversa

Apenas o **enunciado introdutório** do laboratório.

## O que o lab ensina

Agora o lado defensor:
- passou por treinamento;
- tomou mais precauções;
- alterou serviços;
- modificou a comunicação do software.

## Ideia principal

Este lab representa um ambiente:
- mais maduro;
- mais adaptado;
- mais consciente;
- menos permissivo.

## Takeaways

- o defensor também aprende;
- repetir a abordagem anterior cegamente é erro;
- é necessário reavaliar o ambiente após mudanças de serviço e comunicação.

---

# Tabela de flags mais importantes do módulo

| Flag / opção | Função principal | Observação prática |
|---|---|---|
| `-sn` | Host discovery sem port scan | Ótimo para mapear hosts vivos |
| `-PE` | ICMP Echo Request | Em rede local pode ser precedido por ARP |
| `--packet-trace` | Mostra pacotes enviados/recebidos | Excelente para estudo |
| `--reason` | Mostra por que o Nmap deu um resultado | Muito útil para interpretar `open`, `closed` e `filtered` |
| `-sS` | SYN scan | Rápido, muito usado |
| `-sT` | TCP Connect scan | Completa o handshake |
| `-sU` | UDP scan | Mais lento e mais ambíguo |
| `-p-` | Todas as portas TCP | Scan completo |
| `--top-ports=10` | Top 10 portas mais frequentes | Bom para reconhecimento rápido |
| `-F` | Fast scan (top 100 portas) | Bom baseline |
| `-sV` | Service/version detection | Etapa essencial após descobrir porta aberta |
| `-v` / `-vv` | Verbosidade | Mostra progresso e descobertas em tempo real |
| `-oN` | Salva saída normal | Leitura humana |
| `-oG` | Salva saída grepable | Parsing shell |
| `-oX` | Salva saída XML | Integração e automação |
| `-oA` | Salva todos os formatos | Melhor escolha geral |
| `-sC` | Scripts default do NSE | Ótimo baseline de enumeração |
| `--script vuln` | Scripts de triagem de vulnerabilidade | Útil para priorização |
| `-A` | Scan agressivo | Conveniente, porém mais ruidoso |
| `--stats-every=5s` | Mostra progresso periódico | Útil em scans longos |
| `--initial-rtt-timeout` | Ajusta timeout RTT inicial | Baixo demais pode perder host |
| `--max-rtt-timeout` | Ajusta timeout RTT máximo | Controle fino de espera |
| `--max-retries` | Define número de tentativas | Menos retries = mais chance de falso negativo |
| `--min-rate` | Define taxa mínima de pacotes | Muito útil em ambiente controlado |
| `-T0` a `-T5` | Timing templates | Equilíbrio entre discrição, velocidade e confiabilidade |

---

# Erros e armadilhas comuns

## 1. Assumir que “sem resposta” = host morto
Pode ser:
- firewall;
- filtro ICMP;
- política de descarte;
- atraso de rede.

## 2. Tratar `filtered` como “sem valor”
Na verdade, `filtered` entrega informação sobre o comportamento defensivo.

## 3. Achar que `-sV` sempre mostra tudo
Nem sempre.  
Banners manuais podem vazar mais do que o resumo do Nmap.

## 4. Usar tuning agressivo sem comparar resultados
Velocidade sem validação pode esconder:
- hosts;
- portas;
- respostas lentas.

## 5. Rodar NSE sem pensar no ruído
Alguns scripts são seguros, outros são mais agressivos.

## 6. Acreditar que um único scan fecha a enumeração
Em muitos casos, o bom resultado vem da combinação de:
- quick scan;
- full scan;
- version detection;
- enumeração manual;
- validação dos outputs.

---

# Resumo final do módulo

Este módulo construiu a base completa de **enumeração com Nmap**.

Ao longo das sessões, o aprendizado consolidado foi:

- a enumeração como etapa mais importante do reconhecimento;
- a lógica de host discovery;
- a leitura correta de estados de porta;
- diferenças entre scans TCP e UDP;
- version detection e banner grabbing;
- uso prático do NSE;
- importância de salvar saídas e saber reutilizá-las;
- impacto do tuning de performance;
- leitura conceitual da interação entre scans, firewalls, IDS e IPS.

## Em uma frase

**O módulo ensina que usar o Nmap bem não é só executar comandos; é entender profundamente o que a rede respondeu, o que ela escondeu e por quê.**

---

# Próximos pontos de revisão

Quando você for revisar este módulo no futuro, vale revisar especialmente:

1. diferença entre **ARP** e **ICMP** no host discovery;
2. estados `open`, `closed`, `filtered`, `open|filtered`;
3. diferenças entre `-sS`, `-sT` e `-sU`;
4. interpretação de banners e limites do `-sV`;
5. quando usar `-oA`;
6. quando usar `-sC`, `-A` e `--script vuln`;
7. impacto de `--min-rate`, `--max-retries` e templates `-T`;
8. como controles defensivos alteram a leitura da enumeração.

---

# Checklist de domínio

Use este checklist para autoavaliação rápida:

- [x] Entendo por que enumeração é a base do processo ofensivo
- [x] Sei usar `-sn` para descobrir hosts
- [x] Sei diferenciar ARP ping de ICMP Echo Request
- [x] Sei interpretar estados de porta no Nmap
- [x] Sei a diferença entre `-sS`, `-sT` e `-sU`
- [x] Sei usar `-sV` para detecção de versão
- [x] Sei quando complementar o Nmap com `nc` e `tcpdump`
- [x] Sei salvar resultados em `.nmap`, `.gnmap` e `.xml`
- [x] Sei usar `-oA` como prática padrão
- [x] Sei usar `-sC` e scripts específicos do NSE
- [x] Entendo que tuning de performance pode introduzir falso negativo
- [x] Entendo, em nível conceitual, como firewall/IDS/IPS afetam a enumeração

---

---

# Apêndice — Cheat Sheet oficial do módulo (HTB)

Este apêndice consolida, em português, o conteúdo do **cheat sheet oficial** fornecido pelo módulo.

## Scanning options

| Opção | Significado |
|---|---|
| `10.10.10.0/24` | Range de rede alvo |
| `-sn` | Desabilita port scanning |
| `-Pn` | Desabilita ICMP Echo Requests |
| `-n` | Desabilita resolução DNS |
| `-PE` | Realiza ping scan com ICMP Echo Requests |
| `--packet-trace` | Mostra todos os pacotes enviados e recebidos |
| `--reason` | Mostra a razão de um resultado específico |
| `--disable-arp-ping` | Desabilita ARP ping |
| `--top-ports=<num>` | Escaneia o número definido de top portas mais frequentes |
| `-p-` | Escaneia todas as portas |
| `-p22-110` | Escaneia as portas entre 22 e 110 |
| `-p22,25` | Escaneia apenas as portas 22 e 25 |
| `-F` | Escaneia as top 100 portas |
| `-sS` | Realiza TCP SYN scan |
| `-sA` | Realiza TCP ACK scan |
| `-sU` | Realiza UDP scan |
| `-sV` | Detecta serviços e versões |
| `-sC` | Executa scripts NSE da categoria `default` |
| `--script <script>` | Executa os scripts NSE especificados |
| `-O` | Realiza detecção de sistema operacional |
| `-A` | Executa OS detection, service detection e traceroute |
| `-D RND:5` | Usa 5 decoys aleatórios |
| `-e` | Define a interface de rede usada no scan |
| `-S 10.10.10.200` | Define IP de origem |
| `-g` | Define porta de origem |
| `--dns-server <ns>` | Faz resolução DNS usando o nameserver especificado |

## Output options

| Opção | Significado |
|---|---|
| `-oA <filename>` | Salva resultados em todos os formatos disponíveis |
| `-oN <filename>` | Salva em formato normal |
| `-oG <filename>` | Salva em formato grepable |
| `-oX <filename>` | Salva em formato XML |

## Performance options

| Opção | Significado |
|---|---|
| `--max-retries <num>` | Define o número de retries no scan |
| `--stats-every=5s` | Exibe o status do scan a cada 5 segundos |
| `-v` / `-vv` | Exibe saída verbosa |
| `--initial-rtt-timeout 50ms` | Define RTT inicial |
| `--max-rtt-timeout 100ms` | Define RTT máximo |
| `--min-rate 300` | Define a quantidade mínima de pacotes enviados simultaneamente |
| `-T <0-5>` | Define o timing template |

## Como usar este apêndice

Este cheat sheet serve como referência rápida para:

- revisar flags sem reler o módulo inteiro;
- lembrar a função de opções frequentes;
- montar scans de reconhecimento mais rapidamente;
- revisar output e tuning de performance.


# Conclusão

**Módulo concluído:** `Network Enumeration with Nmap`

Este fechamento do módulo deixa uma base muito boa para tudo que vier depois em reconhecimento, enumeração e validação de superfície de ataque.  
A partir daqui, o ganho mais importante não é decorar comando isolado, e sim desenvolver uma leitura mais madura de:

- protocolo;
- resposta;
- contexto;
- defesa;
- evidência.