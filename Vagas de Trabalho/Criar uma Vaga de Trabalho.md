#api #vagas #departamento #post

### Propriedades

| endpoint  | `/department/{departmentId}/job`                                          |
| --------- | ------------------------------------------------------------------------- |
| método    | **POST**                                                                  |
| cabeçalho | `Authorization: Bearer <token>`                                           |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Criar%20Nova%20Vaga) |

---

### Descrição
Esta rota permite criar uma nova vaga de trabalho em um departamento específico. Apenas o dono da empresa ou um administrador vinculado a empresa podem realizar essa operação. O identificador único do departamento (`departmentId`) deve ser fornecido como parte da URL. 

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`departmentId`|`string`|Identificador único do departamento (UUID).|_Obrigatório_|

---

### Exemplo de Payload (Request Body)

#### Criar Nova Vaga
```json
{
  "title": "Desenvolvedor Backend",
  "description": "Desenvolver e manter APIs RESTful utilizando Java Spring Boot."
}
```

### Parâmetros do Payload

| **Chave**      | **Tipo**           | **Descrição**                                | **Obrigatoriedade** |
| -------------- | ------------------ | -------------------------------------------- | ------------------- |
| `title`        | `string`           | Título da vaga.                              | _Obrigatório_       |
| `description`  | `string`           | Descrição detalhada da vaga.                 | _Obrigatório_       |

---

### Exemplo de Resposta

#### Sucesso - `201 Created`
```json
{
	"id": "99c79b9a-c226-4597-90af-cd2a9dd6229b",
	"title": "Desenvolvedor Backend",
	"description": "Desenvolver e manter APIs RESTful utilizando Java Spring Boot.",
	"createdAt": "2025-01-16T14:49:44.613282",
	"updatedAt": null,
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
#### Erro de Regra de Negócio - **400 Bad Request**
Caso o usuário tente criar uma vaga com um nome que já existe naquele departamento:
```json
{
	"status": 400,
	"message": "A job vacancy with this name already exists in the department.",
	"timestamp": "2025-01-16T15:19:57.673449"
}
```

#### Erro de Autorização - **401 Unauthorized**
Caso o token de autenticação seja inválido ou esteja ausente:
```json
{
  "status": 401,
  "message": "Access denied. Please ensure your token is correct and active.",
  "errors": [
    "Full authentication is required to access this resource"
  ],
  "timestamp": "2025-01-15T14:46:00.654321"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado ao departamento:
```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T14:47:00.987654"
}
```

#### Departamento Não Encontrado - **404 Not Found**
Caso o `departmentId` fornecido não corresponda a um departamento existente:
```json
{
	"status": 404,
	"message": "The department with the given ID does not exist.",
	"timestamp": "2025-01-16T15:17:55.927848"
}
```

#### Erro de Validação - **400 Bad Request**
Caso algum campo do payload seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "title: must be a non-empty string"
  ],
  "timestamp": "2025-01-15T14:49:00.789123"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado ao departamento podem acessar esta rota.
- Certifique-se de que o `departmentId` fornecido seja um UUID válido.
- Todos os campos obrigatórios devem ser preenchidos corretamente para evitar erros de validação.