# Próximos passos — Agentes

Este arquivo mantém o backlog antes de avançar para monitoramento, FIM, SCA, vulnerabilidades e resposta.

## 1. Múltiplos grupos e prioridade

Objetivo:

- adicionar o mesmo agente a mais de um grupo;
- aplicar valores conflitantes de forma controlada;
- observar a prioridade;
- verificar o resultado da configuração mesclada.

A documentação informa que o grupo atribuído mais recentemente tem maior prioridade em conflitos.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/grouping-agents.html

## 2. Queue / Buffer / Antiflooding

Objetivo:

- entender `client_buffer`;
- observar `queue_size`;
- observar `events_per_second`;
- entender warning/full/flood;
- testar comportamento sem provocar perda desnecessária de eventos.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/antiflooding.html

## 3. Disconnect / Reconnect

Teste controlado:

```text
active
  ↓
parar serviço do agente
  ↓
keep alive deixa de atualizar
  ↓
disconnected
  ↓
iniciar serviço
  ↓
active
```

Objetivo:

- entender os estados;
- observar tempo de detecção;
- validar reconexão;
- registrar Stats antes/depois.

## 4. Segurança do enrollment

A baseline inicial está com:

```xml
<use_password>no</use_password>
```

Próximo estudo:

- password authentication;
- validação da identidade do Manager;
- validação da identidade do Agent;
- certificados;
- riscos de enrollment aberto.

Referências:

- https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/auth.html
- https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/security-options/using-password-authentication.html

## 5. Upgrade remoto

O Wazuh permite upgrade remoto dos agentes a partir do Manager usando pacotes WPK.

O laboratório só executará upgrade quando existir uma versão apropriada para testar.

Regra operacional importante:

> o agente deve estar na mesma versão ou em versão anterior à do Manager.

Referências:

- https://documentation.wazuh.com/current/user-manual/agent/agent-management/remote-upgrading/index.html
- https://documentation.wazuh.com/current/upgrade-guide/wazuh-agent/index.html

## 6. Remoção e re-enrollment

Será feito no final da trilha Windows.

Precisamos distinguir:

```text
remover cadastro do Manager
        ≠
desinstalar software no endpoint
```

Depois será testado:

- remoção;
- desinstalação;
- limpeza;
- novo enrollment;
- reassociação a grupo;
- validação.

Referências:

- https://documentation.wazuh.com/current/user-manual/agent/agent-management/remove-agents/index.html
- https://documentation.wazuh.com/current/installation-guide/uninstalling-wazuh/agent.html

## 7. Linux

Instalar um agente Linux e repetir:

- enrollment;
- grupo;
- labels;
- configuração efetiva;
- Stats;
- queue;
- disconnect/reconnect.

Também testar anti-tampering, que atualmente é suportado apenas em endpoints Linux.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/anti-tampering.html

## 8. macOS e demais sistemas

Documentar os sistemas disponíveis conforme houver ambiente de teste real.

## Critério para fechar a área de Agentes

A área será considerada concluída quando tivermos validado:

- Windows;
- Linux;
- grupos e múltiplos grupos;
- configuração centralizada;
- queue;
- ciclo de conexão;
- segurança do enrollment;
- upgrade;
- remoção e re-enrollment;
- anti-tampering quando aplicável.
