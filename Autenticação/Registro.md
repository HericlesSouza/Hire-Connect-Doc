  #api #autenticação #registro #post #auth

### Propriedades

| endpoint | `/auth/register`                   |
| -------- | ---------------------------------- |
| método   | __POST__                           |
| corpo    | [PAYLOAD](#Exemplo%20De%20Payload) |

---

### Descrição
Esta rota permite o registro de um novo usuário no sistema. Através do método `POST` no endpoint `/auth/register`, é possível enviar os dados necessários para a criação de um perfil de usuário com o tipo e permissões apropriadas.

---

### Exemplo de Payload (Request Body):
#### Registro de um usuário

```json
{
  "name": "João Silva",
  "email": "joao.silva@example.com",
  "password": "senhaSegura123",
  "img_url": "https://example.com/imagens/joao.jpg",
  "type": "FREELANCER"
}
```


#### Parâmetros do Payload

| **Chave**  | **Tipo**                                                    | **Descrição**                                                                                                               | **Obrigatoriedade** |
| ---------- | ----------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- | ------------------- |
| `name`     | `string`                                                    | Nome completo do usuário. Campo obrigatório com limite de 50 caracteres.                                                    | *Obrigatório*       |
| `email`    | `string`                                                    | Endereço de e-mail do usuário, que deve ser único e válido, com um máximo de 50 caracteres.                                 | *Obrigatório*       |
| `password` | `string`                                                    | Senha do usuário com até 150 caracteres.                                                                                    | *Obrigatório*       |
| `img_url`  | `string`                                                    | URL para a imagem de perfil do usuário. Campo opcional, podendo conter até 2000 caracteres.                                 | Opcional            |
| `type`     | [UserRole](../Utilitários/Usuários/Tipos%20de%20Usuário.md) | Define o perfil de acesso e categoria do usuário no sistema. Deve corresponder a um dos valores predefinidos em *UserRole*. | *Obrigatório*       |

---
### Exemplo de Resposta

#### Sucesso - `201 Created`
Se o registro for bem-sucedido, a resposta será:
```json
{ 
	"id": 12345, 
	"name": "João Silva", 
	"email": "joao.silva@example.com", 
	"img_url": "https://example.com/profile.jpg", 
	"type": "FREELANCER", 
	"createdAt": "2024-11-01T12:00:00Z" 
}
```

---
### Exceções
#### Erro de Validação - `400 Bad Request`
Caso algum parâmetro esteja incorreto ou falte um campo obrigatório, por exemplo:
```json
{
	"status": 400,
	"message": "Validation error",
	"errors": [
		"password: password is required"
	],
	"timestamp": "2024-09-19T16:38:25.309715"
}
```

#### Conflito - `409 Conflict`
Se o e-mail fornecido já estiver em uso por outro usuário, a resposta será:
```json
{
	"status": 409,
	"message": "The email joao.silva@example.com already exists.",
	"errors": [
		"Email is already registered.",
		"Please use a different email address."
	],
	"timestamp": "2024-09-19T16:47:24.284168"
}
```
