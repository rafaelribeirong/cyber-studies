# Configuração efetiva e Stats

## Objetivo

Validar o que o agente realmente está usando e verificar seu estado operacional.

## Configuration

Caminho:

```text
Endpoints
→ <AGENT_NAME>
→ Configuration
```

A consulta permitiu verificar, entre outros pontos:

- Manager configurado;
- porta de comunicação;
- enrollment;
- configuração remota;
- criptografia;
- tempo de reconexão;
- auto restart;
- módulos ativos/desativados;
- grupos;
- labels.

Isso é importante porque configurar algo no Manager não prova, por si só, que o endpoint recebeu a configuração.

```text
configuração definida
       ↓
configuração distribuída
       ↓
configuração efetivamente aplicada
```

## Stats

Caminho:

```text
Endpoints
→ <AGENT_NAME>
→ Stats
```

No laboratório foram observados:

```text
Status: connected
Buffer: enabled
Message buffer: 0
Last ack: atualizado
Last keep alive: atualizado
```

## Interpretação

### Status connected

O agente está conectado ao Manager.

### Last keep alive

Mostra a última sinalização periódica do agente.

### Last ack

Mostra confirmação de comunicação do Manager.

### Buffer enabled

O agente possui mecanismo de fila para controlar envio de eventos.

### Message buffer = 0

Não significa que nenhum evento está sendo coletado.

Significa que, naquele momento, não existiam eventos acumulados aguardando envio.

## Queue / antiflooding

O Wazuh Agent utiliza uma fila do tipo leaky bucket para evitar que picos de eventos sobrecarreguem a rede ou o Manager.

A fila poderá ser configurada por `<client_buffer>`.

Esse assunto será aprofundado no próximo ciclo.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/antiflooding.html

## Resultado

O agente foi considerado operacional porque:

- permaneceu `connected`;
- apresentava keep alive recente;
- recebia ACK;
- buffer estava habilitado;
- não havia backlog acumulado;
- configuração centralizada foi confirmada.
