# Camada de rede

## O que é

A camada de rede (Camada 3 do modelo OSI) permite que dispositivos finais troquem dados entre redes diferentes. Os principais protocolos são o IPv4 e o IPv6, além de protocolos de roteamento (OSPF) e de mensagens (ICMP).

## Quatro operações básicas

- **Endereçamento de dispositivos finais**: cada host precisa de um endereço IP único.
- **Encapsulamento**: a camada de rede embrulha a PDU da camada de transporte em um pacote, adicionando o cabeçalho IP com os endereços de origem e destino.
- **Roteamento**: roteadores escolhem o melhor caminho para levar o pacote até o destino. Cada roteador atravessado é um salto.
- **Desencapsulamento**: quando o pacote chega ao destino, o cabeçalho IP é removido e os dados seguem para a camada de transporte.

## Encapsulamento IP

O IP encapsula o segmento vindo da camada de transporte adicionando um cabeçalho IP, formando o pacote. Esse cabeçalho é examinado por roteadores ao longo do caminho, mas os dados da camada de transporte não mudam durante o trajeto (exceto quando há NAT no IPv4).

## Características do IP

- **Sem conexão**: não existe uma conexão dedicada criada antes do envio dos dados, como enviar uma carta sem avisar o destinatário.
- **Melhor esforço**: o IP não garante entrega. É um protocolo não confiável por natureza, sem confirmação de recebimento.
- **Independente da mídia**: funciona da mesma forma independente do meio físico (cobre, fibra, sem fio).

Por causa disso, o IP tem baixa sobrecarga: quem cuida do controle de fluxo e confiabilidade é o TCP, na camada de transporte.

## MTU e fragmentação

Cada meio físico tem um tamanho máximo de PDU que consegue transportar, a MTU. Se um roteador precisa encaminhar um pacote IPv4 para um meio com MTU menor, ele pode fragmentar o pacote, o que gera latência. Pacotes IPv6 não são fragmentados por roteadores.

## Pacote IPv4

Cabeçalho de tamanho variável (20 a 60 bytes com opções). Campos principais:

- **Versão**: 4 bits, valor 0100 (identifica IPv4).
- **DiffServ (DS)**: 8 bits, define prioridade do pacote.
- **TTL**: 8 bits, limita a vida útil do pacote. Decrementado a cada roteador; ao chegar a zero, o pacote é descartado e um ICMP de tempo excedido é enviado à origem.
- **Protocolo**: 8 bits, indica o protocolo de camada superior (ICMP = 1, TCP = 6, UDP = 17).
- **Checksum de cabeçalho**: detecta corrupção no cabeçalho.
- **Endereço IP origem**: 32 bits, sempre unicast.
- **Endereço IP destino**: 32 bits, pode ser unicast, multicast ou broadcast.

Outros campos (IHL, Tamanho Total, Identificação, Flags, Deslocamento do Fragmento, Opções, Preenchimento) servem para validação e fragmentação do pacote.

## Limitações do IPv4

- **Esgotamento de endereços**: cerca de 4 bilhões de endereços disponíveis, insuficiente para a demanda atual.
- **Falta de conectividade ponta a ponta**: o NAT permite compartilhar um único IP público entre vários dispositivos, mas esconde o endereço interno, o que atrapalha tecnologias que precisam de conexão direta.
- **Complexidade adicional**: o NAT era pensado como solução de transição, mas aumenta a complexidade da rede e gera latência.

## IPv6

Criado pela IETF nos anos 90 para resolver essas limitações. Principais vantagens:

- **Espaço de endereços muito maior**: 128 bits contra 32 bits do IPv4, o que dá cerca de 340 undecilhões de endereços.
- **Cabeçalho simplificado**: menos campos, tamanho fixo de 40 bytes, processamento mais eficiente.
- **Elimina a necessidade de NAT**: com tantos endereços públicos disponíveis, não é mais necessário traduzir endereços privados em públicos.

### Campos do cabeçalho IPv6

- **Versão**: 4 bits, valor 0110.
- **Classe de tráfego**: equivalente ao DS do IPv4.
- **Etiqueta de fluxo**: 20 bits, indica que pacotes do mesmo fluxo recebem o mesmo tratamento pelos roteadores.
- **Comprimento da carga útil**: 16 bits, tamanho dos dados sem contar o cabeçalho.
- **Próximo cabeçalho**: equivalente ao campo Protocolo do IPv4.
- **Limite de salto**: substitui o TTL. Decrementado a cada roteador; ao chegar a zero, descarta o pacote e envia ICMPv6 de tempo excedido.
- **Endereço IPv6 de origem e destino**: 128 bits cada.

O IPv6 não tem checksum de cabeçalho, já que essa função fica por conta das camadas inferior e superior, o que melhora a performance. Pode ter cabeçalhos de extensão opcionais para fragmentação, segurança e suporte à mobilidade. Diferente do IPv4, roteadores não fragmentam pacotes IPv6.
