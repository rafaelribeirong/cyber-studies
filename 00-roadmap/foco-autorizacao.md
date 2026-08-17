# Roadmap de pesquisa: falhas de autorização

## Objetivo

Desenvolver profundidade em falhas de autorização de aplicações web e APIs, evitando estudar muitas classes de vulnerabilidade ao mesmo tempo.

O foco é compreender como identidades, papéis, objetos, propriedades e regras de negócio se relacionam e como decisões incorretas de autorização geram exposição ou alteração indevida de dados.

## Escopo inicial

1. **HTTP e ferramentas de análise**
   - Métodos, cabeçalhos, parâmetros, cookies e corpos de requisição
   - Códigos de resposta e diferenças de comportamento
   - Repetição e comparação de requisições em proxy interceptador

2. **Modelo de acesso**
   - Usuário, sessão, papel e permissão
   - Objeto, proprietário e organização
   - Ações de leitura, criação, alteração e exclusão

3. **IDOR e BOLA**
   - Referências previsíveis ou controladas pelo cliente
   - Acesso a objetos pertencentes a outro usuário
   - Ausência de validação no servidor

4. **Autorização horizontal e vertical**
   - Comparação entre dois usuários do mesmo nível
   - Comparação entre usuário comum e administrador
   - Endpoints e funções não apresentados pela interface

5. **Isolamento multi-tenant**
   - Separação de dados entre empresas ou clientes
   - Uso inseguro de identificadores de organização
   - Escopo de consultas e permissões por tenant

6. **BOPLA e Mass Assignment**
   - Propriedades sensíveis expostas ou alteráveis
   - Campos como papel, status, proprietário e organização
   - Diferença entre o modelo interno e o contrato público da API

7. **Lógica de negócio**
   - Regras de aprovação
   - Mudanças de estado
   - Ações fora de sequência
   - Permissões dependentes do contexto

8. **Etapa posterior**
   - Autenticação
   - Sessões
   - Recuperação de conta
   - JWT

## Método de estudo

Para cada tema:

1. Estudar os conceitos
2. Criar ou utilizar um laboratório autorizado
3. Mapear usuários, papéis, objetos e ações
4. Formular hipóteses de falha
5. Comparar requisições e respostas
6. Registrar evidências sanitizadas
7. Avaliar impacto técnico e de negócio
8. Documentar a correção esperada
9. Relacionar a descoberta com riscos e controles

## Critérios de conclusão

Um tema será considerado concluído quando houver:

- Uma explicação autoral
- Pelo menos um laboratório documentado
- Um checklist próprio de testes
- Um exemplo de impacto
- Recomendações de prevenção
- Uma publicação resumida para o LinkedIn

## Limites

Nenhum teste será realizado fora do escopo autorizado. Informações de clientes, alvos reais, credenciais ou evidências sensíveis não serão armazenadas neste repositório.
