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

Hexadecimal é um sistema de numeração de base 16. Usa os dígitos 0-9 e as letras A-F pra representar valores de 0 a 15. É usado em rede pra representar endereços IPv6 e endereços MAC Ethernet.

### Por que hexadecimal existe

Binário puro é longo demais pra escrever e ler. Hexadecimal resolve isso porque **cada dígito hexadecimal representa exatamente 4 bits**. Assim, ao invés de escrever 10101000, escrevemos A8.

### Tabela de conversão: binário ↔ decimal ↔ hexadecimal

| Binário | Decimal | Hexadecimal |
|:---:|:---:|:---:|
| 0000 | 0  | 0 |
| 0001 | 1  | 1 |
| 0010 | 2  | 2 |
| 0011 | 3  | 3 |
| 0100 | 4  | 4 |
| 0101 | 5  | 5 |
| 0110 | 6  | 6 |
| 0111 | 7  | 7 |
| 1000 | 8  | 8 |
| 1001 | 9  | 9 |
| 1010 | 10 | A |
| 1011 | 11 | B |
| 1100 | 12 | C |
| 1101 | 13 | D |
| 1110 | 14 | E |
| 1111 | 15 | F |

### Convertendo um octeto binário pra hexadecimal

Como cada dígito hex vale 4 bits, o truque é dividir os 8 bits do octeto em dois grupos de 4 e converter cada grupo separado.

Exemplo: octeto **10101000**

| Grupo 1 | Grupo 2 |
|:---:|:---:|
| 1010 | 1000 |
| = 10 = **A** | = 8 = **8** |

Resultado: **A8**

Outro exemplo: octeto **11000000**

| Grupo 1 | Grupo 2 |
|:---:|:---:|
| 1100 | 0000 |
| = 12 = **C** | = 0 = **0** |

Resultado: **C0**

### IPv6 e hextetos

Endereços IPv6 têm 128 bits, escritos no formato `x:x:x:x:x:x:x:x`, onde cada "x" é um **hexteto**: um bloco de 16 bits representado por 4 dígitos hexadecimais.

Não faz diferença entre maiúsculas e minúsculas (`A8` e `a8` são equivalentes).

Exemplo de hexteto: os bits `1010100000000001` viram `A801` em hexadecimal.
## Por que isso importa

Dispositivos de rede só entendem binário; humanos trabalham melhor em decimal/hex. Saber converter entre os três sistemas é essencial pra entender endereçamento IPv4 e IPv6.
