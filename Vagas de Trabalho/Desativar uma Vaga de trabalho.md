#api #vagas #delete

### Propriedades

| endpoint  | `/department/{departmentId}/job/{jobId}` |
| --------- | ---------------------------------------- |
| método    | **DELETE**                               |
| cabeçalho | `Authorization: Bearer <token>`          |
| corpo     | Nenhum                                   |

---

### Descrição

Esta rota permite desativar uma vaga específica, definindo o campo `is_active` como `false`. Apenas o dono da empresa ou um administrador vinculado à vaga podem realizar essa operação. O identificador único da vaga (`jobId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`jobId`|`string`|Identificador único da vaga (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a desativação da vaga for bem-sucedida, a resposta conterá os dados da vaga desativada:

```json
{
	"id": "397c9e07-18e3-499d-b363-8bb460a63d67",
	"title": "Aoooba",
	"description": "Desenvolver aplicações web utilizando React e Spring Boot.",
	"createdAt": "2025-01-16T15:35:27.163745",
	"updatedAt": "2025-01-16T18:44:39.708933",
	"department": {
		"id": "b79ce10d-9a4e-4770-bcb4-351fbba85063",
		"name": "Departamento de TI",
		"description": "Responsável por gerenciar a infraestrutura de tecnologia.",
		"createdAt": "2025-01-16T14:43:14.237416",
		"updatedAt": null
	},
	"isActive": false
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
  "timestamp": "2025-01-15T16:01:00.456789"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado à vaga:
```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T16:02:00.789123"
}
```

#### Vaga Não Encontrada - **404 Not Found**
Caso o `jobId` fornecido não corresponda a uma vaga existente:
```json
{
	"status": 404,
	"message": "The job with the given ID does not exist in the specified department.",
	"timestamp": "2025-01-16T18:40:53.381158"
}
```

---

### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado à vaga podem acessar esta rota.
- Certifique-se de que o `jobId` fornecido seja um UUID válido.
- A operação não remove a vaga do banco de dados, apenas define o campo `is_active` como `false`, indicando que ela está desativada.