# Sistemas de Numeração em Redes: Binário e Hexadecimal

## Binário e IPv4

Endereços IPv4 são armazenados em binário (32 bits, divididos em 4 octetos de 8 bits cada), mas usamos notação decimal pontilhada (ex: 192.168.10.10) porque é mais fácil pra gente lidar.

**Conversão decimal → binário:** usa a tabela de valores posicionais (128, 64, 32, 16, 8, 4, 2, 1). Pra cada posição, verifica se o número do octeto é maior ou igual ao valor. Se sim, marca 1 e subtrai; se não, marca 0 e passa pra próxima.

Exemplo: 192 → 11000000 (128+64)
### Exemplo: convertendo 192 para binário

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1   | 1  | 0  | 0  | 0 | 0 | 0 | 0 |

**192 = 128 + 64 → binário: 11000000**

### Exemplo: convertendo 168 para binário

| 128 | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| 1   | 0  | 1  | 0  | 0 | 0 | 0 | 0 |

**168 = 128 + 32 → binário: 10101000**

## Hexadecimal e IPv6

Hexadecimal é base 16 (dígitos 0-9 e letras A-F). Usado em endereços IPv6 e endereços MAC.

Endereços IPv6 têm 128 bits, formato x:x:x:x:x:x:x:x, onde cada "x" é um **hexteto**: 16 bits representados por 4 dígitos hexadecimais. Não diferencia maiúsculas de minúsculas.

## Por que isso importa

Dispositivos de rede só entendem binário; humanos trabalham melhor em decimal/hex. Saber converter entre os três sistemas é essencial pra entender endereçamento IPv4 e IPv6.
