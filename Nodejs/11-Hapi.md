# 1 - O que é o Hapi?

- Um framework para construir aplicações e seviços
- Similar ao Express.js
- Softwar de código aberto
- Criado por Eran Hammer, Arquiteto de Plataforma Móvel no Walmart
- Permite que os desenvolvedores se concentrem em escrever lógica de aplicativo reutilizável em ves de gastar tempo construindo infraestrutura

## Funcionalidades

- **Authentication e Authorization**: esquemas e estratégias de autenticação e autorização
- **Armazenamento em cache**: cache do lado do cliente e do servidor (catbox)
- **Roteamento**: permite configurar como o aplicativo da Web ou rotas da API devem ser exibidas
- **Validação**: validação de schema de objetos (Joi)
- **Cookies**: opção de configuração para uso de cookies
- **Logging**: métodos nativos para geração de logs
- **Tratamento de Erros**: conjunto de utilitários para retornar objetos de erro compatíveis com HTTP (Boom)
- **Monitoramento de Processos**: monitorar e reportar uma variedade de eventos (Good)

## Hapi vs Express

### Express

- Um pouco menos opinativo que o Hapi, sendo menos abstraído do Node
- Parece uma aplicação nativa em Node.js
- Desenvolvedores mais experientes o preferem por sua familiaridade
- Usa _middlewares_ para fornecer acesso ao _pipeline_ de solicitação/resposta

### Hapi

- Tem uma estrutura "padrão" para ser seguida
- Implementação mais abstrata do Node
- Usa plugins para extender suas funcionalidades
- Há geralmente um plugin em Hapi para cada middleware no Express

# Prática

[git](https://github.com/hmschreiner/node-hapijs)
