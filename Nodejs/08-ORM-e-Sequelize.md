# ORM - Mapeamento objeto-relacional

- ORM, o que são?

- Exemplos
  - Hibernate (Java)
  - DataMapper (Rubby)
  - Sequelize (Node)

## Sequelize

- Compatível com os bancos de dados:
  - Mysql
  - Sqlite
  - Postgresql
  - SQL Server
- Versátil
- Robusto

# Prática

https://github.com/gmeneguz/intro_node_pg_sequelize

```
Project
├── node_modules
├── sequelize
|     └── models
|     | └── models
|     |      ├── evento.js
|     │      ├── index.js
|     │      └── participante.js
│     ├── _database.js
│     ├── 1_create.js
│     ├── 2_insert.js
│     ├── 3_select.js
│     └── 4_select_advanced.js
├── package-lock.json
└── package.json
```

> package.json

```JSON
{
  "name": "aula_node_postgres",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {
    "pg": "^7.8.1",
    "sequelize": "^4.44.4"
  },
  "devDependencies": {},
  "scripts": {},
  "author": "",
  "license": "ISC"
}
```

> evento.js

```JS
const Sequelize = require('sequelize')
const sequelize = require('../_database')


const Evento = sequelize.define('evento', {
  nome: {
    type: Sequelize.STRING
  },
})
module.exports = Evento

const Participante = require('./participante')
Evento.belongsToMany(Participante, {through: 'evento_participante'});
```

> index.js

```JS
const sequelize = require('../_database')

const models = {
  evento: require('./evento'),
  participante: require('./participante'),
  sequelize: sequelize
}

module.exports = models
```

> participante.js

```JS
const Sequelize = require('sequelize')
const sequelize = require('../_database')

const Participante = sequelize.define('participante', {
  nome: {
    type: Sequelize.STRING
  },
})

module.exports = Participante

const Evento = require('./evento')
Participante.belongsToMany(Evento, {through: 'evento_participante'});
```

> \_database.js

```JS
const Sequelize = require("sequelize");

const sequelize = new Sequelize({
  host: "localhost",
  database: "dio",
  username: "postgres",
  password: "postgres",
  dialect: "postgres",
  port: 5432,
  logging: true,
});

module.exports = sequelize;

// Test DB Conenction //
async function test() {
  try {
    let result = await sequelize.authenticate();
    console.log(
      "Sequelize realizou a conexão com o banco de dados com sucesso!"
    );
  } catch (error) {
    console.error("Houve um erro ao conectar com o banco de dados:");
    console.error(error);
    process.exit();
  }
}

test();
```

> 1_create.js

```JS
const models = require('./models')

async function create(){
  await models.sequelize.sync({force: true})
  await models.sequelize.close()

  console.log("Banco sincronizado");
}
create()
```

> 2_insert.js

```JS
const models = require('./models')

async function insert(){

  //Eventos
  const eventoNode = await models.evento.create({nome: 'Encontro de Nodejs'})
  const eventoPostgres = await models.evento.create({nome: 'Encontro de Postgresql'})

  //Participantes
  const carlos  = await models.participante.create({nome: 'Carlos'})
  const augusto = await models.participante.create({nome: 'Augusto'})
  const janaina = await models.participante.create({nome: 'Janaína'})
  const rafael  = await models.participante.create({nome: 'Rafael'})


  //Participantes em eventos
  await eventoNode.setParticipantes([carlos, augusto, janaina])
  await eventoPostgres.setParticipantes([janaina, rafael])

  await models.sequelize.close()

  console.log("Dados Inseridos");
}
insert()
```

> 3_select.js

```JS
const models = require('./models')

async function select(){
  console.log("\n");

  //Eventos
  const eventos = await models.evento.findAll()
  eventos.forEach((evento) => {
    console.log("Evento: ", evento.nome)
  })
  console.log("\n");

  //Participantes
  const participantes  = await models.participante.findAll()
  participantes.forEach((participante) => {
    console.log("Participante: ", participante.nome)
  })
  console.log("\n");

  //Participantes em eventos
  const eventosComParticipantes = await models.evento.findAll({
    include: [
      {
        model: models.participante
      }
    ]
  })
  eventosComParticipantes.forEach((evento) => {
    console.log("Evento: ", evento.nome)
    evento.participantes.forEach((participante) => {
      console.log("----------> Participante: ", participante.nome)
    })
  })
  console.log("\n");

  await models.sequelize.close()
}
select()
```

> 4_select_advanced.js

```JS
const Sequelize = require('sequelize')
const models = require('./models')
const Op = Sequelize.Op

async function select(){

  //Regra 1 : Listar apenas eventos que tenham Nodejs no nome
  //Regra 2 : Dentro desse(s) evento(s), listar apenas pessoas que NÃO tenham a letra "o" no nome

  const eventosComParticipantes = await models.evento.findAll({
    where: {
      nome: {
        [Op.like]: '%Nodejs%'
      }
    },
    include: [
      {
        model: models.participante,
        where: {
          nome: {
              [Op.notLike]: '%o%'
          },
        }
      }
    ]
  })
  eventosComParticipantes.forEach((evento) => {
    console.log("Evento: ", evento.nome)
    evento.participantes.forEach((participante) => {
      console.log("----------> Participante: ", participante.nome)
    })
  })
  console.log("\n");

  await models.sequelize.close()
}
select()
```

```
npm install --save sequelize
node sequelize/_database.js
node sequelize/1_create.js
node sequelize/2_insert.js
node sequelize/3_select.js
node sequelize/4_select_advanced.js
```
