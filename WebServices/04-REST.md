# 1 - REST

- REST - Representational State Transfer.
- É um estilo de arquitetura de software que define a implementação de um serviço web.
- Podem trabalhar com os formatos XML, JSON ou outros.

## Vantagens

- Permite integrações entre aplicações e também entre cliente e servidor em páginas web e aplicações.
- Utiliza dos métodos HTTP para definir a operação que está sendo efetuada.
- Arquitetura de fácil compreensão.

## Estrutura do REST

- Cliente
  - Requisição HTTP
    - GET, POST, PUT, DELETE ...
- Servidor
  - Retorna mensagem
    - Texto, JSON, XML

> Quando uma aplicação web disponibiliza um conjunto de rotinas e padrões através de serviços web podemos chamar esse conjunto de API.

## API

- API - Application Programming Interface.
- São conjuntos de rotinas documentados e dispoibilizados por uma alicação para que outras aplicações possam consumir suas funcionalidades
- Ficou popular com o aumento dos serviços web.
- As maiores plataformas de tecnologia disponibilizam API's para acessos de suas funcionalidades algumas delas são: Facebook, Twitterm Telegram, Whatsapp, Github...

## Principais Métodos HTTP

- GET - Solicita a represntação de um recurso.
- POST - Solicita a criação de um recurso.
- DELETE - Solicita a exclusão de um recurso.
- PUT - Solicita a atualização de um recurso.

## JSON

- JSON - JavaScript Object Notation.
- Formatação leve utilizada para troca de mensagens entre sistemas.
- Usa-se de uma estrutura de chave e valor e também de listas ordenadas.
- Um dos formatos mais populares e mais utilzados para troca de mensagens entre sistemas.

```JSON
{
    "nome": "Os vingadores",
    "ano_lancamento": "2019",
    "personagens":[
        {
            "nome": "Thanos"
        },
        {
            "nome": "Homem de Ferro"
        },
        {
            "nome": "Thor"
        }
    ]
}
```

## Código de Estado

- Usado pelo servidor para avisar o cliente sobre o estado da operação solicitada.

  - 1xx - Informativo
  - 2xx - Sucesso
  - 3xx - Redirecionamento
  - 4xx - Erro do Cliente
  - 5xx - Erro do Servidor

[HTTP Codes](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/Status)
