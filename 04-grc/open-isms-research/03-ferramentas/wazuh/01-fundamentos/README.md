# 01 — Fundamentos do Wazuh

## O que é o Wazuh

Wazuh é uma plataforma de segurança que combina coleta de telemetria de endpoints, análise de eventos, inventário, avaliação de configuração, detecção de vulnerabilidades, monitoramento de integridade, regras de detecção e recursos de resposta.

No laboratório, a arquitetura central é composta por:

```text
Endpoint
   │
   │ Wazuh Agent
   ▼
Wazuh Manager
   │
   ├── analisa eventos
   ├── aplica regras e decoders
   ├── administra agentes
   └── coordena capacidades
   │
   ▼
Wazuh Indexer
   │
   ▼
Wazuh Dashboard
```

## Componentes centrais

### Wazuh Manager

Recebe dados dos agentes, administra enrollment e grupos, aplica regras, processa eventos e coordena módulos do servidor.

### Wazuh Indexer

Armazena e indexa dados de segurança para consulta e visualização.

### Wazuh Dashboard

Interface web para operação, configuração, investigação e administração.

### Wazuh Agent

Software instalado no endpoint monitorado. Coleta informações do sistema e envia telemetria ao Manager.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/index.html

## Dashboard primeiro

Neste projeto, sempre que existir uma função equivalente no Dashboard, ela será usada primeiro.

CLI/API ficam para:

- funções ausentes na interface;
- validação técnica;
- automação;
- troubleshooting;
- cenários avançados.

## Portas iniciais

| Porta | Finalidade |
|---|---|
| 1514/TCP | comunicação normal do agente com o Manager |
| 1515/TCP | enrollment automático |
| 55000/TCP | Wazuh Server API |
| 443/TCP | Dashboard, conforme a instalação |

Referência de enrollment:
https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/requirements.html

## Três camadas de configuração

Um conceito essencial é separar:

```text
Manager: /var/ossec/etc/ossec.conf
        │
        └── comportamento do servidor Wazuh

Agent: ossec.conf local
        │
        └── comportamento local do endpoint

Grupo: agent.conf
        │
        └── configuração centralizada distribuída pelo Manager
```

O `agent.conf` não substitui todo o `ossec.conf`; somente opções suportadas para configuração centralizada podem ser enviadas aos agentes.

Referência:
https://documentation.wazuh.com/current/user-manual/reference/centralized-configuration.html

## Capacidades que serão estudadas separadamente

- System Inventory / IT Hygiene;
- File Integrity Monitoring;
- Security Configuration Assessment;
- Vulnerability Detection;
- coleta de logs;
- regras e decoders;
- Threat Hunting;
- MITRE ATT&CK;
- malware detection;
- Active Response;
- integrações;
- administração e RBAC;
- retenção e archives;
- conformidade e evidências.

A existência de uma função no Dashboard não significa que um alerta, vulnerabilidade ou falha de benchmark represente automaticamente um incidente. Cada módulo será estudado separadamente antes de conclusões operacionais.
