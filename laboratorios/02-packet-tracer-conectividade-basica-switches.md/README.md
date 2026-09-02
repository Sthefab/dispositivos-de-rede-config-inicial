# Implementação de Conectividade Básica — Packet Tracer

## Sobre o lab

Atividade prática no Cisco Packet Tracer com foco em configuração básica de switches e implementação de conectividade em uma rede local, usando endereçamento IP em switches e hosts.

## Topologia

- 2 switches (S1 e S2) conectados entre si
- PC1 conectado ao S1
- PC2 conectado ao S2

## O que foi configurado

**Switches (S1 e S2)**
- Hostname
- Endereço IP e máscara de sub-rede na interface VLAN1 (SVI), com `no shutdown`
- Configuração salva na NVRAM (`copy running-config startup-config`)

**PCs**
- Endereço IP e máscara de sub-rede (PC1: 192.168.1.1 / PC2: 192.168.1.2)

## Testes de conectividade

- Ping do PC1 para S1, S2 e PC2
- Ping do PC2 para S1, S2 e PC1
- Ping via CLI dos switches para os demais dispositivos da rede

Todos os testes tiveram sucesso, com 0% de perda de pacote (alguns pings iniciais retornaram 80%, resolvido repetindo o teste — comportamento esperado pela resolução ARP na primeira tentativa).

## Resultado

**Pontuação: 100/100** — 14/14 itens de avaliação corretos, cobrindo endereçamento IPv4, configuração de hostname e gerenciamento de configuração.

## Aprendizados

- Diferença entre configurar um host e configurar a interface de gerenciamento (VLAN1) de um switch
- Por que um switch precisa de endereço IP mesmo operando por MAC (gerenciamento remoto)
- O que faz o comando `no shutdown` numa interface
- Importância do `copy running-config startup-config` pra não perder configurações
- Verificação de conectividade em camadas (host → switch → switch → host remoto)
