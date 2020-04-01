# 1 - Funções

> Definição: \
> Conjunto de códigos que são executados <b>dentro de uma transação</b> com a finalidade de facilitar a programação e obter o reaproveitamento/reutilização de códigos.
>
> Existem 4 tipos de funções:
>
> -   query language functions (funções escritas em SQL)
> -   procedural language functions (funções escritas em, por exemplo, PL/pgSQL ou PL/py)
> -   internal functions
> -   C-languages functions
>
> Porém, o foco aqui é falar sobre <b>USER DEFINED FUNCTIONS</b>.
> Funções que podem ser criadas pelo usuário.

## Linguagens

-   SQL
-   PL/PGSQL
-   PL/PY
-   PL/PHP
-   PL/RUBY
-   PL/Java
-   PL/LUA
-   ...

[Procedural Languages](https://www.postgresql.org/docs/11/external-pl.html)

## Definição

```SQL
CREATE [ OR REPLACE ] FUNCTION
    name ( [ [ argmod ] [ argname ] argtype [ {DEFAULT | = } default_expr] [,...] ] )
    [ RETURNS rettype | RETURNS TABLE (column_name column_type [,...]) ]
    { LANGUAGE lang_name
      | TRANSFORM { FOR TYPE type_name} [,...]
      | WINDOWS
      | IMMUTABLE | STABLE | VOLATILE | [ NOT ] LEAKPROOF
      | CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT
      | [ EXTERNAL ] SECURITY INVOKER | [ EXTERNAL ] SECURITY DEFINER
      | PARALLEL { UNSAFE | RESTRICTED | SAFE}
      | COST execution_cost
      | ROWS results_rows
      | SET configuration_parameter { TO value | = value | FROM CURRENT }
      | AS 'definition'
      | AS 'obj_file', 'link_symbol'
    }...
```

## Idempotência

> CREATE <b>OR REPLACE</b> FUNCTION [nome da função]
>
> -   Mesmo nome
> -   Mesmo tipo de retorno
> -   Mesmo número de parâmetros/argumentos

## Returns

-   Tipo de retorno (data type)

    -   INTEGER
    -   CHAR / VARCHAR
    -   BOOLEAN
    -   ROW
    -   TABLE
    -   JSON

## Segurança

-   SECURITY
    -   INVOKER
        > Padrão. A função é executada com as permissões de quem executada.
    -   DEFINER
        > A função é executada com as permissões de quem criou a função.

## Comportamento

-   IMMUTABLE
    > Não pde alterar o banco de dados.\
    > Funções que garatem o mesmo resultado para os mesmo argumentos/parâmetros da função. Evitar a utilização de selects, pois tabelas podem sofrer alterações.
-   STABLE
    > Não pde alterar o banco de dados.\
    > Funções que garatem o mesmo resultado para os mesmo argumentos/parâmetros da função. Trabalha melhor com tipos de current_timestamp e outros tipos variáveis. Podem conter selects.
-   IMMUTABLE
    > Comportamento padrão. Aceita todos os cenários.

## Boas Práticas

-   CALLED ON NULL INPUT
    > Padrão. Se qualquer um dos parâmetros/argumentos for NULL, a função será executada.
-   RETURNS NULL ON NULL INPUT
    > Se qualquer um dos parâmetros/argumentos for NULL, a função retornará NULL.

## Recursos

-   COST
    > Custo/row em unidade de CPU
-   ROWS
    > Números estimado de linhas que será analisada pelo planner

## SQL Functions

> Não é possível utilizar <b>TRANSAÇÕES</b>

```SQL
CREATE OR REPLACE FUNCTION fc_somar(INTEGER,INTEGER)
RETURNS INTEGER
LANGUAGE SQL
AS $$
        SELECT $1 + $2
$$;

CREATE OR REPLACE FUNCTION fc_somar(num1 INTEGER,num2 INTEGER)
RETURNS INTEGER
LANGUAGE SQL
AS $$
        SELECT num1 + num2
$$;

CREATE OR REPLACE FUNCTION fc_bancos_add(p_numero INTEGER,p_nome VARCHAR, p_ativo BOOLEAN)
RETURNS TABLE (numero INTEGER, nome VARCHAR)
RETURNS NULL ON NULL INPUT
LANGUAGE SQL
AS $$
        INSERT INTO banco (numero, nome, ativo)
        VALUES (p_numero, p_nome, p_ativo);

        SELECT numero, nome
        FROM banco
        WHERE numero = p_numero;
$$;
```

## PLPGSQL

```SQL
CREATE OR REPLACE FUNCTION banco_add (p_numero INTEGER, p_nome VARCHAR, p_ativo BOOLEAN)
RETURNS BOOLEAN
LANGUAGE PLPGSQL
AS $$
    DECLARE variavel_id INTEGER;
    BEGIN
        SELECT INTO variavel_id numero FROM banco WHERE nome = p_nome;

        IF variavel_id IS NULL THEN
            INSERT INTO banco (numero, nome, ativo) VALUES (p_numero, p_nome, p_ativo);
        ELSE
            RETURN FALSE;
        END IF;

        SELECT INTO variavel_id numero FROM banco WHERE nome = p_nome;

        IF variavel_id IS NULL THEN
            RETURN FALSE;
        ELSE
            RETURN TRUE;
        END IF;
    END;
$$;

SELECT banco_add(13, 'Banco azarado', true);
```

# Pratica

## pgAdmin4

```SQL
CREATE OR REPLACE FUNCTION func_somar (INTEGER, INTEGER)
RETURNS INTEGER
SECURITY DEFINER
RETURNS NULL ON NULL INPUT
LANGUAGE SQL
AS $$
        SELECT $1 + $2
$$;
SELECT func_somar(1,null); -- null
---
CREATE OR REPLACE FUNCTION func_somar (INTEGER, INTEGER)
RETURNS INTEGER
SECURITY DEFINER
CALLED ON NULL INPUT
LANGUAGE SQL
AS $$
        SELECT $1 + $2
$$;
SELECT func_somar(1,null); -- null
---
CREATE OR REPLACE FUNCTION func_somar (INTEGER, INTEGER)
RETURNS INTEGER
SECURITY DEFINER
CALLED ON NULL INPUT
LANGUAGE SQL
AS $$
        SELECT COALESCE($1,0) + COALESCE($2,0);
$$;
SELECT func_somar(1,null);--1
SELECT func_somar(null, 2);--2
------------------------------------------
CREATE OR REPLACE FUNCTION bancos_add (p_numero INTEGER, p_nome VARCHAR, p_ativo BOOLEAN)
RETURNS INTEGER
SECURITY INVOKER
LANGUAGE PLPGSQL
CALLED ON NULL INPUT
AS $$
	DECLARE variavel_id INTEGER;
	BEGIN
		IF p_numero IS NULL OR p_nome IS NULL OR p_ativo IS NULL THEN
			RETURN 0;
		END IF;

		SELECT INTO variavel_id numero FROM banco WHERE numero = p_numero;

		IF variavel_id IS NULL THEN
			INSERT INTO banco(numero, nome, ativo)
			VALUES (p_numero, p_nome, p_ativo);
		ELSE
			RETURN variavel_id;
		END IF;

		RETURN variavel_id;
	END;
$$;
SELECT bancos_add(5432, 'Banco Novo', null); -- 0
SELECT bancos_add(5432, 'Banco Novo', false); -- 5432
----
CREATE OR REPLACE FUNCTION bancos_add (p_numero INTEGER, p_nome VARCHAR, p_ativo BOOLEAN)
RETURNS INTEGER
SECURITY INVOKER
LANGUAGE PLPGSQL
CALLED ON NULL INPUT
AS $$
	DECLARE variavel_id INTEGER;
	BEGIN
		IF p_numero IS NULL OR p_nome IS NULL OR p_ativo IS NULL THEN
			RETURN 0;
		END IF;

		SELECT INTO variavel_id numero FROM banco WHERE numero = p_numero;

		IF variavel_id IS NULL THEN
			INSERT INTO banco(numero, nome, ativo)
			VALUES (p_numero, p_nome, p_ativo);
		ELSE
			RETURN variavel_id;
		END IF;

		SELECT INTO variavel_id numero FROM banco WHERE numero = p_numero;
		RETURN variavel_id;
	END;
$$;
SELECT bancos_add(5433, 'Banco Novo em outra porta', true); -- 5433
SELECT numero, nome, ativo FROM banco WHERE numero = 5433
```
