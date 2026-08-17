# Cyber Studies

Repositório público de pesquisas, laboratórios autorizados e anotações próprias sobre Segurança da Informação, segurança de aplicações web e APIs, Blue Team e GRC.

O objetivo é documentar minha evolução técnica e conectar vulnerabilidades, impacto ao negócio, riscos e controles de segurança. O conteúdo é autoral e não reproduz materiais de cursos, normas ou ambientes de clientes.

## Foco atual: falhas de autorização

Minha especialização inicial em Bug Bounty está concentrada em uma família de vulnerabilidades:

1. IDOR e BOLA
2. Autorização horizontal e vertical
3. Isolamento entre organizações, empresas e clientes
4. BOPLA e Mass Assignment
5. Lógica de negócio relacionada a permissões
6. Posteriormente: autenticação, sessão e JWT

Essa escolha permite estudar profundamente como aplicações web e APIs decidem quem pode visualizar, alterar ou executar cada ação.

## Trilha de pesquisa

| Etapa | Tema | Status |
|---|---|---|
| 1 | HTTP, requisições, respostas e proxies | Em andamento |
| 2 | Identidade, papéis, objetos e permissões | Em andamento |
| 3 | IDOR/BOLA e autorização horizontal | Planejado |
| 4 | Autorização vertical e funções administrativas | Planejado |
| 5 | Isolamento multi-tenant | Planejado |
| 6 | BOPLA, Mass Assignment e propriedades sensíveis | Planejado |
| 7 | Lógica de negócio relacionada a permissões | Planejado |
| 8 | Autenticação, sessão e JWT | Futuro |

## Publicações

- [Roadmap de pesquisa em autorização](00-roadmap/foco-autorizacao.md)
- [Segurança da Informação e Cibersegurança](01-fundamentos/seguranca-da-informacao-e-ciberseguranca.md)
- [Fundamentos de falhas de autorização em aplicações e APIs](02-web-api-security/autorizacao/README.md)

## Como as pesquisas serão documentadas

Cada publicação deve apresentar:

- Contexto e objetivo
- Conceitos estudados
- Hipótese ou risco avaliado
- Ambiente autorizado utilizado
- Procedimento executado
- Evidências sem informações sensíveis
- Resultado e aprendizado
- Impacto técnico e de negócio
- Recomendações de correção
- Relação com riscos e controles de segurança
- Referências consultadas

## Estrutura do repositório

```text
cyber-studies/
├── 00-roadmap/                  # foco, sequência e progresso
├── 01-fundamentos/              # redes, HTTP, Linux e conceitos essenciais
├── 02-web-api-security/         # pesquisas de Web e API Security
│   └── autorizacao/             # especialização atual
├── 03-blue-team/                # detecção, Wazuh e SIEM
├── 04-grc/                      # riscos, controles e integração técnico-GRC
├── templates/                   # modelos para novas publicações
└── ETHICS.md                    # limites éticos e regras de publicação
```

As pastas futuras serão adicionadas conforme houver conteúdo real, evitando uma estrutura composta apenas por arquivos vazios.

## Modelos disponíveis

- [Modelo de pesquisa](templates/modelo-pesquisa.md)
- [Modelo de laboratório](templates/modelo-laboratorio.md)

## Uso ético

As pesquisas são realizadas somente em laboratórios próprios, aplicações deliberadamente vulneráveis ou programas com autorização explícita. Não são publicados dados de clientes, credenciais, informações pessoais, códigos internos, detalhes de ambientes reais ou descobertas sem autorização de divulgação.

Consulte as [regras éticas do repositório](ETHICS.md).

## Autor

Rafael Ribeiro  
[LinkedIn](https://www.linkedin.com/in/rafaelribeirong/)
