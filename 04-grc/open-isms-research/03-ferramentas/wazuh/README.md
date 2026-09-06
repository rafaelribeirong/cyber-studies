# Wazuh — Laboratório, operação e conformidade

## Objetivo

Este diretório reúne estudos práticos sobre Wazuh com foco em:

- implantação e administração da plataforma;
- monitoramento e detecção;
- hardening e vulnerabilidades;
- resposta a eventos;
- geração de evidências técnicas;
- apoio a controles e processos relacionados à ISO/IEC 27001 e ISO/IEC 27701.

O objetivo é manter dois tipos de conteúdo:

1. **estudo técnico**, com testes, erros, troubleshooting e aprendizado;
2. **guia reutilizável para clientes**, contendo apenas procedimentos já testados e validados.

## Estrutura

```text
wazuh/
├── README.md
├── 01-fundamentos/
├── 02-implantacao/
├── 03-agentes/
├── 04-monitoramento/
├── 05-deteccao-e-resposta/
├── 06-administracao/
├── 07-conformidade/
└── 08-guias-para-clientes/
```

## Padrão dos estudos

Cada laboratório deve registrar, quando aplicável:

1. objetivo;
2. pré-requisitos;
3. ambiente utilizado;
4. procedimento executado;
5. comandos e configurações;
6. validação;
7. resultado esperado;
8. erros encontrados;
9. troubleshooting;
10. evidências geradas;
11. relação com riscos e controles;
12. referências oficiais.

## Padrão dos guias para clientes

Os guias para clientes devem conter apenas procedimentos previamente testados no laboratório.

Estrutura recomendada:

1. objetivo;
2. aplicabilidade;
3. pré-requisitos;
4. procedimento;
5. validação;
6. resultado esperado;
7. evidências;
8. troubleshooting;
9. rollback;
10. referências;
11. relação com ISO/IEC 27001 e ISO/IEC 27701, quando aplicável.

## Classificação de relação com controles

Para evitar afirmar que uma ferramenta atende isoladamente a um requisito normativo, os mapeamentos devem usar uma das classificações abaixo:

- **IMPLEMENTA** — a funcionalidade executa parte relevante de um controle técnico;
- **APOIA** — a funcionalidade auxilia um processo maior;
- **EVIDÊNCIA** — a funcionalidade produz registros ou informações úteis para comprovação;
- **FORA DO ESCOPO** — o atendimento depende principalmente de processo, pessoas, contrato, política ou outra tecnologia.

## Referências principais

- Documentação oficial do Wazuh: https://documentation.wazuh.com/current/
- Release notes do Wazuh: https://documentation.wazuh.com/current/release-notes/index.html
- ISO/IEC 27001
- ISO/IEC 27701

> Este projeto não reproduz o conteúdo integral das normas ISO. Os mapeamentos representam análise técnica e devem ser validados contra cópias licenciadas das normas quando utilizados profissionalmente.
