# HTB ACADEMY · EXPORT FINAL DO MÓDULO

# Information Gathering - Web Edition

Versão reestruturada com a teoria consolidada em português e a resolução prática inserida logo após a seção correspondente, mantendo o visual do export anterior e um fluxo de leitura mais natural.

- **Perfil:** Easy · Tier 2
- **Foco:** Web reconnaissance ativo e passivo
- **Estrutura:** cada seção teórica seguida do hands-on correspondente, quando houver
- **Preservação:** logs e respostas finais mantidos a partir do material prático do chat

Leitura sugerida: use o mapa do módulo para navegar rapidamente e, em seguida, percorra cada seção em ordem. Onde houve resolução prática no chat, ela foi inserida imediatamente após a teoria equivalente para facilitar revisão, comparação e estudo.

## Mapa do módulo

| # | Section | Tradução | Hands-on |
|---|---------|----------|----------|
| 1 | Introduction | Introdução | — |
| 2 | WHOIS | WHOIS | — |
| 3 | Utilising WHOIS | Utilizando WHOIS | Sim |
| 4 | DNS | DNS | — |
| 5 | Digging DNS | Investigando DNS | Sim |
| 6 | Subdomains | Subdomínios | — |
| 7 | Subdomain Bruteforcing | Bruteforce de Subdomínios | Sim |
| 8 | DNS Zone Transfers | Transferências de Zona DNS | Sim |
| 9 | Virtual Hosts | Hosts Virtuais | Sim |
| 10 | Certificate Transparency Logs | Logs de Transparência de Certificados | — |
| 11 | Fingerprinting | Fingerprinting | Sim |
| 12 | Crawling | Crawling | — |
| 13 | robots.txt | robots.txt | — |
| 14 | Well-Known URIs | URIs Bem Conhecidas | — |
| 15 | Creepy Crawlies | Crawlers Web | Sim |
| 16 | Search Engine Discovery | Descoberta por Mecanismos de Busca | — |
| 17 | Web Archives | Arquivos da Web | Sim |
| 18 | Automating Recon | Automatizando o Reconhecimento | — |
| 19 | Skills Assessment | Avaliação de Habilidades | Sim |

## Seções consolidadas

As seções abaixo mantêm o fluxo único de revisão: primeiro a síntese teórica da seção, depois a resolução prática correspondente quando ela foi efetivamente trabalhada no chat. Se não houve bloco prático separado para uma seção específica, isso é indicado de forma discreta para manter a cadência visual do documento.

## Seção 1/19 — Introduction

**Tradução:** Introdução

Web reconnaissance é a base de uma avaliação web consistente. Antes de buscar falhas, o objetivo é entender a superfície do alvo: ativos publicados, subdomínios, IPs, tecnologias e possíveis pontos de entrada.

- Objetivos principais: identificar ativos, descobrir informação oculta, analisar a superfície de ataque e coletar inteligência útil para etapas posteriores.
- Recon ativo interage diretamente com o alvo e inclui técnicas como port scanning, banner grabbing, service enumeration, OS fingerprinting e web spidering.
- Recon passivo usa fontes públicas, como search engines, WHOIS, DNS, web archives, redes sociais e repositórios de código, reduzindo a chance de detecção.
- A escolha entre técnicas ativas e passivas depende do escopo, da maturidade do alvo e do nível de ruído aceitável.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 2/19 — WHOIS

WHOIS é um protocolo e também um conjunto de bases que armazenam dados de registro de domínios. Para reconnaissance, ele ajuda a entender quem registrou o domínio, quando ele foi criado, quando expira e quais contatos e name servers estão associados.

- Campos úteis: domain name, registrar, registrant, administrative contact, technical contact, creation date, expiration date e name servers.
- O histórico de WHOIS ajuda a observar mudanças de titularidade, registrador e infraestrutura ao longo do tempo.
- Mesmo quando dados sensíveis são mascarados, WHOIS ainda entrega contexto sobre maturidade do domínio e relação com provedores e registrars.
Nenhum hands-on separado foi preservado no chat para esta seção.

O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 3/19 — Utilising WHOIS

**Tradução:** Utilizando WHOIS

A seção mostra WHOIS em uso prático em cenários como investigação de phishing, análise de malware e produção de inteligência de ameaça. O valor não está apenas no campo isolado, mas na correlação entre datas, registrante, infraestrutura e padrões repetidos.

- Domínios recém-criados, registrantes ocultos e name servers suspeitos podem reforçar hipótese de phishing.
- Para malware e C2, WHOIS pode sugerir localização, registrar permissivo e infraestrutura reaproveitada.
- Em threat intelligence, padrões entre múltiplos domínios ajudam a perfilar campanhas, aliases e táticas do ator.
### Hands-on da seção

**WHOIS**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 1) WHOIS

#### Perguntas

- Perform a WHOIS lookup against the paypal.com domain. What is the registrar Internet Assigned Numbers Authority (IANA) ID number?
- What is the admin email contact for the tesla.com domain (also in-scope for the Tesla bug bounty program)?
#### Respostas finais

- PayPal Registrar IANA ID: 292
- Tesla admin email contact: admin@dnstinations.com
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~]
└─$ whois paypal.com
Domain Name: PAYPAL.COM
Registry Domain ID: 8017040_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.markmonitor.com
Registrar URL: http://www.markmonitor.com
Updated Date: 2026-03-06T01:40:28Z
Creation Date: 1999-07-15T05:32:11Z
Registry Expiry Date: 2026-07-15T05:32:11Z
Registrar: MarkMonitor Inc.
Registrar IANA ID: 292
Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
Registrar Abuse Contact Phone: +1.2086851750
Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
Name Server: NS1-PCHNET.PAYPAL.COM
Name Server: NS2-PCHNET.PAYPAL.COM
Name Server: PDNS100.ULTRADNS.COM
Name Server: PDNS100.ULTRADNS.NET

DNSSEC: signedDelegation
DNSSEC DS Data: 34800 13 2
D9E64BA8C8718FD93B596F9D109D9DAC47C3F557312201DFCCE5DD4128C08F50
DNSSEC DS Data: 7037 13 2
9778F2FF96889EBED549795DEAA40A6113F1899AF7CA8DD7947FDDFECA9A190B
URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2026-04-13T19:47:49Z <<<
For more information on Whois status codes, please visit https://icann.org/epp
NOTICE: The expiration date displayed in this record is the date the
registrar's sponsorship of the domain name registration in the registry is
currently set to expire. This date does not necessarily reflect the expiration
date of the domain name registrant's agreement with the sponsoring
registrar. Users may consult the sponsoring registrar's Whois database to
view the registrar's reported date of expiration for this registration.

TERMS OF USE: You are not authorized to access or query our Whois
database through the use of electronic processes that are high-volume and
automated except as reasonably necessary to register domain names or
modify existing registrations; the Data in VeriSign Global Registry
Services' ("VeriSign") Whois database is provided by VeriSign for
information purposes only, and to assist persons in obtaining information
about or related to a domain name registration record. VeriSign does not
guarantee its accuracy. By submitting a Whois query, you agree to abide
by the following terms of use: You agree that you may use this Data only
for lawful purposes and that under no circumstances will you use this Data
to: (1) allow, enable, or otherwise support the transmission of mass
unsolicited, commercial advertising or solicitations via e-mail, telephone,
or facsimile; or (2) enable high volume, automated, electronic processes
that apply to VeriSign (or its computer systems). The compilation,
repackaging, dissemination or other use of this Data is expressly
prohibited without the prior written consent of VeriSign. You agree not to
use electronic processes that are automated and high-volume to access or
query the Whois database except as reasonably necessary to register
domain names or modify existing registrations. VeriSign reserves the right
to restrict your access to the Whois database in its sole discretion to ensure
operational stability. VeriSign may restrict or terminate your access to the
Whois database for failure to abide by these terms of use. VeriSign
reserves the right to modify these terms at any time.
The Registry database contains ONLY .COM, .NET, .EDU domains and
Registrars.
Domain Name: paypal.com
Registry Domain ID: 8017040_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.markmonitor.com
Registrar URL: http://www.markmonitor.com
Updated Date: 2026-03-06T17:29:09+0000
Creation Date: 1999-07-15T05:32:11+0000
Registrar Registration Expiration Date: 2026-07-15T00:00:00+0000
Registrar: MarkMonitor, Inc.
Registrar IANA ID: 292
Registrar Abuse Contact: https://corp.markmonitor.com/domain/ui/abuse-report
Registrar Abuse Contact Phone: +1.2086851750
Domain Status: clientUpdateProhibited (https://www.icann.org/epp#clientUpdateProhibited)
Domain Status: clientTransferProhibited (https://www.icann.org/
epp#clientTransferProhibited)
Domain Status: clientDeleteProhibited (https://www.icann.org/epp#clientDeleteProhibited)
Domain Status: serverUpdateProhibited (https://www.icann.org/epp#serverUpdateProhibited)
Domain Status: serverTransferProhibited (https://www.icann.org/
epp#serverTransferProhibited)
Domain Status: serverDeleteProhibited (https://www.icann.org/epp#serverDeleteProhibited)
Registrant Organization: PayPal Inc.
Registrant Country: US
Registrant Email: Select Request Email Form at https://domains.markmonitor.com/whois/
paypal.com
Tech Email: Select Request Email Form at https://domains.markmonitor.com/whois/
paypal.com
Name Server: ns1-pchnet.paypal.com
Name Server: pdns100.ultradns.com
Name Server: ns2-pchnet.paypal.com
Name Server: pdns100.ultradns.net

DNSSEC: signedDelegation
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2026-04-13T19:47:39+0000 <<<
For more information on WHOIS status codes, please visit:
https://www.icann.org/resources/pages/epp-status-codes
If you wish to contact this domain’s Registrant or Technical
contact, and such email address is not visible above, you may do so via our web
form, pursuant to ICANN’s Temporary Specification. To verify that you are not a
robot, please enter your email address to receive a link to a page that
facilitates email communication with the relevant contact(s).
Web-based WHOIS:
https://domains.markmonitor.com/whois/contact/paypal.com
If you have a legitimate interest in viewing the non-public WHOIS details, send
your request and the reasons for your request to whoisrequest@markmonitor.com
and specify the domain name in the subject line. We will review that request and
may ask for supporting documentation and explanation.
The data in MarkMonitor’s WHOIS database is provided for information purposes,
and to assist persons in obtaining information about or related to a domain
name’s registration record. While MarkMonitor believes the data to be accurate,
the data is provided "as is" with no guarantee or warranties regarding its
accuracy.
By submitting a WHOIS query, you agree that you will use this data only for
lawful purposes and that, under no circumstances will you use this data to:
(1) allow, enable, or otherwise support the transmission by email, telephone,
or facsimile of mass, unsolicited, commercial advertising, or spam; or
(2) enable high volume, automated, or electronic processes that send queries,
data, or email to MarkMonitor (or its systems) or the domain name contacts (or

its systems).
MarkMonitor reserves the right to modify these terms at any time.
By submitting this query, you agree to abide by this policy.
MarkMonitor Domain Management(TM)
Protecting companies and consumers in a digital world.
Visit MarkMonitor at https://www.markmonitor.com
Contact us at +1.8007459229
In Europe, at +44.02032062220
-┌──(demone㉿hackingmachine)-[~]
└─$ whois tesla.com
Domain Name: TESLA.COM
Registry Domain ID: 187902_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.markmonitor.com
Registrar URL: http://www.markmonitor.com
Updated Date: 2024-10-02T10:15:20Z
Creation Date: 1992-11-04T05:00:00Z
Registry Expiry Date: 2026-11-03T05:00:00Z
Registrar: MarkMonitor Inc.
Registrar IANA ID: 292
Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
Registrar Abuse Contact Phone: +1.2086851750
Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
Name Server: A1-12.AKAM.NET
Name Server: A10-67.AKAM.NET
Name Server: A12-64.AKAM.NET
Name Server: A28-65.AKAM.NET
Name Server: A7-66.AKAM.NET
Name Server: A9-67.AKAM.NET
Name Server: EDNS69.ULTRADNS.BIZ
Name Server: EDNS69.ULTRADNS.COM
Name Server: EDNS69.ULTRADNS.NET
Name Server: EDNS69.ULTRADNS.ORG
DNSSEC: unsigned
URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2026-04-13T19:48:04Z <<<
For more information on Whois status codes, please visit https://icann.org/epp

NOTICE: The expiration date displayed in this record is the date the
registrar's sponsorship of the domain name registration in the registry is
currently set to expire. This date does not necessarily reflect the expiration
date of the domain name registrant's agreement with the sponsoring
registrar. Users may consult the sponsoring registrar's Whois database to
view the registrar's reported date of expiration for this registration.
TERMS OF USE: You are not authorized to access or query our Whois
database through the use of electronic processes that are high-volume and
automated except as reasonably necessary to register domain names or
modify existing registrations; the Data in VeriSign Global Registry
Services' ("VeriSign") Whois database is provided by VeriSign for
information purposes only, and to assist persons in obtaining information
about or related to a domain name registration record. VeriSign does not
guarantee its accuracy. By submitting a Whois query, you agree to abide
by the following terms of use: You agree that you may use this Data only
for lawful purposes and that under no circumstances will you use this Data
to: (1) allow, enable, or otherwise support the transmission of mass
unsolicited, commercial advertising or solicitations via e-mail, telephone,
or facsimile; or (2) enable high volume, automated, electronic processes
that apply to VeriSign (or its computer systems). The compilation,
repackaging, dissemination or other use of this Data is expressly
prohibited without the prior written consent of VeriSign. You agree not to
use electronic processes that are automated and high-volume to access or
query the Whois database except as reasonably necessary to register
domain names or modify existing registrations. VeriSign reserves the right
to restrict your access to the Whois database in its sole discretion to ensure
operational stability. VeriSign may restrict or terminate your access to the
Whois database for failure to abide by these terms of use. VeriSign
reserves the right to modify these terms at any time.
The Registry database contains ONLY .COM, .NET, .EDU domains and
Registrars.
Domain Name: tesla.com
Registry Domain ID: 187902_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.markmonitor.com

Registrar URL: http://www.markmonitor.com
Updated Date: 2024-10-02T10:15:20+0000
Creation Date: 1992-11-04T05:00:00+0000
Registrar Registration Expiration Date: 2026-11-03T00:00:00+0000
Registrar: MarkMonitor, Inc.
Registrar IANA ID: 292
Registrar Abuse Contact: https://corp.markmonitor.com/domain/ui/abuse-report
Registrar Abuse Contact Phone: +1.2086851750
Domain Status: clientUpdateProhibited (https://www.icann.org/epp#clientUpdateProhibited)
Domain Status: clientTransferProhibited (https://www.icann.org/
epp#clientTransferProhibited)
Domain Status: clientDeleteProhibited (https://www.icann.org/epp#clientDeleteProhibited)
Domain Status: serverUpdateProhibited (https://www.icann.org/epp#serverUpdateProhibited)
Domain Status: serverTransferProhibited (https://www.icann.org/
epp#serverTransferProhibited)
Domain Status: serverDeleteProhibited (https://www.icann.org/epp#serverDeleteProhibited)
Registrant Name: Domain Administrator
Registrant Organization: DNStination Inc.
Registrant Street: 3450 Sacramento Street, Suite 405
Registrant City: San Francisco
Registrant State/Province: CA
Registrant Postal Code: 94118
Registrant Country: US
Registrant Phone: +1.4155319335
Registrant Phone Ext:
Registrant Fax: +1.4155319336
Registrant Fax Ext:
Registrant Email: admin@dnstinations.com
Tech Name: Domain Administrator
Tech Phone: +1.4155319335
Tech Email: admin@dnstinations.com
Name Server: edns69.ultradns.com
Name Server: a7-66.akam.net
Name Server: edns69.ultradns.org

Name Server: a10-67.akam.net
Name Server: a28-65.akam.net
Name Server: edns69.ultradns.biz
Name Server: a1-12.akam.net
Name Server: edns69.ultradns.net
Name Server: a12-64.akam.net
Name Server: a9-67.akam.net
DNSSEC: unsigned
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2026-04-13T19:48:13+0000 <<<
For more information on WHOIS status codes, please visit:
https://www.icann.org/resources/pages/epp-status-codes
If you wish to contact this domain’s Registrant or Technical
contact, and such email address is not visible above, you may do so via our web
form, pursuant to ICANN’s Temporary Specification. To verify that you are not a
robot, please enter your email address to receive a link to a page that
facilitates email communication with the relevant contact(s).
Web-based WHOIS:
https://domains.markmonitor.com/whois/contact/tesla.com
If you have a legitimate interest in viewing the non-public WHOIS details, send
your request and the reasons for your request to whoisrequest@markmonitor.com
and specify the domain name in the subject line. We will review that request and
may ask for supporting documentation and explanation.
The data in MarkMonitor’s WHOIS database is provided for information purposes,
and to assist persons in obtaining information about or related to a domain
name’s registration record. While MarkMonitor believes the data to be accurate,
the data is provided "as is" with no guarantee or warranties regarding its
accuracy.
By submitting a WHOIS query, you agree that you will use this data only for
lawful purposes and that, under no circumstances will you use this data to:
(1) allow, enable, or otherwise support the transmission by email, telephone,
or facsimile of mass, unsolicited, commercial advertising, or spam; or
(2) enable high volume, automated, or electronic processes that send queries,
data, or email to MarkMonitor (or its systems) or the domain name contacts (or
its systems).
MarkMonitor reserves the right to modify these terms at any time.
By submitting this query, you agree to abide by this policy.
MarkMonitor Domain Management(TM)
Protecting companies and consumers in a digital world.
Visit MarkMonitor at https://www.markmonitor.com
Contact us at +1.8007459229
In Europe, at +44.02032062220

---

```

---

## Seção 4/19 — DNS

O Domain Name System traduz nomes amigáveis em endereços IP e organiza a resolução por uma hierarquia de root servers, TLDs, servidores autoritativos e resolvers. Para web recon, DNS revela ativos, dependências e pistas de organização interna.

- Conceitos-chave: A, AAAA, CNAME, MX, NS, TXT, SOA, SRV e PTR; cada registro responde a uma pergunta diferente sobre a infraestrutura.
- O arquivo hosts permite mapear nome para IP manualmente, útil para testes locais e validação de vHosts.
- Registros DNS ajudam a mapear servidores de e-mail, subdomínios, integrações com terceiros e mudanças na topologia do ambiente.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 5/19 — Digging DNS

**Tradução:** Investigando DNS

A seção introduz ferramentas práticas para consultar DNS, com destaque para o dig. O objetivo é entender como consultar tipos específicos de registro e interpretar corretamente as respostas.

- Ferramentas citadas: dig, nslookup, host, dnsenum, fierce, dnsrecon, theHarvester e serviços online de lookup.
- Consultas comuns: A, AAAA, MX, NS, TXT, CNAME, SOA, reverse lookup e +trace para acompanhar a cadeia de resolução.
- Saber ler header, answer section, TTL, servidor respondente e flags do dig é tão importante quanto executar o comando.
- Consultas ANY não devem ser superestimadas; muitos servidores modernos limitam esse comportamento.
### Hands-on da seção

**DNS básicos: A, PTR e MX**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 2) DNS básicos: A, PTR e MX

#### Perguntas

- Which IP address maps to inlanefreight.com?
- Which domain is returned when querying the PTR record for 134.209.24.248?
- What is the full domain returned when you query the mail records for facebook.com?
#### Respostas finais

- A inlanefreight.com: 134.209.24.248
- PTR 134.209.24.248: inlanefreight.com.
- MX facebook.com: smtpin.vvv.facebook.com.
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy]
└─$ dig +short A inlanefreight.com
134.209.24.248
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy]
└─$ nslookup inlanefreight.com
Server:
192.168.15.1
Address:
192.168.15.1#53
Non-authoritative answer:
Name:
inlanefreight.com
Address: 134.209.24.248
Name:

inlanefreight.com
Address: 2a03:b0c0:1:e0::32c:b001
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy]
└─$ dig +short -x 134.209.24.248
inlanefreight.com.
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy]
└─$ nslookup 134.209.24.248
248.24.209.134.in-addr.arpa
name = inlanefreight.com.
Authoritative answers can be found from:
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy]
└─$ dig +short MX facebook.com
10 smtpin.vvv.facebook.com.
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy]
└─$ host -t mx facebook.com
facebook.com mail is handled by 10 smtpin.vvv.facebook.com.

---

```

---

## Seção 6/19 — Subdomains

**Tradução:** Subdomínios

Subdomínios ampliam significativamente a superfície de ataque. Eles podem hospedar ambientes de desenvolvimento, staging, portais administrativos, aplicações legadas e conteúdo sensível não visível no domínio principal.

- Subdomínio é conceito DNS; normalmente é publicado por registros A, AAAA ou CNAME.
- Ambientes dev/stage frequentemente têm controles mais fracos e expõem funcionalidades ainda não endurecidas.
- A melhor descoberta combina enumeração ativa com consulta a fontes passivas, como CT logs e buscadores.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 7/19 — Subdomain Bruteforcing

**Tradução:** Bruteforce de Subdomínios

O bruteforce de subdomínios testa sistematicamente nomes prováveis contra um domínio- alvo. A qualidade da wordlist impacta diretamente o resultado.

- Fluxo: escolher wordlist, gerar candidatos, consultar DNS e validar as respostas obtidas.
- Wordlists podem ser genéricas, direcionadas por contexto ou customizadas com base em inteligência já coletada.
- Ferramentas citadas: dnsenum, fierce, dnsrecon, amass, assetfinder e puredns.
- Descoberta não é o fim; os nomes encontrados precisam ser validados e priorizados.
### Hands-on da seção

**Brute force de subdomínios**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 3) Brute force de subdomínios

#### Pergunta

- Using the known subdomains for inlanefreight.com (www, ns1, ns2, ns3, blog, support, customer), find any missing subdomains by brute-forcing possible domain names. Provide your answer with the complete subdomain, e.g., www.inlanefreight.com.
#### Resposta final

- Subdomínio faltante: my.inlanefreight.com
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-
top1million-20000.txt -r
dnsenum VERSION:1.3.1
-----

inlanefreight.com

-----

Host's addresses:
__________________
inlanefreight.com.

300

IN

A

134.209.24.248

300
300

IN
IN

A
A

178.128.39.165
206.189.119.186

Name Servers:
______________
ns1.inlanefreight.com.
ns2.inlanefreight.com.
Mail (MX) Servers:
___________________

Trying Zone Transfers and getting Bind Versions:
_________________________________________________
Trying Zone Transfer for inlanefreight.com on ns1.inlanefreight.com ...
AXFR record query failed: Connection timed out
Trying Zone Transfer for inlanefreight.com on ns2.inlanefreight.com ...
AXFR record query failed: Connection timed out
Scraping inlanefreight.com subdomains from Google:
___________________________________________________

Brute forcing with /usr/share/seclists/Discovery/DNS/subdomainstop1million-20000.txt:
_______________________________________________________________________________________

www.inlanefreight.com.
134.209.24.248
ns1.inlanefreight.com.
ns2.inlanefreight.com.
blog.inlanefreight.com.
ns3.inlanefreight.com.
support.inlanefreight.com.
my.inlanefreight.com.
customer.inlanefreight.com.

300

IN

A

289
289
300
300
300
300
300

IN
IN
IN
IN
IN
IN
IN

A
A
A
A
A
A
A

Performing
recursion:
______________________
---- Checking subdomains NS records ---Can't perform recursion no NS records.
Launching Whois
Queries:
_________________________
whois ip result:
206.189.0.0/16
whois ip result:
whois ip result:

206.189.119.0

->

134.209.24.0
178.128.39.0

->
->

inlanefreight.com_________________
178.128.32.0/20
206.189.0.0/16
134.209.0.0/16

134.209.0.0/16
178.128.32.0/20

178.128.39.165
206.189.119.186
134.209.24.248
134.209.24.248
134.209.24.248
134.209.24.248
134.209.24.248

┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ gobuster dns --domain inlanefreight.com --wordlist /usr/share/seclists/Discovery/
DNS/subdomainstop1million-20000.txt --resolver 1.1.1.1
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Domain:
inlanefreight.com
[+] Threads:
10
[+] Resolver:
1.1.1.1
[+] Timeout:
1s
[+] Wordlist:
/usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
===============================================================
Starting gobuster in DNS enumeration mode
===============================================================
www.inlanefreight.com 2a03:b0c0:1:e0::32c:b001,134.209.24.248
ns1.inlanefreight.com 178.128.39.165
ns2.inlanefreight.com 206.189.119.186
support.inlanefreight.com 134.209.24.248
ns3.inlanefreight.com 134.209.24.248
blog.inlanefreight.com 2a03:b0c0:1:e0::32c:b001,134.209.24.248
my.inlanefreight.com 134.209.24.248
Progress: 857 / 19966 (4.29%)[ERROR] error on word www.es: lookup
www.es.inlanefreight.com.: i/o timeout
customer.inlanefreight.com 2a03:b0c0:1:e0::32c:b001,134.209.24.248
Progress: 19966 / 19966 (100.00%)
===============================================================
Finished
===============================================================

---

```

---

## Seção 8/19 — DNS Zone Transfers

**Tradução:** Transferências de Zona DNS

Zone transfer é uma função legítima usada para replicar uma zona entre servidores DNS, normalmente via AXFR. Quando mal configurada, pode vazar a zona inteira para clientes não autorizados.

- Uma AXFR bem-sucedida pode revelar subdomínios, IPs, NS, MX, CNAME, A/AAAA e outros registros da zona.
- O risco está no controle de acesso: zone transfer deve ser permitida apenas a secundários autorizados.
- Mesmo quando falha, o teste pode revelar comportamento e configuração do servidor autoritativo.
### Hands-on da seção

**DNS Zone Transfer — contagem e endereços**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 4) DNS Zone Transfer — contagem e endereços

#### Perguntas

- After performing a zone transfer for the domain inlanefreight.htb on the target system, how many DNS records are retrieved from the target system's name server?
- Within the zone record transferred above, find the ip address for ftp.admin.inlanefreight.htb.
- Within the same zone record, identify the largest IP address allocated within the 10.10.200 IP range.
#### Respostas finais

- Quantidade de registros: 22
- IP de ftp.admin.inlanefreight.htb: 10.10.34.2
- Maior IP na faixa 10.10.200.x: 10.10.200.14
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ dig @10.129.37.188 inlanefreight.htb AXFR
; <<>> DiG 9.20.20-1-Debian <<>> @10.129.37.188 inlanefreight.htb AXFR
; (1 server found)
;; global options: +cmd
inlanefreight.htb.
604800 IN
SOA
inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400
2419200 604800
inlanefreight.htb.
604800 IN
NS
ns.inlanefreight.htb.
admin.inlanefreight.htb. 604800 IN
A
10.10.34.2
ftp.admin.inlanefreight.htb. 604800 IN A
10.10.34.2
careers.inlanefreight.htb. 604800 IN
A

10.10.34.50
dc1.inlanefreight.htb. 604800 IN
A
10.10.34.16
dc2.inlanefreight.htb. 604800 IN
A
10.10.34.11
internal.inlanefreight.htb. 604800 IN
A
127.0.0.1
admin.internal.inlanefreight.htb. 604800 IN A
10.10.1.11
wsus.internal.inlanefreight.htb. 604800 IN A
10.10.1.240
ir.inlanefreight.htb.
604800 IN
A
10.10.45.5
dev.ir.inlanefreight.htb. 604800 IN
A
10.10.45.6

ns.inlanefreight.htb.
604800 IN
A
127.0.0.1
resources.inlanefreight.htb. 604800 IN A
10.10.34.100
securemessaging.inlanefreight.htb. 604800 IN A 10.10.34.52
test1.inlanefreight.htb. 604800 IN
A
10.10.34.101
us.inlanefreight.htb.
604800 IN
A
10.10.200.5
cluster14.us.inlanefreight.htb. 604800 IN A
10.10.200.14
messagecenter.us.inlanefreight.htb. 604800 IN A 10.10.200.10
ww02.inlanefreight.htb. 604800 IN
A
10.10.34.112
www1.inlanefreight.htb. 604800 IN
A
10.10.34.111
inlanefreight.htb.
604800 IN
SOA
inlanefreight.htb. root.inlanefreight.htb. 2 604800 86400
2419200 604800
;; Query time: 139 msec
;; SERVER: 10.129.37.188#53(10.129.37.188) (TCP)
;; WHEN: Mon Apr 13 18:18:54 -03 2026
;; XFR size: 22 records (messages 1, bytes 594)
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ dig @10.129.37.188 inlanefreight.htb AXFR | grep -v '^;' | grep -v '^$' | wc -l
22
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ dig @10.129.37.188 inlanefreight.htb AXFR | grep 'ftp.admin.inlanefreight.htb' | awk
'{print $5}'
10.10.34.2
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ dig @10.129.37.188 inlanefreight.htb AXFR | grep '10\.10\.200\.' | awk '{print $5}'
| sort -t '.' -k4,4n |
tail -n 1
10.10.200.14

---

```

---

## Seção 9/19 — Virtual Hosts

**Tradução:** Hosts Virtuais

Virtual hosts permitem que vários sites compartilhem o mesmo IP e a mesma porta, sendo diferenciados principalmente pelo header Host. Isso significa que nem todo host útil aparece no DNS público.

- Subdomínio é conceito DNS; vHost é conceito da configuração do web server.
- Name-based virtual hosting é o modelo mais comum e usa o Host header para decidir qual conteúdo servir.
- vHosts podem existir sem registro DNS público; por isso, host fuzzing e uso do arquivo hosts são importantes.
- Ferramentas citadas: Gobuster, Feroxbuster e ffuf.
### Hands-on da seção

**Brute-force de vHosts (prefixos web/vm/br/a/su)**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 5) Brute-force de vHosts (prefixos web/vm/br/a/su)

#### Perguntas

- What is the full subdomain that is prefixed with "web"?
- What is the full subdomain that is prefixed with "vm"?
- What is the full subdomain that is prefixed with "br"?
- What is the full subdomain that is prefixed with "a"?
- What is the full subdomain that is prefixed with "su"?
#### Respostas finais

- web: web17611.inlanefreight.htb
- vm: vm5.inlanefreight.htb
- br: browse.inlanefreight.htb
- a: admin.inlanefreight.htb
- su: support.inlanefreight.htb
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ ffuf -u http://inlanefreight.htb:31844/ -H 'Host: FUZZ.inlanefreight.htb' \
-w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -fs 116
/'___\ /'___\
/'___\
/\ \__/ /\ \__/ __ __ /\ \__/
\ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\
\ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/
\ \_\
\ \_\ \ \____/ \ \_\
\/_/
\/_/
\/___/
\/_/
v2.1.0-dev
________________________________________________
:: Method
:: URL
:: Wordlist
:: Header

: GET
: http://inlanefreight.htb:31844/

: FUZZ: /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt
: Host: FUZZ.inlanefreight.htb

:: Follow redirects : false
:: Calibration
: false
:: Timeout
: 10
:: Threads
: 40
:: Matcher
: Response status: 200-299,301,302,307,401,403,405,500
:: Filter
: Response size: 116
________________________________________________
blog
[Status: 200, Size: 98, Words: 4, Lines: 2, Duration: 227ms]
forum
[Status: 200, Size: 100, Words: 4, Lines: 2, Duration: 231ms]
admin
[Status: 200, Size: 100, Words: 4, Lines: 2, Duration: 215ms]
support
[Status: 200, Size: 104, Words: 4, Lines: 2, Duration: 211ms]
vm5
[Status: 200, Size: 96, Words: 4, Lines: 2, Duration: 223ms]
browse
[Status: 200, Size: 102, Words: 4, Lines: 2, Duration: 227ms]
web17611
[Status: 200, Size: 106, Words: 4, Lines: 2, Duration: 223ms]
:: Progress: [19966/19966] :: Job [1/1] :: 133 req/sec :: Duration: [0:02:47] :: Errors:
0 ::
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -H 'Host: web17611.inlanefreight.htb' http://154.57.164.70:31844/ -I
HTTP/1.1 200 OK
Server: nginx/1.26.1
Date: Mon, 13 Apr 2026 21:28:33 GMT
Content-Type: text/html
Content-Length: 106
Last-Modified: Fri, 14 Jun 2024 01:12:54 GMT
Connection: keep-alive
ETag: "666b9916-6a"
Accept-Ranges: bytes
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -H 'Host: vm5.inlanefreight.htb' http://154.57.164.70:31844/ -I
HTTP/1.1 200 OK
Server: nginx/1.26.1
Date: Mon, 13 Apr 2026 21:28:39 GMT
Content-Type: text/html
Content-Length: 96
Last-Modified: Fri, 14 Jun 2024 01:12:54 GMT
Connection: keep-alive
ETag: "666b9916-60"
Accept-Ranges: bytes
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -H 'Host: browse.inlanefreight.htb' http://154.57.164.70:31844/ -I
HTTP/1.1 200 OK
Server: nginx/1.26.1
Date: Mon, 13 Apr 2026 21:28:46 GMT
Content-Type: text/html
Content-Length: 102
Last-Modified: Fri, 14 Jun 2024 01:12:54 GMT
Connection: keep-alive
ETag: "666b9916-66"
Accept-Ranges: bytes
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -H 'Host: admin.inlanefreight.htb' http://154.57.164.70:31844/ -I
HTTP/1.1 200 OK
Server: nginx/1.26.1
Date: Mon, 13 Apr 2026 21:28:56 GMT

Content-Type: text/html
Content-Length: 100
Last-Modified: Fri, 14 Jun 2024 01:12:54 GMT
Connection: keep-alive
ETag: "666b9916-64"
Accept-Ranges: bytes
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -H 'Host: support.inlanefreight.htb' http://154.57.164.70:31844/ -I
HTTP/1.1 200 OK
Server: nginx/1.26.1
Date: Mon, 13 Apr 2026 21:29:02 GMT
Content-Type: text/html
Content-Length: 104
Last-Modified: Fri, 14 Jun 2024 01:12:54 GMT
Connection: keep-alive
ETag: "666b9916-68"
Accept-Ranges: bytes

---

```

---

## Seção 10/19 — Certificate Transparency Logs

**Tradução:** Logs de Transparência de Certificados

CT logs são registros públicos e auditáveis de certificados emitidos. Para recon, eles são uma das melhores fontes passivas para descoberta de subdomínios reais já usados em certificados.

- O campo SAN costuma revelar múltiplos nomes associados ao domínio e seus subdomínios.
- Além de subdomínios atuais, CT logs também ajudam a encontrar nomes históricos ou ligados a certificados antigos e expirados.
- Ferramentas destacadas: crt.sh e Censys; a API do crt.sh facilita automação com curl e jq.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 11/19 — Fingerprinting

Fingerprinting busca identificar a stack real do alvo: servidor web, CMS, frameworks, componentes de segurança e sinais de misconfiguração. Isso permite direcionar melhor as próximas etapas do recon.

- Técnicas principais: banner grabbing, análise de HTTP headers, observação do comportamento da aplicação e leitura do conteúdo da página.
- Sinais como Server, X-Powered-By, rotas específicas, wp-json, comentários e arquivos públicos ajudam a identificar tecnologias.
- Ferramentas citadas: Wappalyzer, BuiltWith, WhatWeb, Nmap, Netcraft, wafw00f, Nikto e curl.
- Detectar WAF cedo é importante porque ele pode alterar e filtrar o comportamento observado.
### Hands-on da seção

**Fingerprinting de `app.inlanefreight.local` e**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 6) Fingerprinting de `app.inlanefreight.local` e

`dev.inlanefreight.local`

#### Perguntas

- Determine the Apache version running on app.inlanefreight.local on the target system.
- Which CMS is used on app.inlanefreight.local on the target system?
- On which operating system is the dev.inlanefreight.local webserver running in the target system?
#### Respostas finais

- Apache version: 2.4.41
- CMS: Joomla
- OS: Ubuntu
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ echo '10.129.37.192 app.inlanefreight.local dev.inlanefreight.local' | sudo tee -a /
etc/hosts
[sudo] password for demone:
10.129.37.192 app.inlanefreight.local dev.inlanefreight.local
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -I http://app.inlanefreight.local
HTTP/1.1 200 OK
Date: Mon, 13 Apr 2026 21:45:26 GMT
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: 72af8f2b24261272e581a49f5c56de40=id0uqajrmofgjq6aoo4dbq3c69; path=/;
HttpOnly
Permissions-Policy: interest-cohort=()
Expires: Wed, 17 Aug 2005 00:00:00 GMT
Last-Modified: Mon, 13 Apr 2026 21:45:34 GMT
Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Pragma: no-cache
Content-Type: text/html; charset=utf-8
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ whatweb http://app.inlanefreight.local
http://app.inlanefreight.local [200 OK] Apache[2.4.41], Bootstrap,
Cookies[72af8f2b24261272e581a49f5c56de40],
Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)],
HttpOnly[72af8f2b24261272e581a49f5c56de40], IP[10.129.37.192], JQuery,
MetaGenerator[Joomla! - Open Source
Content Management], OpenSearch[http://app.inlanefreight.local/index.php/component/
search/?
layout=blog&id=9&Itemid=101&format=opensearch], Script, Title[Home],
UncommonHeaders[permissionspolicy]
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -I http://dev.inlanefreight.local
HTTP/1.1 200 OK
Date: Mon, 13 Apr 2026 21:45:56 GMT
Server: Apache/2.4.41 (Ubuntu)
Set-Cookie: 02a93f6429c54209e06c64b77be2180d=3kmhrcchvfer7vpqbedktm7ba4; path=/;
HttpOnly
Expires: Wed, 17 Aug 2005 00:00:00 GMT
Last-Modified: Mon, 13 Apr 2026 21:46:04 GMT

Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Pragma: no-cache
Content-Type: text/html; charset=utf-8

---

```

---

## Seção 12/19 — Crawling

Crawling, ou spidering, automatiza a navegação por links reais do site. Diferente de fuzzing, ele descobre páginas e recursos a partir do que já está referenciado pela própria aplicação.

- Parte de uma seed URL, extrai links e continua o processo iterativamente.
- Estratégias: breadth-first para mapear largura e depth-first para aprofundar rapidamente em um ramo.
- Além de links, um crawler pode coletar comentários, metadados, arquivos sensíveis, imagens, mídia e estruturas de navegação.
- O valor real do crawling está na correlação entre achados e no contexto em que aparecem.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 13/19 — robots.txt

robots.txt orienta crawlers legítimos sobre o que rastrear ou não em um site. Não é mecanismo de segurança, mas frequentemente denuncia diretórios e áreas que o site preferiria não expor a indexação.

- Estrutura: blocos por user-agent com diretivas como Disallow, Allow, Crawl-delay e Sitemap.
- Disallow costuma ser a diretiva mais valiosa em recon, pois pode apontar para admin, private, backup, old e áreas similares.
- O arquivo também pode revelar crawler traps/honeypots e sitemaps úteis para ampliar a enumeração.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 14/19 — Well-Known URIs

**Tradução:** URIs Bem Conhecidas

O diretório /.well-known/, padronizado pelo RFC 8615, centraliza metadados e configurações de protocolos e serviços. Em recon, é uma fonte previsível de endpoints e informações estruturadas.

- Exemplos úteis: security.txt, change-password, assetlinks.json, mta-sts.txt e openid- configuration.
- O endpoint openid-configuration pode revelar issuer, authorization endpoint, token endpoint, userinfo endpoint, jwks_uri, scopes e response types.
- O valor está na previsibilidade: se a aplicação usa certo protocolo, existe boa chance de haver um caminho padronizado dentro de /.well-known/.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 15/19 — Creepy Crawlies

**Tradução:** Crawlers Web

A seção apresenta ferramentas de crawling web e destaca o uso de Scrapy com um spider customizado para reconnaissance. O objetivo não é apenas visitar páginas, mas estruturar a coleta em categorias úteis.

- Ferramentas citadas: Burp Suite Spider, OWASP ZAP, Scrapy e Apache Nutch.
- Com ReconSpider, a saída é organizada em JSON com e-mails, links, external_files, js_files, form_fields, imagens, mídia e comentários.
- O maior ganho do crawling automatizado está na coleta estruturada, pronta para revisão e correlação posterior.
- A ética operacional continua obrigatória: autorização, controle de volume e cuidado com os recursos do alvo.
### Hands-on da seção

**Crawling — localização dos futuros reports**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 7) Crawling — localização dos futuros reports

#### Pergunta

- After spidering inlanefreight.com, identify the location where future reports will be stored.
#### Resposta final

- Domínio identificado: inlanefreight-comp133.s3.amazonaws.htb
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ python3 ReconSpider.py http://inlanefreight.com
python3: can't open file '/home/demone/Desktop/HTB Academy/Information Gathering - Web

Edition/
ReconSpider.py': [Errno 2] No such file or directory
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ find ~ -iname 'ReconSpider.py' 2>/dev/null
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ sudo find / -iname 'ReconSpider.py' 2>/dev/null
[sudo] password for demone:
/usr/share/reconspider/reconspider.py
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ python3 /usr/share/reconspider/reconspider.py http://inlanefreight.com
/usr/share/reconspider/reconspider.py:6: SyntaxWarning: invalid escape sequence '\_'
\______
\ ____
____ ____
____
/
_____/_____ |__| __| _/___________
__________
_________
__
___
\______
\ ____
____ ____
____
/
_____/_____ |__| __| _/___________
|
_// __ \_/ ___\/ _ \ /
\
\_____ \\____ \| |/ __ |/ __ \_ __ \
|
|
\ ___/\ \__( <_> )
| \ /
\ |_> > / /_/ \ ___/| | \/
|____|_ /\___ >\___ >____/|___| / /_______ /
__/|__\____ |\___ >__|
\/
\/
\/
\/
\/|__|
\/
\/
Seems like you haven't add your shodan API key in /etc/reconspider/reconspider.conf,
Please add the missing
API keys
Add your ipstack api key to /etc/reconspider/reconspider.conf

Resolução manual na unha com `wget`
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ mkdir -p spider
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ wget --mirror --convert-links --adjust-extension --page-requisites --no-parent
https://
www.inlanefreight.com -P spider
[... download completo preservado no chat ...]

Evidência final encontrada

┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ grep -RniE 'future report|future reports' spider
spider/www.inlanefreight.com/index.php/wp-json/wp/v2/pages/13:1:
{"id":13,"date":"2020-08-06T11:31:30","date_gmt":"2020-08-06T11:31:30","guid":
{"rendered":"https:\/\/
www.inlanefreight.com\/?
page_id=13"},"modified":"2021-02-10T01:46:55","modified_gmt":"2021-02-10T01:46:55","slug
":"news","status":"publish","type":"

\/\/www.inlanefreight.com\/index.php\/news\/","title":{"rendered":"News"},"content":
{"rendered":"\n<p><span
class=\"has-inline-color has-very-dark-gray-color\"><strong>Inlanefreight<\/strong>
shareholders approved
today with the required majority of all items on the agenda put to the vote at the
virtual Annual General
Meeting. This included the appropriation of the net profit and thereby the payment of a
dividend of USD 1.10
per share.<\/span><\/p>\n\n\n\n<p><span class=\"has-inline-color has-very-dark-gray-
color\">\u201cIn the last
financial year, we continued to implement our Strategy 2023 vigorously and achieved a
Group net result that
was many times higher than in the previous year. <\/span><\/p>\n\n\n\n<p><span
class=\"has-inline-color hasvery-dark-gray-color\">Also, we have gotten 2020 off to a
relatively good start, which will give us a bit of
tailwind for the rest of the year. The distribution of the dividends is consistent
against the backdrop of the
successful financial year \u2013 as well as a reflection of the close ties we enjoy with
our shareholders,
\u201d said Jeremy Lastman, CEO of <strong>Inlanefreight<\/strong>.<\/span><\/
p>\n\n\n\n<p><span class=\"hasinline-color has-very-dark-gray-color\">For the third
quarter of 2020, <strong>Inlanefreight<\/strong>
recorded earnings before interest and taxes (EBIT) of <strong>USD 276 million<\/strong>,
which is below the
corresponding prior-year figure of USD 243 million. The Group net result declined to
approximately USD 37
million. Earnings before interest, taxes, depreciation, and amortization (EBITDA)
decreased slightly to USD
717 million.<\/span><\/p>\n\n\n\n<hr class=\"wp-block-separator\"\/>\n\n\n\n<h2
class=\"has-text-aligncenter\">Our Report and Goals<\/h2>\n\n\n\n<div class=\"wp-block-
file aligncenter\"><a href=\"https:\/\/
www.inlanefreight.com\/wp-content\/uploads\/2020\/09\/goals.pdf\">Inlanefreight-Goals<\/
a><a href=\"https:\/\/
www.inlanefreight.com\/wp-content\/uploads\/2020\/09\/goals.pdf\" class=\"wp-block-
file__button\"
download>Download<\/a><\/div>\n\n\n\n<!-- TO-DO: change the location of future reports
to inlanefreightcomp133.s3.amazonaws.htb -->\n","protected":false},"excerpt":
{"rendered":"<p>Inlanefreight shareholders
approved today with the required majority of all items on the agenda put to the vote at
the virtual Annual

General Meeting. This included the appropriation of the net profit and thereby the
payment of a dividend of
USD 1.10 per share. \u201cIn the last financial year, we continued to implement our
Strategy […]<\/
p>\n","protected":false},"author":1,"featured_media":0,"parent":0,"menu_order":0,"commen
t_status":"closed","ping_status":"cl
[],"_links":{"self":[{"href":"https:\/\/www.inlanefreight.com\/index.php\/wp-json\/wp\/
v2\/pages\/
13"}],"collection":[{"href":"https:\/\/www.inlanefreight.com\/index.php\/wp-json\/wp\/
v2\/pages"}],"about":
[{"href":"https:\/\/www.inlanefreight.com\/index.php\/wp-json\/wp\/v2\/types\/
page"}],"author":
[{"embeddable":true,"href":"https:\/\/www.inlanefreight.com\/index.php\/wp-json\/wp\/
v2\/users\/
1"}],"replies":[{"embeddable":true,"href":"https:\/\/www.inlanefreight.com\/index.php\/
wp-json\/wp\/v2\/
comments?post=13"}],"version-history":[{"count":7,"href":"https:\/\/
www.inlanefreight.com\/index.php\/wpjson\/wp\/v2\/pages\/13\/revisions"}],"predecessor-
version":[{"id":109,"href":"https:\/\/
www.inlanefreight.com\/index.php\/wp-json\/wp\/v2\/pages\/13\/revisions\/
109"}],"wp:attachment":
[{"href":"https:\/\/www.inlanefreight.com\/index.php\/wp-json\/wp\/v2\/media?
parent=13"}],"curies":
[{"name":"wp","href":"https:\/\/api.w.org\/{rel}","templated":true}]}}
spider/www.inlanefreight.com/index.php/news/index.html:241:<!-- TO-DO: change the
location of future reports
to inlanefreight-comp133.s3.amazonaws.htb -->
spider/www.inlanefreight.com/index.html?p=13.html:241:<!-- TO-DO: change the location of

future reports to
inlanefreight-comp133.s3.amazonaws.htb -->

---

```

---

## Seção 16/19 — Search Engine Discovery

**Tradução:** Descoberta por Mecanismos de Busca

Search engines podem ser usados como fonte poderosa de passive recon. O diferencial está em combinar operadores de busca para localizar páginas, arquivos e padrões específicos já indexados publicamente.

- Operadores centrais: site:, inurl:, filetype:, intitle:, intext:, cache:, allinurl:, allintitle:, aspas, OR, AND, NOT e sinal de menos.
- Google Dorking é o uso estratégico desses operadores para encontrar login pages, documentos públicos, configurações expostas e backups.
- Motores de busca não indexam tudo; por isso, essa técnica complementa, mas não substitui, outras formas de enumeração.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 17/19 — Web Archives

**Tradução:** Arquivos da Web

Web archives, especialmente o Wayback Machine, oferecem uma visão histórica do alvo. Eles ajudam a descobrir páginas, diretórios, subdomínios e conteúdos que já existiram, mesmo que não apareçam mais hoje.

- O fluxo conceitual é: crawling, archiving e accessing de snapshots por data.
- Comparar versões antigas e atuais ajuda a detectar legado, endpoints removidos e padrões de mudança.
- É uma técnica fortemente passiva, útil tanto para OSINT quanto para geração de novas hipóteses de enumeração.
### Hands-on da seção

**Web Archives**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 8) Web Archives

#### Perguntas do bloco

- How many Pen Testing Labs did HackTheBox have on the 8th August 2018?
- How many members did HackTheBox have on the 10th June 2017?
- Going back to March 2002, what website did the facebook.com domain redirect to?
- According to the paypal.com website in October 1999, what could you use to "beam money to anyone"?
- Going back to November 1998 on google.com, what address hosted the non-alpha "Google Search Engine Prototype" of Google?
- Going back to March 2000 on www.iana.org, when exacty was the site last updated?
- According to wikipedia.com snapshot taken on February 9, 2003, how many articles were they already working on in the English version?
#### Respostas trabalhadas no chat

- HTB Pen Testing Labs (08 Aug 2018): 74
- HTB members (10 Jun 2017): 3054
- facebook.com redirect (Mar 2002): http://site.aboutface.com/
- PayPal “beam money to anyone”: Palm 0rganizer
- Google Search Engine Prototype (tentativa discutida no chat): http:// google.stanford.edu/
- IANA last updated (Mar 2000): 17-December-99
- Wikipedia (09 Feb 2003): 104155
#### Observação importante preservada do chat

Durante este bloco houve várias tentativas conflitantes para a pergunta da Wikipedia. A confirmação final no chat veio quando foi colado diretamente o trecho do Wayback Machine: https://web.archive.org/web/20030209061139/http://www.wikipedia.org/ - Wikipedia is a multilingual project to create a complete and accurate open content encyclopedia. We started on January 15, 2001 and are already working on 104155 articles in the English version. Visit the help page and experiment in the sandbox to learn how you can edit any article right now.

#### Resolução final registrada no chat para a Wikipedia

- Resposta correta: 104155 ---
---

## Seção 18/19 — Automating Recon

**Tradução:** Automatizando o Reconhecimento

Automação acelera, padroniza e amplia o recon. Em vez de executar várias tarefas repetitivas manualmente, ferramentas modulares conseguem consolidar headers, WHOIS, SSL, DNS, subdomínios, crawling, diretórios e dados históricos em um só fluxo.

- Vantagens: eficiência, escalabilidade, consistência, cobertura mais ampla e integração com outras fontes e ferramentas.
- Frameworks citados: FinalRecon, Recon-ng, theHarvester, SpiderFoot e OSINT Framework.
- O FinalRecon é o foco prático da seção por reunir vários módulos úteis em uma interface simples, com saída reaproveitável.
Nenhum hands-on separado foi preservado no chat para esta seção. O fluxo visual foi mantido e a seção segue apenas com a consolidação teórica.

---

## Seção 19/19 — Skills Assessment

**Tradução:** Avaliação de Habilidades

A etapa final não introduz teoria nova: ela exige integração prática das técnicas do módulo. O objetivo é demonstrar domínio do fluxo de reconnaissance, e não apenas repetir comandos isolados.

- As habilidades explicitamente cobradas incluem WHOIS, análise de robots.txt, bruteforce de subdomínios e crawling com análise dos resultados.
- Conforme novos subdomínios e hosts forem descobertos, eles devem ser adicionados ao arquivo hosts para viabilizar navegação e validação local.
- O assessment mede processo, correlação e tomada de decisão, não apenas respostas finais.
### Hands-on da seção

**Skill Assessment Final — `154.57.164.71:32489`**

*Transcrição prática preservada do chat, inserida logo após a teoria correspondente.*

#### 9) Skill Assessment Final — `154.57.164.71:32489`

#### vHost base

- inlanefreight.htb
#### Perguntas

- What is the IANA ID of the registrar of the inlanefreight.com domain?
- What http server software is powering the inlanefreight.htb site on the target system?
- What is the API key in the hidden admin directory that you have discovered on the target system?
- After crawling the inlanefreight.htb domain on the target system, what is the email address you have found?
- What is the API key the inlanefreight.htb developers will be changing too?
#### Respostas finais

- Registrar IANA ID: 468
- HTTP server software: nginx
- API key atual (admin hidden): e963d863ee0e82ba7080fbf558ca0d3f
- Email encontrado no crawl: 1337testing@inlanefreight.htb
- Nova API key: ba988b835be4aa97d068941dc852ff33
#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ echo '154.57.164.71 inlanefreight.htb' | sudo tee -a /etc/hosts
154.57.164.71 inlanefreight.htb
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ whois inlanefreight.com | grep "IANA"
Registrar IANA ID: 468
Registrar IANA ID: 468
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -I http://inlanefreight.htb:32489
HTTP/1.1 200 OK
Server: nginx/1.26.1
Date: Mon, 13 Apr 2026 23:16:53 GMT
Content-Type: text/html
Content-Length: 120

Last-Modified: Thu, 01 Aug 2024 09:35:23 GMT
Connection: keep-alive
ETag: "66ab56db-78"
Accept-Ranges: bytes
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ gobuster vhost -u http://inlanefreight.htb:32489 -w /usr/share/seclists/Discovery/
DNS/subdomainstop1million-110000.txt --append-domain -t 100
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:
http://inlanefreight.htb:32489
[+] Method:
GET
[+] Threads:
100
[+] Wordlist:
/usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
[+] User Agent:
gobuster/3.8.2
[+] Timeout:
10s
[+] Append Domain:
true
[+] Exclude Hostname Length:
false
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
#www.inlanefreight.htb:32489 Status: 400 [Size: 157]
#mail.inlanefreight.htb:32489 Status: 400 [Size: 157]
#smtp.inlanefreight.htb:32489 Status: 400 [Size: 157]

#pop3.inlanefreight.htb:32489 Status: 400 [Size: 157]
web1337.inlanefreight.htb:32489 Status: 200 [Size: 104]
Progress: 114442 / 114442 (100.00%)
===============================================================
Finished
===============================================================

```

#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ echo '154.57.164.71 web1337.inlanefreight.htb' | sudo tee -a /etc/hosts
154.57.164.71 web1337.inlanefreight.htb
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -s http://web1337.inlanefreight.htb:32489/robots.txt
User-agent: *
Allow: /index.html
Allow: /index-2.html
Allow: /index-3.html
Disallow: /admin_h1dd3n
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -I http://web1337.inlanefreight.htb:32489/admin_h1dd3n
HTTP/1.1 301 Moved Permanently
Server: nginx/1.26.1
Date: Mon, 13 Apr 2026 23:22:34 GMT
Content-Type: text/html
Content-Length: 169
Location: http://web1337.inlanefreight.htb/admin_h1dd3n/
Connection: keep-alive
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -s http://web1337.inlanefreight.htb:32489/admin_h1dd3n/
<!DOCTYPE html><html><head><title>web1337 admin</title></head><body><h1>Welcome to
web1337 admin site</
h1><h2>The admin panel is currently under maintenance, but the API is still accessible
with the key
e963d863ee0e82ba7080fbf558ca0d3f</h2></body></html>
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]

└─$ gobuster vhost -u http://web1337.inlanefreight.htb:32489 -w /usr/share/seclists/
Discovery/DNS/subdomainstop1million-110000.txt --append-domain -t 500
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:
http://web1337.inlanefreight.htb:32489
[+] Method:
GET
[+] Threads:
500
[+] Wordlist:
/usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
[+] User Agent:
gobuster/3.8.2
[+] Timeout:
10s
[+] Append Domain:
true
[+] Exclude Hostname Length:
false
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
dev.web1337.inlanefreight.htb:32489 Status: 200 [Size: 123]
#www.web1337.inlanefreight.htb:32489 Status: 400 [Size: 157]
#mail.web1337.inlanefreight.htb:32489 Status: 400 [Size: 157]
#smtp.web1337.inlanefreight.htb:32489 Status: 400 [Size: 157]
#pop3.web1337.inlanefreight.htb:32489 Status: 400 [Size: 157]
Progress: 114442 / 114442 (100.00%)
===============================================================
Finished
===============================================================

```

#### Logs preservados

```text
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ echo '154.57.164.71 dev.web1337.inlanefreight.htb' | sudo tee -a /etc/hosts
154.57.164.71 dev.web1337.inlanefreight.htb
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ curl -s http://dev.web1337.inlanefreight.htb:32489
<!DOCTYPE html><html><head><title>Page 1</title></head><body><h1>Page 1</h1><a
href="index-334.html">Next</
a></body></
html>
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ mkdir -p devcrawl

┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ wget --mirror --convert-links --adjust-extension --page-requisites --no-parent -e
robots=off \
http://dev.web1337.inlanefreight.htb:32489/ -P devcrawl
[... 100 páginas baixadas no crawl ...]

Evidência final do email e da nova API key
┌──(demone㉿hackingmachine)-[~/Desktop/HTB Academy/Information Gathering - Web Edition]
└─$ grep -RniE 'inlanefreight\.htb|[a-f0-9]{32}|api|key|mail' devcrawl
devcrawl/dev.web1337.inlanefreight.htb:32489/index-105.html:1:<!DOCTYPE
html><html><head><title>Page 105</
title></head><body><h1>Page 105</h1><a href="index-166.html">Next</a><!-- Remember to
change the API key to
ba988b835be4aa97d068941dc852ff33 -->
devcrawl/dev.web1337.inlanefreight.htb:32489/index-80.html:1:<!DOCTYPE
html><html><head><title>Page 80</
title></head><body><h1>Page 80</h1><a href="index-635.html">Next</a><p>Contact us at
1337testing@inlanefreight.htb</p>

---

Apêndice preservado do chat
Os blocos abaixo preservam o resumo consolidado das respostas e o fechamento do
material prático original, mantidos aqui para não perder nenhum conteúdo já registrado.

APÊNDICE PRESERVADO

Resumo consolidado das respostas do módulo

10) Resumo consolidado das respostas do módulo
WHOIS
• 292
• admin@dnstinations.com

DNS
• 134.209.24.248
• inlanefreight.com.
• smtpin.vvv.facebook.com.
• my.inlanefreight.com

AXFR
• 22
• 10.10.34.2
• 10.10.200.14

vHosts
• web17611.inlanefreight.htb
• vm5.inlanefreight.htb
• browse.inlanefreight.htb
• admin.inlanefreight.htb
• support.inlanefreight.htb

Fingerprinting
• 2.4.41
• Joomla
• Ubuntu

Crawling
• inlanefreight-comp133.s3.amazonaws.htb

Web Archives
• 74

• 3054
• http://site.aboutface.com/
• Palm 0rganizer
• http://google.stanford.edu/
• 17-December-99
• 104155

Skill Assessment
• 468
• nginx
• e963d863ee0e82ba7080fbf558ca0d3f
• 1337testing@inlanefreight.htb
• ba988b835be4aa97d068941dc852ff33
---

APÊNDICE PRESERVADO

```

---

## Fechamento

11) Fechamento Status do módulo no chat: finalizado. Pedido do usuário no encerramento: finalizou, modulo acabado, export final desse modulo
