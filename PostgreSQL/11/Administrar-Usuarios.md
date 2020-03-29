# 1 - Conceitos users/roles/groups
> Definição:
> Roles (papéis ou funções), users (usuários) e grupo de usuários são "contas", perfis de atuação em um banco de dados, que possuem permissões em comum ou específicas.
> 
> Nas versões anteriores do PosgreSQL 8.1, usuários e roles tinham comportamentos diferentes. Atualmenre, roles e users são alias. 
> É possível que roles pertençam a outras roles;

# 2 - Administrando users/roles/groups

````
CREATE ROLE name [[WITH] option [...]]
where option can be:

  SUPERUSER | NOSUPERUSER
| CREATEDB | NOCREATEDB
| CREATEROLE | NOCREATEROLE
| INHERIT | NOINHERIT
| LOGIN | NOLOGIN
| REPLICATION | NOREPLICATION
| BYPASSRLS | NOBYPASSRLS
| CONNECTION LIMIT connlimit
| [ENCRYPTED] PASSWORD 'password | PASSWORD NULL
| VALID UNTIL 'timestamp'
| IN ROLE role_name [,...]
| IN GROUP role_name [,...]
| ROLE  role_name [,...]
| ADMIN  role_name [,...]
| USER  role_name [,...]
| SYSYD  uid
````

### EXEMPLO

````
CREATE ROLE administradores
    CREATEDB
    CREATEROLE
    INHERIT
    NOLOGIN
    REPLICATION
    BYPASSRLS
    CONNECTION LIMIT -1
````
````
CREATE ROLE professores
    NOCREATEDB
    NOCREATEROLE
    INHERIT
    NOLOGIN    
    NOBYPASSRLS
    CONNECTION LIMIT 10
````
````
CREATE ROLE alunos
    NOCREATEDB
    NOCREATEROLE
    INHERIT
    NOLOGIN    
    NOBYPASSRLS
    CONNECTION LIMIT 90
````

## Associação entre roles
> Quando uma role assume as permissões de outra role. Necessário a opção INHERIT

> No Momento de criação da role:
> - IN ROLE (passa a pertencer a role informada)
> - ROLE (a role informada passa a pertencer a nova role)

> Ou após a criação da role:
> - GRANT [role a ser concedida] TO [ role a assumir as permisões]

### EXEMPLO

````
CREATE ROLE professores
    NOCREATEDB
    NOCREATEROLE
    INHERIT
    NOLOGIN    
    NOBYPASSRLS
    CONNECTION LIMIT -1
````

````
CREATE ROLE daniel LOGIN CONNECTION LIMIT 1 PASSWORD '123' IN ROLE professores;
- A role daniel passa a assumir as permissões da role professores
````
````
CREATE ROLE daniel LOGIN CONNECTION LIMIT 1 PASSWORD '123' ROLE professores;
- A role professores passa a fazer parte da role daniel assumindo suas permissões 
````

````
CREATE ROLE daniel LOGIN CONNECTION LIMIT 1 PASSWORD '123';
GRANT professores TO daniel;
````

## Desassociar membros entre roles
````
REVOKE [role que será revogada] daniel [role que terá suas permissões revogadas];
REVOKE professores FROM daniel;
````

## Alterando uma role
````
ALTER ROLE role_specification [WITH] option [...]
where option can be:

  SUPERUSER | NOSUPERUSER
| CREATEDB | NOCREATEDB
| CREATEROLE | NOCREATEROLE
| INHERIT | NOINHERIT
| LOGIN | NOLOGIN
| REPLICATION | NOREPLICATION
| BYPASSRLS | NOBYPASSRLS
| CONNECTION LIMIT connlimit
| [ENCRYPTED] PASSWORD 'password | PASSWORD NULL
| VALID UNTIL 'timestamp'
````

## Excluindo uma role
````
DROP ROLE role_specification;
````


# 3 - Administrando acesssos (GRANT)
> Definição:
> São os privilégios de acesso aos objetos do banco de dados.

Privilégios:
````
-- tabela                       -- function
-- coluna                       -- language
-- sequence                     -- large objects
-- database                     -- schema
-- domain                       -- tablespace
-- foreign data wrapper         -- type
-- foreign server
````

### DATABASE
````
GRANT {{CREATE | CONNECT | TEMPORARY | TEMP} [,...] | ALL [PRIVILEGES}
    ON DATABASE database_name [,...]
    TO role_specification [,...] [WITH GRANT OPTION]
````

### SCHEMA
````
GRANT {{CREATE | USAGE} [,...] | ALL [PRIVILEGES}
    ON SCHEMA schema_name [,...]
    TO role_specification [,...] [WITH GRANT OPTION]
````

### TABLE
````
GRANT {{SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER} [,...] | ALL [PRIVILEGES}
    ON {[TABLE] table_name [,...] | ALL TABLES IN SCHEMA schema_name [,...]}
    TO role_specification [,...] [WITH GRANT OPTION]
````

## REVOKE 
> Retira as permissões da role

### DATABASE
````
REVOKE [GRANT OPTION FOR]
    {{CREATE | CONNECT | TEMPORARY | TEMP} [,...] | ALL [PRIVILEGES}
    ON DATABASE database_name [,...]
    FROM {[GROUP] role_name | PUBLIC} [,...]
    [CASCADE | RESTRICT]
````

### SCHEMA
````
REVOKE [GRANT OPTION FOR]
    {{CREATE | USAGE} [,...] | ALL [PRIVILEGES}
    ON SCHEMA schema_name [,...]
    FROM {[GROUP] role_name | PUBLIC} [,...]
    [CASCADE | RESTRICT]
````

### TABLE
````
REVOKE [GRANT OPTION FOR]
    {{SELECT | INSERT | UPDATE | DELETE | TRUNCATE | REFERENCES | TRIGGER} [,...] | ALL [PRIVILEGES}
    ON {[TABLE] table_name [,...] | ALL TABLES IN SCHEMA schema_name [,...]}
    FROM {[GROUP] role_name | PUBLIC} [,...]
    [CASCADE | RESTRICT]
````

### REVOGANDO TODAS AS PERMISSÕES (SIMPLIFICADO)

````
REVOKE ALL ON ALL TABLE IN SCHEMA [schema] FROM [role];
REVOKE ALL ON ALL SCHEMA [schema] FROM [role];
REVOKE ALL ON ALL DATABASE [database] FROM [role];
````

# Pratica 

### pgAdmin4
````
CREATE ROLE professores NOCREATEDB NOCREATEROLE INHERIT NOLOGIN NOBYPASSRLS CONNECTION LIMIT 10;
ALTER ROLE professores PASSWORD '123';

-- CREATE ROLE daniel LOGIN PASSWORD '123';
-- DROP ROLE daniel;
-- CREATE ROLE daniel LOGIN PASSWORD '123' IN ROLE professores;
-- CREATE ROLE daniel LOGIN PASSWORD '123' ROLE professores;

CREATE TABLE teste (nome varchar);
GRANT ALL ON TABLE teste TO professores;
CREATE ROLE daniel INHERIT LOGIN PASSWORD '123' IN ROLE professores;

REVOKE professores FROM daniel;
````

### Terminal(PowerShell)
````
user> psql -U postgress
Senha para usuário postgres:

postgres=# \du
                                    Lista de roles
 Nome da role |                         Atributos                         | Membro de
--------------+-----------------------------------------------------------+-----------
 porfessores  | NÒo pode efetuar login                                   +| {}
              | 10 conex§es                                               |
 postgres     | Super-usußrio, Cria role, Cria BD, ReplicaþÒo, Ignora RLS | {}

postgres=# \q

--
user> psql -U porfessores auladb
Senha para usuário porfessores:
psql: FATAL:  role "porfessores" is not permitted to log in
--

--
> psql -U daniel auladb
Senha para usuário daniel:

auladb=> SELECT nome FROM teste;
 nome
------
(0 registro)

auladb=> SELECT nome FROM teste;
ERROR:  permission denied for table teste
````

