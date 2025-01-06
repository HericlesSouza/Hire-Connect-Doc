#api #empresa  #post
### Propriedades

| endpoint  | `/company`                                |
| --------- | ----------------------------------------- |
| método    | __POST__                                  |
| cabeçalho | `Authorization: Bearer <token>`           |
| corpo     | [PAYLOAD](#Cadastro%20de%20uma%20empresa) |

---
### Descrição 
Este endpoint permite que usuários autenticados do tipo **COMPANY** registrem uma nova empresa no sistema. Após o registro, o usuário será automaticamente vinculado como proprietário da empresa criada.

---
### Exemplo de Payload (Request Body):
#### Cadastro de uma empresa

```json 
{
	"name": "Empresa Exemplo",
	"description": "contato@empresaexemplo.com",
}
```

#### Parâmetros do Payload

| **Chave**     | **Tipo** | **Descrição**         | **Obrigatoriedade** |
| ------------- | -------- | --------------------- | ------------------- |
| `name`        | `string` | Nome da empresa.      | _Obrigatório_       |
| `description` | `string` | Descrição da empresa. | _Obrigatório_       |

---
### Exemplo de Resposta

#### Sucesso - `201 Created`
```json
{ 
	"id": 12345, 
	"name": "Empresa Exemplo",
	"description": "contato@empresaexemplo.com",
	"createdAt": "2025-01-03T19:09:36.590708",
	"updatedAt": null,
	"owner": {
		"id": 12345, 
		"name": "João Silva", 
		"email": "joao.silva@example.com", 
		"img_url": "https://example.com/profile.jpg", 
		"typeUser": "COMPANY",
		"createdAt": "2024-12-26T20:51:45.099723",
		"updatedAt": null
	}
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
		"name: name is required"
	],
	"timestamp": "2024-09-19T16:38:25.309715"
}
```

#### Erro de Empresa Associada - **400 Bad Request**
Caso o usuário já tenha uma empresa associada a ele:
```json
{
	"status": 400,
	"message": "User already has a company associated",
	"timestamp": "2024-12-29T23:50:11.827065"
}
```


#### Erro de Autorização - **401 Unauthorized**
Caso o token de autenticação não seja válido ou esteja ausente, o sistema retornará o seguinte erro:

```json
{
	"status": 401,
	"message": "Access denied. Please ensure your token is correct and active.",
	"errors": [
		"Full authentication is required to access this resource"
	],
	"timestamp": "2024-12-29T23:31:45.900211"
}
```

#### Erro de Permissão - **403 Forbidden**
Caso o usuário não tenha permissão suficiente para realizar a operação (por exemplo, um usuário que não seja do tipo `COMPANY` tentando criar uma empresa), o sistema retornará o seguinte erro:

```json
{
	"status": 403,
	"message": "Access denied: You do not have permission to access this resource.",
	"timestamp": "2025-01-03T18:56:22.315201"
}
```
