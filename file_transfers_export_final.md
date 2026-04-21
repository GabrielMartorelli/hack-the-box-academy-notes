# File Transfers
**HTB Academy • Export final do módulo**
---
## Visão geral do módulo
- **Dificuldade:** Medium
- **Tier:** Tier 0
- **Tempo estimado:** 3 horas
- **Última atualização informada:** 4 anos atrás
- **Estrutura:** 10 seções • 2 hands-ons finais intercalados • 0 assessments
- **Pré-requisitos citados:** Introduction to Networking, Linux Fundamentals, Web Requests
- **Badge/Cubes:** 10 cubes

## Descrição
Módulo focado em transferência de arquivos durante assessments, cobrindo métodos em Windows e Linux, uso de utilitários nativos, web servers, técnicas living off the land, detecção e noções de evasão.

## Síntese do módulo
O módulo constrói repertório operacional para mover arquivos entre atacante e alvo em ambientes restritos. Ele percorre downloads e uploads em Windows e Linux, uso de linguagens de programação como canal de transferência, métodos alternativos com Netcat, WinRM e RDP, proteção de arquivos sensíveis, upload over HTTP/S, binários LOLBAS/GTFOBins e a telemetria que esses métodos deixam para defesa.

## Mapa das seções
- **Seção 01 — File Transfers**
- **Seção 02 — Windows File Transfer Methods**
- **Seção 03 — Linux File Transfer Methods**
- **Seção 04 — Transferring Files with Code**
- **Seção 05 — Miscellaneous File Transfer Methods**
- **Seção 06 — Protected File Transfers**
- **Seção 07 — Catching Files over HTTP/S**
- **Seção 08 — Living off The Land**
- **Seção 09 — Detection**
- **Seção 10 — Evading Detection**

## Como este export está organizado
A teoria foi consolidada seção a seção em pt-BR, mantendo a lógica do módulo e intercalando as duas hands-ons finais exatamente nos pontos em que elas fazem mais sentido no fluxo: após a seção de métodos em Windows e após a seção de métodos em Linux.

---
## Seção 01 — File Transfers

### Resumo teórico
A seção de abertura posiciona a transferência de arquivos como capability operacional central em qualquer assessment. O cenário apresentado mostra que mover um arquivo para o alvo ou trazê-lo de volta quase nunca é um processo linear: filtros de saída, bloqueio de ferramenta nativa, políticas de aplicação, AV/EDR, firewall e restrições de protocolo podem derrubar a primeira tentativa e obrigar o operador a mudar de rota.

A mensagem principal é que file transfer não é fim em si mesmo. Ele suporta enumeração, execução de tooling, privilege escalation, coleta de evidência e exfiltração autorizada em laboratório. Por isso, o módulo inteiro é construído como um guia de repertório: quanto mais métodos o profissional dominar, maior a chance de encontrar um caminho viável quando HTTP, FTP, SMB, PowerShell ou qualquer outro recurso estiver limitado pelo ambiente.

### Pontos-chave
- Transferência de arquivos é necessidade recorrente em pós-exploração, troubleshooting e coleta de artefatos.
- Host controls e network controls influenciam o sucesso da operação de forma equivalente.
- Ferramenta nativa não significa ferramenta garantida: o método pode ser monitorado ou bloqueado.
- O valor do módulo está em construir contingência operacional, não em decorar um único fluxo.

### O que vale fixar
Em file transfer, o diferencial não é conhecer um método perfeito; é saber alternar rapidamente entre métodos imperfeitos conforme o ambiente.

---
## Seção 02 — Windows File Transfer Methods

### Resumo teórico
A seção trabalha downloads e uploads em Windows com foco em ferramentas e componentes já presentes no sistema. O módulo parte de PowerShell, Base64, HTTP/HTTPS, SMB, FTP e WebDAV para mostrar que o ecossistema Windows oferece múltiplos caminhos práticos para mover arquivos sem depender imediatamente de binários externos.

Além dos downloads clássicos com PowerShell, a seção destaca execução em memória, diferenças entre clientes HTTP, erros comuns ligados a Internet Explorer legacy components e TLS, uso de SMB autenticado, automação com arquivos de comando para FTP e variações de upload via HTTP e WebDAV. O aprendizado central é que o operador precisa escolher o canal de acordo com o tipo de shell, a conectividade disponível, a sensibilidade do arquivo e o nível de ruído que o método tende a gerar.

### Pontos-chave
- Base64 funciona como método de contingência para arquivos menores e shells limitadas.
- HTTP/HTTPS costuma ser o caminho mais conveniente, mas pode falhar por filtros, parsing ou certificado.
- SMB continua forte em ecossistema Windows, porém guest access e porta 445 podem estar bloqueados.
- WebDAV é alternativa valiosa quando SMB direto não é viável.

### O que vale fixar
No Windows, download e upload são problemas parecidos, mas não idênticos: o receptor, o protocolo e a autenticação mudam totalmente a escolha do método.

### Hands-on da seção — Windows: download com wget, upload de ZIP e hashing no alvo

#### Enunciado preservado
```text
10.129.201.55 - Download the file flag.txt from the web root using wget from the Pwnbox. Submit the contents of the file as your answer.
Upload the attached file named upload_win.zip to the target using the method of your choice. Once uploaded, unzip the archive, and run "hasher upload_win.txt" from the command line. Submit the generated hash as your answer.
RDP to 10.129.201.55 (ACADEMY-MISC-MS02), with user "htb-student" and password "HTB_@cademy_stdnt!"
```

#### Respostas finais registradas
- **Flag do web root:** `b1a4ca918282fcd96004565521944a3b`
- **Hash de upload_win.txt:** `f458303ea783c224c6b4e7ef7f17eb9d`

#### Procedimento prático consolidado
1. O flag.txt foi baixado diretamente do Pwnbox com wget e validado com cat, retornando o valor final correto.
2. O compartilhamento redirecionado via \tsclient\share permitiu listar o ZIP no host Windows, mas a cópia direta para Desktop falhou com Access is denied.
3. Do lado do Pwnbox, o ZIP foi revisado e ficou claro que o arquivo estava com owner root e sem leitura adequada para o processo do python3 -m http.server.
4. Após corrigir a disponibilidade do arquivo e expor um servidor Python em 8080, o host Windows baixou o ZIP com Invoke-WebRequest para o diretório temporário.
5. O arquivo foi extraído com Expand-Archive e o comando hasher foi executado sobre upload_win.txt, produzindo o hash final validado.

#### Comandos e saídas preservados

**Download inicial do flag no Pwnbox**

```text
wget http://10.129.201.55/flag.txt -O flag.txt
cat flag.txt

b1a4ca918282fcd96004565521944a3b
```

**Validação do problema de cópia via share redirecionada**

```text
C:\Users\htb-student>dir \\tsclient\share
 Directory of \\tsclient\share
04/20/2026 03:44 PM               194 upload_win.zip

C:\Users\htb-student>copy \\tsclient\share\upload_win.zip %USERPROFILE%\Desktop\
Access is denied.
```

**Diagnóstico do arquivo no Pwnbox**

```text
ls -lah
-rwxr-x--- 1 root root 194 Apr 20 19:44 upload_win.zip
file upload_win.zip
upload_win.zip: regular file, no read permission
```

**Download, extração e hashing no Windows**

```text
PS C:\Users\htb-student> iwr http://10.10.14.179:8080/upload_win.zip -OutFile $env:TEMP\upload_win.zip
PS C:\Users\htb-student> Expand-Archive -LiteralPath $env:TEMP\upload_win.zip -DestinationPath $env:TEMP\upload_win -Force
PS C:\Users\htb-student> hasher $env:TEMP\upload_win\upload_win.txt
f458303ea783c224c6b4e7ef7f17eb9d
```

---
## Seção 03 — Linux File Transfer Methods

### Resumo teórico
A seção espelha o raciocínio de Windows no ecossistema Linux, mas agora priorizando ferramentas extremamente comuns em distribuições Unix-like, como wget, cURL, Bash, Python, web servers leves e SCP. O módulo usa um cenário em que um atacante tentou múltiplos métodos em sequência para reforçar que o operador não deve depender de uma ferramenta única.

Além de downloads web tradicionais, a seção cobre Base64 encode/decode, execução em fluxo usando pipes, fallback com /dev/tcp do Bash, uso de SSH/SCP, uploads via servidor HTTP preparado para receber arquivos e a criação rápida de web servers com Python, PHP ou Ruby. A parte conceitual mais importante é que Linux favorece composição: a combinação de ferramentas simples muitas vezes resolve o problema sem exigir binários adicionais.

### Pontos-chave
- wget e cURL são os pilares de download web em Linux.
- Pipes facilitam execução em fluxo e reduzem dependência de artefato intermediário.
- SCP é uma rota muito forte quando a porta 22 e credenciais válidas estão disponíveis.
- Servidor HTTP temporário no host comprometido pode simplificar coleta de arquivo para a máquina de ataque.

### O que vale fixar
Em Linux, repertório significa combinar utilitários disponíveis, direção do tráfego e simplicidade operacional até encontrar o fluxo que o ambiente aceita.

### Hands-on da seção — Linux: download com Python, upload via scp, extração e hashing no alvo

#### Enunciado preservado
```text
10.129.234.168 Download the file flag.txt from the web root using Python from the Pwnbox. Submit the contents of the file as your answer.
Upload the attached file named upload_nix.zip to the target using the method of your choice. Once uploaded, SSH to the box, extract the file, and run "hasher <extracted file>" from the command line. Submit the generated hash as your answer.
SSH to 10.129.234.168 (ACADEMY-MISC-NIX04), with user "htb-student" and password "HTB_@cademy_stdnt!" You can use gunzip to extract the file with the command: gunzip -S .zip upload_nix.zip
```

#### Respostas finais registradas
- **Flag do web root:** `5d21cf3da9c0ccb94f709e2559f3ea50`
- **Hash do arquivo upload_nix extraído:** `159cfe5c65054bbadb2761cfa359c8b0`

#### Procedimento prático consolidado
1. O flag.txt foi baixado com um one-liner em Python usando urllib.request e validado localmente com cat.
2. O upload_nix.zip estava em uma shared folder local, exigindo sudo para listagem, cópia para o diretório do módulo e ajuste de owner e permissões.
3. Embora um python3 -m http.server tenha sido usado para validar a exposição local do ZIP, a transferência final foi feita de forma direta com scp para /tmp/upload_nix.zip no alvo Linux.
4. No host de destino, o arquivo foi extraído com gunzip -S .zip e o hasher foi executado sobre o arquivo resultante upload_nix.
5. O hash final validado fechou o hands-on e confirmou que o fluxo de download, upload e pós-processamento no alvo funcionou corretamente.

#### Comandos e saídas preservados

**Download do flag via Python no Pwnbox**

```text
python3 -c "import urllib.request; open('flag.txt','wb').write(urllib.request.urlopen('http://10.129.234.168/flag.txt').read())"
cat flag.txt

5d21cf3da9c0ccb94f709e2559f3ea50
```

**Cópia do ZIP da shared folder local**

```text
sudo ls /media/sf_Downloads
sudo cp /media/sf_Downloads/upload_nix.zip ~/Desktop/HTB\ Academy/File\ Transfers
sudo chown demone:demone upload_nix.zip
sudo chmod 644 upload_nix.zip
```

**Transferência final com scp**

```text
scp upload_nix.zip htb-student@10.129.234.168:/tmp/upload_nix.zip
...
upload_nix.zip 100% 194 1.3KB/s 00:00
```

**Extração e hashing no alvo Linux**

```text
ssh htb-student@10.129.234.168
cd /tmp/
gunzip -S .zip upload_nix.zip
hasher upload_nix

159cfe5c65054bbadb2761cfa359c8b0
```

---
## Seção 04 — Transferring Files with Code

### Resumo teórico
A seção mostra que linguagens presentes no host podem funcionar como mecanismo de file transfer quando utilitários tradicionais não estão disponíveis ou não são a melhor escolha. O módulo percorre Python, PHP, Ruby, Perl, JavaScript e VBScript como formas programáveis de baixar ou enviar conteúdo usando poucas linhas de código.

O padrão mental se repete em quase todas as linguagens: abrir uma URL, recuperar o conteúdo, tratá-lo como string ou stream e gravar localmente, ou então enviar um arquivo em uma requisição HTTP. Em Windows, cscript entra como engine útil para JavaScript e VBScript; em Linux, Python e PHP são as rotas mais naturais por frequência e flexibilidade.

### Pontos-chave
- One-liners são valiosos em ambientes restritos e shells frágeis.
- A linguagem que já existe no host pode substituir ferramentas de transferência ausentes.
- O mesmo raciocínio serve para download e upload, especialmente com HTTP GET e POST.
- Versão da linguagem e bibliotecas disponíveis mudam a sintaxe e o comportamento real.

### O que vale fixar
Código não é só automação; em file transfer ele vira ferramenta improvisada quando o host já oferece o runtime certo.

---
## Seção 05 — Miscellaneous File Transfer Methods

### Resumo teórico
Esta seção amplia o repertório com canais alternativos que fogem do trio HTTP/SMB/FTP. Netcat e Ncat aparecem como utilitários extremamente flexíveis para enviar e receber arquivos por conexões TCP simples, enquanto PowerShell Remoting e RDP mostram que canais de administração remota também podem servir para movimentação de arquivos entre sistemas Windows.

O ponto mais importante é a direção da conexão. Dependendo do que o firewall permite, pode fazer mais sentido o alvo escutar e o atacante conectar, ou o inverso. Em ambientes Windows internos, WinRM/PowerShell Remoting e RDP via redirecionamento de recursos locais podem resolver o problema sem qualquer servidor web auxiliar.

### Pontos-chave
- Netcat/Ncat funcionam bem quando um lado pode escutar e o outro pode se conectar.
- Bash /dev/tcp continua sendo fallback útil na ausência de nc/ncat.
- WinRM permite copiar arquivo além de executar comandos remotos.
- RDP pode ser usado como canal de transferência manual por pasta ou drive redirecionado.

### O que vale fixar
Em métodos diversos, a escolha correta costuma depender menos da ferramenta e mais de quem pode abrir a conexão e com quais privilégios.

---
## Seção 06 — Protected File Transfers

### Resumo teórico
A seção muda o foco de “como transferir” para “como proteger o conteúdo transferido”. Quando o arquivo contém material sensível, como credenciais, dados de autenticação ou evidência de alto impacto, o módulo recomenda preferir canais cifrados como SSH, SFTP e HTTPS e, quando necessário, criptografar o próprio arquivo antes de movê-lo.

No Windows, o exemplo usa um script PowerShell para cifrar e decifrar strings e arquivos. No Linux, o OpenSSL cumpre o mesmo papel com algoritmos simétricos e senha forte. A seção também reforça minimização de dados: em muitos cenários, não é necessário remover do ambiente o dado mais sensível possível para demonstrar o risco.

### Pontos-chave
- Conteúdo sensível exige tratamento diferenciado antes, durante e depois da transferência.
- Criptografia do arquivo complementa, mas não substitui, o uso de canal seguro.
- Senha forte e única é parte essencial da proteção.
- Data minimization é postura profissional, não detalhe opcional.

### O que vale fixar
Transferência segura não é só uma questão de protocolo: é também uma questão de proteger o próprio arquivo e limitar o que realmente precisa sair do ambiente.

---
## Seção 07 — Catching Files over HTTP/S

### Resumo teórico
A seção volta ao uso de HTTP e HTTPS, agora focando em receber arquivos com segurança operacional melhor. Em vez de usar apenas servidores simples e efêmeros, o módulo mostra a montagem de um endpoint de upload mais controlado com Nginx, diretório dedicado, permissões ajustadas e método HTTP específico para escrita.

O aprendizado mais importante não é apenas o endpoint aceitar upload, mas fazê-lo sem exposição desnecessária. Isso inclui revisar conflitos de porta, checar logs, validar gravação real no diretório esperado e evitar que a pasta de upload fique navegável ou publicamente listável.

### Pontos-chave
- HTTP/S é um canal forte porque costuma passar bem por firewalls.
- Servidor de upload precisa de diretório dedicado e permissões corretas.
- Logs e conflitos de bind fazem parte do troubleshooting normal.
- Funcionar não basta: o endpoint de upload também precisa ser seguro.

### O que vale fixar
Quando você recebe arquivos por web, segurança do serviço e exposição do diretório importam tanto quanto a transferência em si.

---
## Seção 08 — Living off The Land

### Resumo teórico
A seção formaliza o conceito de Living off the Land, mostrando que muitos binários legítimos do próprio sistema podem ser reaproveitados para download, upload, leitura, escrita e outras funções úteis durante um assessment. Para Windows, o módulo referencia o projeto LOLBAS; para Linux, o GTFOBins.

Os exemplos com CertReq, OpenSSL, Bitsadmin e Certutil reforçam a lógica central: um binário administrativo ou comum pode servir como canal alternativo de file transfer quando ferramentas mais óbvias falham ou chamam atenção demais. Ao mesmo tempo, a seção lembra que disponibilidade não significa discrição automática, especialmente no caso de Certutil.

### Pontos-chave
- LOLBAS e GTFOBins funcionam como catálogos de contingência por capacidade.
- Binário legítimo pode ter uso secundário útil para transferência de arquivos.
- Diferenças de versão e parâmetro importam em ambiente real.
- Repertório de binários nativos amplia flexibilidade operacional.

### O que vale fixar
Living off the Land é menos sobre “trapaça criativa” e mais sobre saber explorar com rapidez o que o host já oferece de forma nativa.

---
## Seção 09 — Detection

### Resumo teórico
A seção fecha o lado ofensivo do módulo mostrando que métodos de transferência deixam rastros observáveis, principalmente em linha de comando, cabeçalhos HTTP e User-Agent. O módulo contrasta blacklist e whitelist de comandos e mostra como diferentes clientes HTTP do ecossistema Windows geram fingerprints distintos mesmo quando baixam exatamente o mesmo arquivo.

Invoke-WebRequest, WinHttpRequest, Msxml2.XMLHTTP, Certutil e BITS aparecem como exemplos de telemetria diferente no lado servidor. A mensagem é clara: ferramentas nativas também deixam assinaturas conhecidas e, para o blue team, baseline de User-Agent e correlação entre processo e rede são extremamente valiosos para hunting e triagem.

### Pontos-chave
- Detecção pode olhar linha de comando, cabeçalhos HTTP e padrão de cliente.
- User-Agent é útil, mas deve ser interpretado dentro do contexto do host e do processo.
- Ferramentas diferentes geram fingerprints diferentes, mesmo com o mesmo objetivo operacional.
- SIEM e baseline tornam sinais simples muito mais úteis para hunting.

### O que vale fixar
Método nativo não é método invisível: toda escolha de file transfer muda a telemetria que a defesa pode observar.

---
## Seção 10 — Evading Detection

### Resumo teórico
A seção final discute em alto nível por que sinais isolados de detecção, como User-Agent ou nome de binário permitido, podem ser frágeis quando usados sozinhos. O módulo mostra conceitualmente que alguns clientes permitem alterar a identificação HTTP e que binários living off the land podem passar por políticas muito simples baseadas apenas em allowlisting nominal.

Do ponto de vista defensivo, a conclusão mais importante é que detecção madura precisa correlacionar processo, rede, contexto, destino, escrita de arquivo e comportamento histórico do host. LOLBAS e GTFOBins, portanto, não servem apenas para contingência ofensiva: eles também ajudam a defesa a mapear superfícies de abuso e a construir cobertura comportamental melhor.

### Pontos-chave
- User-Agent é um sinal fraco quando analisado isoladamente.
- Binário legítimo não é sinônimo de atividade legítima.
- Allowlisting baseada só em nome de processo pode falhar.
- Correlação entre processo, rede e contexto vale mais que indicador único.

### O que vale fixar
O fechamento do módulo aponta para uma conclusão defensiva forte: sinais simples ajudam, mas contexto e correlação são o que realmente sustentam boa detecção.

---
## Fechamento do módulo
O módulo File Transfers fecha um ciclo operacional muito prático: começa mostrando por que transferir arquivos é um problema recorrente em assessment, percorre as principais rotas em Windows e Linux, expande o repertório com código, Netcat, RDP, WinRM e binários Living off the Land, e termina conectando tudo isso a proteção de dados, detecção e contexto defensivo.

### Leitura final para revisão rápida
- File transfer é capability operacional de suporte a enumeração, execução, coleta de evidência e privilege escalation.
- HTTP/S, SMB, FTP, SCP, RDP, WinRM, Netcat e Base64 são peças complementares, não concorrentes.
- Windows e Linux oferecem múltiplos caminhos nativos ou quase nativos para mover arquivos.
- Arquivos sensíveis pedem minimização de coleta, canal seguro e, quando necessário, criptografia do próprio conteúdo.
- LOLBAS e GTFOBins ampliam o repertório usando o que já existe no host.
- Toda escolha de método altera a telemetria observável e influencia a capacidade de detecção do ambiente.