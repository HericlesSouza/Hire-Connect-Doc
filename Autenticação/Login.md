  #api #autenticação #login #post #auth

### Propriedades

| endpoint | `/auth/login`                      |
| -------- | ---------------------------------- |
| method   | __POST__                           |
| body     | [PAYLOAD](#Exemplo%20De%20Payload) |

---
### Descrição
Esta rota permite o login de um usuário existente no sistema. Através do método `POST` no endpoint `/auth/login`, é possível enviar as credenciais do usuário para autenticação. O sistema retorna um token JWT *(JSON Web Token)* para autenticação nas requisições futuras.

---
### Exemplo de Payload (Request Body):

#### Login de um usuário
```json
{
  "email": "joao.silva@example.com",
  "password": "senhaSegura123"
}
```

#### Parâmetros do Payload

| **Chave**  | **Tipo** | **Descrição**                                                       | **Obrigatoriedade** |
| ---------- | -------- | ------------------------------------------------------------------- | ------------------- |
| `email`    | `string` | Endereço de e-mail do usuário. Deve ser válido e já cadastrado.     | _Obrigatório_       |
| `password` | `string` | Senha do usuário. Deve coincidir com a senha cadastrada no sistema. | _Obrigatório_       |

---
### Exemplo de Resposta
#### Sucesso - `200 OK`
 ```json
{ 
	"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
	"user": {
		"id": 12345,
		"name": "João Silva",
		"email": "joao.silva@example.com",
		"type": "FREELANCER"
	},
	"expiresAt": "2024-11-01T14:00:00Z"
}
```

---
### Exceções

#### Erro de Validação - **400 Bad Request**
Caso algum parâmetro esteja incorreto ou falte um campo obrigatório, por exemplo:
```json
{
	"status": 400,
	"message": "Validation error",
	"errors": [
		"email: email is required",
		"password: password is required"
	],
	"timestamp": "2024-09-19T16:38:25.309715"
}
```

#### Credenciais Inválidas - **401 Unauthorized**
Caso o e-mail ou senha estejam incorretos, a resposta será:
```json
{
	"status": 401,
	"message": "Invalid credentials",
	"errors": [
		"Email or password is incorrect."
	],
	"timestamp": "2024-09-19T16:47:24.284168"
}
```

---
### Notas Adicionais
- **Token JWT**: O token JWT retornado deve ser incluído no cabeçalho de autorização (`Authorization: Bearer <token>`) nas requisições subsequentes para garantir acesso autorizado a rotas protegidas.
- **Expiração do Token**: O token tem um tempo de expiração de 1 dia, após o qual será necessário fazer login novamente.