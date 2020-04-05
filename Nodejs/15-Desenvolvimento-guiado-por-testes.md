# 1- Desenvolvimento Guiado por Testes

- O TDD (Test Driven Development) é um processo de software que visa o feedback rápido e validação do comportamento da aplicação
- Os testes se tornam consequência do processo, pois determina o comportamento esperado da implementação
- É dividido em ciclos que são conhecidos como: **Red**, **Green** e **Refactor**

## Construindo APIs Testáveis com Node.js

https://leanpub.com/construindo-apis-testaveis-com-nodejs/read

## Ciclos do TDD

### Red

- O teste é criado antes da funcionalindade ser implementada
- O teste deve quebrar, pois a implementação não existe
- Nesta fase também é verificado erros de sintáxe e semântica

### Green

- A funcionalidade é adicionada para que o teste passe
- A lógica ainda não é necessária, apenas que atenda os requisitos do teste
- Podem ser adicionados **TODOs**, **FIXMEs** e dados estáticos, sendo o suficiente para o teste passar

### Refactor

- A lógica necessária é adicionada ao código
- Com o teste validado nos ciclos anteriores, garantirá que a funcionalidade está sendo implementada corretamente
- Devem ser removidos os dados estáticos e ser feita a implementação real para que o teste volte a passar

```
Vermelho: Escreve o teste que irá falhar
Verde: Implementa o suficiente para passar
Refatora: Refatora com a lógica necessária sem mudar o comportamento
```

## Pirâmide de Testes

- Conceito por Mike Cohn, escritor do livro Succeeding with Agile
- Propõe mais testes de baixo nível (testes de unidade), testes de integração e os testes que envolvem interfaces

```
slow                    $$$
      -----UI-----
     --Integration--
    ------UNIT-------
fast                    $
```

## Tipos de Testes

### Unidade (Unit Tests)

- São de baixo nível, com foco em pequenas partes do software
- Tendem a ser mais rapidamente executados quando comparados com outros testes, pos testam partes isoladas
- O que definde uma unidade é o comportamento e a facilidade de ser isolada das suas dependências

### Integração (Integration Tests)

- Servem para verificar se a comunicação entre os componentes de um sistema está ocorrendo conforme o esperado
- Diferente dos testes de unidade, deve ser testado o comportamento da interação entre as unidades
- O teste pode ser em qualquer nível, seja a interação entre camadas, classes ou até mesmo serviços

### Integração de Contrato(Integration Contract Tests)

- Ganharam força devido ao crescimento das APIs e dos micro serviços
- Garante que os dados que são consumidos de serviços externos continuam compatíveis e funcionando

#### My APP

> expected response

```
{
    "email": String,
    "birthday": Date,
}
```

#### Users Service

> response

```
{
    "name": "John",
    "email": "john@mail.com",
    "born": "23/01/1990",
}
```

> teste quebra

# Prática

[git](https://github.com/hmschreiner/node-tdd)
