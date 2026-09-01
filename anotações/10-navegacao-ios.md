# Navegação IOS

Resumo sobre a CLI do Cisco IOS, baseado no material da Cisco Networking Academy.

## Interface de linha de comando (CLI)

A CLI do Cisco IOS é um programa baseado em texto usado para configurar, monitorar e administrar dispositivos Cisco. Pode ser usada em gerenciamento em banda ou fora de banda.

Quase todos os dispositivos de rede Cisco usam uma CLI parecida, então quem conhece os comandos básicos consegue configurar tanto switch quanto roteador. Quando o dispositivo termina de inicializar, aparece o prompt `Router>`, indicando que já dá pra digitar comandos.

```
Router con0 is now available

Press RETURN to get started!

Router>enable
Router#configure terminal
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#hostname R1
R1(config)#interface gigabitethernet 0/0/0
R1(config-if)#
```

## Modos de comando primários

Por segurança, o IOS separa o acesso em dois modos:

**Modo EXEC do usuário**
Recursos limitados, só comandos básicos de monitoramento. Não permite alterar a configuração do dispositivo. Prompt termina com `>`.

**Modo EXEC privilegiado**
Acesso completo: qualquer comando de monitoramento, configuração e gerenciamento. Modos de configuração mais altos (como configuração global) só são acessados a partir daqui. Prompt termina com `#`.

| Modo | Descrição | Prompt |
| --- | --- | --- |
| EXEC do usuário | Comandos básicos, "somente visualização" | `Switch>` / `Router>` |
| EXEC privilegiado | Acesso total a comandos e recursos | `Switch#` / `Router#` |

## Estrutura básica de comandos

Um comando IOS segue o padrão: comando + palavras chave + argumentos.

**Palavra chave**: parâmetro predefinido pelo sistema (ex: `ip protocols`).
**Argumento**: valor definido pelo usuário (ex: `192.168.10.5`).

Depois de digitar o comando completo, pressiona `Enter` pra enviar ao interpretador.

### Convenções de sintaxe

| Convenção | Significado |
| --- | --- |
| **negrito** | Comando/palavra chave a ser digitada literalmente |
| *itálico* | Argumento cujo valor você fornece |
| `[x]` | Elemento opcional |
| `{x}` | Elemento obrigatório |
| `[x{y\|z}]` | Escolha obrigatória dentro de um elemento opcional |

Exemplos:
- `description` *string*: descreve a finalidade de uma interface.
- `ping` *ip address*: ex. `ping 10.10.10.5`.
- `traceroute` *ip address*: ex. `traceroute 192.168.254.254`.

Comandos complexos, com vários argumentos, aparecem assim:

```
Switch(config-if)# switchport port-security aging { static | time time | type {absolute | inactivity}}
```

A Referência de Comandos do Cisco IOS é a fonte oficial de detalhes de cada comando.

## Teclas de atalho e atalhos

Comandos e palavras chave podem ser abreviados até o mínimo de caracteres que ainda identifica uma opção única. Ex: `configure` pode virar `conf`, mas `con` não funciona porque mais de um comando começa com `con`.

### Edição da linha de comando

| Tecla | Função |
| --- | --- |
| Tab | Completa um comando parcial |
| Backspace | Apaga caractere à esquerda do cursor |
| Ctrl+D | Apaga caractere no cursor |
| Ctrl+K | Apaga do cursor até o fim da linha |
| Esc D | Apaga do cursor até o fim da palavra |
| Ctrl+U / Ctrl+X | Apaga do cursor até o início da linha |
| Ctrl+W | Apaga a palavra à esquerda do cursor |
| Ctrl+A | Move cursor pro início da linha |
| Seta esquerda / Ctrl+B | Move cursor um caractere pra esquerda |
| Esc B | Move cursor uma palavra pra esquerda |
| Esc F | Move cursor uma palavra pra direita |
| Seta direita / Ctrl+F | Move cursor um caractere pra direita |
| Ctrl+E | Move cursor pro fim da linha |
| Seta cima / Ctrl+P | Volta pro comando anterior no histórico |
| Seta baixo / Ctrl+N | Avança pro próximo comando no histórico |
| Ctrl+R / Ctrl+I / Ctrl+L | Reexibe o prompt e a linha atual |

A tecla `Delete` não é reconhecida pelo IOS.

### Prompt "More"

Quando a saída não cabe na tela, aparece `More`:

| Tecla | Função |
| --- | --- |
| Enter | Exibe a próxima linha |
| Espaço | Exibe a próxima tela |
| Qualquer outra tecla (exceto "y") | Encerra a exibição e volta ao prompt |

### Saindo de uma operação

| Tecla | Função |
| --- | --- |
| Ctrl+C | Sai do modo de configuração, volta pro EXEC privilegiado |
| Ctrl+Z | Sai do modo de configuração, volta pro EXEC privilegiado |
| Ctrl+Shift+6 | Aborta DNS, traceroute, ping ou interrompe um processo do IOS |

## Comandos show

Exibem informações sobre configuração e operação do dispositivo. Muito usados por técnicos pra checar status de interfaces, processos e configuração.

| Comando | Usado para |
| --- | --- |
| `show running config` | Ver as configurações atuais |
| `show interfaces` | Ver status da interface e mensagens de erro |
| `show ip interface` | Ver informações de camada 3 de uma interface |
| `show arp` | Ver hosts conhecidos na LAN Ethernet local |
| `show ip route` | Ver informações de roteamento de camada 3 |
| `show protocols` | Ver quais protocolos estão operacionais |
| `show version` | Ver memória, interfaces e licenças do dispositivo |

### show running config

```
R1#show running-config

(Saída omitida)

!
versão 15.5
service timestamps debug datetime msec
service timestamps log datetime msec
service password-encryption
!
hostname R1
!
interface GigabitEthernet0/0/0
 description Link para R2
 ip address 209.165.200.225 255.255.255.252
 negotiation auto
!
interface GigabitEthernet0/0/1
 description Link para a LAN
 ip address 192.168.10.1 255.255.255.0
 negotiation auto
!
router ospf 10
 network 192.168.10.0 0.0.0.255 area 0
 network 209.165.200.224 0.0.0.3 area 0
!
banner motd ^C Apenas acesso autorizado! ^C
!
line con 0
 password 7 14141B180F0B
 login
line vty 0 4
 password 7 00071A150754
 login
 transport input telnet ssh
!
end
R1#
```

### show interfaces

```
R1#show interfaces
GigabitEthernet0/0/0 is up, line protocol is up
  Hardware is ISR4321-2x1GE, address is a0e0.af0d.e140 (bia a0e0.af0d.e140)
  Description: Link para R2
  O endereço da Internet é 209.165.200.225/30
  MTU 1500 bytes, BW 100000 Kbit/sec, DLY 100 usec,
    reliability 255/255, txload 1/255, rxload 1/255
  Encapsulation ARPA, loopback not set
  Keepalive not supported
  Full Duplex, 100Mbps, link type is auto, media type is RJ45
  output flow control is off, input flow control is off
  ARP type: ARPA, ARP Timeout 04:00:00
  Last input 00:00:01, output 00:00:21, output hang never
  Last clearing of "show interface" counters never
  Input queue: 0/375/0/0 (size/max/drops/flushes); Total output drops: 0
  Queueing strategy: fifo
  Output queue: 0/40 (size/max)
  5 minute input rate 0 bits/sec, 0 packets/sec
  5 minute output rate 0 bits/sec, 0 packets/sec
    5127 packets input, 590285 bytes, 0 no buffer
    Received 29 broadcasts (0 IP multicasts)
    0 runts, 0 giants, 0 throttles
    0 input errors, 0 CRC, 0 frame, 0 overrun, 0 ignored
    watchdog, 5043 multicast, 0 pausa de entrada
    0 output errors, 0 collisions, 2 interface resets
    0 unknown protocol drops
    0 babbles, 0 late collision, 0 deferred
    1 lost carrier, 0 no carrier, 0 pause output
    0 output buffer failures, 0 output buffers swapped out
```

### show ip interface

```
R1#show ip interface
GigabitEthernet0/0/0 is up, line protocol is up
  Internet address is 209.165.200.225/30
  Broadcast address is 255.255.255.255
  Address determined by setup command
  MTU is 1500 bytes
  Helper address is not set
  Directed broadcast forwarding is disabled
  Multicast reserved groups joined: 224.0.0.5 224.0.0.6
  Outgoing Common access list is not set
  Outgoing access list is not set
  Inbound Common access list is not set
  Inbound access list is not set
  Proxy ARP is enabled
  Local Proxy ARP is disabled
  Security level is default
  Split horizon is enabled
  ICMP redirects are always sent
  ICMP unreachables are always sent
  ICMP mask replies are never sent
  IP fast switching is enabled
  IP Flow switching is disabled
  IP CEF switching is enabled
  IP CEF switching turbo vector
  IP Null turbo vector
  Associated unicast routing topologies:
        Topology "base", operation state is UP
  IP multicast fast switching is enabled
  IP multicast distributed fast switching is disabled
  IP route cache flags are Fast, CEF
  Router Discovery is disabled
  IP output packet accounting is disabled
  IP access violation accounting is disabled
  TCP/IP header compression is disabled
  RTP/IP header compression is disabled
  Probe proxy name replies are disabled
  Policy routing is disabled
  Network address translation is disabled
  BGP Policy Mapping is disabled
  Input features: MCI Check
  IPv4 WCCP Redirect outbound is disabled
  IPv4 WCCP Redirect inbound is disabled
  IPv4 WCCP Redirect exclude is disabled

(Saída omitida)
```

### show arp

```
R1#show arp
Protocol   Address          Age (min) Hardware Addr Type Interface
Internet   192.168.10.1      - a0e0.af0d.e141 ARPA GigabitEthernet0/0/1
Internet   192.168.10.10     95 c07b.bcc4.a9c0 ARPA GigabitEthernet0/0/1
Internet   209.165.200.225   - a0e0.af0d.e140 ARPA GigabitEthernet0/0/0
Internet   209.165.200.226   138 a03d.6fe1.9d90 ARPA GigabitEthernet0/0/0
R1#
```

### show ip route

```
R1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
   D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
   N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
   E1 - OSPF external type 1, E2 - OSPF external type 2
   i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
   ia - IS-IS inter area, * - candidate default, U - per-user static route
   o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
   a - application route
   + - replicated route, % - next hop override, p - overrides from PfR
O gateway de último recurso é 209.165.200.226 para a rede 0.0.0.0
O*E2 0.0.0.0/0 [110/1] via 209.165.200.226, 02:19:50, Gigabitethernet0/0/0
   10.0.0.0/24 is subnetted, 1 subnets
O   10.1.1.0 [110/3] via 209.165.200.226, 02:05:42, GigabitEthernet0/0/0
   192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C   192.168.10.0/24 is directly connected, GigabitEthernet0/0/1
L   192.168.10.1/32 is directly connected, GigabitEthernet0/0/1
   209.165.200.0/24 is variably subnetted, 3 subnets, 2 masks
C   209.165.200.224/30 is directly connected, GigabitEthernet0/0/0
L   209.165.200.225/32 is directly connected, GigabitEthernet0/0/0
O   209.165.200.228/30 [110/2] via 209.165.200.226, 02:07:19, GigabitEthernet0/0/0
R1#
```

### show protocols

```
R1#show protocols
Valores globais:
  Internet Protocol routing is enabled
GigabitEthernet0/0/0 is up, line protocol is up
  Internet address is 209.165.200.225/30
GigabitEthernet0/0/1 is up, line protocol is up
  Internet address is 192.168.10.1/24
Serial0/1/0 is down, line protocol is down
Serial0/1/1 is down, line protocol is down
GigabitEthernet0 is administratively down, line protocol is down
R1#
```

### show version

```
R1#show version
Software Cisco IOS XE, Versão 03.16.08.S - Versão de Suporte Extended
Cisco IOS Software, ISR Software (X86_64_LINUX_IOSD-UNIVERSALK9-M), Version 15.5(3)S8, RELEASE SOFTWARE
(fc2)
Technical Support: http://www.cisco.com/techsupport
Copyright (c) 1986-2018 by Cisco Systems, Inc.
Compilado Qua 08/08/18 10:48 por mcpre

(Saída omitida)

ROM: IOS-XE ROMMON
R1 uptime is 2 hours, 25 minutes
Uptime for this control processor is 2 hours, 27 minutes
System returned to ROM by reload
System image file is "bootflash:/isr4300-universalk9.03.16.08.S.155-3.S8-ext.SPA.bin"
Last reload reason: LocalSoft

(Saída omitida)

Informações sobre a licença do pacote de tecnologia:
-----------------------------------------------------------------
Technology     Technology-package     Technology-package
               Current      Type          Next reboot
------------------------------------------------------------------
appxk9         appxk9       RightToUse    appxk9
uck9           None         None          None
securityk9     securityk9   Permanent     securityk9
ipbase         ipbasek9     Permanent     ipbasek9
cisco ISR4321/K9 (1RU) processor with 1647778K/6147K bytes of memory.
Processor board ID FLM2044W0LT
2 Gigabit Ethernet interfaces
2 Serial interfaces
32768K bytes de memória de configuração não volátil.
4194304K bytes de memória física.
3207167K bytes de memória flash no flash de inicialização:.
978928K bytes de flash USB at usb0 :.
Configuration register is 0x2102
R1#
```
