# LAB 01 — Validação do ambiente Wazuh 4.14.7

## Objetivo

Validar uma OVA oficial do Wazuh 4.14.7 após a importação no hypervisor, confirmando que os principais componentes estão ativos e que o ambiente possui recursos mínimos para prosseguir com os laboratórios.

## Ambiente utilizado

- Wazuh 4.14.7
- OVA oficial
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- ambiente virtualizado

## 1. Validar os serviços

Execute:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

### Resultado esperado

Os três serviços devem apresentar:

```text
Active: active (running)
```

## 2. Confirmar a versão do Wazuh

Execute:

```bash
sudo /var/ossec/bin/wazuh-control info
```

No laboratório validado, o retorno foi:

```text
WAZUH_VERSION="v4.14.7"
WAZUH_REVISION="rc1"
WAZUH_TYPE="server"
```

## 3. Validar memória

Execute:

```bash
free -h
```

No ambiente de laboratório foram disponibilizados aproximadamente 8 GiB de RAM.

O objetivo desta etapa não é apenas verificar a memória livre naquele instante, mas registrar a capacidade do servidor e acompanhar o consumo conforme agentes e eventos forem adicionados.

## 4. Validar armazenamento

Execute:

```bash
df -h
```

Acompanhe principalmente a partição raiz e o espaço livre disponível.

O Wazuh Indexer armazena os dados indexados e o consumo de disco tende a crescer conforme o volume de eventos aumenta. Portanto, espaço em disco e política de retenção devem ser acompanhados em ambientes reais.

## 5. Validar portas em escuta

Execute:

```bash
ss -lntp
```

No laboratório foram observadas as seguintes portas relevantes:

| Porta | Função observada |
|---|---|
| 1514/TCP | comunicação dos agentes com o Wazuh Manager |
| 1515/TCP | enrollment/registro de agentes |
| 443/TCP | acesso HTTPS ao Wazuh Dashboard |
| 55000/TCP | Wazuh API |
| 22/TCP | SSH |
| 9200/TCP | serviço do Indexer disponível localmente |
| 9300/TCP | comunicação interna do Indexer |

> As portas expostas e permitidas devem ser revisadas conforme a arquitetura, segmentação e política de segurança do ambiente de destino.

## 6. Verificar acesso ao Dashboard

Acesse no navegador:

```text
https://<WAZUH_SERVER_IP>
```

Utilize credenciais válidas do ambiente.

Não publique senhas, tokens, certificados privados ou endereços internos reais em documentação pública.

## 7. Criar ponto de restauração

Antes de iniciar customizações, recomenda-se criar um snapshot da máquina virtual.

Exemplo de identificação:

```text
WAZUH-4.14.7-CLEAN
```

O snapshot deve representar um estado no qual:

- Manager está validado;
- Indexer está validado;
- Dashboard está validado;
- nenhum agente adicional foi configurado;
- nenhuma customização de laboratório foi aplicada.

## Evidências recomendadas

Para uso profissional, podem ser coletadas evidências como:

- status dos três serviços;
- versão do Wazuh;
- capacidade de memória e disco;
- portas em escuta;
- tela do Dashboard acessível;
- registro da criação do snapshot.

Antes do compartilhamento com terceiros, remova ou masque:

- IPs internos;
- nomes de hosts sensíveis;
- usuários;
- senhas;
- tokens;
- chaves;
- informações de clientes.

## Resultado do laboratório

O ambiente Wazuh 4.14.7 foi considerado apto para prosseguir quando:

- Wazuh Manager estava ativo;
- Wazuh Indexer estava ativo;
- Wazuh Dashboard estava ativo;
- versão do servidor foi confirmada;
- recursos da VM foram verificados;
- portas essenciais foram identificadas;
- Dashboard estava acessível.

## Próximo laboratório

Instalação, enrollment e validação do primeiro Wazuh Agent em Windows.

## Referências

- https://documentation.wazuh.com/current/
- https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html
