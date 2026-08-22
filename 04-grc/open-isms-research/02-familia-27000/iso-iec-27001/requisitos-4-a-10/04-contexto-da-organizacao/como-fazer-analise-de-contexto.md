# Como realizar uma análise de contexto para o SGSI

**Status:** guia prático inicial  
**Versão:** 0.1  
**Última revisão:** 22 de agosto de 2026  
**Autor:** Rafael Ribeiro

## Objetivo

Este guia apresenta uma forma prática de produzir a análise exigida pelo item 4.1 da ISO/IEC 27001:2022.

Não existe um modelo único obrigatório. O método precisa gerar informações confiáveis, relevantes, atualizadas e conectadas aos demais processos do Sistema de Gestão de Segurança da Informação (SGSI).

## 1. Preparação

Defina:

- responsável pela condução;
- participantes;
- escopo preliminar da análise;
- período considerado;
- fontes internas e externas;
- critérios de relevância;
- forma de aprovação;
- frequência de revisão.

Participantes normalmente incluem direção, Segurança da Informação, Tecnologia, Recursos Humanos, Jurídico, Compras, Operações, Comercial e responsáveis por continuidade.

## 2. Informações de entrada

### Fontes internas

- planejamento estratégico;
- organograma;
- mapas de processo;
- catálogo de produtos e serviços;
- inventário de ativos;
- arquitetura tecnológica;
- incidentes;
- auditorias;
- indicadores;
- riscos existentes;
- reclamações;
- mudanças organizacionais;
- contratos relevantes;
- entrevistas com responsáveis.

### Fontes externas

- legislação e regulamentos;
- requisitos de clientes;
- relatórios de ameaças;
- mudanças tecnológicas;
- mercado;
- fornecedores;
- notícias setoriais;
- cenários econômicos;
- eventos ambientais;
- requisitos de grupos econômicos;
- orientações de autoridades.

## 3. PESTEL

A [PESTLE analysis do Chartered Institute of Personnel and Development](https://www.cipd.org/en/knowledge/factsheets/pestle-analysis-factsheet/) organiza a pesquisa do ambiente externo.

| Letra | Categoria | Perguntas para o SGSI |
|---|---|---|
| P | Política | Existem conflitos, sanções, instabilidade ou políticas públicas que afetem operações ou fornecedores? |
| E | Econômica | Câmbio, inflação, orçamento ou mercado de trabalho afetam licenças, equipe ou serviços? |
| S | Social | Trabalho remoto, cultura, comportamento e escassez de profissionais afetam a segurança? |
| T | Tecnológica | Quais mudanças, ameaças, obsolescências e dependências tecnológicas são relevantes? |
| E | Ambiental | Eventos climáticos ou ambientais afetam instalações, energia, pessoas ou fornecedores? |
| L | Legal | Quais leis, regulamentos, contratos e decisões de autoridades afetam a informação? |

### Como fazer

1. Defina setor, localidades e período.
2. Pesquise todas as categorias.
3. Registre a fonte de cada fator.
4. Descreva o fator de modo específico.
5. Avalie sua relação com o SGSI.
6. Classifique sua relevância.
7. Defina ação ou acompanhamento.

### Exemplo

| Categoria | Fator externo | Fonte | Efeito possível | Relevância |
|---|---|---|---|---|
| Tecnológica | aumento de ataques a credenciais em nuvem | relatório de ameaças | acesso indevido | Alta |
| Legal | novo prazo contratual para comunicar incidentes | contrato | alteração do processo de resposta | Alta |
| Econômica | aumento de custo de licenças | orçamento | redução de ferramentas | Média |
| Ambiental | enchentes na região da equipe | histórico local | indisponibilidade de pessoas | Média |

## 4. SWOT

A [SWOT analysis do Chartered Institute of Personnel and Development](https://www.cipd.org/en/knowledge/factsheets/swot-analysis-factsheet/) separa fatores internos e externos:

- Strengths - forças internas;
- Weaknesses - fraquezas internas;
- Opportunities - oportunidades externas;
- Threats - ameaças externas.

### Perguntas

| Quadrante | Perguntas |
|---|---|
| Forças | Quais capacidades ajudam o SGSI a atingir seus resultados? |
| Fraquezas | Quais limitações internas dificultam segurança, conformidade ou continuidade? |
| Oportunidades | Quais mudanças externas podem melhorar proteção, eficiência ou confiança? |
| Ameaças | Quais fatores externos podem causar incidentes, perdas ou descumprimentos? |

### Exemplo preenchido

| Forças | Fraquezas |
|---|---|
| apoio da direção | dependência de um administrador |
| autenticação multifator | processo de mudanças informal |
| monitoramento centralizado | inventário incompleto |

| Oportunidades | Ameaças |
|---|---|
| automação de respostas | aumento de ransomware |
| treinamento especializado | ataques a fornecedores |
| certificação ISO/IEC 27001 | escassez de profissionais |

### Limitação da SWOT

A SWOT não conclui sozinha quais fatores são relevantes nem o que deve ser feito. Após o preenchimento, cada item precisa ser avaliado e encaminhado.

## 5. Oficina de contexto

### Roteiro sugerido

1. Apresentar objetivo e escopo.
2. Revisar estratégia, produtos e serviços.
3. Apresentar informações coletadas.
4. Preencher PESTEL.
5. Preencher SWOT.
6. Eliminar duplicidades.
7. Transformar expressões genéricas em questões específicas.
8. Avaliar relação com o SGSI.
9. Classificar relevância.
10. Definir encaminhamentos e responsáveis.
11. Aprovar o resultado.

Uma afirmação como “tecnologia muda rapidamente” é genérica. Uma questão útil seria “o sistema principal utiliza componente sem suporte a partir de dezembro de 2026, podendo aumentar vulnerabilidades e indisponibilidade”.

## 6. Critério de relevância

Um modelo simples pode usar duas perguntas, com notas de 1 a 5:

- impacto potencial sobre os resultados do SGSI;
- possibilidade de a questão produzir efeito no período analisado.

Multiplique as notas:

| Resultado | Classificação |
|---:|---|
| 1 a 4 | Baixa |
| 5 a 9 | Média |
| 10 a 16 | Alta |
| 17 a 25 | Crítica |

Essa escala é um exemplo autoral, não uma exigência da ISO.

Também é possível considerar automaticamente relevante uma questão que:

- imponha obrigação legal ou contratual aplicável;
- possa alterar o escopo;
- possa comprometer serviço crítico;
- possa afetar confidencialidade, integridade ou disponibilidade de forma significativa;
- exija decisão da direção.

## 7. Modelo de matriz de contexto

| ID | Tipo | Categoria | Questão | Fonte | Efeito no SGSI | Impacto | Possibilidade | Relevância | Encaminhamento | Responsável | Revisão |
|---|---|---|---|---|---|---:|---:|---|---|---|---|
| CTX-01 | Externa | Tecnológica | aumento de ataques a credenciais | relatório de ameaças | acesso indevido | 5 | 4 | Crítica | avaliar risco e revisar autenticação | Segurança | Trimestral |
| CTX-02 | Interna | Pessoas | dependência de um administrador | entrevista com TI | indisponibilidade e perda de conhecimento | 4 | 4 | Alta | plano de sucessão e documentação | TI | Semestral |

## 8. Encaminhamento

Uma questão relevante pode gerar:

- risco ou oportunidade do SGSI;
- risco de segurança da informação;
- mudança no escopo;
- novo objetivo;
- revisão de política;
- controle adicional;
- plano de continuidade;
- revisão de fornecedor;
- necessidade de competência;
- comunicação;
- monitoramento periódico.

Nem toda questão precisa virar risco. O encaminhamento depende de sua natureza.

## 9. Aprovação e revisão

Registre:

- participantes;
- data;
- fontes;
- critérios;
- decisões;
- justificativas;
- responsáveis;
- aprovador;
- próxima revisão.

Revise periodicamente e quando ocorrer:

- mudança estratégica;
- novo produto ou serviço;
- aquisição ou fusão;
- incidente relevante;
- alteração legal ou contratual;
- mudança tecnológica;
- novo fornecedor crítico;
- alteração de localidade;
- evento ambiental relevante.

## Canal e materiais complementares

Espaço reservado para futuro vídeo explicativo do autor.

## Fontes de apoio

- [ISO/IEC 27001:2022](https://www.iso.org/standard/27001)
- [CIPD - SWOT analysis](https://www.cipd.org/en/knowledge/factsheets/swot-analysis-factsheet/)
- [CIPD - PESTLE analysis](https://www.cipd.org/en/knowledge/factsheets/pestle-analysis-factsheet/)
- [U.S. Economic Development Administration - SWOT analysis](https://www.eda.gov/resources/comprehensive-economic-development-strategy/content/swot-analysis)

## Histórico

| Versão | Data | Alteração |
|---|---|---|
| 0.1 | 22/08/2026 | Estrutura inicial |
