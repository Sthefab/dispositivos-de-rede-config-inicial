# Packet Tracer - Configurações Iniciais do Roteador

Atividade prática de configuração básica de um roteador Cisco (R1) via CLI, cobrindo proteção de acesso, banners de aviso e backup de configuração.

## Objetivos

- Verificar a configuração padrão do roteador
- Definir e verificar a configuração inicial (hostname, MOTD, senhas)
- Salvar o arquivo de configuração atual

## Topologia

PCA conectado a R1 via cabo console (RS 232).

## Parte 1 - Verificar Configuração Padrão

Acesso ao modo EXEC privilegiado:

Router> enable
Router#

Verificação da configuração em execução:

Router# show running-config

Verificação da NVRAM (vazia por padrão):

Router# show startup-config
% startup-config is not present

A mensagem aparece porque nenhuma configuração foi salva na NVRAM ainda, o roteador está rodando apenas com a configuração padrão em RAM.

## Parte 2 - Configuração Inicial

Hostname:

Router(config)# hostname R1

Mensagem do dia (MOTD):

R1(config)# banner motd ^C Unauthorized access is strictly prohibited^C

Senhas:

R1(config)# enable secret itsasecret
R1(config)# line console 0
R1(config-line)# password letmein
R1(config-line)# login
R1(config-line)# exit
R1(config)# service password-encryption

## Parte 3 - Salvar Configuração

Salvar na NVRAM:

R1# copy running-config startup-config

Versão curta e inequívoca do comando:

R1# copy run start

Verificar conteúdo da NVRAM:

R1# show startup-config

## Resultado

Atividade concluída com pontuação máxima: 64/64 pontos, todos os itens de avaliação corretos (Console, Enable Secret, Host Name, Service Password Encryption, Startup Config, Banner MOTD).
