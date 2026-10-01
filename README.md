## Resolução

O projeto foi reorganizado no padrão MVC, sem View (a resposta é JSON), sem mudar o comportamento da API.

- **models/usuarioModel.js:** guarda todas as queries SQL (`criarUsuario`, `listarUsuarios`, `atualizarUsuario`, `deletarUsuario`). Não usa `req` nem `res`.
- **controllers/usuarioController.js:** recebe `req` e `res`, chama o model e define o status HTTP e a resposta.
- **routes/usuarioRoutes.js:** liga cada endpoint (`GET /`, `POST /`, `PUT /:id`, `DELETE /:id`) à função do controller.
- **server.js:** cria o Express e conecta as rotas.


## Testes

Base URL: `http://localhost:3002`

**Teste 1 — Listar usuários**

```bash
curl -i http://localhost:3002/
```

Esperado: `200` e array com os 5 usuários.

**Teste 2 — Criar usuário válido**

```bash
curl -i -X POST http://localhost:3002/ -H "Content-Type: application/json" -d '{"nome":"Miguel Silva","email":"miguel.silva@exemplo.com"}'
```

Esperado: `201` e `{ "id": 6, "nome": "Miguel SIlva", "email":miguel.silva@exemplo.com" }`.

**Teste 3 — Criar usuário sem email**

```bash
curl -i -X POST http://localhost:3002/ -H "Content-Type: application/json" -d '{"nome":"Usuário Incompleto"}'
```

Esperado: `500` e `{ "erro": "Falha ao criar usuário" }`.

**Teste 4 — Criar usuário com e-mail duplicado**

```bash
curl -i -X POST http://localhost:3002/ -H "Content-Type: application/json" -d '{"nome":"Outro Nome","email":"miguel.silva@exemplo.com"}'
```

Esperado: `500` e `{ "erro": "Falha ao criar usuário" }`.

**Teste 5 — Atualizar usuário existente** 

```bash
curl -i -X PUT http://localhost:3002/6 -H "Content-Type: application/json" -d '{"nome":"Miguel Marinho Silva"}'
```

Esperado: `200` e `{ "mensagem": "Atualizado com sucesso" }`. Só o nome muda; o email continua o mesmo.

**Teste 6 — Atualizar usuário que não existe**

```bash
curl -i -X PUT http://localhost:3002/999999 -H "Content-Type: application/json" -d '{"nome":"Não Importa"}'
```

Esperado: `404` e `{ "erro": "Usuário não encontrado" }`.

**Teste 7 — Remover usuário existente** 

```bash
curl -i -X DELETE http://localhost:3002/6
```

Esperado: `200` e `{ "mensagem": "Deletado com sucesso" }`.

**Teste 8 — Remover usuário que não existe**

```bash
curl -i -X DELETE http://localhost:3002/999999
```

Esperado: `404` e `{ "erro": "Usuário não encontrado" }`.