# Instalação do agente Windows

## Objetivo

Instalar, registrar e validar um agente Wazuh em Windows utilizando o fluxo orientado pelo Dashboard.

Além de mostrar o procedimento, este documento explica o que significa cada informação usada durante a instalação.

## Antes de começar

Para o agente funcionar corretamente, o endpoint Windows precisa conseguir alcançar o Wazuh Manager pela rede.

No nosso laboratório:

```text
Windows endpoint
      │
      │ rede local
      ▼
Wazuh Manager
```

Se existir firewall, rota incorreta, endereço errado ou serviço indisponível, o agente pode ser instalado normalmente e mesmo assim não conseguir se conectar.

## Caminho no Dashboard

```text
Wazuh Dashboard
→ Agents management
→ Summary
→ Deploy new agent
```

A opção **Deploy new agent** não instala o agente diretamente no computador.

Ela funciona como um assistente: fazemos algumas escolhas no Dashboard e ele gera o comando adequado para executar no endpoint.

## 1. Selecionar o sistema operacional e o pacote

No laboratório selecionamos:

```text
Windows
MSI 32/64 bits
```

### O que é MSI

MSI é um formato de pacote utilizado pelo Windows Installer.

Em termos simples, é um arquivo de instalação do Windows.

O pacote do Wazuh contém os arquivos necessários para instalar o Wazuh Agent e registrar seu serviço no sistema operacional.

## 2. Server address

O campo **Server address** informa ao agente onde o Wazuh Manager pode ser encontrado.

Ele pode ser preenchido com um endereço IP ou com um nome de host/FQDN resolvível pelo endpoint.

Exemplos:

```text
<WAZUH_SERVER_IP>
```

ou:

```text
wazuh.example.internal
```

### O que é endereço IP

Um endereço IP identifica um equipamento ou interface dentro de uma rede IP.

No nosso laboratório usamos um endereço privado da rede local.

### O que é FQDN

FQDN significa **Fully Qualified Domain Name**.

É um nome completo utilizado para localizar um equipamento por DNS, por exemplo:

```text
wazuh.empresa.local
```

Em um ambiente permanente, um FQDN ou endereço IP estável evita ter que alterar agentes porque o endereço do Manager mudou.

### Não confundir Manager IP com endpoint IP

O campo Server address deve apontar para o **Manager**, não para a própria máquina Windows.

```text
Windows endpoint
      │
      │ envia dados para
      ▼
Wazuh Manager
```

## 3. Agent name

O **Agent name** é o nome pelo qual o endpoint será identificado dentro do Wazuh.

Exemplo:

```text
<AGENT_NAME>
```

É recomendável utilizar uma convenção de nomes compreensível.

Exemplos conceituais:

```text
WIN-FINANCEIRO-01
SRV-WEB-01
NOTEBOOK-LAB-01
```

O nome deve ser único no Manager.

Isso evita confundir dois equipamentos diferentes como se fossem o mesmo agente.

## 4. Grupo

No laboratório usamos:

```text
windows-lab
```

Associar o agente a um grupo durante o deployment permite que ele já seja registrado dentro da estrutura de configuração desejada.

Isso é útil porque grupos podem receber configurações centralizadas pelo `agent.conf`.

## 5. Comando gerado pelo Dashboard

Depois das escolhas, o Dashboard gera um comando para ser executado no Windows.

Exemplo equivalente ao nosso fluxo:

```powershell
Invoke-WebRequest `
  -Uri https://packages.wazuh.com/4.x/windows/wazuh-agent-4.14.7-1.msi `
  -OutFile $env:TEMP\wazuh-agent.msi

msiexec.exe /i $env:TEMP\wazuh-agent.msi /q `
  WAZUH_MANAGER="<WAZUH_SERVER_IP>" `
  WAZUH_AGENT_GROUP="windows-lab" `
  WAZUH_AGENT_NAME="<AGENT_NAME>"
```

### O que `Invoke-WebRequest` faz

O PowerShell utiliza `Invoke-WebRequest` para baixar o pacote MSI do agente.

### O que é `$env:TEMP`

É uma referência ao diretório temporário do usuário/sistema no Windows.

No exemplo, o instalador é baixado para essa pasta temporária.

### O que `msiexec.exe` faz

`msiexec.exe` é o mecanismo do Windows usado para instalar pacotes MSI.

### O que significa `/i`

Indica uma operação de instalação do pacote.

### O que significa `/q`

Executa a instalação em modo silencioso, sem o assistente gráfico tradicional.

### Variáveis do deployment

As opções seguintes passam informações para a instalação:

```text
WAZUH_MANAGER     → endereço do Manager
WAZUH_AGENT_GROUP → grupo do agente
WAZUH_AGENT_NAME  → nome do agente
```

Essas informações evitam que seja necessário configurar manualmente cada parâmetro depois da instalação.

Referência:
https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/deployment-variables/deployment-variables-windows.html

## 6. Serviço do Wazuh Agent

Após a instalação, o Wazuh Agent funciona como um serviço do Windows.

Um **serviço** é um programa executado em segundo plano pelo sistema operacional, sem depender de uma janela aberta pelo usuário.

Isso permite que o agente continue funcionando mesmo sem o usuário abrir manualmente um aplicativo.

O serviço precisa estar iniciado para que o agente possa executar suas funções normalmente.

## Serviço iniciado não significa agente conectado

Esse é um ponto importante.

Quando o Windows informa que o serviço iniciou corretamente, isso significa apenas:

```text
software instalado
+
serviço Windows executando
```

Ainda não prova que:

- o endereço do Manager está correto;
- as portas estão acessíveis;
- o enrollment funcionou;
- o Manager aceitou o agente;
- a comunicação está ocorrendo.

Por isso:

```text
serviço iniciado
      ≠
agente conectado ao Manager
```

## 7. Validar no Dashboard

Depois da instalação, voltamos ao Dashboard para verificar o agente.

Caminho:

```text
Agents management
→ Summary / Endpoints
```

No nosso laboratório, validamos informações como:

```text
Status: active
Version: Wazuh v4.14.7
Group: windows-lab
OS: Windows
```

### Status `active`

Significa que o Manager está reconhecendo o agente como conectado/ativo naquele momento.

É uma validação muito mais importante do que apenas verificar se o serviço iniciou no Windows.

### Version

Mostra a versão do Wazuh Agent instalada no endpoint.

Isso é importante para administração, compatibilidade e futuros upgrades.

### Group

Mostra a qual grupo o agente está associado.

### OS

Identifica o sistema operacional detectado no endpoint.

## Estados que ainda vamos estudar

O Wazuh pode apresentar estados diferentes para agentes.

### `active`

O agente está se comunicando normalmente com o Manager.

### `disconnected`

O agente existia e já se comunicava, mas deixou de manter comunicação dentro do período esperado.

### `pending`

Indica um agente cujo processo de conexão/enrollment ainda não chegou ao estado normal esperado.

### `never connected`

O registro do agente existe no Manager, mas ele ainda não estabeleceu uma comunicação válida.

Esses estados serão testados de forma controlada mais adiante, em vez de apenas decorados teoricamente.

## Resultado do laboratório

O laboratório confirmou o fluxo completo:

```text
Dashboard gerou deployment
      ↓
pacote foi instalado no Windows
      ↓
serviço foi iniciado
      ↓
agente realizou enrollment
      ↓
Manager recebeu conexão
      ↓
Dashboard exibiu o agente como active
```

## Referências

- instalação do Wazuh Agent:
  https://documentation.wazuh.com/current/installation-guide/wazuh-agent/index.html
- requisitos de enrollment:
  https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/requirements.html
- variáveis de deployment Windows:
  https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/deployment-variables/deployment-variables-windows.html
- troubleshooting de enrollment:
  https://documentation.wazuh.com/current/user-manual/agent/agent-enrollment/troubleshooting.html
