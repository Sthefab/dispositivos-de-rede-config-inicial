# Estrutura do Endereço IPv4

## 📌 Rede e Host
Um endereço IPv4 é hierárquico, com 32 bits divididos em:
- **Parte de rede** → deve ser igual em todos os dispositivos da mesma rede
- **Parte de host** → deve ser exclusiva para cada dispositivo dentro da rede

A **máscara de sub-rede** é o que define onde termina a parte de rede e começa a parte de host.

## 🎭 Máscara de Sub-Rede
- Formada por uma sequência de bits `1` seguida de uma sequência de bits `0`
- É comparada com o endereço IPv4 bit a bit (da esquerda pra direita)
- Não contém a rede nem o host, apenas indica **onde procurar** cada parte no endereço

## ✂️ Comprimento de Prefixo (Notação de Barra)
Forma simplificada de representar a máscara de sub-rede, contando quantos bits são `1`.

| Máscara de Sub-Rede | Prefixo |
|---|---|
| 255.0.0.0 | /8 |
| 255.255.0.0 | /16 |
| 255.255.255.0 | /24 |
| 255.255.255.128 | /25 |
| 255.255.255.192 | /26 |
| 255.255.255.224 | /27 |
| 255.255.255.240 | /28 |
| 255.255.255.248 | /29 |
| 255.255.255.252 | /30 |

**Exemplo de notação:** `192.168.10.10/24` (equivale a `192.168.10.10 255.255.255.0`)

## 🔢 AND Lógico — Como Descobrir o Endereço de Rede
Operação booleana bit a bit entre o **endereço IPv4** e a **máscara de sub-rede**:

| A | B | A AND B |
|---|---|---|
| 1 | 1 | 1 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 0 | 0 | 0 |

Só resulta em `1` quando **ambos** os bits são `1`.

### Exemplo prático
| | Decimal | Binário |
|---|---|---|
| Endereço IPv4 | 192.168.10.10 | 11000000.10101000.00001010.00001010 |
| Máscara de Sub-Rede | 255.255.255.0 | 11111111.11111111.11111111.00000000 |
| **Endereço de Rede (AND)** | **192.168.10.0** | 11000000.10101000.00001010.00000000 |

➡️ Resultado: `192.168.10.0/24`

O AND lógico entre o endereço do host e a máscara de sub-rede é o que revela **a qual rede aquele host pertence**.

## 🌐 Notação CIDR — Prefixos Intermediários (VLSM)
Além dos prefixos "fechados" (/8, /16, /24), o CIDR permite qualquer valor intermediário, dividindo a rede em blocos menores e mais flexíveis.

| Máscara de Sub-Rede | Prefixo |
|---|---|
| 255.255.192.0 | /18 |
| 255.255.224.0 | /19 |
| 255.255.240.0 | /20 |
| 255.255.248.0 | /21 |
| 255.255.252.0 | /22 |
| 255.255.254.0 | /23 |

Repara que os valores de /25 a /30 (já listados acima) seguem a mesma lógica, só que no **último** octeto em vez do terceiro.

## 🧮 Método do Bloco — Descobrindo o Endereço de Rede sem fazer AND bit a bit
Atalho pra encontrar o endereço de rede sem precisar converter tudo pra binário manualmente.

**Passo 1:** Identifique em qual octeto o prefixo "cai"
- /8 → 1º octeto | /16 → 2º octeto | /24 → 3º octeto | /25 a /30 → 4º octeto

**Passo 2:** Calcule os bits extras usados nesse octeto → `bits extras = prefixo − octeto anterior fechado`

**Passo 3:** Calcule o tamanho do bloco → `bloco = 256 ÷ 2^(bits extras)`

**Passo 4:** Monte os blocos contando de "bloco" em "bloco" a partir de 0, e veja em qual intervalo o endereço cai

| Prefixo | Bits extras | Bloco |
|---|---|---|
| /17 | 1 | 128 |
| /18 | 2 | 64 |
| /19 | 3 | 32 |
| /20 | 4 | 16 |
| /21 | 5 | 8 |
| /22 | 6 | 4 |
| /23 | 7 | 2 |
| /24 | 8 | 1 |

### Exemplo prático
`192.168.65.3/18` → prefixo cai no 3º octeto, bloco = 64. Blocos: 0, 64, 128, 192. O número 65 está entre 64 e 127.

➡️ Endereço de rede: **192.168.64.0**
➡️ Broadcast: **192.168.127.255**
➡️ Hosts válidos: **192.168.64.1** até **192.168.127.254**

## 🧑‍💻 Calculando Endereços Disponíveis para Hosts
Fórmula: `2^(bits de host) − 2`

O `−2` remove o **endereço de rede** e o **endereço de broadcast**, que não podem ser atribuídos a hosts.

### Exemplo prático
Rede `10.100.16.0` com máscara `255.255.252.0`
1. Converter a máscara pra prefixo: `255.255.252.0` = **/22**
2. Bits de host: `32 − 22 = 10`
3. Endereços disponíveis: `2^10 − 2 = 1024 − 2 = 1022`

➡️ Resultado: **1022 endereços** disponíveis para atribuição a hosts
