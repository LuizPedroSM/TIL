# 1 - Funções Agregadas
- AVG
- COUNT (opção:HAVING)
- MAX
- MIN
- SUM
  
[Funçoes](https://www.postgresql.org/docs/11/functions-aggregate.html)


````SQL
-- Mostrar as colunas de uma tabela
SELECT * FROM information_schema.columns WHERE table_name = 'banco';
SELECT column_name, data_type FROM information_schema.columns WHERE table_name = 'banco';

SELECT * FROM cliente_transacoes;

SELECT AVG(valor) FROM cliente_transacoes;

SELECT COUNT(numero) FROM cliente;
SELECT COUNT(numero), email FROM cliente WHERE email ILIKE '%gmail.com' GROUP BY email;

SELECT MAX(numero) FROM cliente;
SELECT MIN(numero) FROM cliente;

SELECT MAX(valor) FROM cliente_transacoes;
SELECT MIN(valor) FROM cliente_transacoes;

SELECT MAX(valor), tipo_transacao_id FROM cliente_transacoes GROUP BY tipo_transacao_id;
SELECT MIN(valor), tipo_transacao_id FROM cliente_transacoes GROUP BY tipo_transacao_id;

SELECT column_name, data_type FROM information_schema.columns WHERE table_name = 'cliente_transacoes';
SELECT COUNT(id), tipo_transacao_id FROM cliente_transacoes GROUP BY tipo_transacao_id HAVING COUNT(id) > 150;

SELECT SUM(valor) FROM cliente_transacoes;
SELECT SUM(valor), tipo_transacao_id FROM cliente_transacoes GROUP BY tipo_transacao_id;
SELECT MAX(valor), tipo_transacao_id FROM cliente_transacoes GROUP BY tipo_transacao_id ORDER BY tipo_transacao_id ASC;
SELECT MAX(valor), tipo_transacao_id FROM cliente_transacoes GROUP BY tipo_transacao_id ORDER BY tipo_transacao_id DESC;
````