# 1 - O que é uma CLI

- Ferramenta que disponibiliza uma interface de linha de comando para executar tarefas no terminal.
- Normalmente são criadas através de Shell Script
- Automatiza uma tarefa através de um arquivo executável
- Pode ser facilmente distribuído em várias plataformas

## GUI vs CLI

### GUI

> Copiando um arquivo:

1. Abrir o gerneciador de arquivos
2. Navegar entre os diretórios até achar o desejado
3. Selecionar todos os arquivos que terminam com ".js"
4. Copiar os arquivos
5. Trocar de diretório no gerenciador de arquivos
6. Colar os arquivos

### CLI

```
@user> cp *.js ~/Documentos/PastaDestino
```

## Por que criar uma CLI em Node.js?

- A popularidade do Node.js se dá ao rico ecossistema de pacotes
- Mais de 900.000 pacotes registrados no NPM
- CLIs podem ser facilmente distribuídas e consumidas em múltiplas plataformas
- Explorar o ecossistema, incluindo sua grande quantidade de pacotes focados em CLI

## CLIs em Node.js

- NPM
- Yarn
- Babel
- Gulp
- Grunt
- Webpack

## Prática

- Criar uma CLI simples para procurar arquivos em um diretório
- Instalar local para desenvolvimento e testes

```
search-files-cli
├── bin
│    └── search-files-cli
└── package.json
```

> search-files-cli

```js
#!/usr/bin/env node
const fs = require("fs");
const { join } = require("path");

const fileName = process.argv.splice(2, process.argv.length - 1).join();

function searchFiles(filter, startPath = ".") {
  const files = fs.readdirSync(startPath);

  files.map((filePath) => {
    const fullFilePath = join(startPath, filePath);
    const statFilePath = fs.lstatSync(fullFilePath);

    if (statFilePath.isDirectory()) {
      return searchFiles(filter, fullFilePath);
    }

    if (fullFilePath.indexOf(filter) !== -1) {
      console.log(fullFilePath);
    }
  });
}

searchFiles(fileName);
```

> package.json

```JSON
{
  "name": "search-files-cli",
  "version": "1.0.0",
  "description": "Exemplo de CLI para procurar arquivos em uma pasta",
  "preferGlobal": true,
  "bin": {
    "search-files": "./bin/search-files-cli"
  },
  "files": [
    "./bin"
  ],
  "keywords": [],
  "author": "Henrique Schreiner",
  "license": "MIT"
}
```

> comandos

```PowerShell
npm link
search-files .json
```
