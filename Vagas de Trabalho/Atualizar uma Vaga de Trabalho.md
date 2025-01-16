#api #vagas #put

### Propriedades

| endpoint  | `/department/{departmentId}/job/{jobId}`                               |
| --------- | ---------------------------------------------------------------------- |
| método    | **PUT**                                                                |
| cabeçalho | `Authorization: Bearer <token>`                                        |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Atualizar%20Vaga) |

---

### Descrição
Esta rota permite atualizar as informações de uma vaga específica. Apenas o dono da empresa ou um administrador vinculado à vaga podem realizar essa operação. O identificador único da vaga (`jobId`) deve ser fornecido como parte da URL.

### Parâmetros da Rota

| **Parâmetro** | **Tipo** | **Descrição**                       | **Obrigatoriedade** |
| ------------- | -------- | ----------------------------------- | ------------------- |
| `jobId`       | `string` | Identificador único da vaga (UUID). | _Obrigatório_       |

---

### Exemplo de Payload (Request Body)
#### Atualizar Vaga
```json
{
  "title": "Desenvolvedor Fullstack",
  "description": "Desenvolver aplicações web utilizando React e Spring Boot."
}
```


---
### Parâmetros do Payload

|**Chave**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`title`|`string`|Título atualizado da vaga.|_Obrigatório_|
|`description`|`string`|Descrição atualizada da vaga.|_Obrigatório_|
|`requirements`|`array of strings`|Lista atualizada de requisitos para a vaga.|_Obrigatório_|
|`salary`|`number`|Salário atualizado da vaga.|_Obrigatório_|
|`type`|`string`|Tipo atualizado de contrato (ex.: `CLT`, `PJ`).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a atualização da vaga for bem-sucedida, a resposta conterá os dados atualizados da vaga:
```json
{
	"id": "397c9e07-18e3-499d-b363-8bb460a63d67",
	"title": "Desenvolvedor Fullstack",
	"description": "Desenvolver aplicações web utilizando React e Spring Boot.",
	"createdAt": "2025-01-16T15:35:27.163745",
	"updatedAt": "2025-01-16T17:07:36.489604",
	"department": {
		"id": "b79ce10d-9a4e-4770-bcb4-351fbba85063",
		"name": "Departamento de TI",
		"description": "Responsável por gerenciar a infraestrutura de tecnologia.",
		"createdAt": "2025-01-16T14:43:14.237416",
		"updatedAt": null
	},
	"isActive": true
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
  "timestamp": "2025-01-15T15:11:00.789123"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado à vaga:
```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T15:12:00.321456"
}
```

#### Vaga Não Encontrada - **404 Not Found**
Caso o `jobId` fornecido não corresponda a uma vaga existente:
```json
{
  "status": 404,
  "message": "Job not found",
  "details": "The job with the given ID does not exist.",
  "timestamp": "2025-01-15T15:13:00.987654"
}
```

#### Erro de Validação - **400 Bad Request**
Caso algum campo do payload seja inválido ou mal formatado:
```json
{
	"status": 400,
	"message": "Validation error",
	"errors": [
		"title: title is required."
	],
	"timestamp": "2025-01-16T17:09:52.472144"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado à vaga podem acessar esta rota.
- Certifique-se de que o `jobId` fornecido seja um UUID válido.