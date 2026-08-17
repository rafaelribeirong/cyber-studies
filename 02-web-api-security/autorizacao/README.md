# Fundamentos de falhas de autorização em aplicações e APIs

## Autenticação e autorização

Autenticação responde à pergunta: **quem é o usuário?**

Autorização responde: **o que esse usuário pode fazer com este recurso, neste contexto?**

Uma aplicação pode autenticar corretamente e ainda possuir falhas graves de autorização. Isso acontece quando o servidor aceita uma ação sem validar adequadamente o usuário, o objeto, a função ou a organização envolvida.

## Principais categorias

| Categoria | Pergunta de teste | Exemplo de impacto |
|---|---|---|
| IDOR/BOLA | Um usuário consegue acessar o objeto de outro? | Exposição ou alteração de dados |
| Autorização horizontal | Usuários do mesmo nível estão isolados? | Acesso à conta de outro usuário |
| Autorização vertical | Usuário comum executa função administrativa? | Escalada de privilégio |
| Isolamento multi-tenant | Uma empresa acessa dados de outra? | Vazamento entre clientes |
| BOPLA | A API expõe propriedades não autorizadas? | Visualização de campos sensíveis |
| Mass Assignment | O cliente altera propriedades protegidas? | Mudança de papel ou proprietário |
| Lógica de negócio | A ação respeita estado, sequência e contexto? | Aprovação ou operação indevida |

## Modelo mental para análise

Antes de testar, é necessário identificar:

- **Identidades:** quais usuários participam do fluxo
- **Papéis:** quais funções cada usuário possui
- **Objetos:** quais registros ou recursos são manipulados
- **Ações:** o que pode ser lido, criado, alterado ou excluído
- **Propriedades:** quais campos são públicos, privados ou administrativos
- **Contexto:** organização, proprietário, estado do processo e relacionamentos

## Fluxo de teste em laboratório autorizado

1. Criar dois usuários comuns e, quando disponível, um usuário administrativo
2. Executar a mesma ação com cada perfil
3. Registrar as requisições e respostas esperadas
4. Identificar parâmetros, objetos e propriedades controlados pelo cliente
5. Comparar as decisões de acesso do servidor
6. Alterar apenas um elemento por teste
7. Confirmar o impacto sem acessar mais dados do que o necessário
8. Registrar a evidência de forma sanitizada

Uma diferença de resposta não prova sozinha uma vulnerabilidade. É necessário confirmar que houve acesso, alteração ou execução não autorizada e descartar comportamentos esperados da aplicação.

## Causa comum

A interface pode esconder botões e páginas, mas isso não representa um controle de segurança. A decisão deve ser validada no servidor para cada requisição e para cada objeto.

Outras causas frequentes incluem:

- Consultas sem filtro pelo proprietário ou organização
- Confiança em identificadores enviados pelo cliente
- Validação apenas do papel, sem validar o objeto
- Modelos internos expostos diretamente pela API
- Regras inconsistentes entre endpoints
- Permissões verificadas apenas na interface

## Prevenção

- Negar acesso por padrão
- Validar autorização no servidor em todas as ações
- Aplicar controles no nível do objeto e da propriedade
- Vincular consultas ao usuário e ao tenant autorizados
- Não confiar em papéis, proprietários ou organizações enviados pelo cliente
- Utilizar contratos de entrada e saída com campos explicitamente permitidos
- Criar testes automatizados negativos para cada papel
- Registrar eventos relevantes para investigação e auditoria

## Conexão com GRC

A vulnerabilidade é técnica, mas seu impacto deve ser analisado no contexto do negócio. Uma falha de autorização pode afetar confidencialidade, privacidade, contratos, conformidade e confiança dos clientes.

A documentação deve permitir responder:

- Qual ativo foi afetado?
- Quais dados ou operações estavam expostos?
- Quem poderia explorar a falha?
- Qual seria o impacto?
- Qual controle falhou?
- Como a correção será validada?
- Como evitar recorrência?

## Próximos passos da pesquisa

- Criar um laboratório de IDOR/BOLA
- Produzir uma matriz de usuários, objetos e permissões
- Criar um checklist de autorização horizontal
- Documentar um caso de isolamento entre organizações
- Comparar BOLA, BOPLA e Mass Assignment
