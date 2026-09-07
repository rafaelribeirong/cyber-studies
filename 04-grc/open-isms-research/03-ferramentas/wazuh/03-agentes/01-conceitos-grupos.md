# Conceitos, enrollment e grupos

## Objetivo

Entender os conceitos básicos necessários antes de instalar e administrar agentes Wazuh.

A ideia deste documento é explicar os termos como se o leitor já tivesse noções básicas de redes e sistemas operacionais, mas ainda não conhecesse o funcionamento do Wazuh em profundidade.

## O que é um endpoint

Um **endpoint** é um equipamento que queremos monitorar.

Exemplos:

- computador Windows de um usuário;
- servidor Linux;
- notebook macOS;
- servidor de aplicação;
- máquina virtual.

No nosso laboratório, o endpoint é o computador Windows onde instalamos o Wazuh Agent.

## O que é um agente

O **Wazuh Agent** é um software instalado no endpoint monitorado.

Ele funciona como um sensor local: observa informações do sistema operacional e envia dados relevantes para o Wazuh Manager.

Uma forma simples de visualizar é:

```text
Endpoint
   │
   │ possui o Wazuh Agent
   ▼
Wazuh Agent
   │
   │ coleta e envia informações
   ▼
Wazuh Manager
```

O agente não é o Dashboard e não é o Manager.

Cada componente possui uma função diferente:

```text
Agent     → coleta dados no endpoint
Manager   → recebe, processa e analisa dados
Indexer   → armazena/indexa dados para pesquisa
Dashboard → apresenta e permite administrar a plataforma
```

O agente pode fornecer informações relacionadas a:

- logs;
- inventário de hardware e software;
- integridade de arquivos;
- configurações de segurança;
- processos e serviços;
- usuários e grupos;
- eventos do sistema operacional;
- outros módulos suportados.

Isso não significa que todos esses recursos precisam ser estudados ao mesmo tempo. Cada capacidade será analisada separadamente ao longo do laboratório.

Referência:
https://documentation.wazuh.com/current/getting-started/components/wazuh-agent.html

## O que significa telemetria

**Telemetria** é um termo usado para os dados enviados por um sistema sobre seu próprio funcionamento ou estado.

No contexto do Wazuh, podemos usar esse termo de forma ampla para informações que o agente coleta e envia ao Manager.

Exemplo:

```text
Windows gera evento
      ↓
Agent coleta
      ↓
Agent envia ao Manager
      ↓
Manager processa
```

Nem toda informação coletada se transforma automaticamente em um alerta. O tratamento depende do tipo de dado, regras e módulos envolvidos.

## Instalação e enrollment são coisas diferentes

É importante separar dois conceitos.

### Instalação

Instalar o Wazuh Agent significa colocar o software do agente dentro do sistema operacional.

No Windows, por exemplo, instalamos um pacote MSI e passamos a ter o serviço do Wazuh Agent instalado na máquina.

### Enrollment

**Enrollment** é o processo de registrar esse agente no Wazuh Manager.

Em termos simples:

```text
Instalar o software
      ↓
Registrar o agente no Manager
      ↓
Criar/associar identidade do agente
      ↓
Permitir comunicação segura entre Agent e Manager
```

Portanto:

```text
agente instalado
      ≠
agente corretamente registrado e conectado
```

É possível o software estar instalado e o serviço iniciado, mas o agente não conseguir se registrar ou se comunicar com o Manager.

## Portas usadas no laboratório

No fluxo padrão estudado:

```text
1515/TCP → enrollment
1514/TCP → comunicação normal do agente
55000/TCP → Wazuh Server API
```

### O que significa TCP

TCP é um protocolo de transporte utilizado em redes.

Quando escrevemos `1514/TCP`, estamos dizendo que o serviço está utilizando a porta lógica 1514 através do protocolo TCP.

### Porta 1515/TCP

É utilizada no processo de enrollment automático do agente.

Ela participa do momento em que o agente precisa ser registrado no Manager.

### Porta 1514/TCP

Depois que o agente já está registrado, a comunicação normal entre o Agent e o Manager ocorre pela porta configurada para esse canal; no laboratório, 1514/TCP.

### Porta 55000/TCP

É associada à Wazuh Server API.

Não é a porta utilizada pelo agente para enviar normalmente sua telemetria ao Manager.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/requirements.html

## O que é um grupo de agentes

Um **grupo** é uma forma de reunir agentes que devem compartilhar determinado contexto ou configuração.

Exemplos de grupos possíveis:

```text
windows
linux
servidores
estacoes
producao
laboratorio
filial-rio
```

No nosso laboratório usamos:

```text
default
windows-lab
```

O grupo `default` já existe no Wazuh e é utilizado quando o agente não foi associado a um grupo específico.

Criamos `windows-lab` para separar nosso agente Windows de laboratório.

## Grupo não é apenas organização visual

Esse ponto é importante.

Um grupo não serve somente para deixar os endpoints organizados no Dashboard.

Ele também pode possuir arquivos de configuração compartilhados que serão distribuídos aos agentes pertencentes ao grupo.

No Manager, cada grupo possui sua área compartilhada, por exemplo:

```text
/var/ossec/etc/shared/windows-lab/
```

Um dos arquivos mais importantes é:

```text
agent.conf
```

Caminho utilizado pelo Dashboard:

```text
Agents management
→ Groups
```

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/grouping-agents.html

## O que é o `agent.conf`

O `agent.conf` é um arquivo usado para **configuração centralizada dos agentes**.

Em vez de entrar manualmente em cada computador para fazer a mesma alteração, podemos definir uma configuração no Manager e distribuí-la para os agentes de um grupo.

Exemplo conceitual:

```text
Manager
  │
  └── grupo windows-lab
        │
        └── agent.conf
              │
              ▼
       agentes do grupo
```

Isso ajuda principalmente quando existem muitos endpoints.

Imagine 100 computadores Windows. Alterar manualmente o mesmo parâmetro em 100 máquinas seria trabalhoso e sujeito a erros.

Com configuração centralizada, podemos administrar parte desse comportamento a partir do Manager.

Exemplos de recursos que possuem opções suportadas em configuração centralizada incluem:

- FIM;
- Rootcheck;
- coleta de logs;
- labels;
- SCA;
- Syscollector;
- client buffer;
- Osquery;
- outros parâmetros suportados pela configuração centralizada.

Importante: **nem toda configuração existente no `ossec.conf` pode ser enviada pelo `agent.conf`**. É necessário consultar a documentação para saber quais seções são suportadas.

Referência:
https://documentation.wazuh.com/current/user-manual/reference/centralized-configuration.html

## O que é um checksum

Um **checksum** é um valor calculado a partir do conteúdo de um arquivo ou conjunto de dados.

Ele funciona como uma espécie de impressão digital do conteúdo.

Exemplo simplificado:

```text
Arquivo A
conteúdo: ABC
checksum: 12345
```

Se o conteúdo mudar:

```text
Arquivo A
conteúdo: ABCD
checksum: 98765
```

O valor também tende a mudar.

O objetivo não é mostrar o conteúdo do arquivo, mas permitir perceber que ele é diferente de outra versão.

### Por que o Wazuh mostra um Configuration checksum

No Dashboard, os grupos apresentam um **Configuration checksum**.

Esse valor permite identificar se a configuração compartilhada de um grupo é igual ou diferente da configuração de outro grupo ou de uma versão anterior.

No nosso laboratório aconteceu o seguinte:

```text
default     → agent.conf sem nossa alteração
windows-lab → agent.conf inicialmente igual
```

Como o conteúdo era equivalente, observamos o mesmo checksum.

Depois adicionamos labels ao `agent.conf` de `windows-lab`.

Então:

```text
conteúdo mudou
      ↓
checksum mudou
```

Isso foi uma primeira evidência de que o Manager reconheceu que a configuração compartilhada daquele grupo havia sido alterada.

### O que o checksum não prova sozinho

O checksum mudar não prova, sozinho, que o agente já recebeu e aplicou a configuração.

Ele prova que a configuração do grupo mudou.

Por isso fizemos uma segunda validação diretamente no agente, verificando sua configuração efetiva.

Fluxo completo validado:

```text
agent.conf alterado
      ↓
checksum do grupo mudou
      ↓
Manager distribuiu
      ↓
agente recebeu
      ↓
labels apareceram na configuração efetiva
```

## Múltiplos grupos

Um mesmo agente pode pertencer a mais de um grupo.

Isso é útil quando queremos combinar configurações.

Exemplo:

```text
AGENT-01
├── windows
├── laboratorio
└── monitoramento-especial
```

Nesse cenário, o Wazuh precisa combinar as configurações recebidas desses grupos.

Se duas configurações definirem valores diferentes para o mesmo parâmetro, existe uma regra de prioridade.

A documentação do Wazuh informa que, em conflitos entre grupos, o grupo atribuído mais recentemente possui maior prioridade.

Esse comportamento ainda será testado de forma prática no próximo laboratório.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/grouping-agents.html

## Resumo

Até aqui, os conceitos principais são:

```text
Endpoint   → equipamento monitorado
Agent      → software instalado no endpoint
Enrollment → registro do Agent no Manager
Grupo      → conjunto de agentes que pode compartilhar configuração
agent.conf → arquivo de configuração centralizada do grupo
Checksum   → impressão digital calculada do conteúdo/configuração
```

Esses conceitos são a base para os próximos estudos de agentes.