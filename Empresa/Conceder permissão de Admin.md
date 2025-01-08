#api #admin #company #companyAdmin #post

### Propriedades

| endpoint  | `/company/{companyId}/admins`                                                                |
| --------- | -------------------------------------------------------------------------------------------- |
| método    | **POST**                                                                                     |
| cabeçalho | `Authorization: Bearer <token>`                                                              |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Conceder%20Permiss%C3%A3o%20de%20Admin) |

---

### Descrição

Esta rota permite conceder permissão de administrador a um usuário específico em uma empresa. Apenas o dono da empresa pode realizar esta operação. O identificador único da empresa (`companyId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`companyId`|`string`|Identificador único da empresa (UUID).|_Obrigatório_|

---

### Exemplo de Payload (Request Body)

#### Conceder Permissão de Admin
```json
{
  "userId": "60401f35-7276-405b-88b3-d31aa7f8bd06"
}
```

### Parâmetros do Payload

|**Chave**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`userId`|`string`|Identificador único do usuário a ser promovido a administrador.|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`
```json
{
  "message": "Admin permission granted successfully",
  "companyId": "476dd909-9518-4035-bc58-2870b4f1b19c",
  "userId": "60401f35-7276-405b-88b3-d31aa7f8bd06"
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
	"timestamp": "2025-01-06T17:44:06.946505"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário autenticado não seja o dono da empresa:
```json
{
	"status": 403,
	"message": "Access denied: You do not have permission to access this resource.",
	"timestamp": "2025-01-03T20:30:53.763422"
}
```

#### Empresa ou Usuário Não Encontrado - **404 Not Found**
Caso o `companyId` ou `userId` fornecido não exista no sistema:
```json
{
	"status": 404,
	"message": "User with ID 60401f35-7276-405b-88b3-0d31aa7f8bd0 not found",
	"timestamp": "2025-01-06T17:22:30.739794"
}


{
	"status": 404,
	"message": "Company with ID 60401f35-7276-405b-88b3-0d31aa7f8bd0 not found",
	"timestamp": "2025-01-06T17:22:30.739794"
}

```

#### Erro de Validação - **400 Bad Request**
Caso algum campo do payload seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "userId: userId is required"
  ],
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o **dono da empresa** tem permissão para conceder permissões de administrador.
- Certifique-se de que o `companyId` e o `userId` fornecidos são UUIDs válidos.