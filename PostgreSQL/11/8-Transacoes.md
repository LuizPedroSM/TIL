# 1 - Transações

> Definição \
> Conceito fundamental de todos os sistemas de banco de dados.\
> Conceito de múltiplas etapas/códigos reunidos em apenas 1 transação, onde o resultado precisa ser <b>tudo ou nada</b>.

> Exemplos:

```SQL
UPDATE conta set valor = valor - 100.00 WHERE nome = 'Alice';
UPDATE conta set valor = valor + 100.00 WHERE nome = 'Bob';
---
BEGIN;
    UPDATE conta set valor = valor - 100.00 WHERE nome = 'Alice';
    UPDATE conta set valor = valor + 100.00 WHERE nome = 'Bob';
COMMIT;
---
BEGIN;
    UPDATE conta set valor = valor - 100.00 WHERE nome = 'Alice';
    UPDATE conta set valor = valor + 100.00 WHERE nome = 'Bob';
ROLLBACK;
---
BEGIN;
    UPDATE conta set valor = valor - 100.00 WHERE nome = 'Alice';
SAVEPOINT my_savepoint;
    UPDATE conta set valor = valor + 100.00 WHERE nome = 'Bob';
    -- oops ... não é para o Bob, é para o Wally!!!
ROLLBACK TO my_savepoint;
    UPDATE conta set valor = valor + 100.00 WHERE nome = 'Wally';
COMMIT;
```

# Pratica

## pgAdmin4

```SQL
SELECT numero, nome, ativo FROM banco ORDER BY numero;

UPDATE banco SET ativo = false WHERE numero = 0;

BEGIN;
	UPDATE banco SET ativo = true WHERE numero = 0;
	SELECT numero, nome, ativo FROM banco ORDER BY numero;
ROLLBACK;
---
BEGIN;
	UPDATE banco SET ativo = true WHERE numero = 0;
ROLLBACK;
---
SELECT id, gerente, nome FROM funcionarios;

BEGIN;
	UPDATE funcionarios SET gerente = 2 WHERE id = 3;
SAVEPOINT sf_func;
	UPDATE funcionarios SET gerente = null;
ROLLBACK TO sf_func;
	UPDATE funcionarios SET gerente = 3 WHERE id = 5;
COMMIT;
```
