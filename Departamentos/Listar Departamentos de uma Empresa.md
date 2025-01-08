#api #departamento #empresa #get

### Propriedades

|endpoint|`/company/{companyId}/departments`|
|---|---|
|método|**GET**|
|cabeçalho|`Authorization: Bearer <token>`|

---

### Descrição

Esta rota permite listar todos os departamentos de uma empresa específica. Apenas o dono da empresa (usuário autenticado) ou um administrador vinculado à empresa podem acessar esta rota. O identificador único da empresa (`companyId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`companyId`|`string`|Identificador único da empresa (UUID).|_Obrigatório_|

---

### Exemplo de Resposta
#### Sucesso - `200 OK`
Se a requisição for bem-sucedida, a resposta retornará uma lista com todos os departamentos da empresa:
```json
[
  {
    "id": "123e4567-e89b-12d3-a456-426614174999",
    "name": "Departamento de TI",
    "description": "Responsável por gerenciar a infraestrutura de tecnologia.",
    "createdAt": "2024-11-22T16:38:25.309715",
    "updatedAt": "2024-11-22T16:38:25.309715"
  },
  {
    "id": "223e4567-e89b-12d3-a456-426614174888",
    "name": "Departamento de RH",
    "description": "Responsável pela gestão de pessoas.",
    "createdAt": "2024-11-21T12:30:10.123456",
    "updatedAt": "2024-11-21T12:30:10.123456"
  }
]
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
	"timestamp": "2025-01-08T15:12:46.062818"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário não seja o dono da empresa ou um administrador vinculado a ela:
```json
{
	"status": 403,
	"message": "Access denied: You do not have permission to access this resource.",
	"timestamp": "2025-01-08T09:59:23.658141"
}
```


#### Empresa Não Encontrada - **404 Not Found**
Caso o `companyId` fornecido não corresponda a nenhuma empresa cadastrada:
```json
{
	"status": 404,
	"message": "The company with the given ID does not exist.",
	"timestamp": "2025-01-08T15:13:57.467323"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas usuários autenticados que sejam donos da empresa ou administradores vinculados a ela podem acessar esta rota.
- Certifique-se de que o `companyId` fornecido é um UUID válido.