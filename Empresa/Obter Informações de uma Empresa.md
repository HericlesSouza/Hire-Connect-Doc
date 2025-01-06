#api #empresa #get

### Propriedades

|endpoint|`/company/{companyId}`|
|---|---|
|método|**GET**|
|cabeçalho|`Authorization: Bearer <token>`|

---

### Descrição

Esta rota permite recuperar informações detalhadas sobre uma empresa específica no sistema. Apenas usuários autenticados podem acessar esta rota. O identificador único da empresa (`companyId`) deve ser fornecido como parte da URL.

---
### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`companyId`|`string`|Identificador único da empresa (UUID).|_Obrigatório_|

---
### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a requisição for bem-sucedida, a resposta conterá as informações da empresa, como segue:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "name": "Empresa Exemplo",
  "description": "Uma empresa especializada em tecnologia.",
  "owner": {
    "id": 12345, 
	"name": "João Silva", 
	"email": "joao.silva@example.com", 
	"img_url": "https://example.com/profile.jpg", 
	"type": "COMPANY", 
	"createdAt": "2024-11-01T12:00:00Z" 
  },
  "createdAt": "2024-11-22T14:00:00Z",
  "updatedAt": "2024-11-22T16:00:00Z"
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
	"timestamp": "2025-01-03T19:29:19.211155"
}
```

#### Empresa Não Encontrada - **404 Not Found**
Caso o `companyId` fornecido não corresponda a nenhuma empresa cadastrada:
```json
{
  "status": 404,
  "message": "Company not found",
  "details": "The company with the given ID does not exist.",
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

--- 
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Certifique-se de que o `companyId` fornecido é um UUID válido.
- Apenas usuários autenticados podem acessar este endpoint.
