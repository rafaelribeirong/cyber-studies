# 02 — Implantação

Esta seção documenta formas de implantar os componentes centrais do Wazuh e validar o ambiente antes de adicionar endpoints.

## Métodos que serão estudados

A documentação oficial apresenta diferentes alternativas de deployment dos componentes centrais.

| Método | Estado no projeto |
|---|---|
| OVA / Virtual Machine | concluído |
| instalação assistida | planejado |
| instalação passo a passo | planejado |
| Docker single-node | planejado |
| Docker multi-node | planejado |
| Kubernetes | planejado |
| AMI / AWS | planejado |
| instalação offline | planejado |
| instalação a partir do código-fonte | planejado |

Referência:
https://documentation.wazuh.com/current/deployment-options/index.html

## OVA

A OVA é uma máquina pré-construída contendo os componentes centrais do Wazuh. É adequada para laboratório, avaliação e cenários em que uma implantação single-node atende ao objetivo.

A documentação oficial informa, para a OVA 4.14.7:

- Amazon Linux 2023;
- Wazuh manager 4.14.7;
- Wazuh indexer 4.14.7;
- Wazuh dashboard 4.14.7;
- arquitetura x86_64/AMD64;
- 4 CPUs;
- 8 GB RAM;
- 50 GB de armazenamento configurado na VM.

A OVA não fornece alta disponibilidade ou escalabilidade distribuída por padrão.

Referência:
https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html

## Laboratórios concluídos

- [Validação do ambiente Wazuh 4.14.7](validacao-ambiente.md)

## Próximas implantações

A próxima instalação central será feita pelo método assistido ou passo a passo, mantendo a OVA atual para os laboratórios funcionais.

Referências:

- instalação assistida:
  https://documentation.wazuh.com/current/installation-guide/wazuh-server/installation-assistant.html
- instalação passo a passo:
  https://documentation.wazuh.com/current/installation-guide/wazuh-server/step-by-step.html
- Docker:
  https://documentation.wazuh.com/current/deployment-options/docker/index.html
- Kubernetes:
  https://documentation.wazuh.com/current/deployment-options/deploying-with-kubernetes/index.html
- offline:
  https://documentation.wazuh.com/current/deployment-options/offline-installation/index.html
