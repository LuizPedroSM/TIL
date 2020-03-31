# 1 - JOINs
> Definição
> - JOIN
> - LEFT JOIN
> - RIGHT JOIN
> - FULL JOIN
> - CROSS JOIN

## JOIN (INNER)

````SQL
SELECT tabela_1.campos, tabela_2.campos 
FROM tabela_1 
JOIN tabela_2 
ON tabela_2.campo = tabela_1.campo
````
#### TBL1
| ID  |   NOME    |
| --- | :-------: |
| 1   | Genivaldo |
| 2   | Emengarda |
| 3   | Hercules  |

#### TBL2
| ID  | TBL_ID |    VALOR |
| --- | :----: | -------: |
| 1   |   1    | R$ 10,00 |
| 2   |   1    | R$ 15,00 |
| 3   |   3    | R$ 50,00 |

#### JOIN
| TBL_1     |  TBL_2   |
| --------- | :------: |
| Genivaldo | R$ 10,00 |
| Genivaldo | R$ 15,00 |
| Hercules  | R$ 50,00 |


## LEFT JOIN (OUTER)

````SQL
SELECT tabela_1.campos, tabela_2.campos 
FROM tabela_1 
LEFT JOIN tabela_2 
ON tabela_2.campo = tabela_1.campo
````
#### TBL1
| ID  |   NOME    |
| --- | :-------: |
| 1   | Genivaldo |
| 2   | Emengarda |
| 3   | Hercules  |

#### TBL2
| ID  | TBL_ID |    VALOR |
| --- | :----: | -------: |
| 1   |   1    | R$ 10,00 |
| 2   |   1    | R$ 15,00 |
| 3   |   3    | R$ 50,00 |

#### LEFT JOIN
| TBL_1     |  TBL_2   |
| --------- | :------: |
| Genivaldo | R$ 10,00 |
| Genivaldo | R$ 15,00 |
| Emengarda |          |
| Hercules  | R$ 50,00 |

## RIGHT JOIN (OUTER)

````SQL
SELECT tabela_1.campos, tabela_2.campos 
FROM tabela_1 
RIGHT JOIN tabela_2 
ON tabela_2.campo = tabela_1.campo
````
#### TBL1
| ID  |   PRODUTO   |
| --- | :---------: |
| 1   | Camisa Polo |
| 2   | Calça Jeans |
| 3   | Saia longa  |

#### TBL2
| ID  | TAMANHO |
| --- | :-----: |
| 1   |    P    |
| 2   |    M    |
| 3   |    G    |

#### 
| TBL_1 | TBL_2 |
| :---: | :---: |
|   1   |   2   |
|   1   |   3   |
|   2   |   2   |


#### RIGHT JOIN
| TBL_1       | TBL_2 |
| ----------- | :---: |
| Camisa Polo |   M   |
| Camisa Polo |   G   |
| Calça Jeans |   M   |
|             |   P   |

## FULL JOIN (FULL OUTER JOIN)

````SQL
SELECT tabela_1.campos, tabela_2.campos 
FROM tabela_1 
FULL JOIN tabela_2 
ON tabela_2.campo = tabela_1.campo
````
#### TBL1
| ID  |   PRODUTO   |
| --- | :---------: |
| 1   | Camisa Polo |
| 2   | Calça Jeans |
| 3   | Saia longa  |

#### TBL2
| ID  | TAMANHO |
| --- | :-----: |
| 1   |    P    |
| 2   |    M    |
| 3   |    G    |

#### 
| TBL_1 | TBL_2 |
| :---: | :---: |
|   1   |   2   |
|   1   |   3   |
|   2   |   2   |


#### FULL JOIN
| TBL_1       | TBL_2 |
| ----------- | :---: |
| Camisa Polo |   M   |
| Camisa Polo |   G   |
| Calça Jeans |   M   |
| Saia longa  |       |
|             |   P   |

## CROSS JOIN 
> Todos os dados de uma tabela serão cruzados com todos os dados da tabela referenciada no CROSS JOIN criando uma matriz.
````SQL
SELECT tabela_1.campos, tabela_2.campos 
FROM tabela_1 
CROSS JOIN tabela_2 
````

#### 
|  ID   |     NOME     |
| :---: | :----------: |
|   1   | José Colméia |
|   2   | Airton Senna |
#### 
|  ID   |  VALOR   |
| :---: | :------: |
|   1   | R$ 10,00 |
|   2   | R$ 15,00 |
|   3   | R$ 25,00 |
#### CROSS JOIN
| TBL_1 = ID | TBL_1 = NOME | TBL_2 = ID | TBL_2 = VALOR |
| :--------: | :----------: | :--------: | :-----------: |
|     1      | José Colméia |     1      |   R$ 10,00    |
|     1      | José Colméia |     2      |   R$ 15,00    |
|     1      | José Colméia |     3      |   R$ 25,00    |
|     2      | Airton Senna |     1      |   R$ 10,00    |
|     2      | Airton Senna |     2      |   R$ 15,00    |
|     2      | Airton Senna |     3      |   R$ 25,00    |

# Pratica 

## pgAdmin4

````SQL

SELECT count(1) FROM banco; -- 151 rows
SELECT count(1) FROM agencia; -- 296 rows

-- 296 rows
SELECT banco.numero, banco.nome, agencia.numero, agencia.nome
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero;

--296 rows
SELECT count(banco.numero)
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero;

--296 rows - 9 bancos tem agencias
SELECT banco.numero
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero
GROUP BY banco.numero;

--296 - 9 bancos tem agencias
SELECT count( distinct banco.numero)
FROM banco
JOIN agencia ON agencia.banco_numero = banco.numero

-- 438 rows
SELECT banco.numero, banco.nome, agencia.numero, agencia.nome
FROM banco
LEFT JOIN agencia ON agencia.banco_numero = banco.numero;

-- 438 rows
SELECT  agencia.numero, agencia.nome, banco.numero, banco.nome
FROM agencia
RIGHT JOIN banco ON banco.numero = agencia.banco_numero;

-- 296 rows
SELECT  agencia.numero, agencia.nome, banco.numero, banco.nome
FROM agencia
LEFT JOIN banco ON banco.numero = agencia.banco_numero;

-- 438 rows
SELECT banco.numero, banco.nome, agencia.numero, agencia.nome
FROM banco
FULL JOIN agencia ON agencia.banco_numero = banco.numero;

-- 197 rows
SELECT 
	banco.nome,
	agencia.nome, 
	conta_corrente.numero, 
	conta_corrente.digito,
	cliente.nome
FROM banco
JOIN agencia 
	ON agencia.banco_numero = banco.numero
JOIN conta_corrente
	-- ON conta_corrente.banco_numero = agencia.banco_numero
	ON conta_corrente.banco_numero = banco.numero
	AND conta_corrente.agencia_numero = agencia.numero
JOIN cliente
	ON cliente.numero = conta_corrente.cliente_numero;
````
