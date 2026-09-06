# Conceitos, enrollment e grupos

## O que é um agente

O Wazuh Agent é instalado no endpoint e atua como sensor da plataforma.

Pode enviar informações relacionadas a:

- logs;
- inventário;
- integridade de arquivos;
- configuração;
- processos e serviços;
- usuários e grupos;
- eventos do sistema;
- outros módulos suportados.

## Enrollment

Instalar o pacote e registrar o agente são conceitos diferentes.

```text
Instalar o Agent
      ↓
Enrollment
      ↓
Manager gera/associa identidade
      ↓
Agent passa a se comunicar
```

Requisitos padrão:

```text
1515/TCP → enrollment automático
1514/TCP → comunicação normal
55000/TCP → API
```

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/requirements.html

## Grupos

Todo agente sem grupo específico pertence ao grupo `default`.

O laboratório criou:

```text
default
windows-lab
```

Caminho pelo Dashboard:

```text
Agents management
→ Groups
→ Add new group
```

Grupos não são apenas organização visual. Cada grupo possui configuração compartilhada.

No Manager:

```text
/var/ossec/etc/shared/<GROUP_NAME>/agent.conf
```

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/grouping-agents.html

## `agent.conf`

O `agent.conf` distribui configuração centralizada aos agentes do grupo.

Exemplos de capacidades suportadas:

- FIM;
- Rootcheck;
- coleta de logs;
- labels;
- SCA;
- Syscollector;
- client buffer;
- Osquery;
- alguns parâmetros de conexão suportados.

Referência:
https://documentation.wazuh.com/current/user-manual/reference/centralized-configuration.html

## Checksum

O Dashboard apresenta um `Configuration checksum` por grupo.

No laboratório:

1. `default` e `windows-lab` começaram com o mesmo conteúdo;
2. ambos tinham o mesmo checksum;
3. o `agent.conf` de `windows-lab` foi alterado;
4. o checksum de `windows-lab` mudou;
5. isso confirmou que o Manager reconheceu uma configuração específica para o grupo.

## Múltiplos grupos

Um agente pode pertencer a mais de um grupo.

Quando há múltiplos grupos, o Wazuh combina arquivos/configurações e, em conflitos, o grupo atribuído mais recentemente possui maior prioridade.

Esse comportamento ainda será testado no próximo laboratório.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/grouping-agents.html
