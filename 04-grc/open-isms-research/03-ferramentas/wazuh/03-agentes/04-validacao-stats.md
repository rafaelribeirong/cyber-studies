# Configuração efetiva e Stats

## Objetivo

Validar o que o agente realmente está utilizando e entender os principais indicadores de comunicação apresentados pelo Wazuh.

A ideia aqui não é apenas olhar números, mas saber o que cada informação significa.

## O que significa configuração efetiva

Configuração efetiva é a configuração que o agente realmente está utilizando naquele momento.

Ela pode ser resultado da combinação de diferentes fontes, como:

- configuração local do agente;
- configuração centralizada recebida pelo Manager;
- associação a grupos;
- parâmetros padrão do Wazuh.

Por isso, não basta saber que uma configuração foi salva no Manager.

Precisamos confirmar se ela realmente chegou ao endpoint.

```text
configuração definida
       ↓
configuração distribuída
       ↓
configuração recebida
       ↓
configuração efetivamente aplicada
```

## Onde consultar no Dashboard

Caminho:

```text
Endpoints
→ <AGENT_NAME>
→ Configuration
```

Essa tela ajuda a responder perguntas como:

- para qual Manager o agente está configurado;
- qual porta utiliza;
- como está configurado o enrollment;
- se configuração remota está habilitada;
- qual intervalo de reconexão é usado;
- quais módulos estão ativos ou desativados;
- quais grupos estão associados;
- quais labels foram aplicadas.

## Por que isso é importante

Imagine que alteramos uma configuração no Manager e assumimos que todos os agentes receberam a mudança.

Se houver problema de comunicação, configuração inválida ou conflito, o resultado real pode ser diferente do esperado.

A tela de Configuration ajuda a validar o estado do endpoint, e não apenas a intenção do administrador.

## O que é Stats

**Stats** significa estatísticas.

No contexto do agente, essa área mostra informações operacionais sobre comunicação, envio de mensagens e fila.

Caminho:

```text
Endpoints
→ <AGENT_NAME>
→ Stats
```

No laboratório observamos informações como:

```text
Status: connected
Buffer: enabled
Message buffer: 0
Messages count: valor acumulado
Messages sent: valor acumulado
Last ack: atualizado
Last keep alive: atualizado
```

## Status `connected`

Quando o Stats mostra:

```text
Status: connected
```

significa que o agente está mantendo comunicação com o Manager naquele momento.

Isso é diferente de apenas ter o software instalado ou o serviço iniciado.

## O que é keep alive

**Keep alive** é uma sinalização periódica usada para indicar que o agente continua presente e se comunicando.

Uma forma simples de entender:

```text
Agent
  │
  ├── "estou ativo"
  │
  ├── "continuo ativo"
  │
  └── "continuo ativo"
        ↓
      Manager
```

Se o Manager deixa de receber essas sinalizações pelo período esperado, o agente pode passar a ser tratado como desconectado.

### Last keep alive

O campo **Last keep alive** mostra quando ocorreu a última sinalização desse tipo.

Se o horário está sendo atualizado, isso é um bom indício de que o agente continua se comunicando.

## O que é ACK

ACK vem de **acknowledgement**, ou confirmação.

Em redes e sistemas, ACK é usado para indicar que determinada comunicação foi reconhecida pelo outro lado.

No contexto apresentado pelo Stats, **Last ack** ajuda a mostrar que existe retorno/confirmacão na comunicação entre agente e Manager.

### Last ack

Se o campo está atualizado, isso indica que o agente vem recebendo confirmações recentes do Manager.

Não deve ser analisado isoladamente; o estado geral deve considerar também status, keep alive e fila.

## O que é buffer

Um **buffer** é uma área temporária usada para armazenar dados antes que eles sejam processados ou enviados.

No agente Wazuh, ele ajuda a lidar com momentos em que eventos são produzidos mais rapidamente do que podem ser transmitidos.

Exemplo:

```text
Eventos gerados
     ↓
   Buffer
     ↓
Envio ao Manager
```

## Buffer enabled

Quando vemos:

```text
Buffer: enabled
```

significa que o mecanismo de buffer/fila do agente está habilitado.

Isso não quer dizer que existem eventos acumulados naquele momento.

Apenas indica que o recurso está disponível para controlar o fluxo de mensagens.

## Message buffer

O campo **Message buffer** representa mensagens que estão acumuladas aguardando envio.

No laboratório observamos:

```text
Message buffer: 0
```

Isso significa que, naquele momento, não existia backlog acumulado na fila.

### O que é backlog

Backlog é uma quantidade de trabalho ou dados que ainda está aguardando processamento ou envio.

Exemplo:

```text
0 mensagens  → sem backlog
100 mensagens → existem 100 mensagens aguardando
```

Portanto:

```text
Message buffer = 0
```

não significa:

```text
nenhum evento foi coletado
```

Significa apenas:

```text
nenhum evento estava parado na fila naquele instante
```

## Messages count e Messages sent

Esses contadores mostram atividade acumulada do agente.

Em termos simples:

```text
Messages count → quantidade contabilizada pelo mecanismo do agente
Messages sent  → quantidade enviada
```

Os números podem não ser idênticos em todos os momentos e não devem ser interpretados isoladamente como falha.

Para análise operacional básica, os sinais mais importantes são:

```text
Status connected
Last keep alive recente
Last ack recente
Message buffer sem crescimento anormal
```

## Queue e antiflooding

O agente possui um mecanismo de fila para controlar a velocidade de envio de eventos.

Isso é importante porque um endpoint pode gerar muitos eventos em pouco tempo.

Sem controle, um pico poderia aumentar o consumo de rede e sobrecarregar o Manager.

O Wazuh utiliza configurações de `client_buffer` para esse comportamento.

Exemplo de parâmetros que estudaremos:

```xml
<client_buffer>
  <disabled>no</disabled>
  <queue_size>5000</queue_size>
  <events_per_second>500</events_per_second>
</client_buffer>
```

### `queue_size`

Define a capacidade da fila de mensagens do agente.

Em termos simples: quantas mensagens podem ficar aguardando antes que a fila chegue ao limite configurado.

### `events_per_second`

Controla a taxa de envio de eventos por segundo.

Isso ajuda a evitar uma rajada muito grande de mensagens enviadas de uma vez.

### Por que se chama antiflooding

**Flood** significa inundação.

Nesse contexto, representa uma quantidade excessiva de eventos sendo enviada rapidamente.

O mecanismo de antiflooding busca controlar esse fluxo.

Esse tópico ainda será testado de forma prática antes de alterarmos valores no laboratório.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/antiflooding.html

## Como avaliamos o agente no laboratório

O agente foi considerado operacional porque observamos em conjunto:

```text
Status = connected
Last keep alive = recente
Last ack = recente
Buffer = enabled
Message buffer = 0
Configuração centralizada = aplicada
```

Nenhum desses itens deve ser usado sozinho como prova absoluta de que todo o monitoramento está perfeito.

Eles confirmam principalmente que o agente está conectado, comunicando e sem backlog aparente naquele momento.

## O que ainda não validamos aqui

Esta etapa não valida, por si só:

- qualidade das regras de detecção;
- FIM;
- SCA;
- vulnerabilidades;
- Active Response;
- retenção de logs;
- capacidade de armazenamento do ambiente.

Esses recursos pertencem a estudos separados.

## Resumo

```text
Configuration   → mostra o que o agente está configurado para usar
Stats           → mostra indicadores operacionais do agente
Keep alive      → sinal periódico de presença/comunicação
ACK             → confirmação de comunicação
Buffer          → armazenamento temporário para controlar fluxo
Message buffer  → mensagens aguardando envio
Backlog         → dados acumulados ainda pendentes
Antiflooding    → controle para evitar excesso de eventos enviados de uma vez
```

## Referências

- administração de agentes:
  https://documentation.wazuh.com/current/user-manual/agent/agent-management/agent-administration.html
- conexão de agentes:
  https://documentation.wazuh.com/current/user-manual/agent/agent-management/agent-connection.html
- antiflooding:
  https://documentation.wazuh.com/current/user-manual/agent/agent-management/antiflooding.html
