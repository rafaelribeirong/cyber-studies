# Wazuh Lab — Segurança, Monitoramento e Evidências para SGSI/SGPI

## Objetivo

Este projeto documenta um laboratório prático de Wazuh com dois objetivos complementares:

1. estudar a plataforma Wazuh em sua versão atual, entendendo arquitetura, agentes, inventário, detecção, monitoramento e resposta;
2. avaliar de forma prática como o Wazuh pode implementar controles técnicos, apoiar processos e produzir evidências para um SGSI e um SGPI.

O laboratório não considera o Wazuh como uma solução que, isoladamente, "atende à ISO". Cada caso será classificado como:

- **Implementação técnica** — o Wazuh executa parte relevante do controle técnico;
- **Apoio / evidência** — o Wazuh fornece monitoramento, inventário, logs ou evidências para um processo maior;
- **Fora do escopo** — depende principalmente de pessoas, processos, contratos, políticas ou outras tecnologias.

## Referências normativas

- ISO/IEC 27001:2022
- ISO/IEC 27001:2022/Amd 1:2024
- ISO/IEC 27701:2025

> Este repositório não reproduz o texto das normas ISO. Os mapeamentos são interpretações técnicas para fins de estudo e devem ser validados contra cópias licenciadas das normas quando utilizados profissionalmente.

## Plataforma do laboratório

- VMware Workstation
- Wazuh OVA oficial
- Wazuh 4.14.x
- Wazuh Manager
- Wazuh Indexer
- Wazuh Dashboard
- Endpoints Windows e Linux
- Sysmon em laboratório Windows quando aplicável

## Estrutura

```text
wazuh/
├── README.md
├── 01-laboratorio/
│   └── README.md
└── 02-conformidade-iso/
    └── README.md
```

## Roadmap técnico

1. Implantação da OVA
2. Arquitetura e componentes
3. Cadastro e gerenciamento de agentes
4. IT Hygiene e inventário
5. Vulnerability Detection
6. Security Configuration Assessment (SCA)
7. File Integrity Monitoring (FIM)
8. Coleta e análise de logs
9. Windows + Sysmon
10. Regras e decoders personalizados
11. MITRE ATT&CK
12. Active Response
13. API e RBAC
14. Integrações e automações
15. Construção de evidências para auditoria

## Fontes principais

- Documentação oficial do Wazuh: https://documentation.wazuh.com/current/
- Máquina virtual oficial: https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html
- Release notes: https://documentation.wazuh.com/current/release-notes/index.html
- ISO/IEC 27001: https://www.iso.org/standard/27001.html
- ISO/IEC 27701: https://www.iso.org/standard/27701.html

## Princípio do projeto

A meta não é apenas gerar alertas. O laboratório deve demonstrar o ciclo completo:

```text
Ativo
  ↓
Inventário
  ↓
Configuração / Hardening
  ↓
Monitoramento
  ↓
Detecção
  ↓
Investigação
  ↓
Resposta
  ↓
Evidência
  ↓
Melhoria contínua
```

Cada laboratório executado deverá registrar objetivo, configuração, teste, resultado, evidência, relação com riscos e possível relação com requisitos ou controles de segurança e privacidade.