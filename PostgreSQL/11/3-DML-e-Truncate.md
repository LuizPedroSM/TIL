# 1 - DML - CRUD
### SELECT
````SQL
SELECT (campos,) FROM tabela [condições]
````
#### Exemplo:
````SQL
SELECT numero, nome FROM banco;
SELECT numero, nome FROM banco WHERE ativo IS TRUE;
SELECT nome FROM cliente WHERE email LIKE '%gmail.com';

SELECT numero FROM agencia WHERE banco_numero IN (SELECT numero FROM banco WHERE nome ILIKE '%Bradesco%');
````

### SELECT - Condição (WHERE / AND / OR)
> WHERE (coluna/condição):\
> - ' = ' 
> - ' > / >= ' 
> - ' <> / != '
> - LIKE
> - ILIKE
> - IN 
> 
> Primeira condição sempre WHERE. Demais condições, AND ou OR.

### SELECT - Idempotência
````SQL
SELECT (campos,) FROM tabela1 WHERE EXISTS (SELECT (campo,) FROM tabela2 WHERE campo1 = valor 1 [AND/OR campoN = valorN]);

Não é uma boa prática. Melhor prática utilizar LEFT JOIN.
````

### INSERT
````SQL
INSERT (campos da tabela,) VALUES (valores,);
INSERT (campos da tabela,) SELECT (valores,);
````

### INSERT - Idempotência
````SQL
INSERT INTO agencia (banco_numero, numero, nome) VALUES (341, 1, 'Centro da cidade');

INSERT INTO agencia (banco_numero, numero, nome) 
SELECT 341, 1, 'Centro da cidade'
WHERE NOT EXISTS (SELECT banco_numero, numero, nome FROM agencia WHERE banco_numero = 341 AND numero = 1 AND nome = 'Centro da cidade');

ON CONFLICT

INSERT INTO agencia (banco_numero, numero, nome) VALUES (341, 1, 'Centro da cidade') ON CONFLICT (banco_numero, numero) DO NOTHING;
````

### UPDATE
````SQL
UPDATE (tabela) SET campo1 = novo_valor WHERE (condição);
````
### DELETE
````SQL
DELETE FROM (tabela) WHERE campo1 = valor;
````

# 2 - Truncate
> Definição: "Esvazia" a tabela.

````SQL
TRUNCATE [TABLE] [ONLY] name [*] [,...]
    [ RESTART IDENTITY | CONTINUE IDENTITY ] [ CASCADE | RESTRICT ]
````

# Pratica 

### pgAdmin4

````SQL
SELECT numero, nome, ativo FROM banco;
SELECT banco_numero, numero, nome FROM agencia;
SELECT numero, nome, email FROM cliente;
SELECT id, nome FROM tipo_transacao;
SELECT banco_numero, agencia_numero, numero, cliente_numero FROM conta_corrente;
SELECT banco_numero, agencia_numero, cliente_numero FROM cliente_transacoes;

CREATE TABLE IF NOT EXISTS teste (
	id SERIAL PRIMARY KEY,
	nome VARCHAR(50) NOT NULL,
	created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

DROP TABLE IF EXISTS teste;

CREATE TABLE IF NOT EXISTS teste (
	cpf VARCHAR(11) NOT NULL,
	nome VARCHAR(50) NOT NULL,
	created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (cpf)
);

INSERT INTO teste (cpf, nome, created_at)
VALUES ('22344566712', 'José Colméia', '2019-07-01 12:00:00');

INSERT INTO teste (cpf, nome, created_at)
VALUES ('22344566712', 'José Colméia', '2019-07-01 12:00:00') ON CONFLICT (cpf) DO NOTHING;

UPDATE teste SET nome = 'Pedro Alvares' WHERE cpf = '22344566712';

SELECT * FROM teste;
````