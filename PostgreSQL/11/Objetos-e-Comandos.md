# 1 - Database, Schemas e Objetos

## Database
> É o banco de dados.
> Grupo de schemas e seus objetos, como tabelas, types, views, funções, entre outros.\
> Seus schemas e objetos não podem ser compartilhados entre si.\
> Cada database é separado um do outro compartilhando apenas usuários/roles e configurações do cluster PostgreSQL.

## Schemas
> É um de objetos, como tabelas, types, views, funções, entre outros.\
> É possível relacionar objetos entre diversos schemas.\
> Por exemplo: schema public e schema curso podem ter tabelas com o mesmo nome (teste por exemplo) relacionando-se entre si.

## Objetos
> São as tabelas, views, funções, types, sequences, entre outros, pertencentes aos schemas.

### Database
````SQL
CREATE DATABASE name
[ [WITH] [OWNER] [=] use_name ]
    [ TEMPLATE [=] template]
    [ ENCONDING [=] enconding]
    [ LC_COLLATE [=] lc_collate]
    [ LC_CTYPE [=] lc_ctype]
    [ TABLESPACE [=] tablespace_name]
    [ ALLOW_CONNECTIONS [=] allowconn]
    [ CONNECTION LIMIT [=] connlimit]
    [ IS_TEMPLATE [=] istemplate]

ALTER DATABASE name RENAME TO new_name
ALTER DATABASE name OWNER TO { new_owner | CURRENT_USER | SESSION_USE }
ALTER DATABASE name SET TABLESPACE new_tablespace

DROP DATABASE [nome]
````

### Schema
````SQL
CREATE SCHEMA schema_name [AUTHORIZATION role_specification]

ALTER SCHEMA name RENAME TO new_name
ALTER SCHEMA name OWNER TO { new_owner | CURRENT_USER | SESSION_USE }

DROP SCHEMA [nome]


--- Melhores práticas ---
CREATE SCHEMA IF NOT EXISTS schema_name [AUTHORIZATION role_specification]

DROP SCHEMA IF EXISTS [nome]
````

# 2 - Tabelas, Colunas e Tipos de dados
> Definição: \
> Conjuntos de dados dispostos em colunas e linhas referentes a um objetivo comum.\
> As colunas são consideradas como "campos da tabela", como atributos da tabela.\
> As linhas de uma tabelas são chamadas também de tuplas, e é onde estão contidos os valores, os dados.


### Exemplo:
````
TABELA = automovel
COLUNA 1 = tipo (carro, moto, aviao, helicoptero)
COLUNA 2 = ano_fabricacao (2010, 2011, 2020)
COLUNA 3 = capacidade_pessoas (1, 2, 350)
COLUNA 4 = fabricante (Honda, Avianca, Yamaha)

TABELA = produto
COLUNA 1 = codigo serial do produto
COLUNA 2 = tipo (vestuário, eletronico, beleza)
COLUNA 3 = preco
````

### Tabela produto:
(inserir dado que ja existe)
<table>
    <thead>
        </tr>
            <th>Nome</th>
            <th>MARCA</th>
            <th>TAMANHO</th>
            <th>COR</th>
        <tr>
    </thead>
    <tbody>
        <tr>
            <td>Camisa</td>
            <td>Hering</td>
            <td>GG</td>
            <td>Branca</td>
        </tr>
        <tr>
            <td>Calça</td>
            <td>Levis</td>
            <td>46</td>
            <td>Preta</td>
        </tr>
        <tr>
            <td>Camisa</td>
            <td>Hering</td>
            <td>GG</td>
            <td>Branca</td>
        </tr>
    </tbody>
</table>

## Primary Key / Chave Primária / PK
> No conceito de modelo de dados relacional e obedecendo as regras de normalização, uma PK é um conjunto de um ou mais campos que nunca se repetem em uma tabela e que seus valores garantem a integridade do dado único e a utilização do mesmo como referência para o relacionamento entre demais tabela.
> - Não pode haver duas ocorrências de uma mesma entidade com o mesmo conteúdo na PK
> - A chave primária não pode ser composta por atributo opcional, ou seja, atributo que aceite nulo.
> - Os atributos identificadores devem ser o conjunto mínimo que pode identificar cada instância de uma entidade.
> - Não devem ser usadas chaves externas.
> - Não deve conter informação volátil.

### Tabela produto:
<table>
    <thead>
        </tr>
            <th>Nome</th>
            <th>MARCA</th>
            <th>TAMANHO</th>
            <th>COR</th>
        <tr>
    </thead>
    <tbody>
        <tr>
            <td>Camisa</td>
            <td>Hering</td>
            <td>GG</td>
            <td>Branca</td>
        </tr>
        <tr>
            <td>Calça</td>
            <td>Levis</td>
            <td>46</td>
            <td>Preta</td>
        </tr>
        <tr>
            <td>Camisa</td>
            <td>Hering</td>
            <td>M</td>
            <td>Branca</td>
        </tr>
    </tbody>
</table>

PK: nome + marca + tamanho

## Foreign Key / Chave Estrangeira / FK
> Campo, ou conjunto de campos que são referências de chaves primárias de outras tabelas ou da mesma tabela.\
> Sua principal função é garantir a integridade referencial entre tabelas.

````
#️⃣ = tabela , 🔑 = Chave Chave Primária ,⚿ = Chave Estrangeira , ⚪ campo

````
````
#️⃣ cliente 
🔑 cpf VARCHAR(11) 
⚪ nome VARCHAR(45) 
⚪ telefone VARCHAR(45) 
````
````
#️⃣ produto 
🔑 numero_serie VARCHAR(10) 
⚪ nome VARCHAR(45) 
⚪ valor DECIMAL 
````
````
#️⃣ pedido 
🔑 numero INT 
⚿ cliente_cpf VARCHAR(11)
⚿ produto_numero_serie VARCHAR(10) 
⚪ nome VARCHAR(45) 
⚪ valor DECIMAL 
⚪ desconto DECIMAL 
````

<span>Cliente</span>

| CPF         |        NOME        |    TELEFONE |
| :---------- | :----------------: | ----------: |
| 23433244567 |  Ermelino Carlos   | 83977863122 |
| 29555467899 |  Arlinda da Graça  | 15988900122 |
| 27335445017 | Gumercindo Orlando | 11988000900 |

<span>Produto</span>

| NUMERO_SERIE |      NOME       |      VALOR |
| :----------- | :-------------: | ---------: |
| 10001        |    Camiseta     |   R$ 59,90 |
| 10005        |   Calça Jeans   |   R$ 99,90 |
| 10099        | Relógio de Ouro | R$ 1515,90 |

<span>Pedido (PK: NUMERO + CLIENTE_CPF + PRODUTO_NUMERO_SERIE) </span>

| NUMERO | CLIENTE_CPF | PRODUTO_NUMERO_SERIE |      VALOR | DESCONTO |
| :----- | :---------- | :------------------: | ---------: | -------: |
| 18015  | 23433244567 |        10001         |   R$ 59,90 |          |
| 18015  | 23433244567 |        10005         |   R$ 99,90 | R$ 10,00 |
| 18016  | 27335445017 |        10099         | R$ 1515,90 |          |

### ou

<span>Pedido (PK: ID) </span>

| ID   | NUMERO | CLIENTE_CPF | PRODUTO_NUMERO_SERIE |      VALOR | DESCONTO |
| :--- | :----- | :---------- | :------------------: | ---------: | -------: |
| 1    | 18015  | 23433244567 |        10001         |   R$ 59,90 |          |
| 2    | 18015  | 23433244567 |        10005         |   R$ 99,90 | R$ 10,00 |
| 3    | 18016  | 27335445017 |        10099         | R$ 1515,90 |          |

## Tipo de dados

- Numeric
- Monetary
- Character
- Date
- Boolean
- XML
- JSON

[Data types](https://www.postgresqltutorial.com/postgresql-data-types/)


# 3 - DML e DDL
> DML \
> Data Manipulation Language \
> INSERT, UPDATE, DELETE, SELECT
> * o select, alguns consideram como DML, outros como DQL, que significa data query language, ou linguagem de consulta de dados.

> DDL \
> Data Definition Language \
> Linguagem de definição de dados\
> CREATE, ALTER, DROP

````SQL
CREATE / ALTER / DROP

CREATE [objeto] [nome do objeto] [opções];
ALTER [objeto] [nome do objeto] [opções];
DROP [objeto] [nome do objeto] [opções];

CREATE DATABASE dadosbancarios;
ALTER DATABASE dadosbancarios OWNER TO diretoria;
DROP DATABASE dadosbancarios;

CREATE SCHEMA IF NOT EXISTS bancos;
ALTER SCHEMA bancos OWNER TO diretoria;
DROP SCHEMA IF EXISTS bancos;
````

````SQL
CREATE / ALTER / DROP - TABELAS

CREATE TABLE [IF NOT EXISTS] [nome da tabela](
    [nome do campo] [tipo] [regras] [opções],
    [nome do campo] [tipo] [regras] [opções],
    [nome do campo] [tipo] [regras] [opções]
);

ALTER TABLE [nome da tabela] [opções];
DROP TABLE [nome da tabela];


CREATE TABLE IF NOT EXISTS banco (
    codigo INTEGER PRIMARY KEY,
    nome VARCHAR(50) NOT NULL,
    data_criacao TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE TABLE IF NOT EXISTS banco (
    codigo INTEGER,
    nome VARCHAR(50) NOT NULL,
    data_criacao TIMESTAMP NOT NULL DEFAULT NOW(),
    PRIMARY KEY(codigo)
);

ALTER TABLE banco ADD COLUMN tem_poupanca BOOLEAN;
DROP TABLE IF NOT EXISTS banco;
````

````SQL
INSERT

INSERT INTO [nome da tabela] ([campos da tabela])
VALUES ([valores de acordo com a ordem dos campos acima,]);

INSERT INTO [nome da tabela] ([campos da tabela])
SELECT [valores de acordo com a ordem dos campos acima,];


INSERT INTO banco (codigo, nome, data_criacao)
VALUES (100, 'Banco do Brasil', now());

INSERT INTO banco (codigo, nome, data_criacao)
SELECT 100, 'Banco do Brasil', now();
````

````SQL
UPDATE

UPDATE [nome da tabela] SET 
    [campo1] = [novo valor do campo1],
    [campo2] = [novo valor do campo2],
...
[WHERE + condições]

ATENÇÂO: Muito cuidado com os updates. Sempre utilize-os com condição.

UPDATE banco SET
    codigo = 500
WHERE codigo = 100;

UPDATE banco SET
    data_criacao = now()
WHERE data_criacao IS NULL;
````

````SQL
DELETE

DELETE FROM [nome da tabela] [WHERE + condições]

ATENÇÂO: Muito cuidado com os deletes. Sempre utilize-os com condição.

DELETE FROM banco SET WHERE codigo = 512;
DELETE FROM banco SET WHERE nome = 'Conta Digital;
````

````SQL
SELECT

SELECT [campos da tabela] FROM [nome da tabela] [WHERE + condições]

Boas Práticas = Evite sempre que puder o SELECT *

SELECT codigo, nome FROM banco;
SELECT codigo, nome FROM banco WHERE data_criacao > '2019-10-15 15:00:00';
````


# Pratica 

## IDEMPOTÊNCIA
> Propiedade que algumas ações/operações possuem possibilitando-as de serem executadas diversas vezes sem alterar o resultado após a aplicação inicial.
>  - IF EXISTS
>  - Comandos pertinentes ao DDL e DML

## Melhores práticas em DDL
> Importante as tabelas possuírem campos que realmente serão utulizados e que sirvam de atributo direto a um objetivo em comum.
> - Criar/Acrescentar colunas que são "atributos básicos" do objeto;\
> Exemplo: Tabela CLIENTE: coluna TELEFONE / coluna AGENCIA_BANCARIA
> - Cuidado com regras (constraints)
> - Cuidado com o excesso de FKs
> - Cuidado com o tamanho indevido de colunas \
> Exemplo: coluna CEP VARCHAR(255)

### pgAdmin4

````SQL
CREATE DATABASE financas;

CREATE TABLE IF NOT EXISTS banco (
	numero INTEGER NOT NULL,
	nome VARCHAR(50) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (numero)
);

CREATE TABLE IF NOT EXISTS agencia (
	banco_numero INTEGER NOT NULL,
	numero INTEGER NOT NULL,
	nome VARCHAR(80) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (banco_numero, numero),
	FOREIGN KEY (banco_numero) REFERENCES banco (numero)
);

CREATE TABLE IF NOT EXISTS cliente (
	numero BIGSERIAL PRIMARY KEY,
	nome VARCHAR(120) NOT NULL,
	email VARCHAR(250) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS conta_corrente (
	banco_numero INTEGER NOT NULL,
	agencia_numero INTEGER NOT NULL,
	cliente_numero BIGINT NOT NULL,
	numero BIGINT NOT NULL,
	digito SMALLINT NOT NULL,	
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (banco_numero, agencia_numero, numero, digito, cliente_numero),
	FOREIGN KEY (banco_numero, agencia_numero) REFERENCES agencia (banco_numero, numero),
	FOREIGN KEY (cliente_numero) REFERENCES cliente (numero)
);

CREATE TABLE IF NOT EXISTS tipo_transacao (
	id SMALLSERIAL PRIMARY KEY,
	nome VARCHAR(50) NOT NULL,
	ativo BOOLEAN NOT NULL DEFAULT TRUE,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS cliente_transacoes (
	id BIGSERIAL PRIMARY KEY,
	banco_numero INTEGER NOT NULL,
	agencia_numero INTEGER NOT NULL,
	conta_corrente_numero BIGINT NOT NULL,
	conta_corrente_digito SMALLINT NOT NULL,
	cliente_numero BIGINT NOT NULL,
	tipo_transacao_id SMALLSERIAL NOT NULL,
	valor NUMERIC(15,2) NOT NULL,
	data_criacao TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	FOREIGN KEY (banco_numero, agencia_numero, conta_corrente_numero, conta_corrente_digito, cliente_numero) 
	REFERENCES conta_corrente (banco_numero, agencia_numero, numero, digito, cliente_numero) 
);