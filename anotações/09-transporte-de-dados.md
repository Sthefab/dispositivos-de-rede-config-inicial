# Transporte de Dados

Resumo sobre a Camada de Transporte (TCP e UDP), baseado no material da Cisco Networking Academy.

## Propósito da camada de transporte

Responsável pela comunicação lógica entre aplicativos rodando em hosts diferentes: é o link entre a camada de aplicação e as camadas de baixo, que cuidam da transmissão física pela rede.

Ela não sabe (e não precisa saber) o tipo de host de destino, a mídia usada, o caminho percorrido pelos dados ou o congestionamento da rede.

**Dois protocolos principais:**
- TCP (Transmission Control Protocol)
- UDP (User Datagram Protocol)

## Responsabilidades da camada de transporte

- **Rastreamento de conversações individuais**: cada fluxo de dados entre app de origem e destino é uma "conversa", rastreada separadamente. Um host pode ter várias conversas simultâneas.
- **Segmentação e remontagem**: divide os dados do app em blocos menores (segmentos no TCP, datagramas no UDP), mais fáceis de transportar. O receptor remonta na ordem certa.
- **Adição de cabeçalho**: cada bloco recebe um cabeçalho com campos que permitem gerenciar a comunicação e remontar o fluxo original pro app certo.
- **Identificação das aplicações**: usa **número de porta** pra saber qual app de destino deve receber os dados. Cada processo que acessa a rede recebe uma porta exclusiva no host.
- **Multiplexação das conversas**: permite que várias conversas usem a mesma rede ao mesmo tempo (segmentação + intercalação), evitando que uma única transmissão (ex: streaming) tome toda a banda.

## TCP x UDP: o que cada um resolve

O IP só cuida de estrutura, endereçamento e roteamento, e não garante entrega. Isso é papel da camada de transporte, que oferece dois protocolos com níveis de confiabilidade diferentes, dependendo da necessidade da aplicação.

---

## TCP (Transmission Control Protocol)

### Características

Protocolo **confiável**, **orientado a conexão**, que garante que todos os dados cheguem ao destino, na ordem certa.

**O que o TCP faz:**
- Numera e rastreia segmentos enviados
- Confirma dados recebidos
- Retransmite dados não confirmados
- Reordena dados que chegam fora de sequência
- Controla a taxa de envio (controle de fluxo)

Pra manter o estado da conversa, o TCP **estabelece uma conexão** antes de trocar dados, por isso é chamado "orientado a conexão" (*connection-oriented*) e "stateful".

### Cabeçalho TCP

Adiciona 20 bytes (160 bits) de overhead. Dez campos principais:

| Campo | Descrição |
| --- | --- |
| Porta de origem | 16 bits, identifica o app de origem |
| Porta de destino | 16 bits, identifica o app de destino |
| Número de sequência | 32 bits, remontagem dos dados |
| Número de confirmação | 32 bits, próximo byte esperado |
| Tamanho do cabeçalho | 4 bits, comprimento do cabeçalho ("data offset") |
| Reservado | 6 bits, uso futuro |
| Bits de controle | 6 bits, flags que indicam função do segmento |
| Tamanho da janela | 16 bits, quantos bytes podem ser aceitos de uma vez |
| Checksum | 16 bits, verificação de erro |
| Urgente | indica se os dados são urgentes |

---

## UDP (User Datagram Protocol)

### Características

Protocolo **mais simples**, **sem conexão** e **sem estado** (*stateless*). Não garante entrega nem controla fluxo: é "melhor esforço" (*best-effort*), sem confirmação de recebimento.

**O que o UDP NÃO faz (por design):**
- Não reordena dados fora de sequência (entrega na ordem que chegou)
- Não reenvia segmentos perdidos
- Não estabelece sessão
- Não avisa o remetente se o destino está disponível

Justamente por não gerenciar nada disso, é mais leve e mais rápido de processar que o TCP.

### Cabeçalho UDP

Só 4 campos, 8 bytes (64 bits), bem mais enxuto que o do TCP:

| Campo | Descrição |
| --- | --- |
| Porta de origem | 16 bits |
| Porta de destino | 16 bits |
| Comprimento | 16 bits, tamanho do datagrama |
| Checksum | 16 bits, verificação de erro |

---

## Quando usar TCP vs UDP

**UDP é melhor quando:**
- Atraso é inaceitável, mas perda de dados é tolerável (ex: VoIP, vídeo/voz ao vivo)
- Transações simples de solicitação e resposta (ex: DNS, DHCP)
- A confiabilidade pode ser tratada pela própria aplicação, ou simplesmente não é necessária (ex: SNMP, TFTP)

**TCP é melhor quando:**
- Todos os dados precisam chegar, íntegros e na ordem certa (ex: bancos de dados, navegadores, e mail, internet banking)
- Vídeo ou áudio armazenado (streaming sob demanda), usando buffer, sondagem de banda e controle de congestionamento

DNS e SNMP usam UDP por padrão, mas podem cair pra TCP em certos casos (ex: DNS usa TCP se a resposta passar de 512 bytes, ou na comunicação entre servidores DNS).

---

## Números de porta

TCP e UDP usam números de porta pra gerenciar várias conversas ao mesmo tempo. Intervalo total: **0 a 65535**.

- **Porta de origem**: gerada dinamicamente pelo host que inicia a conversa.
- **Porta de destino**: identifica o serviço solicitado (ex: porta 80 = HTTP).

### Grupos de portas (IANA)

| Grupo | Intervalo | Descrição |
| --- | --- | --- |
| Bem conhecidas | 0 a 1023 | Reservadas pra serviços comuns (web, e mail, etc.) |
| Registradas | 1024 a 49151 | Atribuídas pela IANA a apps específicos (ex: RADIUS na 1812) |
| Privadas/dinâmicas | 49152 a 65535 | Portas efêmeras, atribuídas dinamicamente pelo SO do cliente |

### Portas conhecidas comuns

| Porta | Protocolo | Aplicação |
| --- | --- | --- |
| 20/21 | TCP | FTP (dados/controle) |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | UDP/TCP | DNS |
| 67/68 | UDP | DHCP (servidor/cliente) |
| 69 | UDP | TFTP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 143 | TCP | IMAP |
| 161 | UDP | SNMP |
| 443 | TCP | HTTPS |

### Pares de sockets

**Socket** é IP + número de porta (ex: `192.168.1.5:1099`). Um **par de sockets** identifica uma conversa única entre cliente e servidor (ex: `192.168.1.5:1099, 192.168.1.7:80`). Permite que vários processos no mesmo host, e várias conexões ao mesmo processo no servidor, sejam diferenciados entre si.

### Comando netstat

Mostra conexões TCP ativas no host: protocolo, endereço local, endereço remoto e estado da conexão. Útil pra detectar conexões suspeitas.

```
C:\>netstat

Active Connections

  Proto    Local Address          Foreign Address           State
  TCP      192.168.1.124:3126     192.168.0.2:netbios-ssn   ESTABLISHED
  TCP      192.168.1.124:3158     207.138.126.152:http      ESTABLISHED
```

A opção `-n` mostra os endereços e portas em forma numérica, sem resolver nomes.

---

## Processo de comunicação TCP

Um servidor não pode ter dois serviços na mesma porta ao mesmo tempo. Uma porta com app ativo é considerada **aberta**.

### Handshake de três vias (estabelecimento de conexão)

1. **SYN**: cliente requisita sessão cliente para servidor.
2. **SYN, ACK**: servidor confirma e requisita sessão servidor para cliente.
3. **ACK**: cliente confirma a sessão servidor para cliente.

Isso valida que: o destino está na rede, tem o serviço ativo na porta certa e sabe que o cliente quer abrir sessão nessa porta.

### Encerramento da sessão

Sessão TCP é full duplex (duas sessões unidirecionais), então fechar as duas exige 4 trocas:

1. **FIN**: cliente não tem mais dados, sinaliza fim.
2. **ACK**: servidor confirma recebimento do FIN.
3. **FIN**: servidor sinaliza fim da sessão dele pro cliente.
4. **ACK**: cliente confirma.

### Bits de controle (flags) do cabeçalho TCP

- **URG**: ponteiro urgente significativo
- **ACK**: confirmação (usado no handshake e no encerramento)
- **PSH**: função push
- **RST**: reseta conexão em erro ou timeout
- **SYN**: sincroniza número de sequência (estabelecimento de conexão)
- **FIN**: não há mais dados a enviar (encerramento de sessão)

---

## Confiabilidade e controle de fluxo (TCP)

### Números de sequência e remontagem

Cada segmento carrega um número de sequência (representa o primeiro byte de dados daquele segmento). O ISN (número de sequência inicial) é definido no handshake e não começa em 1, é praticamente aleatório, por segurança.

Segmentos que chegam fora de ordem ficam retidos no buffer até os que faltam chegarem, aí tudo é remontado na ordem certa.

### Perda e retransmissão

- Método antigo: ACK confirma só o próximo byte esperado. Se faltarem vários segmentos no meio, tudo depois é reenviado (mesmo o que já chegou), gerando duplicação e ineficiência.
- Método atual: **SACK (Selective Acknowledgment)**, negociado no handshake. O receptor confirma exatamente quais blocos recebeu, e a origem reenvia só o que realmente faltou.

TCP usa temporizadores pra saber quando reenviar um segmento não confirmado.

### Controle de fluxo, janela e ACKs

O campo **tamanho da janela** (16 bits) define quantos bytes o destino pode receber e processar de uma vez, sem esperar confirmação.

- Definido inicialmente no handshake de três vias.
- Origem não pode mandar mais que o tamanho da janela sem receber ACK.
- Destino não precisa esperar a janela inteira chegar pra confirmar. Ele vai confirmando conforme processa, e a janela de envio da origem "desliza" (**sliding window**) conforme os ACKs chegam.
- Se o buffer do destino fica cheio, ele reduz o tamanho da janela pra pedir que a origem desacelere.

### Tamanho máximo do segmento (MSS)

Maior quantidade de dados (em bytes) que cabe em um único segmento TCP, sem contar o cabeçalho. Negociado no handshake.

MSS comum em IPv4: **1460 bytes**, calculado como MTU padrão da Ethernet (1500 bytes) menos cabeçalho IPv4 (20 bytes) e cabeçalho TCP (20 bytes).

### Prevenção de congestionamento

Se a origem percebe que segmentos não estão sendo confirmados (ou demoram demais), assume congestionamento e **reduz** o número de bytes enviados sem confirmação. Isso parte da origem, não é o mesmo mecanismo do tamanho de janela definido pelo destino.

---

## Comunicação UDP

- Não estabelece conexão: cabeçalho pequeno, sem tráfego de gerenciamento.
- Não reordena datagramas: entrega na ordem que chegou. Se a ordem importa, é a aplicação que precisa tratar isso.
- Apps servidoras UDP também usam portas bem conhecidas ou registradas: o UDP encaminha o datagrama pro app certo com base na porta.
- Cliente escolhe porta de origem dinamicamente; porta de destino é a porta conhecida ou registrada do serviço.
- Na resposta do servidor, as portas de origem e destino são invertidas em relação à solicitação original.
