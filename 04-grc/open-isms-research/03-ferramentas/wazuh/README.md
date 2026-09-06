# Wazuh — laboratório, operação e conformidade

## Objetivo

Esta área documenta um laboratório progressivo de Wazuh com foco em entender a plataforma na prática, registrar procedimentos reproduzíveis e relacionar capacidades técnicas com segurança da informação e sistemas de gestão.

A regra do projeto é simples:

> entender → configurar → validar → registrar evidência → documentar → relacionar com controles, quando aplicável.

Sempre que o Dashboard oferecer a função necessária, o procedimento será documentado pelo **Wazuh Dashboard**. CLI, API e edição direta de arquivos serão usados quando a interface não oferecer o recurso, quando forem necessários para validação de baixo nível ou para troubleshooting.

## Ambiente-base validado

O laboratório inicial utiliza:

- Wazuh 4.14.7;
- OVA oficial;
- Wazuh Manager;
- Wazuh Indexer;
- Wazuh Dashboard;
- VirtualBox;
- primeiro agente Windows 11.

A documentação oficial da VM confirma que a OVA 4.14.7 inclui os componentes centrais pré-instalados e pode ser importada em VirtualBox ou outro hypervisor compatível com OVA.

Referência:
https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html

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
└── 07-conformidade/
```

## Estado atual

| Área | Estado |
|---|---|
| Fundamentos iniciais | em andamento |
| Implantação por OVA | validada |
| Configuração-base do Manager | baseline inicial validada |
| Agente Windows | instalado e ativo |
| Grupos | validado |
| Configuração centralizada (`agent.conf`) | validada |
| Labels centralizadas | validadas |
| Stats / buffer / keep alive | validação inicial concluída |
| Monitoramento detalhado | próximo ciclo |
| Detecção e resposta | futuro |
| Conformidade | mapeamento inicial |

## Padrão de documentação

Cada estudo deve registrar, quando aplicável:

1. objetivo;
2. conceito;
3. por que o recurso existe;
4. quando usar;
5. pré-requisitos;
6. caminho pelo Dashboard;
7. configuração ou comando;
8. validação;
9. resultado esperado;
10. riscos, limitações e impacto;
11. troubleshooting;
12. evidências;
13. relação com controles, quando aplicável;
14. referências oficiais.

## Regra para evidências públicas

Nunca publicar:

- credenciais;
- tokens;
- chaves privadas;
- certificados privados;
- endereços IP internos reais;
- nomes de host sensíveis;
- números de série;
- dados pessoais.

Os exemplos públicos usam placeholders como:

```text
<WAZUH_SERVER_IP>
<WAZUH_SERVER_FQDN>
<AGENT_NAME>
<GROUP_NAME>
```

## Classificação de relação com controles

O projeto usa quatro classificações para evitar afirmar que uma ferramenta garante conformidade isoladamente:

- **IMPLEMENTA** — executa parte relevante de um controle técnico;
- **APOIA** — auxilia um processo maior;
- **EVIDÊNCIA** — produz registros úteis para comprovação;
- **FORA DO ESCOPO** — depende principalmente de processo, pessoas, contrato, política ou outra tecnologia.

## Referências principais

- Documentação oficial: https://documentation.wazuh.com/current/
- Deployment options: https://documentation.wazuh.com/current/deployment-options/index.html
- Agent administration: https://documentation.wazuh.com/current/user-manual/agent/agent-management/agent-administration.html
- `ossec.conf`: https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/index.html
- Release notes 4.14.7: https://documentation.wazuh.com/current/release-notes/release-4-14-7.html

> Os mapeamentos de ISO/IEC 27001 e ISO/IEC 27701 não reproduzem o conteúdo integral das normas. Devem ser validados contra cópias licenciadas das normas quando utilizados profissionalmente.
