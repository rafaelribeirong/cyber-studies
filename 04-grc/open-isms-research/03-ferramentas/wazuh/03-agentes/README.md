# 03 — Wazuh Agents

## Objetivo

Entender o ciclo de vida completo dos agentes antes de avançar para FIM, vulnerabilidades, SCA, threat hunting ou resposta.

O agente é o componente instalado no endpoint monitorado.

```text
Endpoint
   │
Wazuh Agent
   │
   ├── coleta dados
   ├── recebe configuração permitida
   └── envia telemetria
   │
   ▼
Wazuh Manager
```

## Sistemas suportados

A documentação oficial disponibiliza agentes para:

- Linux;
- Windows;
- macOS;
- Solaris;
- AIX;
- HP-UX.

Referência:
https://documentation.wazuh.com/current/deployment-options/index.html

## Estado dos estudos

| Tema | Estado |
|---|---|
| conceito de agente | concluído |
| enrollment básico | concluído |
| grupo `windows-lab` | concluído |
| instalação Windows | concluído |
| validação `active` | concluído |
| configuração efetiva | concluído |
| configuração centralizada | concluído |
| labels | concluído |
| stats / keep alive / buffer | concluído em nível inicial |
| múltiplos grupos e prioridade | próximo |
| queue / antiflooding | próximo |
| disconnect / reconnect | próximo |
| segurança do enrollment | próximo |
| upgrade remoto | planejado |
| remoção / re-enrollment | planejado |
| Linux | planejado |
| macOS | planejado |
| anti-tampering | planejado para Linux |

## Laboratórios

1. [Conceitos, enrollment e grupos](01-conceitos-grupos.md)
2. [Instalação do agente Windows](02-instalacao-windows.md)
3. [Configuração centralizada e labels](03-configuracao-centralizada-labels.md)
4. [Configuração efetiva e Stats](04-validacao-stats.md)
5. [Próximos passos](05-proximos-passos.md)

## Referência principal

https://documentation.wazuh.com/current/user-manual/agent/agent-management/agent-administration.html
