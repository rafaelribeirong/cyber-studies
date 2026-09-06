# LAB 01 — Validação do ambiente Wazuh 4.14.7

## Objetivo

Validar uma OVA oficial do Wazuh 4.14.7 após a importação no hypervisor, confirmando que os principais componentes estão ativos e que o ambiente está pronto para os laboratórios seguintes.

## Ambiente utilizado

- Wazuh 4.14.7;
- OVA oficial;
- Wazuh Manager;
- Wazuh Indexer;
- Wazuh Dashboard;
- VirtualBox.

## 1. Validar os serviços

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

Resultado esperado:

```text
Active: active (running)
```

## 2. Confirmar a versão

```bash
sudo /var/ossec/bin/wazuh-control info
```

No laboratório:

```text
WAZUH_VERSION="v4.14.7"
WAZUH_REVISION="rc1"
WAZUH_TYPE="server"
```

## 3. Validar memória

```bash
free -h
```

O laboratório foi executado com aproximadamente 8 GiB de RAM.

O objetivo é registrar a capacidade disponível e acompanhar o consumo conforme agentes e volume de eventos forem adicionados.

## 4. Validar armazenamento

```bash
df -h
```

Acompanhe principalmente:

- partição raiz;
- espaço livre;
- crescimento do armazenamento do Indexer;
- impacto de archives e retenção.

A documentação oficial alerta que o arquivamento de todos os eventos pode consumir armazenamento de forma significativa.

Referência:
https://documentation.wazuh.com/current/user-manual/manager/event-logging.html

## 5. Validar portas em escuta

```bash
ss -lntp
```

Portas relevantes observadas/esperadas:

| Porta | Função |
|---|---|
| 1514/TCP | comunicação de agentes |
| 1515/TCP | enrollment |
| 443/TCP | Dashboard |
| 55000/TCP | Wazuh API |
| 22/TCP | SSH, quando habilitado |
| 9200/TCP | Indexer |
| 9300/TCP | comunicação interna do Indexer |

A exposição de portas deve ser compatível com a arquitetura e a segmentação do ambiente.

## 6. Acessar o Dashboard

```text
https://<WAZUH_SERVER_IP>
```

Use credenciais válidas e nunca publique senhas, tokens ou endereços internos reais.

## 7. Criar snapshot

Antes de customizações, crie um ponto de restauração da VM.

Exemplo:

```text
WAZUH-4.14.7-CLEAN
```

Estado esperado do snapshot:

- Manager validado;
- Indexer validado;
- Dashboard validado;
- nenhum agente adicional;
- nenhuma customização do laboratório.

## Resultado

A OVA foi considerada apta quando:

- Manager estava ativo;
- Indexer estava ativo;
- Dashboard estava ativo;
- versão foi confirmada;
- memória e armazenamento foram verificados;
- portas essenciais foram identificadas;
- acesso ao Dashboard funcionou.

## Referências

- https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html
- https://documentation.wazuh.com/current/user-manual/manager/event-logging.html
