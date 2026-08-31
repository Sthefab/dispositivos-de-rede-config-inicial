# Serviços de Endereçamento IP

Resumo sobre DNS e DHCP, baseado no material da Cisco Networking Academy.

## Por que existem

Lembrar endereços IP numéricos é inviável, tanto pra usuário quanto pra configurar manualmente uma rede média/grande. DNS resolve nomes em endereços IP, e DHCP automatiza a atribuição desses endereços aos hosts.

---

## DNS (Domain Name System)

### O que é

Protocolo que converte nomes de domínio (FQDNs, tipo `www.cisco.com`) em endereços IP numéricos, e vice-versa. Se o IP de um domínio muda, é transparente pro usuário, porque o nome continua o mesmo e só é revinculado ao novo endereço.

O DNS usa um único formato de mensagem pra tudo: consultas, respostas, erros e transferência de registros entre servidores.

### Como funciona (passo a passo)

1. Usuário digita um FQDN no navegador.
2. O computador envia uma consulta DNS pro servidor DNS configurado.
3. O servidor DNS casa o FQDN com o IP correspondente.
4. O servidor responde a consulta com o IP.
5. O cliente usa esse IP pra fazer a requisição real.

### Tipos de registro

- **A** - endereço IPv4 do dispositivo final
- **AAAA** - endereço IPv6 do dispositivo final ("quad-A")
- **NS** - servidor de nomes autoritativo
- **MX** - registro de troca de e-mail (email)

Quando o servidor recebe uma consulta, primeiro olha os próprios registros. Se não resolve, contata outros servidores. Quando acha a resposta, guarda em cache temporariamente pro caso de ser pedida de novo.

No Windows, o comando abaixo mostra o cache de DNS local:

```
ipconfig /displaydns
```

A mensagem DNS entre servidores segue sempre a mesma estrutura:

| Campo | Descrição |
| --- | --- |
| Pergunta | A pergunta feita ao servidor de nomes |
| Resposta | Registros de recursos que respondem a pergunta |
| Autoridade | Registros apontando pra uma autoridade |
| Adicional | Registros com informações extras |

### Hierarquia DNS

O DNS é organizado de forma hierárquica, dividido em zonas. Cada servidor DNS é responsável só pela sua zona (seu pedaço da estrutura de nomes). Se recebe uma consulta fora da própria zona, encaminha pra outro servidor responsável. Isso torna o DNS escalável, já que a carga fica distribuída entre vários servidores.

Domínios de nível superior indicam tipo de organização ou país, por exemplo:

- **.com** - empresa/indústria
- **.org** - organização sem fins lucrativos
- **.au** - Austrália
- **.co** - Colômbia

### Comando nslookup

Utilitário disponível nos sistemas operacionais pra consultar servidores de nomes manualmente, resolver hosts específicos e diagnosticar problemas de DNS.

```
C:\Users>nslookup
Default Server:  dns-sj.cisco.com
Address:  171.70.168.183
>www.cisco.com
Server:  dns-sj.cisco.com
Address:  171.70.168.183
Name:    origin-www.cisco.com
Addresses:  2001:420:1101:1::a
          173.37.145.84
Aliases:  www.cisco.com
>cisco.netacad.net
Server:  dns-sj.cisco.com
Address:  171.70.168.183
Name:  cisco.netacad.net
Address:  72.163.6.223
```

---

## DHCP (Dynamic Host Configuration Protocol)

### O que é

Automatiza a atribuição de IPv4, máscara de sub-rede, gateway e outros parâmetros de rede o chamado **endereçamento dinâmico**. A alternativa é o **endereçamento estático**, onde o admin configura tudo manualmente.

Quando um host se conecta, contata um servidor DHCP, que escolhe um endereço de um conjunto pré-configurado chamado **pool** e o atribui ao host.

Em redes grandes ou com muita rotatividade de usuários, DHCP é bem mais eficiente que configurar tudo na mão.

### Período de concessão (lease)

DHCP aloca endereços por um tempo configurável o **período de concessão**. Quando esse período expira, ou o servidor recebe uma mensagem `DHCPRELEASE`, o endereço volta pro pool e pode ser reutilizado. Isso permite que usuários mudem de local e reconectem com facilidade.

### Onde roda o servidor DHCP

- Redes médias/grandes: geralmente um servidor dedicado.
- Redes residenciais: geralmente o próprio roteador que conecta a casa ao ISP.

Na prática, redes costumam misturar os dois modelos: **DHCP** para hosts de uso geral (dispositivos de usuário final) e **endereçamento estático** para equipamentos de rede (roteadores gateway, switches, servidores, impressoras).

> **DHCPv6**: oferece serviço parecido pro IPv6, mas não distribui o gateway padrão esse endereço só é obtido dinamicamente via mensagem *Anúncio do roteador*.

### Mensagens DHCP (fluxo)

1. **DHCPDISCOVER** - cliente transmite em broadcast procurando servidores DHCP disponíveis.
2. **DHCPOFFER** - servidor responde oferecendo uma locação: IPv4, máscara de sub-rede, IP do DNS, IP do gateway padrão e duração da locação.
3. **DHCPREQUEST** - se houver mais de um servidor respondendo, o cliente escolhe uma oferta e transmite essa mensagem identificando o servidor e a oferta aceitas (também usada pra renovar a locação antes de vencer).
4. **DHCPACK** - servidor confirma que a locação foi concedida.
5. **DHCPNAK** - se a oferta não é mais válida, o servidor nega e o processo reinicia do zero (novo `DHCPDISCOVER`).

O servidor DHCP garante que nenhum IP seja atribuído a dois dispositivos ao mesmo tempo. A maioria dos ISPs usa DHCP pra atribuir endereços aos clientes.

**DHCPv6** tem um conjunto de mensagens equivalente: `SOLICIT`, `ADVERTISE`, `INFORMATION REQUEST` e `REPLY`.
