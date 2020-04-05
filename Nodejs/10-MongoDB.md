# 1 - O que é o MongoDB?

- Banco de dados orientado a documentos (document-based) no formato JSON
- É o mais usado no mercado
- Não possui como restrição a necessidade de ter as tabelas e colunas criadas previamente
- É possível armazenar registros sem se preocupar com a estrutura de dados (número de campos, tipos, etc.)
- Guarda dados em documentos ao ínves de tabelas
- Os documentos são agrupados em collection
- Um conjunto de collectons forma um banco de dados

```JSON
{
    name: "sue",
    age: "26",
    status: "A",
    groups:["news", "sports"]
}
```

## Recursos do MongoDb

- **Sharing**: usado para dividir os dados de uma collection entre mais de um servidor
- **Capped Collections**: collections com tamanhos predefinidos e com conteúdo rotativo

## Mongoose

- Solução baseada em esquemas para modelar os dados de uma aplicação
- Possui sistema de conversão de tipos, validação, criação de consultas e hooks para lógica de negócios
- Fornece um mapeamento de objetos do MongoDB similar ao ORM (Object Relational Mapping), conhecido como ODM (Object Data Mapping)
- Traduz os dados do banco de dados para objetos JavaScript para serem utilizados pela aplicação

# Prática

[git](https://github.com/hmschreiner/node-mongoose)
