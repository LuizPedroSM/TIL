# 1 - SQL vs NoSQL

- Existem várias comparações de desmpenho realizadas entre NoSQL e o SQL.
- A velocidade geralmente é o fator mais importante na decisão de qual banco utilizar.
- Há muitas opções de bancos de dados NoSQL com diferentes funcionalidades que podem ser muito úteis.

## O que é o SQL?

- Sigla para "Structed Query Language" (Linguagem de Consulta Estruturada).
- É uma linguagem de consulta a banco de dados relacionais.
- Executa comandos para criar, consultar, atualizar e excluir informações no seu banco de dados (CRUD).
- Segue uma modelagem relacional, pelo fato de que todos os dados são guardados em tabelas.

## O que é o NoSQL?

- Sigla para "Not Only SQL" (Não apenas SQL).
- É o termo utilizado para banco de dados não relacionais de alto desmpenho, onde geralmente não é utilizado o SQL como linguagem de consulta.
- Criado para ter uma performace melhor e uma escalabilidade mais horizontal.

## Tipos de bancos de dados NoSQL

### Documento

- Dados são armazenados como documentos.
- Podem ser descritos como dados no formato de chave-valor, como o padrão JSON. (MongoDB)

### Colunas

- Dados armazenados em linhas particulares de tabela no disco.
- Suporta várias linhas e colunas, bem como sub-colunas. (Cassandra)

### Grafos

- Dados são armazenados na forma de grafos (vértices e arestas). (Neo4j)

### Chave-valor

- Suporta mais carga de dados.
- O conceito é que um determinado valor seja acessado através de uma chave identificadora única. (Riak)

### Resumo

- SQL se baseia no fato de que todos os dados sejam guardados em tabelas.
- NoSQL não se aplica o conceito de schema: uma chave de valor é que é utilizada para recuperar valores, conjunto de colunas ou documentos.

## Diferença entre NoSQL e SQL

- O NoSQL agrupa toda a informação e a guarda no mesmo registro.
- No SQL você precisa ter o relacionamento entre várias tabelas para ter a informação (entidade e relacionamento).
- O SQL tem certa dificuldade em conciliar a demanda por escalabilidade.
- No NoSQL, deve se levar em consideração a modelagem do sistema.
- Um ponto forte do SQL é quanto à consistência das informações.
- O NoSQL garante o último valor atualizado, se não for realizada nenhuma atualização antes da consulta
- Quanto à segurança, ambos estão suscetíveis a ataques
