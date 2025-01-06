#api #empresa #patch

### Propriedades

| endpoint  | `/company`                                                                      |
| --------- | ------------------------------------------------------------------------------- |
| método    | **PATCH**                                                                       |
| cabeçalho | `Authorization: Bearer <token>`                                                 |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Atualizar%20uma%20empresa) |

---

### Descrição
Esta rota permite atualizar informações de uma empresa existente no sistema. Apenas o dono da empresa (usuário autenticado) pode realizar essa operação. Automaticamente atualizará a `empresa` associada ao usuário autenticado.

---

### Exemplo de Payload (Request Body)
#### Atualizar uma empresa:
```json
{
  "name": "Novo Nome da Empresa",
  "description": "Nova descrição para a empresa."
}
```

#### Parâmetros do Payload

| **Chave**     | **Tipo** | **Descrição**                    | **Obrigatoriedade** |
| ------------- | -------- | -------------------------------- | ------------------- |
| `name`        | `string` | Nome atualizado da empresa.      | _Opcional_          |
| `description` | `string` | Descrição atualizada da empresa. | _Opcional_          |

---
### Exemplo de Resposta
#### Sucesso - `200 OK`
Se a atualização for bem-sucedida, a resposta conterá os dados atualizados da empresa:

```json
{
	"id": "80d56980-4a70-4f87-831d-d8eabf677a81",
	"name": "Novo Nome da Empresa",
	"description": "Nova descrição para a empresa.",
	"createdAt": "2025-01-03T19:09:36.590708",
	"updatedAt": "2025-01-03T20:27:24.099356",
	"owner": {
		"id": "fd764e4f-c99c-4f69-a0bc-357ae5154d8c",
		"name": "João Silva",
		"email": "joao.silva@example.com",
		"imgUrl": "https://example.com/profile.jpg",
		"typeUser": "COMPANY",
		"createdAt": "2024-12-26T20:51:45.099723",
		"updatedAt": null
	}
}
```

---

### Exceções
#### Erro de Autorização - **401 Unauthorized**
Caso o token de autenticação seja inválido ou esteja ausente:
```json
{
	"status": 401,
	"message": "Access denied. Please ensure your token is correct and active.",
	"errors": [
		"Full authentication is required to access this resource"
	],
	"timestamp": "2025-01-03T20:29:43.251897"
}
```

#### Empresa Não Encontrada - **404 Not Found**
Caso o usuário autenticado não tenha nenhuma empresa vinculada.
```json
{
	"status": 404,
	"message": "This user does not have any company associated with them.",
	"timestamp": "2025-01-03T20:44:59.865378"
}
```

#### Erro de Validação - **400 Bad Request**
Caso algum campo do payload seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "description: must be a string value"
  ],
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas os campos fornecidos no payload serão atualizados. Campos não incluídos no payload permanecerão inalterados.
- Automaticamente atualizará a `empresa` associada ao usuário autenticado.
