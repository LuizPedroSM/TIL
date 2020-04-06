# 1 - Antes do WebSocket

## Requisições HTTP Tradicionais

> Um cliente requisita dados e o servidor responde passando os dados solicitados:

- http resquest
- http response

## Requisições AJAX (XMLHttpRequest/ fetch)

> Possibilidade de fazer conexões entre um cliente e servidor de forma bidirecional

- http requests
- Data(JSON,XML,Text,....)

## Pooling

- O cliente faz requisições em busca de novos dados regularmente
- Pode ser feito de duas maneiras: **Short Polling** e **Long Polling**

### Short Polling

- Requisições AJAX feitas em intervalos de tempo fixos.

### Long Polling

- Mantém a conexão HTTP aberta até o servidor ter dados disponíveis para passar para o cliente.

# 2 - O que é um WebSocket?

- Uma aplicação TCP que escuta uma porta de um servidor que segue um protocolo específico
- Estabelece uma conexão com o navegador e se comunica com ele
- Define um canal de comunicação full-duplex através de um único socket através da Web
- A conexão é estabelecida uma única vez e a comunicação entre o servidor e o navegador se torna contínua
- Usado em aplicações que requerem atualizações regulares e rápidas a partir de um WebServer (jogos multiplayer, chat, etc)

# Prática

## Socket.IO

- Oferece uma API simples baseadas em eventos
- Permite a comunicação entre o servidor e o cliente em tempo real
- Foi desenvolvido em JavaScript e funciona tanto no Front-end quanto no Back-end (Node.js)
- O mecanismo padrão é o WebSockets, com _fallbacks_ em Flash e AJAX

[git](https://github.com/hmschreiner/node-websockets)
