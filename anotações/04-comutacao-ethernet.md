# Comutação Ethernet

## Ethernet: padrão de fato

Antes dos padrões, equipamentos de fornecedores diferentes muitas vezes não se comunicavam entre si. Com a expansão das redes, surgiram padrões que facilitaram design, produção, treinamento e compatibilidade entre fabricantes. Nesse cenário, a Ethernet se tornou a tecnologia dominante em redes locais com fio, operando nas camadas 1 e 2 do modelo OSI.

O IEEE (802.3) mantém os padrões Ethernet, que evoluíram desde 1973 de 10 Mbps até 100 Gbps ou mais. A notação de cada versão (ex: `100BASE-T`) indica velocidade, tipo de transmissão (banda base) e tipo de cabo.

## Subcamadas e encapsulamento

A camada de enlace se divide em duas subcamadas:

- **LLC**: identifica qual protocolo de camada 3 (IPv4, IPv6) está sendo usado, permitindo que vários protocolos compartilhem a mesma interface.
- **MAC**: cuida do encapsulamento de dados e do controle de acesso ao meio (endereçamento e detecção de erros via FCS).

Redes antigas com hub eram half-duplex e usavam CSMA/CD para evitar colisões. Switches modernos operam em full-duplex, dispensando esse controle de acesso.

## Quadro Ethernet

Tamanho: mínimo 64 bytes, máximo 1518 bytes (sem contar o preâmbulo). Quadros menores são descartados como "fragmentos de colisão"; acima de 1500 bytes de dados são chamados "jumbo".

| Campo | Tamanho | Função |
|---|---|---|
| Preâmbulo + SFD | 8 bytes | Sincronização entre emissor e receptor |
| MAC de destino | 6 bytes | Identifica o receptor pretendido |
| MAC de origem | 6 bytes | Identifica a interface de origem |
| Tipo/Comprimento | 2 bytes | Indica o protocolo de camada superior (ex: `0x86DD` = IPv6) |
| Dados | 46–1500 bytes | Payload, com padding se necessário |
| FCS | 4 bytes | Usa CRC para detectar erros |

## Endereços MAC e hexadecimal

MAC address é um valor de 48 bits, representado em hexadecimal (12 dígitos) porque cada dígito hex equivale a 4 bits binários.

Três tipos:

- **Unicast**: um para um. Usa ARP (IPv4) ou ND (IPv6) para descobrir o MAC de destino.
- **Broadcast**: MAC `FF-FF-FF-FF-FF-FF`, inundado em todas as portas do switch (exceto a de entrada), não passa por roteador. Ex: DHCP, ARP.
- **Multicast**: endereço para um grupo específico (`01-00-5E` para IPv4, `33-33` para IPv6). Também inundado, a menos que haja controle específico (multicast snooping).

## Tabela de endereços MAC (switch)

O switch decide o encaminhamento só com base em MACs de camada 2, ignorando o protocolo superior.

**Aprendizado**: ao receber um quadro, o switch registra o MAC de origem e a porta de entrada na tabela (entradas expiram por padrão em 5 minutos).

**Encaminhamento**:

- Se o MAC de destino está na tabela → envia só para aquela porta (filtragem)
- Se não está → inunda todas as portas exceto a de entrada (unicast desconhecido)
- Broadcast/multicast sempre inundam

Um switch pode ter vários MACs associados à mesma porta (comum quando conectado a outro switch).

Quando o destino está em outra rede, o quadro Ethernet é enviado ao MAC do gateway padrão (roteador), não diretamente ao host remoto — o IP de destino permanece o do host remoto, mas o MAC de destino é o do gateway.
