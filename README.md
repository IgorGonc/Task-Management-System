# Task-Management-System
Este projeto tem como objetivo a criação de um sistema de gerenciamento de tarefas que permita aos usuários criar, ler, atualizar e deletar tarefas. Além disso, o sistema suportará autenticação de usuários e integração com um serviço de email para notificações.

== Requirements

*Must Have*
- CRUD de tarefas
- Autenticação de usuários (login e registro)
- Validação de dados
- Notificações por email
- Persistência de dados em banco de dados relacional

*Should Have*
- Paginação e filtragem de tarefas
- API documentada com Swagger
- Testes automatizados

*Could Have*
- Deploy na nuvem (Heroku ou AWS)
- Integração com serviços de terceiros (ex: Google Calendar)

== Method

=== Arquitetura

A arquitetura da aplicação será baseada em uma API RESTful desenvolvida com Node.js, Express.js para criação das rotas, e um banco de dados PostgreSQL para persistência de dados. Será utilizada a biblioteca JWT para autenticação e o Nodemailer para envio de emails.

[plantuml, arquitetura, png]
----
@startuml
package "Backend" {
  [API Server] --> [Database]
  [API Server] --> [Email Service]
  [User] --> [API Server]
}

package "Components" {
  [Auth Controller] --> [User Service]
  [Task Controller] --> [Task Service]
  [User Service] --> [Database]
  [Task Service] --> [Database]
}
@enduml
----

=== Banco de Dados

A estrutura do banco de dados incluirá duas tabelas principais: `users` e `tasks`.

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) UNIQUE NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE tasks (
    id SERIAL PRIMARY KEY,
    title VARCHAR(100) NOT NULL,
    description TEXT,
    user_id INT REFERENCES users(id),
    status VARCHAR(20) DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
