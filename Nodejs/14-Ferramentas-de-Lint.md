# 1 - Ferramentas de Lint

- O termo _lint_ surgiu em programação da necessidade de implementar algum tipo de checagem automática para previnir e/ou solucionar erros enquanto escrevemos códigos
- **_Lint_** ou **_Linter_** é um programa que executa **checagem de código estático** em procura de erros programáticos e estilísticos
- Depura o código em busca de **possíveis bugs**, verifica **erros de sintaxe**, **complexidade ciclomática** de código

## Exemplo de erros programáticos:

- Falta de ";" quando requerido
- Variáveis declaradas e não usadas
- Uso de variáveis antes de declaradas
- Execução de código depois de declarado retorno em funções
- Uso de imports descontinuados ou não seguros
- Loops infinitos

## Exemplo de erros de estilo:

- Espaçamento indevido
- Falta de indentação
- Ultrapassagem de limites de caracteres por linha
- Uso misto de tipos de aspas (dupla e simples)

# 2 - O que é o ESLint?

- É um projeto open-source criado por Nicholas C. Zakas em 2013
- É uma ferramenta "plugável" para linting em JavaScript
- Já vem com regras padrão para análise de código
- Novas regras podem ser adicionadas ou desativadas de acordo com a necessidade

```
npm i eslint
./node_modules/.bin/eslint --init
```
