# 07 — Conformidade

## Objetivo

Analisar como capacidades do Wazuh podem:

- implementar partes de controles técnicos;
- apoiar processos de segurança;
- produzir evidências;
- fornecer dados para auditoria e monitoramento.

O Wazuh não deve ser tratado como solução que garante conformidade ou certificação isoladamente.

## Classificação

- **IMPLEMENTA**
- **APOIA**
- **EVIDÊNCIA**
- **FORA DO ESCOPO**

## Mapeamento inicial estudado

O mapeamento abaixo é preliminar e não reproduz o texto das normas.

| Tema ISO/IEC 27001:2022 | Capacidade Wazuh | Classificação inicial |
|---|---|---|
| A.5.9 — inventário de ativos | Syscollector / System Inventory | APOIA / EVIDÊNCIA |
| A.8.8 — vulnerabilidades técnicas | Vulnerability Detection | APOIA / EVIDÊNCIA |
| A.8.9 — gestão de configuração | grupos + `agent.conf` + SCA | APOIA / EVIDÊNCIA |
| A.8.15 — logging | coleta, alertas, archives | APOIA / EVIDÊNCIA |
| A.8.16 — atividades de monitoramento | agentes, regras, alertas, Dashboard | IMPLEMENTA parcialmente / EVIDÊNCIA |

### Por que não marcar tudo como “atendido”

Exemplo: detectar uma CVE não prova que o risco foi tratado.

```text
Wazuh detecta
      ↓
organização avalia
      ↓
prioriza
      ↓
corrige / aceita / mitiga
      ↓
mantém evidência
```

O Wazuh participa do processo, mas não substitui governança, decisão de risco, políticas, responsabilidades e procedimentos.

## ISO/IEC 27701

A relação específica com ISO/IEC 27701 ainda não foi detalhada neste laboratório.

O mapeamento será criado apenas quando estudarmos capacidades relevantes para privacidade, registros, monitoramento e evidências do SGPI, evitando associações genéricas sem validação.

## Evidências já produzidas no laboratório

- serviços centrais ativos;
- versão validada;
- agente Windows registrado e `active`;
- agente associado a grupo;
- alteração de checksum do grupo;
- labels distribuídas centralmente;
- configuração efetiva consultada;
- Stats com conexão e buffer;
- baseline do Manager revisada.

## Referências técnicas

- Wazuh agent administration:
  https://documentation.wazuh.com/current/user-manual/agent/agent-management/agent-administration.html
- Centralized configuration:
  https://documentation.wazuh.com/current/user-manual/reference/centralized-configuration.html
- Event logging:
  https://documentation.wazuh.com/current/user-manual/manager/event-logging.html
- Vulnerability Detection:
  https://documentation.wazuh.com/current/user-manual/capabilities/vulnerability-detection/configuring-scans.html

> Para uso profissional, validar o mapeamento contra cópias licenciadas e vigentes das normas aplicáveis.
