# Configuração centralizada e labels

## Objetivo

Entender como o Wazuh distribui configurações do Manager para os agentes de um grupo e validar esse fluxo na prática usando labels.

Este laboratório é importante porque mostra a diferença entre:

```text
configuração criada no Manager
      ↓
configuração reconhecida pelo grupo
      ↓
configuração distribuída
      ↓
configuração realmente aplicada no agente
```

## O que é configuração centralizada

Configuração centralizada significa administrar parte do comportamento dos agentes a partir do Wazuh Manager, sem precisar editar manualmente cada endpoint.

Imagine um ambiente com 200 computadores Windows.

Se todos precisarem receber a mesma configuração, fazer isso máquina por máquina seria demorado e sujeito a erro.

Com grupos e `agent.conf`, podemos trabalhar assim:

```text
Wazuh Manager
      │
      └── grupo windows-lab
              │
              └── agent.conf
                    │
                    ▼
              agentes do grupo
```

## O que é o `agent.conf`

O `agent.conf` é o arquivo usado para compartilhar configurações com os agentes de um grupo.

No laboratório, o grupo foi:

```text
windows-lab
```

E o arquivo correspondente fica na área compartilhada do Manager:

```text
/var/ossec/etc/shared/windows-lab/agent.conf
```

Pelo Dashboard, usamos:

```text
Agents management
→ Groups
→ windows-lab
→ Files
→ agent.conf
```

Importante: o `agent.conf` não substitui completamente o `ossec.conf` local do agente.

Ele é uma camada de configuração centralizada e possui somente as opções oficialmente suportadas para esse mecanismo.

Referência:
https://documentation.wazuh.com/current/user-manual/reference/centralized-configuration.html

## O que são labels

**Labels** são informações adicionais associadas ao agente.

Elas funcionam como etiquetas de contexto.

Uma label normalmente possui:

```text
chave = valor
```

Exemplo:

```text
environment = lab
```

Nesse caso:

```text
environment → chave
lab         → valor
```

## Por que usar labels

O nome do computador nem sempre é suficiente para entender o contexto de um endpoint.

Imagine um alerta vindo de:

```text
PC-0147
```

Esse nome sozinho diz pouco.

Com labels, podemos adicionar contexto:

```text
environment = production
platform = windows
department = finance
criticality = high
```

Agora sabemos que o evento veio de um computador Windows de produção, ligado ao financeiro e considerado crítico.

Isso pode ajudar em:

- filtros;
- pesquisas;
- investigações;
- dashboards;
- organização operacional;
- identificação de contexto dos alertas.

Labels não substituem inventário formal de ativos ou classificação de ativos, mas ajudam a enriquecer os dados monitorados.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/labels.html

## Configuração usada no laboratório

Adicionamos ao `agent.conf` do grupo `windows-lab`:

```xml
<agent_config>
  <labels>
    <label key="environment">lab</label>
    <label key="platform">windows</label>
    <label key="criticality">low</label>
  </labels>
</agent_config>
```

### Interpretação

```text
environment = lab
```

Indica que o endpoint pertence ao ambiente de laboratório.

```text
platform = windows
```

Indica a plataforma utilizada pelo endpoint.

```text
criticality = low
```

Indica uma classificação operacional de baixa criticidade para o nosso laboratório.

Esses nomes não são uma estrutura obrigatória do Wazuh. Eles foram definidos por nós para o laboratório.

Em um ambiente real, a organização deve definir um padrão de labels coerente.

## O que aconteceu depois de salvar

O fluxo esperado era:

```text
agent.conf alterado
      ↓
Manager detecta nova configuração compartilhada
      ↓
configuração do grupo passa a ser diferente
      ↓
Manager distribui ao agente
      ↓
agente aplica as labels
```

Para comprovar isso, fizemos duas validações diferentes.

## Validação 1 — Configuration checksum

Antes da alteração, os grupos `default` e `windows-lab` possuíam configuração compartilhada equivalente e apresentavam o mesmo checksum.

Depois que alteramos o `agent.conf` de `windows-lab`, o checksum mudou.

### O que é checksum

Checksum é um valor calculado a partir do conteúdo de dados.

Ele funciona como uma impressão digital do conteúdo.

Exemplo simplificado:

```text
conteúdo A → checksum X
conteúdo B → checksum Y
```

Se o conteúdo mudar, o valor calculado tende a mudar também.

### O que isso provou no laboratório

Depois da alteração:

```text
checksum default      ≠ checksum windows-lab
```

Isso mostrou que o Manager reconheceu que a configuração compartilhada de `windows-lab` era diferente da configuração de `default`.

### O que isso não provou

O checksum mudar ainda não prova que o endpoint já recebeu a configuração.

Ele comprova a mudança na configuração do grupo.

Por isso foi necessária uma segunda validação.

## Validação 2 — configuração efetiva do agente

Abrimos:

```text
Endpoints
→ <AGENT_NAME>
→ Configuration
→ Labels
```

E encontramos:

```text
environment = lab
platform = windows
criticality = low
```

Essa foi a confirmação de que as labels não estavam apenas salvas no Manager: elas haviam chegado ao agente e apareciam em sua configuração efetiva.

## Configuração definida x configuração aplicada

Esse conceito é importante para administração de qualquer ferramenta centralizada.

```text
Configuração definida
```

Significa que o administrador salvou uma alteração.

```text
Configuração aplicada
```

Significa que o componente de destino realmente recebeu e passou a utilizar aquela configuração.

No laboratório, validamos os dois lados.

## Resultado do laboratório

O fluxo completo foi validado:

```text
agent.conf alterado
      ↓
checksum do grupo mudou
      ↓
Manager reconheceu a alteração
      ↓
configuração foi distribuída
      ↓
agente recebeu
      ↓
labels apareceram no endpoint
```

## Limitação importante

Nem toda opção do `ossec.conf` pode ser configurada centralmente pelo `agent.conf`.

Antes de enviar uma configuração aos agentes, é necessário verificar se aquela seção é suportada pela configuração centralizada.

Também é importante considerar conflitos quando um agente pertence a múltiplos grupos. Esse comportamento será testado no próximo laboratório.

## Referências

- configuração centralizada:
  https://documentation.wazuh.com/current/user-manual/reference/centralized-configuration.html
- labels:
  https://documentation.wazuh.com/current/user-manual/agent/agent-management/labels.html
- grupos de agentes:
  https://documentation.wazuh.com/current/user-manual/agent/agent-management/grouping-agents.html
