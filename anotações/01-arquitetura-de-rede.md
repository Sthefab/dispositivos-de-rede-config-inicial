# 01 - Arquitetura de Rede

## O que é arquitetura de rede

É basicamente o conjunto de tecnologias, regras (protocolos) e infraestrutura física que fazem os dados se moverem pela rede. A rede deixou de ser só um jeito de trocar dados e virou algo que conecta pessoas, dispositivos e informação o tempo todo.

Pra uma rede funcionar bem e crescer, ela precisa seguir 4 pilares:

## 1. Tolerância a falhas

É a capacidade da rede de continuar funcionando mesmo quando alguma coisa falha, limitando quantos dispositivos são afetados.

Isso é possível porque existem vários caminhos possíveis entre origem e destino (isso se chama **redundância**). Se um caminho cai, o tráfego passa por outro sem o usuário nem perceber.

Uma das formas de garantir isso é a **comutação por pacotes**: uma mensagem (tipo um e-mail ou vídeo) é quebrada em pedaços menores, os pacotes. Cada pacote carrega o endereço de origem e destino, e os roteadores decidem o melhor caminho pra cada um na hora, com base nas condições da rede naquele momento. Por isso pacotes de uma mesma mensagem podem seguir rotas diferentes e ainda assim chegar certinho no destino.

## 2. Escalabilidade

É a rede conseguir crescer (novos usuários, novos serviços) sem perder desempenho pra quem já tá usando.

Isso é possível porque os projetos de rede seguem padrões e protocolos já estabelecidos. Assim, fabricantes de hardware e software não precisam reinventar a roda toda vez, só se adaptam ao padrão existente.

## 3. Qualidade de serviço (QoS)

É o mecanismo que prioriza certos tipos de tráfego quando a rede está congestionada.

**Congestionamento** acontece quando a demanda por largura de banda é maior do que a rede consegue entregar. Largura de banda é medida em bits por segundo (bps).

Quando isso acontece, os dispositivos guardam os pacotes na memória até conseguir enviar. O QoS entra aqui: ele prioriza tráfego sensível ao tempo, como chamada de voz, em vez de, por exemplo, o carregamento de uma página. O que importa pro QoS é o *tipo* de tráfego, não o conteúdo dele.

## 4. Segurança de rede

Duas frentes principais:
- **Segurança da infraestrutura**: proteger fisicamente os equipamentos e impedir acesso não autorizado ao software de gerenciamento deles.
- **Segurança da informação**: proteger os dados que trafegam e os dados armazenados nos dispositivos.

Os 3 pilares da segurança da informação:
- **Confidencialidade**: só quem tem permissão consegue acessar e ler o dado.
- **Integridade**: garante que o dado não foi alterado no meio do caminho.
- **Disponibilidade**: garante que o usuário autorizado consegue acessar o serviço quando precisa.

---

## Projeto de rede hierárquica

### Endereço físico x endereço lógico

Pensa assim: o **endereço MAC** é como o nome de uma pessoa. Não muda, é fixo, vem "de fábrica" na placa de rede (NIC). Por isso é chamado de **endereço físico**.

Já o **endereço IP** é como o endereço de uma casa. Pode mudar dependendo de onde o dispositivo está conectado na rede. Por isso é chamado de **endereço lógico**, ele é atribuído com base na localização do host na rede.

O IP tem duas partes:
- Uma identifica a **rede** (igual pra todo mundo que tá na mesma rede local)
- Outra identifica o **host** especificamente (única dentro daquela rede)

Pra um computador se comunicar, ele precisa dos dois: MAC (quem é) e IP (onde tá), do mesmo jeito que uma carta precisa do nome e do endereço da pessoa.

### Por que dividir a rede (analogia hierárquica)

Se todo mundo na internet fosse identificado só pelo MAC, sem organização por rede/localização, seria impossível achar alguém específico entre milhões de dispositivos.

Além disso, redes Ethernet muito grandes geram muito tráfego de **broadcast** (mensagem que vai pra todo mundo da rede), o que consome banda e derruba a performance. Por isso não dá pra colocar todo mundo numa rede só: divide-se em redes menores, usando um **modelo de design hierárquico**.

### As 3 camadas do modelo hierárquico

**Camada de acesso**
Onde os dispositivos do usuário final se conectam (geralmente via switch ou access point). Todo mundo nessa camada costuma compartilhar a mesma porção de rede do IP. Se a mensagem é pra alguém da mesma rede local, ela fica por ali. Se for pra outra rede, sobe pra camada de distribuição.

**Camada de distribuição**
Conecta redes diferentes entre si e controla o fluxo de tráfego entre elas. Tem switches mais robustos e roteadores (ou switches layer 3) fazendo o roteamento entre redes.

**Camada de núcleo (core)**
É o backbone, a espinha dorsal de alta velocidade, com conexões redundantes. Função principal: transportar grandes volumes de dados rapidamente entre as redes. Usa equipamentos de altíssimo desempenho, tipo um Cisco Catalyst 9600.

---

## Resumo rápido

| Camada | Função principal |
|---|---|
| Acesso | Conecta os dispositivos do usuário |
| Distribuição | Conecta e controla o tráfego entre redes |
| Núcleo | Transporta dados em alta velocidade entre tudo |

**4 pilares de uma rede confiável**: tolerância a falhas, escalabilidade, QoS, segurança.
