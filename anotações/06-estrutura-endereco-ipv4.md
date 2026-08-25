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
1 AND 1 = 1
0 AND 1 = 0
1 AND 0 = 0
0 AND 0 = 0


Só resulta em `1` quando **ambos** os bits são `1`.

### Exemplo prático

| | Decimal | Binário |
|---|---|---|
| Endereço IPv4 | 192.168.10.10 | 11000000.10101000.00001010.00001010 |
| Máscara de Sub-Rede | 255.255.255.0 | 11111111.11111111.11111111.00000000 |
| **Endereço de Rede (AND)** | **192.168.10.0** | 11000000.10101000.00001010.00000000 |

➡️ Resultado: `192.168.10.0/24`

O AND lógico entre o endereço do host e a máscara de sub-rede é o que revela **a qual rede aquele host pertence**.
