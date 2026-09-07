# 03 — Wazuh Agents

## Objetivo

Entender o ciclo de vida completo dos agentes antes de avançar para FIM, vulnerabilidades, SCA, threat hunting, coleta avançada de logs ou resposta.

A documentação desta área foi escrita para alguém que já possui noções básicas de computador, sistema operacional e rede, mas ainda está aprendendo Wazuh.

Por isso, os documentos não mostram apenas **o que fazer**. Sempre que um termo técnico importante aparece, a ideia é explicar também **o que ele significa e por que ele existe**.

## Padrão adotado

Para cada assunto, sempre que aplicável, vamos seguir esta sequência:

```text
O que é
→ para que serve
→ por que isso importa
→ como funciona
→ como configurar
→ como validar
→ o que o resultado significa
→ limitações
→ referência oficial
```

A intenção não é transformar cada página em um livro, mas também evitar instruções do tipo "clique aqui e pronto" sem explicar o conceito.

## O que é um agente em uma frase

O **Wazuh Agent** é o software instalado no equipamento monitorado para coletar informações locais e se comunicar com o Wazuh Manager.

```text
Endpoint
   │
Wazuh Agent
   │
   ├── coleta dados
   ├── recebe configurações suportadas
   └── envia informações
   │
   ▼
Wazuh Manager
```

## Componentes relacionados

Durante o estudo é importante não confundir os componentes:

```text
Agent     → fica no endpoint
Manager   → processa e administra dados dos agentes
Indexer   → indexa/armazena dados pesquisáveis
Dashboard → interface gráfica de administração e análise
```

Os detalhes de arquitetura ficam na área de Fundamentos. Aqui o foco é o ciclo de vida do Agent.

## Sistemas suportados

A documentação oficial disponibiliza agentes para plataformas como:

- Linux;
- Windows;
- macOS;
- Solaris;
- AIX;
- HP-UX.

Nem todos serão documentados apenas teoricamente. Sempre que possível, o projeto prioriza teste real antes de considerar um item concluído.

Referências:

- https://documentation.wazuh.com/current/deployment-options/index.html
- https://documentation.wazuh.com/current/getting-started/components/wazuh-agent.html

## Estado dos estudos

| Tema | Estado |
|---|---|
| conceito de endpoint e agente | concluído |
| instalação x enrollment | concluído |
| portas básicas de comunicação | concluído |
| grupo `windows-lab` | concluído |
| instalação Windows | concluído |
| validação `active` | concluído |
| configuração efetiva | concluído |
| configuração centralizada | concluído |
| `agent.conf` | concluído |
| labels | concluído |
| checksum | concluído |
| Stats / keep alive / ACK / buffer | concluído em nível inicial |
| múltiplos grupos e prioridade | próximo |
| queue / antiflooding | próximo |
| disconnect / reconnect | próximo |
| segurança do enrollment | próximo |
| upgrade remoto | planejado |
| remoção / re-enrollment | planejado |
| Linux | planejado |
| macOS | planejado conforme ambiente disponível |
| anti-tampering | planejado para Linux |

## Ordem de leitura

1. [Conceitos, enrollment e grupos](01-conceitos-grupos.md)
2. [Instalação do agente Windows](02-instalacao-windows.md)
3. [Configuração centralizada e labels](03-configuracao-centralizada-labels.md)
4. [Configuração efetiva e Stats](04-validacao-stats.md)
5. [Próximos passos](05-proximos-passos.md)

## O que já foi comprovado na prática

Até este ponto, o laboratório já demonstrou:

```text
Agent instalado no Windows
      ↓
Enrollment realizado
      ↓
Agent conectado ao Manager
      ↓
Agent associado ao grupo windows-lab
      ↓
agent.conf alterado no Manager
      ↓
checksum do grupo alterado
      ↓
configuração distribuída
      ↓
labels visíveis na configuração efetiva
      ↓
Stats confirmando comunicação ativa
```

## O que ainda não faz parte desta etapa

Mesmo que o Dashboard já mostre dados relacionados a outros módulos, ainda não consideramos como estudados:

- FIM;
- SCA;
- Vulnerability Detection;
- análise de alertas;
- MITRE ATT&CK;
- Active Response;
- threat hunting.

Esses temas serão tratados depois que a base de Agentes estiver sólida.

## Referência principal

https://documentation.wazuh.com/current/user-manual/agent/agent-management/agent-administration.html
