# Footprinting — Notas Finais do Módulo

**Módulo:** Footprinting  
**Dificuldade:** Medium  
**Tier:** 2  
**Estimativa:** 2 dias  
**Seções:** 21  
**Exercícios interativos:** 11  
**Assessment:** 0  
**Cubes:** 20

## Visão geral do módulo

Este módulo ensina técnicas de footprinting aplicadas aos serviços mais comuns encontrados em ambientes corporativos e de infraestrutura. O objetivo é aprender a **coletar o máximo de informação útil antes de qualquer ação invasiva**, usando observação, enumeração controlada e correlação de pistas técnicas e organizacionais.

Ao longo do conteúdo, o módulo trabalha tanto a parte conceitual — metodologia, camadas de enumeração, serviços e protocolos — quanto a parte prática em ambientes de laboratório. O foco está em transformar reconhecimento em entendimento real do alvo: quais serviços existem, o que eles revelam, como estão configurados e como essas informações se conectam.

### Tópicos principais
- princípios de enumeração
- metodologia de enumeração
- domínio, recursos em nuvem e staff
- FTP, SMB, NFS, DNS, SMTP, IMAP/POP3, SNMP
- MySQL, MSSQL e Oracle TNS
- IPMI
- protocolos de gerenciamento remoto em Linux e Windows
- labs de footprinting em três níveis de dificuldade

### Pré-requisitos sugeridos
- Linux Fundamentals
- Network Enumeration with Nmap
- Introduction to Networking
- Windows Fundamentals

## Estrutura do módulo

1. Enumeration Principles  
2. Enumeration Methodology  
3. Domain Information  
4. Cloud Resources  
5. Staff  
6. FTP  
7. SMB  
8. NFS  
9. DNS  
10. SMTP  
11. IMAP / POP3  
12. SNMP  
13. MySQL  
14. MSSQL  
15. Oracle TNS  
16. IPMI  
17. Linux Remote Management Protocols  
18. Windows Remote Management Protocols  
19. Footprinting Lab - Easy  
20. Footprinting Lab - Medium  
21. Footprinting Lab - Hard

## Como este export foi organizado

As seções teóricas aparecem na ordem do módulo. Quando existe hands-on correspondente, ele foi inserido **logo após a parte teórica da mesma seção**, preservando o fluxo de estudo: **teoria primeiro, prática depois**.

O conteúdo prático abaixo foi integrado a partir do recorte validado do seu hands-on, mantendo perguntas, respostas e logs juntos em suas respectivas seções.


## Seção 01 — Enumeration Principles

**Resumo teórico**

Enumeração é o processo de coletar e correlacionar informações úteis sobre um alvo a partir de diferentes fontes. O módulo diferencia bem **OSINT puramente passivo** de **enumeração ativa**: OSINT trabalha só com o que já está publicamente exposto, enquanto enumeração inclui interação direta e repetida com serviços, domínios, hosts e protocolos.

A ideia central da seção é que o objetivo não é “chegar logo no sistema”, mas **descobrir todos os caminhos plausíveis até ele**. Isso exige olhar para o que é visível, para o que não é visível e para o que os indícios sugerem. Em vez de forçar uma rota única, o analista precisa construir imagem, contexto e possibilidades.

A seção também reforça uma mentalidade investigativa. Algumas perguntas-guia são: o que conseguimos ver? O que isso nos permite inferir? O que pode estar faltando? O que o que não vemos diz sobre o alvo? Em outras palavras, a enumeração bem-feita depende tanto da coleta quanto da leitura crítica dos resultados.

**Pontos-chave**
- Distinguir OSINT passivo de enumeração ativa.
- Sempre buscar mais de um caminho possível para atingir o objetivo.
- Tratar ausência de informação como dado relevante.
- Construir entendimento do alvo antes de pensar em exploração.


## Seção 02 — Enumeration Methodology

**Resumo teórico**

A seção apresenta uma metodologia em camadas para organizar a enumeração e evitar lacunas. Em vez de uma sequência rígida de comandos, a proposta é um **modelo mental** aplicável a diferentes ambientes. O processo é dinâmico e adaptável, porque cada descoberta influencia a próxima etapa.

As camadas apresentadas são: **Internet Presence**, **Gateway**, **Accessible Services**, **Processes**, **Privileges** e **OS Setup**. Em termos práticos, começamos entendendo presença pública, exposição, fronteiras e serviços visíveis; depois ampliamos para processos internos, permissões e configuração do sistema operacional.

A grande lição é que a metodologia não é um checklist cego. Ela serve para estruturar raciocínio, organizar notas e garantir que nenhuma dimensão importante do alvo fique esquecida.

**Pontos-chave**
- A metodologia é iterativa, não linear.
- Cada camada aprofunda o entendimento do alvo.
- A parte mais explorada neste módulo é a de **serviços acessíveis**.
- O objetivo é criar um processo repetível e adaptável.


## Seção 03 — Domain Information

**Resumo teórico**

Domínio é um componente central da presença de uma empresa na internet. A seção mostra que, antes mesmo de interagir ativamente com serviços, já é possível descobrir bastante coisa sobre a organização a partir de website, certificados, DNS público, registros associados e ferramentas de terceiros.

A análise de certificados e logs de transparência pode revelar subdomínios, nomes internos ou padrões de nomenclatura. Ferramentas como `crt.sh`, mecanismos de busca e serviços de busca por IP/host também ajudam a identificar hosts diretamente expostos, provedores terceirizados, SaaS utilizados e outros ativos relacionados ao domínio.

A seção também mostra como registros DNS — A, MX, NS, TXT, CNAME, PTR e SOA — ajudam a entender infraestrutura, serviços de e-mail, provedores externos, verificações de domínio e integrações administrativas. Isso tudo compõe o mapa inicial da organização.

**Pontos-chave**
- O domínio ajuda a mapear presença pública, serviços e terceiros.
- Certificados e CT logs revelam subdomínios e padrões de nome.
- Registros DNS entregam contexto técnico e organizacional.
- Enumeração de domínio bem-feita reduz o esforço cego nas etapas seguintes.


## Seção 04 — Cloud Resources

**Resumo teórico**

A seção mostra que recursos em nuvem fazem parte da infraestrutura cotidiana das empresas e frequentemente deixam rastros úteis para footprinting. Isso inclui storage público ou mal configurado, documentos indexados, objetos estáticos referenciados em páginas web e buckets associados a nomes da organização.

Além do uso direto de buscadores, a seção discute análise de **source code**, referências em páginas e metadados que apontam para buckets, blobs, arquivos PDF, imagens, scripts e outros objetos externos. Também aparecem serviços de terceiros especializados em catalogar recursos públicos, permitindo localizar conteúdo vinculado ao nome da empresa.

O ponto crítico é que erros humanos em ambientes cloud podem vazar arquivos sensíveis, credenciais, chaves e artefatos de automação. Por isso, recursos cloud devem ser tratados como parte essencial da superfície de reconhecimento.

**Pontos-chave**
- Buckets, blobs e objetos expostos podem entregar dados valiosos.
- Código-fonte e referências HTML ajudam a localizar recursos cloud.
- Ferramentas de busca e indexação ampliam muito a descoberta passiva.
- Cloud mal configurada frequentemente leva a vazamento de segredos.


## Seção 05 — Staff

**Resumo teórico**

A seção trata do valor de identificar pessoas ligadas à organização. Perfis públicos, vagas, descrições de carreira, publicações técnicas e repositórios podem revelar tecnologias utilizadas, times internos, linguagens, frameworks, bancos de dados, fornecedores e áreas de responsabilidade.

Além disso, funcionários costumam expor mais do que pretendem em fóruns, GitHub, LinkedIn e páginas pessoais. Isso inclui nomes de usuário, estruturas de projeto, convenções de nomenclatura, nomes de sistemas, bibliotecas internas e até segredos acidentalmente publicados em repositórios.

O foco aqui é usar o elemento humano como fonte de contexto técnico. Entender quem trabalha com quê ajuda a inferir stack, possíveis superfícies expostas e até padrões de contas e acessos.

**Pontos-chave**
- Pessoas ajudam a revelar tecnologias e estrutura organizacional.
- Vagas e perfis públicos entregam stack e prioridades técnicas.
- Repositórios e postagens podem conter artefatos sensíveis.
- Nomes e funções de funcionários enriquecem toda a enumeração.


## Seção 06 — FTP

**Resumo teórico**

FTP é um dos protocolos clássicos de transferência de arquivos. A seção revisa a separação entre **canal de controle** e **canal de dados**, a diferença entre modo **ativo** e **passivo**, e o uso tradicional das portas **21/TCP** e **20/TCP**. O módulo também cita o TFTP como variante muito mais simples e menos segura.

Em termos de administração, a seção usa **vsFTPd** para ilustrar configurações comuns. Alguns parâmetros definem se login anônimo é permitido, se uploads podem ser feitos, se diretórios podem ser criados e se o usuário anônimo consegue gravar no servidor. Do ponto de vista de footprinting, isso muda completamente o valor do serviço.

A enumeração de FTP pode revelar versão, banner, estrutura de diretórios, permissões aparentes e arquivos interessantes. A seção também mostra que Nmap possui scripts específicos para FTP, e que TLS/SSL em FTP pode ser inspecionado com ferramentas como OpenSSL quando aplicável.

**Pontos-chave**
- FTP separa controle e dados e pode operar em modo ativo ou passivo.
- Configurações como login anônimo e escrita são especialmente sensíveis.
- Diretórios listados pelo serviço costumam gerar ótimas pistas.
- Scripts do Nmap ajudam a validar exposição, banner e permissões aparentes.

### Hands-on da seção

```text
Perguntas:
Which version of the FTP server is running on the target system? Submit the entire banner as the answer.
Enumerate the FTP server and find the flag.txt file. Submit the contents of it as the answer.
Respostas:
220 InFreight FTP v1.1
HTB{b7skjr4c76zhsds7fzhd4k3ujg7nhdjre}
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy]
■■$ nc -nv 10.129.33.79 21
(UNKNOWN) [10.129.33.79] 21 (ftp) open
220 InFreight FTP v1.1
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy]
■■$ ftp 10.129.33.79
Connected to 10.129.33.79.
220 InFreight FTP v1.1
Name (10.129.33.79:demone): anonymous
331 Anonymous login ok, send your complete email address as your password
Password:
230 Anonymous access granted, restrictions apply
ftp> ls
-rw-r--r-1 ftpuser ftpuser
39 Nov 8 2021 flag.txt
ftp> get flag.txt
ftp> bye
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy]
■■$ cat flag.txt


HTB{b7skjr4c76zhsds7fzhd4k3ujg7nhdjre}
```


## Seção 07 — SMB

**Resumo teórico**

SMB é o protocolo de compartilhamento de arquivos e recursos amplamente usado em ambientes Windows e também em Linux via **Samba**. A seção revisa SMB/CIFS, a relação com NetBIOS, o uso das portas **139/TCP** e **445/TCP**, e as diferenças entre versões mais antigas e implementações mais modernas.

Do lado administrativo, o módulo mostra como shares são definidos, nomeados e configurados. Parâmetros como visibilidade do share, permissão de leitura e escrita, uso de guest, máscaras de criação e permissões especiais mudam a exposição do serviço e podem causar vazamentos ou acesso excessivo.

Na prática de footprinting, ferramentas como `smbclient`, `rpcclient`, `smbmap`, `crackmapexec` e `enum4linux-ng` ajudam a enumerar shares, usuários, grupos, domínio, comentários de serviço e permissões aparentes. SMB é um serviço que frequentemente recompensa correlação cuidadosa entre nomes de share, comentários e estrutura interna.

**Pontos-chave**
- SMB/Samba é central para compartilhamento de arquivos em redes internas.
- Comentários e nomes de shares costumam entregar contexto valioso.
- Guest access, browseable e writable são configurações críticas.
- Ferramentas de enumeração podem revelar muito sem necessidade de exploração agressiva.

### Hands-on da seção

```text
Perguntas:
What version of the SMB server is running on the target system? Submit the entire banner as the answer.
What is the name of the accessible share on the target?
Connect to the discovered share and find the flag.txt file. Submit the contents as the answer.
Find out which domain the server belongs to.
Find additional information about the specific share we found previously and submit the customized version of that specific share as the answer.
What is the full system path of that specific share? (format: "/directory/names")
Respostas:
Samba smbd 4
sambashare
HTB{o873nz4xdo873n4zo873zn4fksuhldsf}
DEVOPS
InFreight SMB v3.1
/home/sambauser
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nmap -Pn -sV -sC -p139,445 10.129.33.85
139/tcp open netbios-ssn Samba smbd 4
445/tcp open netbios-ssn Samba smbd 4
nbstat: DEVSMB / DEVOPS
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ smbclient -N -L //10.129.33.85
Sharename
Type
Comment
print$
Disk
Printer Drivers
sambashare
Disk
InFreight SMB v3.1
IPC$
IPC
IPC Service (InlaneFreight SMB server (Samba, Ubuntu))
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ smbclient //10.129.33.85/sambashare -N
smb: \> cd contents
smb: \contents\> ls
flag.txt
smb: \contents\> get flag.txt smb_flag.txt
smb: \contents\> exit
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ cat smb_flag.txt
HTB{o873nz4xdo873n4zo873zn4fksuhldsf}
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ rpcclient -U "" -N 10.129.33.85 -c 'netsharegetinfo sambashare'
netname: sambashare
remark: InFreight SMB v3.1
path:
C:\home\sambauser\
```


## Seção 08 — NFS

**Resumo teórico**

NFS é o protocolo de compartilhamento de arquivos típico do ecossistema Unix/Linux. A seção destaca a relação entre **rpcbind**/**portmapper**, o serviço NFS em si e o uso comum das portas **111/TCP/UDP** e **2049/TCP/UDP**. Também explica a diferença entre versões e a importância do arquivo `exports`.

A configuração do NFS define quais diretórios são exportados e para quais hosts ou sub-redes. Opções como `rw`, `insecure`, `no_root_squash`, `no_subtree_check` e `sync` alteram bastante o risco operacional e a exposição. O destaque maior vai para `no_root_squash`, porque ele muda diretamente a relação entre root remoto e permissões locais.

Em footprinting, o fluxo costuma envolver descobrir exports, montar shares, listar conteúdo, observar donos de arquivos, UIDs/GIDs e identificar o que isso diz sobre usuários e permissões do sistema remoto.

**Pontos-chave**
- NFS expõe diretórios inteiros do sistema de arquivos.
- `exports` é a peça central de configuração.
- `no_root_squash` é uma das opções mais sensíveis.
- Montar um share NFS pode revelar usuários, grupos e material muito útil para correlação.

### Hands-on da seção

```text
Perguntas:
Enumerate the NFS service and submit the contents of the flag.txt in the "nfs" share as the answer.
Enumerate the NFS service and submit the contents of the flag.txt in the "nfsshare" share as the answer.
Respostas:
HTB{hjglmvtkjhlkfuhgi734zthrie7rjmdze}
HTB{8o7435zhtuih7fztdrzuhdhkfjcn7ghi4357ndcthzuc7rtfghu34}
Logs:


■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ showmount -e 10.129.202.5
/var/nfs
10.0.0.0/8
/mnt/nfsshare 10.0.0.0/8
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ sudo mount -t nfs 10.129.202.5:/var/nfs /mnt/htb_nfs
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ sudo mount -t nfs 10.129.202.5:/mnt/nfsshare /mnt/htb_nfsshare
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ cat /mnt/htb_nfs/flag.txt
HTB{hjglmvtkjhlkfuhgi734zthrie7rjmdze}
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ cat /mnt/htb_nfsshare/flag.txt
HTB{8o7435zhtuih7fztdrzuhdhkfjcn7ghi4357ndcthzuc7rtfghu34}
```


## Seção 09 — DNS

**Resumo teórico**

DNS é um dos pilares da infraestrutura de rede e conecta nomes a endereços, serviços e outros metadados. A seção revisa tipos de servidores DNS, principais registros e a lógica de resolução, destacando que DNS não serve apenas para nomes públicos: ele também pode expor detalhes internos, e-mail, delegações e estruturas inteiras de zona.

O módulo usa **BIND** para ilustrar configuração local, arquivos de zona e reverse lookup. Também discute opções perigosas como `allow-query`, `allow-recursion`, `allow-transfer` e estatísticas de zona, que podem tornar o serviço muito mais verboso do que deveria.

Em footprinting, DNS pode ser consultado de forma extremamente rica com `dig`, brute force de subdomínios e até **AXFR**, quando zone transfer está habilitada indevidamente. A zona certa pode revelar hosts internos, TXT records, controladores de domínio, naming conventions e muito mais.

**Pontos-chave**
- DNS expõe estrutura, serviços, e-mail e contexto organizacional.
- Zone transfer indevida é um dos vazamentos mais valiosos.
- BIND e arquivos de zona ajudam a entender a lógica do serviço.
- Subdomínio, TXT e reverse lookup enriquecem bastante o reconhecimento.

### Hands-on da seção

```text
Perguntas:
Interact with the target DNS using its IP address and enumerate the FQDN of it for the "inlanefreight.htb" domain.
Identify if its possible to perform a zone transfer and submit the TXT record as the answer. (Format: HTB{...})
What is the IPv4 address of the hostname DC1?
What is the FQDN of the host where the last octet ends with "x.x.x.203"?
Respostas:
ns.inlanefreight.htb
HTB{DN5_z0N3_7r4N5F3r_iskdufhcnlu34}
10.129.34.16
win2k.dev.inlanefreight.htb
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ dig axfr inlanefreight.htb @10.129.42.195
ns.inlanefreight.htb.
604800 IN
A
127.0.0.1
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ dig axfr internal.inlanefreight.htb @10.129.42.195
internal.inlanefreight.htb. 604800 IN TXT "HTB{DN5_z0N3_7r4N5F3r_iskdufhcnlu34}"
dc1.internal.inlanefreight.htb. 604800 IN A
10.129.34.16
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ for sub in $(cat /usr/share/seclists/Discovery/DNS/fierce-hostlist.txt); do
dig ${sub}.dev.inlanefreight.htb @10.129.42.195 +short | sed "s|^|${sub}.dev.inlanefreight.htb |"
done | grep '\.203$'
win2k.dev.inlanefreight.htb 10.12.3.203
```


## Seção 10 — SMTP

**Resumo teórico**

SMTP é o protocolo principal de envio de e-mail. A seção cobre o fluxo entre cliente, submission agent, relay e delivery agent, além das portas mais importantes: **25/TCP**, **465/TCP** e **587/TCP**. O módulo também explica comandos fundamentais como `HELO`, `EHLO`, `VRFY`, `MAIL FROM`, `RCPT TO`, `DATA` e `QUIT`.

Do ponto de vista de configuração, a seção usa **Postfix** para ilustrar defaults e controles relevantes. Ela reforça que algumas opções podem transformar o serviço em problema sério, como configurações permissivas de relay ou exposição excessiva de comandos e respostas de enumeração.

Em footprinting, SMTP é útil não só pelo banner, mas também porque pode entregar naming conventions, domínios internos, usuários válidos e comportamento do servidor com pouco ruído. O módulo reforça que isso deve ser feito com cuidado, especialmente em servidores de produção.

**Pontos-chave**
- SMTP entrega mais do que banner: nomes, domínios e usuários podem surgir.
- `VRFY` e comportamento do servidor ajudam na validação de contas.
- Open relay é uma configuração crítica de segurança.
- Enumeração cuidadosa de e-mail costuma render pistas importantes para outras etapas.

### Hands-on da seção

```text
Perguntas:
Enumerate the SMTP service and submit the banner, including its version as the answer.
Enumerate the SMTP service even further and find the username that exists on the system. Submit it as the answer.
Respostas:
220 InFreight ESMTP v2.11
robin
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nc -nv 10.129.33.137 25
220 InFreight ESMTP v2.11
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ smtp-user-enum -M VRFY -U footprinting-wordlist.txt -t 10.129.33.137 -w 30


10.129.33.137: robin exists
```


## Seção 11 — IMAP / POP3

**Resumo teórico**

A seção apresenta **POP3** e **IMAP** como protocolos de acesso a e-mail já entregue. POP3 é mais simples e focado em download de mensagens; IMAP é mais rico, trabalha com pastas e sincronização. As portas padrão destacadas são **110/995** para POP3 e **143/993** para IMAP.

O módulo usa **Dovecot** para ilustrar configuração padrão e opções administrativas. Também mostra comandos úteis de cada protocolo e destaca que certificados, banners, nomes organizacionais e estruturas de mailbox podem aparecer já na fase de enumeração.

Do ponto de vista de footprinting, esses serviços podem revelar organização, FQDN, versão customizada, endereços administrativos e até conteúdo útil em caixas e pastas acessíveis. Quando combinados com credenciais válidas, o valor para correlação sobe muito.

**Pontos-chave**
- IMAP e POP3 podem vazar identidade organizacional e contexto administrativo.
- Certificados e banners ajudam a confirmar FQDN e nome da organização.
- Pastas de e-mail muitas vezes entregam dados sensíveis e históricos.
- IMAP costuma ser mais rico para enumeração do que POP3.

### Hands-on da seção

```text
Perguntas:
Figure out the exact organization name from the IMAP/POP3 service and submit it as the answer.
What is the FQDN that the IMAP and POP3 servers are assigned to?
Enumerate the IMAP service and submit the flag as the answer. (Format: HTB{...})
What is the customized version of the POP3 server?
What is the admin email address?
Try to access the emails on the IMAP server and submit the flag as the answer. (Format: HTB{...})
Respostas:
InlaneFreight Ltd
dev.inlanefreight.htb
HTB{roncfbw7iszerd7shni7jr2343zhrj}
InFreight POP3 v9.188
devadmin@inlanefreight.htb
HTB{983uzn8jmfgpd8jmof8c34n7zio}
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy]
■■$ nmap -Pn -sV -sC -p110,143,993,995 10.129.13.63
ssl-cert: Subject: commonName=dev.inlanefreight.htb/organizationName=InlaneFreight Ltd
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy]
■■$ nc -nv 10.129.13.63 110
+OK InFreight POP3 v9.188
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy]
■■$ nc -nv 10.129.13.63 143
* OK [CAPABILITY ...] HTB{roncfbw7iszerd7shni7jr2343zhrj}
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy]
■■$ openssl s_client -quiet -connect 10.129.13.63:993
a1 LOGIN robin robin
a3 SELECT "DEV.DEPARTMENT.INT"
a5 FETCH 1 BODY[]
Subject: Flag
From: CTO <devadmin@inlanefreight.htb>
HTB{983uzn8jmfgpd8jmof8c34n7zio}
```


## Seção 12 — SNMP

**Resumo teórico**

SNMP é um protocolo de monitoramento e gerenciamento de dispositivos de rede, servidores e serviços. A seção cobre as diferenças entre **SNMPv1**, **v2c** e **v3**, destacando que versões antigas dependem de **community strings** e não fornecem o mesmo nível de proteção que o v3.

Também são explicados conceitos como **MIB** e **OID**, que são fundamentais para entender como informações são organizadas e consultadas. Em serviços mal configurados, SNMP pode expor descrição de sistema, localização, contatos, pacotes instalados, scripts em execução e outros detalhes muito valiosos.

A enumeração apresentada usa ferramentas como `snmpwalk`, `onesixtyone` e `braa`, mostrando que descobrir a community string certa pode abrir uma visão surpreendentemente ampla do host.

**Pontos-chave**
- SNMP pode revelar muito mais do que versão e hostname.
- Community strings fracas transformam o serviço em fonte rica de dados.
- MIBs e OIDs ajudam a interpretar corretamente os resultados.
- SNMPv2c ainda aparece bastante e merece atenção especial em redes internas.

### Hands-on da seção

```text
Perguntas:
Enumerate the SNMP service and obtain the email address of the admin. Submit it as the answer.
What is the customized version of the SNMP server?
Enumerate the custom script that is running on the system and submit its output as the answer.
Respostas:
devadmin@inlanefreight.htb
InFreight SNMP v0.91
HTB{5nMp_fl4g_uidhfljnsldiuhbfsdij44738b2u763g}
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ snmpwalk -v2c -c public 10.129.34.100
iso.3.6.1.2.1.1.4.0 = STRING: "devadmin <devadmin@inlanefreight.htb>"
iso.3.6.1.2.1.1.6.0 = STRING: "InFreight SNMP v0.91"
iso.3.6.1.2.1.25.1.7.1.3.1.1.4.70.76.65.71 = STRING: "HTB{5nMp_fl4g_uidhfljnsldiuhbfsdij44738b2u763g}"
```


## Seção 13 — MySQL

**Resumo teórico**

MySQL é um SGBD relacional muito comum em aplicações web e ambientes Linux. A seção revisa o modelo cliente-servidor, o uso de **SQL** para consultar e alterar dados e a presença frequente do MySQL em stacks como **LAMP** e **LEMP**.

O módulo também chama atenção para bancos internos e metadados como `mysql`, `information_schema`, `performance_schema` e `sys`, além de opções administrativas que podem mudar a exposição do serviço, como parâmetros ligados a usuário, senha, debug e operações de import/export.

Para footprinting, a porta crítica é **3306/TCP**. O foco da enumeração inicial é identificar versão, validar acesso com credenciais, listar bases e entender o papel do banco no ambiente.

**Pontos-chave**
- MySQL costuma guardar o “coração” da aplicação.
- A porta 3306 deve ser imediatamente associada ao serviço.
- Resultados automatizados precisam de validação manual.
- `information_schema` e `sys` ajudam a entender estrutura e metadados.

### Hands-on da seção

```text
Perguntas:
Enumerate the MySQL server and determine the version in use. (Format: MySQL X.X.XX)
During our penetration test, we found weak credentials "robin:robin". We should try these against the MySQL server. What is the email address of the customer "Otto
Lang"?
Respostas:
MySQL 8.0.27
ultrices@google.htb
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nmap -Pn -sV -p3306 10.129.34.104
3306/tcp open mysql
MySQL 8.0.27-0ubuntu0.20.04.1
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ mysql --skip-ssl -h 10.129.34.104 -u robin -p
MySQL [customers]> SELECT * FROM myTable WHERE name LIKE '%Otto%';
| 88 | Otto Lang | ultrices@google.htb |
```


## Seção 14 — MSSQL

**Resumo teórico**

Microsoft SQL Server é o SGBD relacional da Microsoft e aparece com frequência em ambientes Windows corporativos, especialmente quando há aplicações .NET e integração forte com identidade do domínio. A seção cita clientes como **SSMS** e destaca o uso de `mssqlclient.py` para interação prática.

Também são revisadas as bases padrão de sistema — `master`, `model`, `msdb`, `tempdb` e `resource` — e a importância de entender autenticação Windows, nome da instância, hostname e **named pipes** quando o serviço está exposto.

Em termos de footprinting, o serviço normalmente escuta em **1433/TCP**, e scripts do Nmap conseguem entregar hostname, instance name, versão do MSSQL e outras informações muito úteis para notas de reconhecimento.

**Pontos-chave**
- MSSQL costuma estar fortemente ligado a Windows e AD.
- `mssqlclient.py` é ferramenta importante para enumeração prática.
- Instância, hostname e named pipes são dados valiosos.
- Bases padrão ajudam a separar o que é default do que é específico do ambiente.

### Hands-on da seção

```text
Perguntas:
Enumerate the target using the concepts taught in this section. List the hostname of MSSQL server.
Connect to the MSSQL instance running on the target using the account (backdoor:Password1), then list the non-default database present on the server.
Respostas:
ILF-SQL-01
Employees
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nmap -Pn -sV -p1433 --script ms-sql-info,ms-sql-ntlm-info 10.129.34.109
Target_Name: ILF-SQL-01
DNS_Computer_Name: ILF-SQL-01
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ impacket-mssqlclient backdoor:Password1@10.129.34.109 -windows-auth
SELECT name FROM sys.databases WHERE name NOT IN ('master','model','msdb','tempdb');
Employees
```


## Seção 15 — Oracle TNS

**Resumo teórico**

Oracle TNS é a camada de comunicação usada no ecossistema Oracle para resolver nomes, conectar clientes, encaminhar conexões e organizar o acesso a instâncias e serviços. A seção destaca a porta **1521/TCP**, o papel do **listener** e a importância de entender conceitos como **SID** e **service name**.

Os arquivos **`tnsnames.ora`** e **`listener.ora`** aparecem como peças-chave da configuração: o primeiro define a lógica de conexão do lado cliente; o segundo, o comportamento do listener do lado servidor. O módulo também introduz o **ODAT** como ferramenta de enumeração para ambientes Oracle.

Na prática, footprinting de Oracle gira em torno de descobrir listener, SID, credenciais válidas e o contexto da instância. Uma vez autenticado, o analista consegue enumerar tabelas, privilégios e até dados administrativos muito sensíveis.

**Pontos-chave**
- Listener Oracle em 1521/TCP é indicador central.
- SID e service name são conceitos obrigatórios.
- `tnsnames.ora` e `listener.ora` ajudam a entender a arquitetura do serviço.
- ODAT e SQL*Plus aparecem como ferramentas de trabalho importantes.

### Hands-on da seção

```text
Pergunta:
Enumerate the target Oracle database and submit the password hash of the user DBSNMP as the answer.
Respostas:
E066D214D5421CCC
S:59354E99120C523F77232A8CCFDE5E780591FCE14109EEE2C86F4A9B4E8F
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ odat all -s 10.129.205.19
SIDs found on the 10.129.205.19:1521 server: XE
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ sqlplus scott/tiger@10.129.205.19/XE as sysdba
SQL> select name, password, spare4 from sys.user$ where name='DBSNMP';
DBSNMP
E066D214D5421CCC
S:59354E99120C523F77232A8CCFDE5E780591FCE14109EEE2C86F4A9B4E8F
```


## Seção 16 — IPMI

**Resumo teórico**

IPMI é um padrão de gerenciamento fora de banda que permite administrar hardware independentemente do sistema operacional principal. A seção mostra que esse tipo de serviço costuma aparecer por meio de controladoras como **iLO**, **iDRAC** e implementações de **BMC**, geralmente usando **UDP 623**.

Como o BMC opera abaixo do sistema operacional, o impacto de uma exposição indevida é muito alto: o serviço pode permitir monitoramento, console remoto, reboot, mudanças de boot e outras funções administrativas de grande sensibilidade.

O módulo reforça que a combinação de BMC exposto, credenciais padrão e segmentação ruim torna IPMI especialmente perigoso em redes internas. O foco da enumeração é identificar versão, autenticação e risco operacional do serviço.

**Pontos-chave**
- IPMI é gerenciamento fora de banda e independe do SO.
- UDP 623 é a porta associada ao protocolo.
- BMC exposto é achado de alto impacto.
- Credenciais padrão e isolamento fraco amplificam muito o risco.

### Hands-on da seção

```text
Perguntas:
What username is configured for accessing the host via IPMI?
What is the account's cleartext password?
Respostas:
admin
trinity
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ msfconsole
use auxiliary/scanner/ipmi/ipmi_dumphashes
set RHOSTS 10.129.34.114
run
Hash found: admin:fd9f252e82000000015629431657dad95c2a9c74ad940548ac96bba8d4ae0f21d7bb9bb614bfe8a0a123456789abcdefa123456789abcdef140561646d696e:09c8723bc864d8b9e68f9e7
1a63ed83135cb7390
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ hashcat -m 7300 hash.txt /usr/share/wordlists/rockyou.txt
...:trinity
```


## Seção 17 — Linux Remote Management Protocols

**Resumo teórico**

A seção reúne os principais mecanismos de administração remota em ambientes Linux. O destaque principal é o **SSH**, com autenticação por senha e por chave pública, configuração via `sshd_config` e avaliação de opções perigosas como root login, senhas vazias, forwarding e tunneling.

Além de SSH, o módulo cobre **rsync** como serviço de sincronização e compartilhamento eficiente, normalmente em **873/TCP**, e os antigos **r-services** (`rlogin`, `rsh`, `rcp`, `rwho`, `rusers`) nas portas **512/513/514**, mostrando como relações de confiança indevida em `hosts.equiv` e `.rhosts` podem criar risco elevado.

A ideia principal é entender que serviços de gerenciamento remoto não são “apenas acesso”, mas superfícies ricas em identidade, trust relationships, automação e movimentação lateral.

**Pontos-chave**
- SSH é o padrão moderno de administração remota em Linux.
- Chaves SSH expostas são achados críticos.
- Rsync pode vazar arquivos e diretórios inteiros.
- `hosts.equiv` e `.rhosts` são sinais fortes de confiança mal distribuída.


## Seção 18 — Windows Remote Management Protocols

**Resumo teórico**

No ecossistema Windows, a sessão destaca três canais principais de gerenciamento remoto: **RDP**, **WinRM** e **WMI**. RDP oferece acesso gráfico e normalmente usa **3389/TCP**, com atenção especial para **NLA**, TLS e o comportamento do certificado.

O **WinRM** é a base do PowerShell Remoting e de muita automação administrativa em ambientes Windows, usando **5985/TCP** (HTTP) e **5986/TCP** (HTTPS). Já o **WMI** fornece uma camada ampla de consulta e administração sobre objetos internos do sistema, normalmente iniciando comunicação por **135/TCP**.

A principal lição é que, em ambientes Windows, esses serviços revelam muito sobre papel do host, naming, domínio e postura administrativa, sendo essenciais para footprinting interno.

**Pontos-chave**
- RDP, WinRM e WMI formam o trio central de gerenciamento remoto Windows.
- NLA e detalhes do handshake RDP devem ser anotados.
- WinRM é forte indicador de administração via PowerShell.
- WMI expõe enorme visibilidade sobre processos, serviços e configuração.


## Seção 19 — Footprinting Lab - Easy

**Resumo teórico do briefing**

O primeiro servidor do laboratório fica em uma sub-rede interna e deve ser investigado com foco em **coleta máxima de informação**. O cliente quer entender que tipo de dado pode ser obtido dos serviços expostos e como isso poderia ser usado contra a própria infraestrutura.

Há duas pistas relevantes: a equipe já encontrou a credencial `ceiqwer1234` e também identificou que o nome de um funcionário aparece em um post do fórum do HTB. O objetivo final da prova é localizar e ler o conteúdo de um arquivo `flag.txt` armazenado no host.

O ponto de aprendizagem aqui é transformar footprinting cuidadoso em resultado prático sem partir para exploração agressiva ou exploits, respeitando o fato de que o host é de produção.

**Pontos-chave**
- Foco total em enumeração metódica e correlação de pistas.
- Uso controlado de credenciais já descobertas.
- Proibição de exploração agressiva.
- Objetivo final: localizar e ler `flag.txt`.

### Hands-on da seção

```text
Pergunta:
Enumerate the server carefully and find the flag.txt file. Submit the contents of this file as the answer.
Resposta:
HTB{7nrzise7hednrxihskjed7nzrgkweunj47zngrhdbkjhgdfbjkc7hgj}
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nmap -Pn -sC -sV -p21,22,53,2121 10.129.34.125
21/tcp
open ftp
ProFTPD Server (ftp.int.inlanefreight.htb) [10.129.34.125]
2121/tcp open ccproxy-ftp ProFTPD Server (Ceil's FTP) [10.129.34.125]
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ ftp ftp.internal.inlanefreight.htb 2121
ceil
qwer1234
ftp> cd .ssh
ftp> get id_rsa
ftp> bye
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ chmod 600 id_rsa
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ ssh ceil@10.129.34.125 -i id_rsa
ceil@NIXEASY:~$ cat /home/flag/flag.txt
HTB{7nrzise7hednrxihskjed7nzrgkweunj47zngrhdbkjhgdfbjkc7hgj}
```


## Seção 20 — Footprinting Lab - Medium

**Resumo teórico do briefing**

O segundo servidor é amplamente acessível por usuários da rede interna, o que o torna alvo natural de atacantes e, por isso, parte importante do escopo do teste. A missão continua sendo coletar o máximo possível de informação sobre o host e encontrar formas de usar esse conhecimento contra o próprio servidor.

Como mecanismo de prova, foi criada uma conta chamada **HTB**. O objetivo final do laboratório é **obter as credenciais desse usuário**. Isso muda o foco da enumeração: em vez de procurar apenas um artefato, passa a ser necessário localizar evidências, arquivos, bases, shares ou serviços que levem especificamente a segredos de autenticação.

**Pontos-chave**
- Host de alto valor por ser acessível a muitos usuários internos.
- Enumeração orientada por objetivo.
- Foco em segredos, identidade e contexto de conta.
- Objetivo final: descobrir a senha do usuário `HTB`.

### Hands-on da seção

```text
Pergunta:
Enumerate the server carefully and find the username "HTB" and its password. Then, submit this user's password as the answer.
Resposta:
lnch7ehrdn43i7AoqVPK4zWR
Logs:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ showmount -e 10.129.202.41
/TechSupport (everyone)


■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ sudo grep -RniE 'alex|pass|user|cred' /mnt/techsupport
alex
user="alex"
password="lol123!mD"
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ smbclient //10.129.202.41/devshare -U 'alex%lol123!mD'
smb: \> get important.txt
smb: \> exit
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ cat important.txt
sa:87N1ns@slls83
Relato do chat:
No SQL Server Management Studio, em accounts -> dbo.devsacc -> Edit Top 200 Rows, foi localizada a linha:
157 HTB lnch7ehrdn43i7AoqVPK4zWR
```


## Seção 21 — Footprinting Lab - Hard

**Resumo teórico do briefing**

O terceiro servidor do laboratório é ainda mais crítico: ele atua como **MX**, **management server** e **backup server** de contas internas do domínio. Esse tipo de host concentra dados sensíveis, integrações, histórico administrativo e superfícies ligadas à identidade da organização.

Assim como no lab Medium, foi criada uma conta chamada **HTB** e o objetivo é obter as credenciais desse usuário. A diferença é que agora o analista precisa priorizar melhor a enumeração com base no papel do servidor, entendendo o valor especial de serviços ligados a e-mail, gerenciamento e backup.

Este lab treina leitura contextual do ativo: mais do que enumerar portas, o analista precisa entender **o papel do host na infraestrutura** para decidir onde estão as melhores pistas.

**Pontos-chave**
- Host crítico e multifuncional.
- Alta probabilidade de conter segredos e dados de identidade.
- Enumeração deve ser orientada pela função do servidor.
- Objetivo final: obter as credenciais do usuário `HTB`.

### Hands-on da seção

```text
Pergunta:
Enumerate the server carefully and find the username "HTB" and its password. Then, submit HTB's password as the answer.
Resposta:
cr3n4o7rzse7rzhnckhssncif7ds
Complemento e hands-on final desta pergunta:
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nmap -Pn -sC -sV -p22,110,143,993,995 10.129.202.20
Starting Nmap 7.98 ( https://nmap.org ) at 2026-04-09 21:49 -0300
Nmap scan report for 10.129.202.20
Host is up (0.14s latency).
PORT
STATE SERVICE VERSION
22/tcp open ssh
OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
110/tcp open pop3
Dovecot pop3d
143/tcp open imap
Dovecot imapd (Ubuntu)
993/tcp open ssl/imap Dovecot imapd (Ubuntu)
995/tcp open ssl/pop3 Dovecot pop3d
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nmap -Pn -sU --top-ports 100 10.129.202.20
PORT
STATE
SERVICE
68/udp open|filtered dhcpc
161/udp open
snmp
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp.txt 10.129.202.20
Scanning 1 hosts, 3219 communities
10.129.202.20 [backup] Linux NIXHARD 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ snmpwalk -v2c -c backup 10.129.202.20 | grep -i 'tom\|NMds\|backup'
iso.3.6.1.2.1.25.1.7.1.2.1.2.6.66.65.67.75.85.80 = STRING: "/opt/tom-recovery.sh"
iso.3.6.1.2.1.25.1.7.1.2.1.3.6.66.65.67.75.85.80 = STRING: "tom NMds732Js2761"
iso.3.6.1.2.1.25.1.7.1.3.1.1.6.66.65.67.75.85.80 = STRING: "chpasswd: (user tom) pam_chauthtok() failed, error:"
iso.3.6.1.2.1.25.1.7.1.3.1.2.6.66.65.67.75.85.80 = STRING: "chpasswd: (user tom) pam_chauthtok() failed, error:
chpasswd: (line 1, user tom) password not changed
Changing password for tom."
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.1 = STRING: "chpasswd: (user tom) pam_chauthtok() failed, error:"
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.3 = STRING: "chpasswd: (line 1, user tom) password not changed"
iso.3.6.1.2.1.25.1.7.1.4.1.2.6.66.65.67.75.85.80.4 = STRING: "Changing password for tom."


■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ openssl s_client -quiet -connect 10.129.202.20:993
Connecting to 10.129.202.20
Can't use SSL_get_servername
depth=0 CN=NIXHARD
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN=NIXHARD
verify return:1
* OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE LITERAL+ AUTH=PLAIN] Dovecot (Ubuntu) ready.
a1 LOGIN tom NMds732Js2761
a1 OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS THREAD=ORDEREDSUBJECT MULTIAPPEND URL-PARTIAL
CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS BINARY MOVE
SNIPPET=FUZZY PREVIEW=FUZZY LITERAL+ NOTIFY SPECIAL-USE] Logged in
a2 LIST "" "*"
* LIST (\HasNoChildren) "." Notes
* LIST (\HasNoChildren) "." Meetings
* LIST (\HasNoChildren \UnMarked) "." Important
* LIST (\HasNoChildren) "." INBOX
a2 OK List completed (0.014 + 0.000 + 0.013 secs).
a3 SELECT INBOX
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
* 1 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1636509064] UIDs valid
* OK [UIDNEXT 2] Predicted next UID
a3 OK [READ-WRITE] Select completed (0.006 + 0.000 + 0.005 secs).
a4 FETCH 1 BODY[]
* 1 FETCH (BODY[] {3661}
HELO dev.inlanefreight.htb
MAIL FROM:<tech@dev.inlanefreight.htb>
RCPT TO:<bob@inlanefreight.htb>
DATA
From: [Admin] <tech@inlanefreight.htb>
To: <tom@inlanefreight.htb>
Date: Wed, 10 Nov 2010 14:21:26 +0200
Subject: KEY
-----BEGIN OPENSSH PRIVATE KEY----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
-----END OPENSSH PRIVATE KEY----)
a4 OK Fetch completed (0.007 + 0.000 + 0.006 secs).
a5 LOGOUT
* BYE Logging out
a5 OK Logout completed (0.001 + 0.000 secs).
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ nano id_rsaa
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ chmod 600 id_rsaa
■■■(demone■hackingmachine)-[~/Desktop/HTB Academy/Footprint]
■■$ ssh -i id_rsaa tom@10.129.202.20
The authenticity of host '10.129.202.20 (10.129.202.20)' can't be established.
ED25519 key fingerprint is: SHA256:AtNYHXCA7dVpi58LB+uuPe9xvc2lJwA6y7q82kZoBNM
This host key is known by the following other names/addresses:
~/.ssh/known_hosts:41: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.129.202.20' (ED25519) to the list of known hosts.
** WARNING: connection is not using a post-quantum key exchange algorithm.
** This session may be vulnerable to "store now, decrypt later" attacks.
** The server may need to be upgraded. See https://openssh.com/pq.html
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-90-generic x86_64)
* Documentation:
* Management:
* Support:

https://help.ubuntu.com
https://landscape.canonical.com
https://ubuntu.com/advantage

System information as of Fri 10 Apr 2026 12:57:12 AM UTC
System load: 0.0
Usage of /:
68.6% of 5.40GB
Memory usage: 29%
Swap usage:
0%

Processes:
166
Users logged in:
0
IPv4 address for ens192: 10.129.202.20

* Super-optimized for small spaces - read how we shrank the memory
footprint of MicroK8s to make it the smallest full K8s around.
https://ubuntu.com/blog/microk8s-memory-optimisation


0 updates can be applied immediately.
The list of available updates is more than a week old.
To check for new updates run: sudo apt update
Last login: Wed Nov 10 02:51:52 2021 from 10.10.14.20
tom@NIXHARD:~$ id
uid=1002(tom) gid=1002(tom) groups=1002(tom),119(mysql)
tom@NIXHARD:~$ groups
tom mysql
tom@NIXHARD:~$ systemctl status mysql
● mysql.service - MySQL Community Server
Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
Active: active (running) since Fri 2026-04-10 00:42:08 UTC; 15min ago
Process: 893 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)
Main PID: 980 (mysqld)
Status: "Server is operational"
Tasks: 37 (limit: 2278)
Memory: 415.6M
CGroup: /system.slice/mysql.service
■■980 /usr/sbin/mysqld
Warning: some journal files were not opened due to insufficient permissions.
tom@NIXHARD:~$ mysql -u tom -p -h 127.0.0.1
Enter password:
Welcome to the MySQL monitor. Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.27-0ubuntu0.20.04.1 (Ubuntu)
Copyright (c) 2000, 2021, Oracle and/or its affiliates.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> SHOW DATABASES;
+--------------------+
| Database
|
+--------------------+
| information_schema |
| mysql
|
| performance_schema |
| sys
|
| users
|
+--------------------+
5 rows in set (0.02 sec)
mysql> USE users;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> SHOW TABLES;
+-----------------+
| Tables_in_users |
+-----------------+
| users
|
+-----------------+
1 row in set (0.00 sec)
mysql> SELECT * FROM users WHERE username LIKE "%HTB%";
+------+----------+------------------------------+
| id
| username | password
|


+------+----------+------------------------------+
| 150 | HTB
| cr3n4o7rzse7rzhnckhssncif7ds |
+------+----------+------------------------------+
1 row in set (0.01 sec)
mysql> DESCRIBE users;
+----------+-------------+------+-----+---------+-------+
| Field
| Type
| Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id
| int
| YES |
| NULL
|
|
| username | varchar(50) | YES |
| NULL
|
|
| password | varchar(50) | YES |
| NULL
|
|
+----------+-------------+------+-----+---------+-------+
3 rows in set (0.01 sec)
mysql> SELECT username,password FROM users WHERE username LIKE "%HTB%";
+----------+------------------------------+
| username | password
|
+----------+------------------------------+
| HTB
| cr3n4o7rzse7rzhnckhssncif7ds |
+----------+------------------------------+
1 row in set (0.01 sec)
```
