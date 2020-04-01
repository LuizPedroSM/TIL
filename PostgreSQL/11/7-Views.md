# 1 - Views

> Definição:\
> View são visões.\
> São "camadas" para as tabelas.\
> São "alias" para uma ou mais queries.\
> Aceitam comando de SELECT, INSERT*, UPDATE* e DELETE\*.\
>
> -   Somente as views que fazem referência a uma tabela. Caso tenha JOINs só aceita SELECTs.

```SQL
CREATE [OR REPLACE] [TEMP | TEMPORARY] [RECURSIVE] VIEW name [ (column_name [,...]) ]
    [WITH (view_option_name [= view_option_value] [,...])]
    AS query
    [WITH [CASCADED | LOCAL] CHECK OPTION]
```

### Idempotência

```SQL
CREATE OR REPLACE VIEW vw_bancos AS (
    SELECT numero, nome, ativo
    FROM banco
);
SELECT numero, nome, ativo FROM vw_bancos;

CREATE OR REPLACE VIEW vw_bancos (banco_numero, banco_nome, banco_ativo) AS(
    SELECT numero, nome, ativo
    FROM banco
);
SELECT banco_numero, banco_nome, banco_ativo FROM vw_bancos;
```

## Insert, Update e Delete

```SQL
CREATE OR REPLACE VIEW vw_bancos AS (
    SELECT numero, nome, ativo
    FROM banco
);
SELECT numero, nome, ativo FROM vw_bancos;
```

> -   Funcionam apenas para VIEWs com apenas 1 tabela

```SQL
INSERT INTO vw_bancos (numero, nome, ativo) VALUES (100, 'Banco CEM', TRUE);
UPDATE vw_bancos SET nome = 'Banco 100' WHERE numero = 100;
DELETE FROM vw_bancos WHERE numero = 100;
```

## Temporary

```SQL
CREATE OR REPLACE TEMPORARY VIEW vw_bancos AS (
    SELECT numero, nome, ativo
    FROM banco
);
SELECT numero, nome, ativo FROM vw_bancos;
```

> VIEW presente apenas na sessão do usuário.\
> Se você se desconectar e conectar novamente, a VIEW não estará disponível.

## Recursive

```SQL
CREATE OR REPLACE RECURSIVE VIEW (nome_da_view) (campos_da_view) AS (
    SELECT base
    UNION ALL
    SELECT campos
    FROM tabela_base
    JOIN (nome_da_view)
);
```

> -   Obrigatório a existência dos campos da VIEW
> -   UNION ALL

```SQL
CREATE TABLE IF NOT EXISTS funcionarios (
    id SERIAL NOT NULL,
    nome VARCHAR(50),
    gerente INTEGER,
    PRIMARY KEY (id),
    FOREIGN KEY (gerente) REFERENCES funcionarios (id)
);

INSERT INTO funcionarios (nome, gerente) VALUE ('Ancelmo', null);
INSERT INTO funcionarios (nome, gerente) VALUE ('Beatriz', 1);
INSERT INTO funcionarios (nome, gerente) VALUE ('Magno', 1);
INSERT INTO funcionarios (nome, gerente) VALUE ('Cremilda', 2);
INSERT INTO funcionarios (nome, gerente) VALUE ('Wagner', 4);
```

| ID  |   NOME   | GERENTE |
| :-- | :------: | :-----: |
| 1   | Ancelmo  |         |
| 2   | Beatriz  |    1    |
| 3   |  Magno   |    1    |
| 4   | Cremilda |    2    |
| 5   |  Wagner  |    4    |

```SQL
SELECT id, nome, gerente FROM funcionarios WHERE gerente IS NULL
```

| ID  |  NOME   | GERENTE |
| :-- | :-----: | :-----: |
| 1   | Ancelmo |         |

```SQL
SELECT id, nome, gerente FROM funcionarios WHERE id = 999
```

| ID  | NOME | GERENTE |
| :-- | :--: | :-----: |


```SQL
SELECT id, nome, gerente FROM funcionarios WHERE gerente IS NULL
UNION ALL
SELECT id, nome, gerente FROM funcionarios WHERE id = 999
```

| ID  |  NOME   | GERENTE |
| :-- | :-----: | :-----: |
| 1   | Ancelmo |         |

```SQL
CREATE OR REPLACE RECURSIVE VIEW vw_funcionarios (id, gerente, funcionario) AS (
    SELECT id, gerente, nome
    FROM funcionarios
    WHERE gerente IS NULL
    UNION ALL
    SELECT funcionarios.id, funcionarios.gerente, funcionarios.nome
    FROM funcionarios
    JOIN vw_funcionarios ON vw_funcionarios.id = funcionarios.gerente
);
SELECT id, gerente, funcionario FROM vw_funcionarios
```

```SQL
CREATE OR REPLACE RECURSIVE VIEW vw_funcionarios (id, gerente, funcionario) AS (
    SELECT id, CAST(AS VARCHAR) AS gerente, nome
    FROM funcionarios
    WHERE gerente IS NULL
    UNION ALL
    SELECT funcionarios.id, funcionarios.gerente, funcionarios.nome
    FROM funcionarios
    JOIN vw_funcionarios ON vw_funcionarios.id = funcionarios.gerente
    JOIN funcionarios.gerentes ON gerentes.id = vw_funcionarios.id
);
SELECT id, gerente, funcionario FROM vw_funcionarios
```

### With Options

```SQL
CREATE OR REPLACE VIEW vw_bancos AS (
    SELECT numero, nome, ativo
    FROM banco
);
INSERT INTO vw_bancos (numero, nome, ativo) VALUES (100,'Banco CEM', FALSE)
-- OK

CREATE OR REPLACE VIEW vw_bancos AS (
    SELECT numero, nome, ativo
    FROM banco
    WHERE ativo IS TRUE
) WITH LOCAL CHECK OPTION;
INSERT INTO vw_bancos (numero, nome, ativo) VALUES (100,'Banco CEM', FALSE)
-- ERRO

---
CREATE OR REPLACE VIEW vw_bancos_ativos AS (
    SELECT numero, nome, ativo
    FROM banco
    WHERE ativo IS TRUE
) WITH LOCAL CHECK OPTION;

CREATE OR REPLACE VIEW vw_bancos_maiores_que_100 AS (
    SELECT numero, nome, ativo
    FROM vw_banco
    WHERE numero > 100
) WITH LOCAL CHECK OPTION;

INSERT INTO vw_bancos_maiores_que_100 (numero, nome, ativo) VALUES (99 ,'Banco DIO', FALSE)
-- ERRO
INSERT INTO vw_bancos_maiores_que_100 (numero, nome, ativo) VALUES (200 ,'Banco DIO', FALSE)
-- ERRO

---
CREATE OR REPLACE VIEW vw_bancos_ativos AS (
    SELECT numero, nome, ativo
    FROM banco
    WHERE ativo IS TRUE
);

CREATE OR REPLACE VIEW vw_bancos_maiores_que_100 AS (
    SELECT numero, nome, ativo
    FROM vw_banco
    WHERE numero > 100
) WITH LOCAL CHECK OPTION;

INSERT INTO vw_bancos_maiores_que_100 (numero, nome, ativo) VALUES (99 ,'Banco DIO', FALSE)
-- ERRO
INSERT INTO vw_bancos_maiores_que_100 (numero, nome, ativo) VALUES (200 ,'Banco DIO', FALSE)
-- OK

---
CREATE OR REPLACE VIEW vw_bancos_ativos AS (
    SELECT numero, nome, ativo
    FROM banco
    WHERE ativo IS TRUE
);

CREATE OR REPLACE VIEW vw_bancos_maiores_que_100 AS (
    SELECT numero, nome, ativo
    FROM vw_banco
    WHERE numero > 100
) WITH CASCADED CHECK OPTION;

INSERT INTO vw_bancos_maiores_que_100 (numero, nome, ativo) VALUES (99 ,'Banco DIO', FALSE)
-- ERRO
INSERT INTO vw_bancos_maiores_que_100 (numero, nome, ativo) VALUES (200 ,'Banco DIO', FALSE)
-- ERRO
```

# Pratica

## pdAdmin4

```SQL
SELECT numero, nome, ativo FROM banco;
CREATE OR REPLACE VIEW vw_bancos AS (
	SELECT numero, nome, ativo
	FROM banco
);

---
SELECT numero, nome, ativo FROM vw_bancos;
CREATE OR REPLACE VIEW vw_bancos_2 (banco_numero, banco_nome, banco_ativo) AS(
	SELECT numero, nome, ativo
	FROM banco
);

---
SELECT banco_numero, banco_nome, banco_ativo FROM vw_bancos_2;

INSERT INTO vw_bancos_2 (banco_numero, banco_nome, banco_ativo) VALUES (51, 'Banco Boa Ideia', TRUE);

SELECT banco_numero, banco_nome, banco_ativo FROm vw_bancos_2 WHERE banco_numero = 51;
SELECT numero, nome, ativo FROM banco WHERE numero = 51;

UPDATE vw_bancos_2 SET banco_ativo = FALSE WHERE banco_numero = 51;
DELETE FROM vw_bancos_2 WHERE banco_numero = 51

---
CREATE OR REPLACE TEMPORARY VIEW vw_agencia AS (
	SELECT nome FROM agencia
);

SELECT nome FROM vw_agencia;

---
CREATE OR REPLACE VIEW vw_bancos_ativos AS (
	SELECT numero, nome, ativo
	FROM banco
	WHERE ativo IS TRUE
) WITH LOCAL CHECK OPTION;

INSERT INTO vw_bancos_ativos (numero, nome, ativo) VALUES (51, 'Banco Boa Ideia', FALSE); -- row violates check option

---
CREATE OR REPLACE VIEW vw_bancos_com_a AS (
	SELECT numero, nome, ativo
	FROM vw_bancos_ativos
	WHERE nome ILIKE 'a%'
) WITH LOCAL CHECK OPTION;

SELECT numero, nome, ativo FROM vw_bancos_com_a;

INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (333, 'Alfa Omega', true); -- OK
INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (332, 'Beta Omega', true); -- row violates check option
INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (331, 'Alfa Omega', false); -- row violates check option

---
CREATE OR REPLACE VIEW vw_bancos_ativos AS (
	SELECT numero, nome, ativo
	FROM banco
	WHERE ativo IS TRUE
);

CREATE OR REPLACE VIEW vw_bancos_com_a AS (
	SELECT numero, nome, ativo
	FROM vw_bancos_ativos
	WHERE nome ILIKE 'a%'
) WITH LOCAL CHECK OPTION;

INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (330, 'Alfa Omega', false); -- OK

---
CREATE OR REPLACE VIEW vw_bancos_ativos AS (
	SELECT numero, nome, ativo
	FROM banco
	WHERE ativo IS TRUE
);

CREATE OR REPLACE VIEW vw_bancos_com_a AS (
	SELECT numero, nome, ativo
	FROM vw_bancos_ativos
	WHERE nome ILIKE 'a%'
) WITH CASCADED CHECK OPTION;

INSERT INTO vw_bancos_com_a (numero, nome, ativo) VALUES (551, 'Alfa Gama Beta', false); -- row violates check option
```

```SQL
CREATE TABLE IF NOT EXISTS funcionarios (
    id SERIAL,
    nome VARCHAR(50),
    gerente INTEGER,
    PRIMARY KEY (id),
    FOREIGN KEY (gerente) REFERENCES funcionarios(id)
);

INSERT INTO funcionarios (nome, gerente) VALUES ('Ancelmo', null);
INSERT INTO funcionarios (nome, gerente) VALUES ('Beatriz', 1);
INSERT INTO funcionarios (nome, gerente) VALUES ('Magno', 2);
INSERT INTO funcionarios (nome, gerente) VALUES ('Cremilda', 3);
INSERT INTO funcionarios (nome, gerente) VALUES ('Wagner', 4);

CREATE TABLE IF NOT EXISTS funcionarios (
    id SERIAL,
    nome VARCHAR(50),
    gerente INTEGER,
    PRIMARY KEY (id),
    FOREIGN KEY (gerente) REFERENCES funcionarios(id)
);

SELECT id, nome, gerente FROM funcionarios WHERE gerente IS NULL
UNION ALL
SELECT id, nome, gerente FROM funcionarios WHERE id = 999; -- Apenas para exemplificar

--
CREATE OR REPLACE RECURSIVE VIEW vw_func (id, gerente, funcionarios) AS (
	SELECT id, gerente, nome
	FROM funcionarios
	WHERE gerente is NULL

	UNION ALL

	SELECT funcionarios.id, funcionarios.gerente, funcionarios.nome
	FROM funcionarios
	JOIN vw_func ON vw_func.id = funcionarios.gerente
);
SELECT id, gerente, funcionarios FROM vw_func;
```
