# Módulo 2 — Nuvem e Virtualização

## Tipos de nuvem

Existem quatro modelos principais de nuvem:

- **Pública**: serviços disponibilizados à população em geral, geralmente via internet, gratuitos ou em modelo pay-as-you-go.
- **Privada**: voltada para uma entidade ou empresa específica; pode ser mantida internamente ou gerenciada por terceiros com segurança de acesso estrita.
- **Híbrida**: combina duas ou mais nuvens (ex: parte privada, parte pública) conectadas por uma arquitetura única, com níveis de acesso diferentes por usuário.
- **Comunitária**: criada para uso exclusivo de uma comunidade específica, com necessidades funcionais personalizadas (ex: setor de saúde, que precisa seguir HIPAA).

## Servidores dedicados vs. virtualização

Historicamente, cada servidor corporativo rodava um único serviço, com toda a RAM, processamento e disco dedicados exclusivamente a ele. Isso gerava dois problemas principais:

1. **Ponto único de falha**: se o servidor caísse, o serviço ficava indisponível.
2. **Subutilização**: servidores ficavam ociosos por longos períodos, desperdiçando energia e espaço (server sprawl).

A virtualização resolve isso ao permitir que múltiplos sistemas operacionais rodem sobre o mesmo hardware físico, através de uma camada de abstração chamada **hypervisor**.

## Vantagens da virtualização

**Redução de custo:**
- Menos equipamento físico necessário
- Menor consumo de energia e refrigeração
- Menos espaço físico requerido

**Benefícios adicionais:**
- Prototipagem mais rápida (labs isolados)
- Provisionamento de servidor mais ágil
- Maior tempo de atividade (tolerância a falhas redundante)
- Recuperação de desastres aprimorada (failover automatizado/testável)
- Suporte legado (mais tempo para migrar sistemas antigos)

## Hypervisors

| Tipo | Abordagem | Onde roda | Uso comum |
|------|-----------|-----------|-----------|
| Tipo 1 | Bare-metal | Direto no hardware | Servidores corporativos, data centers |
| Tipo 2 | Hospedada | Sobre um SO existente (Windows/Linux/macOS) | Uso individual, testes, labs |

Hypervisors Tipo 1 têm acesso direto ao hardware, o que os torna mais eficientes, escaláveis e robustos. Hypervisors Tipo 2 são mais simples de usar, já que não exigem software de console de gerenciamento separado.
