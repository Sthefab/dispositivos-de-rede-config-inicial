# Packet Tracer - Navegue no IOS

Atividade prática do Cisco Networking Academy (curso Conceitos Básicos de Redes), focada em navegação básica no Cisco IOS.

## Objetivo

Praticar o acesso à CLI de um switch via cabo de console, explorar a ajuda contextual do IOS, transitar entre os modos EXEC (usuário e privilegiado) e configurar o relógio do dispositivo usando o comando `clock`.

## Topologia

Um PC1 conectado a um switch S1 via cabo de console (RS-232).

## O que foi feito

- Conexão do PC1 ao S1 usando cabo de console
- Acesso ao terminal com as configurações padrão de porta (9600 bps)
- Exploração da ajuda contextual do IOS (`?`, `t?`, `te?`)
- Uso do tab completion para completar comandos
- Transição do modo EXEC usuário (`S1>`) para o EXEC privilegiado (`S1#`) com `enable`
- Entrada no modo de configuração global com `configure terminal`
- Ajuste do relógio do switch com `clock set`, testando também comandos incompletos, ambíguos e com parâmetros inválidos pra entender as mensagens de erro do IOS

## Resultado

Atividade concluída com 20/20 pontos, todas as conexões (console e RS-232) validadas corretamente entre PC1 e S1.

## Aprendizados

A ajuda sensível ao contexto do IOS (`?`) e o tab completion são essenciais pra quem tá começando com CLI de rede, ajudam a descobrir comandos disponíveis sem precisar decorar tudo de cara. Também ficou mais claro como o IOS sinaliza erros diferentes: comando incompleto, comando ambíguo e entrada inválida cada um tem uma mensagem própria, o que facilita bastante o diagnóstico na hora de configurar um dispositivo.
