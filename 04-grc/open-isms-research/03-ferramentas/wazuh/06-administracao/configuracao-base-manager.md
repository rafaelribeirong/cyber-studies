# Configuração-base do Wazuh Manager

## Objetivo

Entender o `ossec.conf` do Manager antes de habilitar recursos indiscriminadamente.

O arquivo principal do Manager é:

```text
/var/ossec/etc/ossec.conf
```

A documentação recomenda backup antes de alterações, pois um erro de configuração pode impedir os serviços do Wazuh de iniciar.

Referência:
https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/index.html

## Regra adotada

Não transformar automaticamente:

```text
disabled=yes → no
enabled=no   → yes
```

Cada módulo precisa ser entendido antes de ser habilitado. Alguns aumentam uso de CPU, memória e disco; outros dependem de software adicional ou alteram comportamento de resposta.

## Baseline inicial observada

| Recurso | Estado | Decisão |
|---|---|---|
| JSON de alertas | ativo | manter |
| alerts.log | ativo | manter |
| `logall` | desativado | manter por enquanto |
| `logall_json` | desativado | manter por enquanto |
| e-mail | desativado | estudar depois |
| comunicação de agentes | 1514/TCP | manter |
| enrollment | 1515/TCP | manter |
| Rootcheck | ativo | manter |
| CIS-CAT | desativado | estudar pré-requisitos |
| Osquery | desativado | estudar integração |
| Syscollector | ativo | manter |
| SCA | ativo | manter |
| Vulnerability Detection | ativo | manter |
| Indexer connector | ativo | manter |
| FIM / Syscheck | ativo | manter |
| Active Response | comandos definidos, resposta não ativada | manter |
| Ruleset | ativo | manter |
| Rule test | ativo | manter |
| Cluster | desativado | correto para single-node |

## `logall` e `logall_json`

A configuração permanece:

```xml
<logall>no</logall>
<logall_json>no</logall_json>
```

Quando habilitados, o Manager passa a arquivar todos os eventos recebidos, inclusive eventos que não geraram alertas.

Arquivos:

```text
/var/ossec/logs/archives/archives.log
/var/ossec/logs/archives/archives.json
```

Isso é útil para:

- análise histórica;
- investigação;
- threat hunting;
- retenção de evidências;
- requisitos de logging.

Porém, a própria documentação alerta que archives podem consumir recursos significativos de armazenamento ao longo do tempo. Por isso, a habilitação será estudada junto com retenção, rotação e indexação.

Referência:
https://documentation.wazuh.com/current/user-manual/manager/event-logging.html

## Comunicação de agentes

```xml
<remote>
  <connection>secure</connection>
  <port>1514</port>
  <protocol>tcp</protocol>
</remote>
```

A porta 1514/TCP é usada pela comunicação normal do agente após enrollment.

Referência:
https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/remote.html

## Enrollment

Baseline:

```xml
<auth>
  <disabled>no</disabled>
  <remote_enrollment>yes</remote_enrollment>
  <port>1515</port>
  <use_password>no</use_password>
</auth>
```

Conceitos:

```text
1515/TCP → enrollment
1514/TCP → comunicação normal do agente
55000/TCP → Wazuh Server API
```

`use_password=no` foi mantido apenas para o laboratório inicial. Password authentication, validação de identidade do Manager e validação do Agent serão estudadas no módulo de segurança de enrollment.

Referências:

- https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/auth.html
- https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/requirements.html
- https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/security-options/using-password-authentication.html

## Syscollector

Ativo por padrão na baseline:

```xml
<wodle name="syscollector">
  <disabled>no</disabled>
  <interval>1h</interval>
  <scan_on_start>yes</scan_on_start>
</wodle>
```

Coleta informações de inventário. O estudo detalhado será feito em System Inventory / IT Hygiene.

## SCA

Ativo:

```xml
<sca>
  <enabled>yes</enabled>
  <scan_on_start>yes</scan_on_start>
  <interval>12h</interval>
</sca>
```

O módulo será estudado separadamente antes de interpretar falhas de benchmark como risco real.

## Vulnerability Detection

Ativo:

```xml
<vulnerability-detection>
  <enabled>yes</enabled>
  <index-status>yes</index-status>
  <feed-update-interval>60m</feed-update-interval>
</vulnerability-detection>
```

O Manager correlaciona inventário recebido dos agentes com conteúdo de vulnerabilidades.

Referência:
https://documentation.wazuh.com/current/user-manual/capabilities/vulnerability-detection/configuring-scans.html

## FIM / Syscheck no Manager

O bloco `<syscheck>` existente no `ossec.conf` do Manager monitora o próprio host onde o Manager está instalado.

Isso não deve ser confundido com o FIM de um endpoint Windows ou Linux. O FIM dos agentes será configurado localmente ou por `agent.conf`.

## CIS-CAT

Permanece desativado:

```xml
<wodle name="cis-cat">
  <disabled>yes</disabled>
</wodle>
```

Não será habilitado apenas alterando o valor. Pré-requisitos e finalidade serão estudados em laboratório próprio.

## Osquery

Permanece desativado:

```xml
<wodle name="osquery">
  <disabled>yes</disabled>
</wodle>
```

O uso do módulo depende da integração com Osquery.

## Active Response

Ter comandos como:

```xml
<command>
  <name>firewall-drop</name>
  ...
</command>
```

não significa que o bloqueio automático esteja ativo.

Uma ação só é vinculada a eventos quando um bloco `<active-response>` apropriado é configurado.

Active Response será estudado com:

- regra de acionamento;
- alvo;
- timeout;
- exceções;
- rollback;
- validação.

## Cluster

A OVA usada é single-node, portanto:

```xml
<cluster>
  ...
  <disabled>yes</disabled>
</cluster>
```

Isso está correto para o laboratório atual.

O cluster será estudado em uma implantação distribuída.

## Exemplo completo

Arquivo de referência do laboratório:

- [`exemplos/ossec-manager-baseline.xml`](exemplos/ossec-manager-baseline.xml)

Use como material de estudo, não como configuração universal. Ajuste endereços, certificados, retenção, integrações e requisitos do ambiente antes de aplicar.

## Resultado

A baseline foi revisada sem habilitar módulos apenas por estarem desativados. O laboratório permanece funcional para continuar o estudo de agentes e capacidades.

## Referências

- `ossec.conf`: https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/index.html
- auth: https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/auth.html
- event logging: https://documentation.wazuh.com/current/user-manual/manager/event-logging.html
- centralized configuration: https://documentation.wazuh.com/current/user-manual/reference/centralized-configuration.html
