# 1 - CTE - Common Table Expressions
> Definição \
> Forma auxiliar de organizar "statements", ou seja, blocos de códigos, para consultas muito grandes, gerando tabelaas temporárias e criando relacionamentos entre elas.
>
> Dentro dos statements podem ter SELECTs,INSERTs, UPDATEs ou DELETEs.

## WITH STATEMENTS
````SQL
WITH [nome1] AS (
    SELECT (campos,)
    FROM tabela_A
    [WHERE]
),[nome2] AS (
    SELECT (campos,)
    FROM tabela_B
    [WHERE]
)
SELECT [nome1].(campos,),[nome2].(campos,)
FROM [nome1]
JOIN [nome2]...
````

# Pratica

## pgAdmin4

````SQL
-- 151 rows
WITH tbl_tmp_banco AS (
    SELECT numero, nome 
	FROM banco
)
SELECT numero, nome 
FROM tbl_tmp_banco;
-- 1 rows
WITH params AS (
    SELECT 213 AS banco_numero
), tbl_tmp_banco AS (
    SELECT numero, nome 
	FROM banco
	JOIN params ON params.banco_numero = banco.numero
)
SELECT numero, nome 
FROM tbl_tmp_banco;
-- 1 rows
SELECT banco.numero, banco.nome
FROM banco
JOIN (
	SELECT 213 AS banco_numero
)params ON params.banco_numero = banco.numero;

-- 2018 rows
WITH clientes_e_transacoes AS (
    SELECT 
		cliente.nome AS cliente_nome,
		tipo_transacao.nome AS tipo_transacao_nome,
		cliente_transacoes.valor AS tipo_transacao_valor
	FROM cliente_transacoes
	JOIN cliente ON cliente.numero = cliente_transacoes.cliente_numero
	JOIN tipo_transacao ON tipo_transacao.id = cliente_transacoes.tipo_transacao_id
)
SELECT cliente_nome, tipo_transacao_nome, tipo_transacao_valor
FROM clientes_e_transacoes;

-- 217 rows
WITH clientes_e_transacoes AS (
    SELECT 
		cliente.nome AS cliente_nome,
		tipo_transacao.nome AS tipo_transacao_nome,
		cliente_transacoes.valor AS tipo_transacao_valor
	FROM cliente_transacoes
	JOIN cliente ON cliente.numero = cliente_transacoes.cliente_numero
	JOIN tipo_transacao ON tipo_transacao.id = cliente_transacoes.tipo_transacao_id
	JOIN banco on banco.numero = cliente_transacoes.banco_numero AND banco.nome ILIKE '%Itaú%'
)
SELECT cliente_nome, tipo_transacao_nome, tipo_transacao_valor
FROM clientes_e_transacoes;
````