# Resolução de Endereços

Resumo sobre ARP (Address Resolution Protocol) em redes IPv4, baseado no material da Cisco Networking Academy.

## Visão geral

Se a rede usa IPv4, o ARP é o protocolo que mapeia endereços IPv4 para endereços MAC. Todo quadro Ethernet Layer 2 carrega dois endereços:

- **MAC de destino** - o do dispositivo de destino na mesma rede local. Se o destino estiver em outra rede, o MAC usado é o do gateway padrão (roteador).
- **MAC de origem** - o da NIC Ethernet do host que envia.

Pra mandar um pacote a outro host na mesma rede IPv4, o dispositivo precisa saber o IPv4 e o MAC do destino. O IPv4 já é conhecido ou resolvido pelo nome; o MAC precisa ser descoberto é aí que entra o ARP.

**Duas funções principais do ARP:**
- Resolver endereços IPv4 em endereços MAC
- Manter uma tabela de mapeamentos IPv4 → MAC

## Tabela ARP (cache ARP)

Quando um pacote vai ser encapsulado num quadro Ethernet, o dispositivo consulta uma tabela guardada na RAM, chamada tabela ARP ou cache ARP, procurando o MAC correspondente ao IPv4.

- Se o IPv4 de destino está na mesma rede do IPv4 de origem, o dispositivo procura o IPv4 de destino na tabela ARP.
- Se está em rede diferente, procura o IPv4 do **gateway padrão** na tabela ARP.

Em ambos os casos, a busca é por um IPv4 e o MAC correspondente. Cada entrada da tabela vincula um IPv4 a um MAC (um "mapa"). Se o IPv4 é encontrado, o MAC correspondente vira o MAC de destino do quadro. Se não é encontrado, o dispositivo dispara uma requisição ARP.

## Requisição ARP

Enviada quando o dispositivo precisa do MAC de um IPv4 que não tem na tabela. É encapsulada direto num quadro Ethernet (sem cabeçalho IPv4):

- **MAC de destino**: broadcast `FF-FF-FF-FF-FF-FF` - todas as NICs da LAN aceitam e processam.
- **MAC de origem**: MAC de quem envia a requisição.
- **Tipo**: `0x806` - indica à NIC que recebe que os dados devem ir pro processo ARP.

Como é broadcast, o switch inunda a requisição em todas as portas (menos a de origem). Todo dispositivo da LAN recebe e processa pra ver se o IPv4 de destino bate com o dele. Roteadores não encaminham esse broadcast pra outras interfaces. Só o dispositivo dono do IPv4 responde os outros ignoram.

## Resposta ARP

Só o dispositivo com o IPv4 de destino responde, em unicast:

- **MAC de destino**: MAC de quem fez a requisição.
- **MAC de origem**: MAC de quem está respondendo.
- **Tipo**: `0x806`.

Só quem enviou a requisição recebe a resposta. Ao receber, grava o par IPv4/MAC na própria tabela ARP daí em diante, pacotes pra esse IPv4 já saem encapsulados com o MAC certo.

Se ninguém responder, o pacote é descartado (não dá pra montar o quadro).

As entradas da tabela têm timestamp e expiram se o dispositivo não receber nenhum quadro daquele IPv4/MAC antes do tempo acabar. Também é possível inserir entradas estáticas manualmente essas não expiram sozinhas, só removendo na mão.

> **IPv6**: não usa ARP. Usa um processo parecido chamado **ICMPv6 Neighbour Discovery (ND)**, com mensagens de requisição e anúncio de vizinho equivalentes às requisições e respostas ARP do IPv4.

## ARP em comunicações remotas (outra rede)

Quando o IPv4 de destino não está na mesma rede do IPv4 de origem, o dispositivo de origem manda o quadro pro **gateway padrão** (interface do roteador local), usando o MAC do gateway como MAC de destino.

O IPv4 do gateway padrão fica salvo na configuração IPv4 do host. Ao montar um pacote, o host compara o IPv4 de destino com o próprio pra saber se estão na mesma rede de Camada 3. Se não estiverem, ele busca o MAC do gateway na tabela ARP e se não tiver entrada, dispara o processo ARP normalmente.

## Remoção de entradas da tabela ARP

Um temporizador de cache ARP remove entradas não usadas depois de um tempo, que varia por sistema operacional. No Windows mais recente, entradas ficam entre 15 e 45 segundos.

Também dá pra remover manualmente (via comando), parcial ou totalmente. Depois de removida, o processo de requisição/resposta ARP precisa rodar de novo pra reinserir o mapa.

## Tabelas ARP nos dispositivos

**Roteador Cisco** — comando `show ip arp`:

```
R1# show ip arp
Protocol   Address           Age (min)  Hardware Addr   Type  Interface
Internet   192.168.10.1            -    a0e0.af0d.e140  ARPA  GigabitEthernet0/0/0
Internet   209.165.200.225         -    a0e0.af0d.e141  ARPA  GigabitEthernet0/0/1
Internet   209.165.200.226         1    a03d.6fe1.9d91  ARPA  GigabitEthernet0/0/1
```

**PC com Windows** - comando `arp -a`:

```
C:\Users\PC> arp -a
Interface: 192.168.1.124 --- 0x10
Internet Address     Physical Address     Type
192.168.1.1          c8-d7-19-cc-a0-86    dynamic
192.168.1.101        08-3e-0c-f5-f7-77    dynamic
192.168.1.110        08-3e-0c-f5-f7-56    dynamic
192.168.1.112        ac-b3-13-4a-bd-d0    dynamic
192.168.1.117        08-3e-0c-f5-f7-5c    dynamic
192.168.1.126        24-77-03-45-5d-c4    dynamic
192.168.1.146        94-57-a5-0c-5b-02    dynamic
192.168.1.255        ff-ff-ff-ff-ff-ff    static
224.0.0.22           01-00-5e-00-00-16    static
224.0.0.251           01-00-5e-00-00-fb    static
239.255.255.250      01-00-5e-7f-ff-fa    static
255.255.255.255      ff-ff-ff-ff-ff-ff    static
```

## Problemas de ARP - broadcast e falsificação (spoofing)

Requisições ARP são broadcast, então todo dispositivo da rede local processa. Numa rede corporativa normal o impacto é mínimo, mas se muitos dispositivos ligarem ao mesmo tempo e começarem a acessar a rede junto, pode haver queda temporária de performance até os MACs necessários serem descobertos.

**Risco de segurança**: um agente de ameaça pode usar **falsificação de ARP** pra fazer um **ataque de envenenamento ARP (ARP poisoning)**. Ele responde a uma requisição ARP se passando por outro dispositivo (ex: o gateway padrão), enviando seu próprio MAC. A vítima grava esse MAC errado na tabela ARP e passa a mandar os pacotes pro atacante.

**Mitigação**: switches de nível corporativo usam **inspeção dinâmica de ARP (DAI - Dynamic ARP Inspection)**.
