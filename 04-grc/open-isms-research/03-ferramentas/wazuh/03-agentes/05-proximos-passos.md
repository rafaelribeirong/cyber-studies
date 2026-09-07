# Próximos passos — Agentes

Este arquivo mantém o backlog do módulo de Agentes antes de avançarmos para monitoramento, FIM, SCA, vulnerabilidades, detecção e resposta.

A proposta é que cada tema futuro seja estudado com o mesmo padrão:

```text
o que é
→ por que existe
→ quando usar
→ como configurar
→ como validar
→ limitações e riscos
→ referência oficial
```

## 1. Múltiplos grupos e prioridade

### O que significa múltiplos grupos

Um agente pode pertencer a mais de um grupo ao mesmo tempo.

Exemplo:

```text
AGENT-01
├── windows
├── laboratorio
└── monitoramento-especial
```

Isso permite combinar configurações de diferentes contextos.

### Por que isso é útil

Podemos separar configurações por finalidade.

Exemplo:

```text
windows              → configurações comuns de Windows
laboratorio           → parâmetros específicos de laboratório
monitoramento-especial → configurações adicionais para alguns endpoints
```

### O problema dos conflitos

Se dois grupos definirem valores diferentes para o mesmo parâmetro, o Wazuh precisa decidir qual configuração prevalece.

A documentação informa que, em conflitos entre grupos, o grupo atribuído mais recentemente possui maior prioridade.

### O que vamos testar

- adicionar o mesmo agente a mais de um grupo;
- aplicar valores conflitantes de forma controlada;
- observar a prioridade;
- verificar a configuração mesclada;
- confirmar qual valor chegou ao agente.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/grouping-agents.html

## 2. Queue, Buffer e Antiflooding

### O que é queue

**Queue** significa fila.

É uma estrutura onde mensagens podem aguardar antes de serem enviadas.

### O que é buffer

Buffer é a área temporária utilizada para manter essas mensagens enquanto aguardam envio.

### O que é antiflooding

Antiflooding é o mecanismo usado para controlar picos de mensagens e impedir que o agente envie eventos em uma velocidade excessiva.

### Por que isso existe

Um endpoint pode produzir milhares de eventos rapidamente.

Sem controle, isso poderia causar:

- uso excessivo de rede;
- aumento da carga do Manager;
- crescimento rápido da fila;
- eventual perda de eventos se a fila atingir seu limite.

### O que vamos estudar

- `client_buffer`;
- `queue_size`;
- `events_per_second`;
- crescimento do buffer;
- warning/full/flood;
- comportamento do agente quando há acúmulo;
- limites e riscos de alterar esses valores.

Não vamos alterar valores apenas para "otimizar" sem antes entender o impacto.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/antiflooding.html

## 3. Disconnect e Reconnect

### O que significa disconnect

Um agente fica **disconnected** quando o Manager deixa de receber a comunicação esperada daquele agente pelo período configurado.

Isso pode acontecer por vários motivos:

- serviço do agente parado;
- computador desligado;
- perda de rede;
- firewall;
- problema no Manager;
- configuração incorreta.

### O que significa reconnect

Reconnect é a tentativa de o agente restabelecer comunicação com o Manager depois de uma interrupção.

### Teste controlado planejado

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
agente reconecta
  ↓
active
```

### O que vamos observar

- tempo até o Wazuh detectar a desconexão;
- mudança do status;
- `Last keep alive`;
- comportamento durante a reconexão;
- retorno para `active`;
- Stats antes e depois do teste.

## 4. Segurança do enrollment

### Por que esse tema é importante

Enrollment é o momento em que novos agentes são registrados no Manager.

Se esse processo for mal protegido, um equipamento não autorizado pode tentar se registrar no ambiente.

Nossa baseline inicial de laboratório está com:

```xml
<use_password>no</use_password>
```

Isso foi mantido para simplificar o primeiro laboratório, não como recomendação universal de produção.

### O que vamos estudar

- autenticação por senha;
- arquivo de senha do enrollment;
- validação da identidade do Manager;
- validação da identidade do Agent;
- certificados;
- diferenças entre autenticação e criptografia;
- riscos de enrollment aberto.

### O que é autenticação

Autenticação é o processo de verificar a identidade de quem está tentando se conectar.

### O que é certificado

Certificado digital é um mecanismo usado para associar uma identidade a uma chave criptográfica e permitir validação de confiança entre sistemas.

Não vamos entrar em criptografia avançada neste módulo, mas precisamos entender o papel dos certificados no enrollment.

Referências:

- https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/auth.html
- https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/security-options/using-password-authentication.html
- https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/security-options/manager-identity-verification.html

## 5. Upgrade remoto

### O que é upgrade remoto

É a capacidade de atualizar a versão do Wazuh Agent a partir do Manager, sem acessar manualmente cada endpoint.

Isso é especialmente importante quando existem muitos agentes.

### O que é WPK

WPK é o formato de pacote utilizado pelo mecanismo de upgrade remoto do Wazuh Agent.

O Manager pode transferir o pacote apropriado ao agente e executar o processo de atualização suportado.

### Por que não vamos forçar o teste agora

Nosso Manager e agente estão na mesma versão do laboratório.

Não faz sentido simular uma atualização inexistente apenas para concluir o item.

Vamos executar o laboratório quando houver uma versão apropriada e segura para testar.

### Regra operacional importante

O Manager deve estar na mesma versão ou em versão mais recente que seus agentes para manter a compatibilidade suportada.

Referências:

- https://documentation.wazuh.com/current/user-manual/agent/agent-management/remote-upgrading/index.html
- https://documentation.wazuh.com/current/upgrade-guide/wazuh-agent/index.html

## 6. Remoção e re-enrollment

### Remover agente não é o mesmo que desinstalar

São duas ações diferentes.

```text
Remover do Manager
```

significa excluir o cadastro/identidade daquele agente da administração do Wazuh.

```text
Desinstalar do endpoint
```

significa remover o software Wazuh Agent do sistema operacional.

Portanto:

```text
remover cadastro do Manager
        ≠
desinstalar software do endpoint
```

### O que vamos testar

No final da trilha Windows:

- remover o agente do Manager;
- verificar o efeito no Dashboard;
- desinstalar o agente do Windows;
- verificar resíduos relevantes;
- reinstalar;
- executar novo enrollment;
- associar novamente a um grupo;
- validar configuração e conexão.

Faremos isso no final porque o agente Windows ainda será usado nos outros laboratórios.

Referências:

- https://documentation.wazuh.com/current/user-manual/agent/agent-management/remove-agents/index.html
- https://documentation.wazuh.com/current/installation-guide/uninstalling-wazuh/agent.html

## 7. Linux

### Por que instalar outro sistema operacional

O comportamento geral de Agent e Manager é semelhante, mas instalação, serviços, arquivos, permissões e algumas capacidades mudam conforme o sistema operacional.

Por isso não queremos aprender Wazuh apenas através do Windows.

### O que vamos repetir no Linux

- instalação;
- enrollment;
- grupo;
- labels;
- configuração centralizada;
- configuração efetiva;
- Stats;
- queue;
- disconnect/reconnect.

Depois disso, teremos uma comparação prática entre Windows e Linux.

## 8. Anti-tampering no Linux

### O que é tampering

**Tampering** significa adulteração ou tentativa de manipular um componente.

No contexto do agente, pode incluir tentativas de remover ou desativar o software de monitoramento.

### O que é anti-tampering

É um mecanismo criado para dificultar a remoção não autorizada do agente.

Na documentação atual do Wazuh, esse recurso é aplicável a endpoints Linux.

Por isso ele será estudado no laboratório Linux, e não no Windows.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-management/anti-tampering.html

## 9. macOS e demais sistemas

A documentação do Wazuh possui agentes para diferentes sistemas operacionais.

Não vamos criar documentação baseada apenas em teoria para plataformas que ainda não testamos.

Quando houver ambiente disponível, documentaremos:

- instalação;
- enrollment;
- serviço/processo do agente;
- caminhos importantes;
- configuração;
- validação;
- diferenças em relação a Windows e Linux.

## Critério para fechar a área de Agentes

A área será considerada concluída quando tivermos validado, em nível prático adequado:

- conceito e arquitetura do agente;
- Windows;
- Linux;
- enrollment;
- grupos;
- múltiplos grupos e prioridade;
- configuração centralizada;
- labels;
- configuração efetiva;
- Stats;
- queue e antiflooding;
- ciclo de conexão;
- segurança do enrollment;
- upgrade remoto quando houver cenário válido;
- remoção e re-enrollment;
- anti-tampering quando aplicável.

A partir daí, avançamos para as capacidades que utilizam o agente, como inventário, FIM, SCA, vulnerabilidades, coleta de logs e resposta.