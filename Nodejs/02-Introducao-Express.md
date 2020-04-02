# O que é o Express?

-   Framework web minimalista e rápido para Node.js
-   Fornece uma estrutura e conjunto de recursos robusto para aplicações Web e mobile
-   Dispõe de métodos utilitários HTTP e middlewares para criar uma API rápida e segura.

```
npm install express --save
npm init
node index.js
```

## Atividade Prática

-   Criar uma endpoin para users:
    -   Listar usuários (GET)
    -   Criar usuários (POST)
    -   Modificar usuários (PUT)
    -   Remover usuários (DELETE)

```
Project
├── node_modules
├── routes
│     ├── userRoute.js
│     └── user.json
├── index.js
├── package-lock.json
└── package.json
```

### index.js

```JS
const express = require('express')
const bodyParse = require('body-parser')
const userRoute = require('./routes/userRoute')

const app = express()
const port = 3000

app.use(
    bodyParse.urlencoded(
        {
            extended: false
        }
    )
)

userRoute(app)
app.get('/', (req, res) => res.send('Olá mundo pelo express'))
app.listen(port, () => console.log(`Api rodando na porta ${port}`))
```

### userRoute.js

```JS
const fs = require('fs')
const { join } = require('path')
const filePath = join(__dirname, 'users.json')

const getUsers = () => {
    const data = fs.existsSync(filePath)
        ? fs.readFileSync(filePath)
        : []
    try {
        return JSON.parse(data)
    } catch (error) {
        return []
    }
}

const saveUser = (users) => fs.writeFileSync(filePath, JSON.stringify(users, null, '\t'))

const userRoute = (app) => {
    app.route('/users/:id?')
        .get((req, res) => {
            const users = getUsers()
            res.send({ users })
        })
        .post((req, res) => {
            const users = getUsers()

            users.push(req.body)
            saveUser(users)

            res.status(201).send('OK')
        })
        .put((req, res) => {
            const users = getUsers()

            saveUser(users.map(user => {
                if (user.id === req.params.id) {
                    return {
                        ...user,
                        ...req.body
                    }
                }
                return user
            }))

            res.status(200).send('OK')
        })
        .delete((req, res) => {
            const users = getUsers()

            saveUser(users.filter(user => user.id !== req.params.id))

            res.status(200).send('OK')
        })
}
module.exports = userRoute
```
