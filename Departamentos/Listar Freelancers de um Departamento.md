#api #departamento #freelancers #get

### Propriedades

|endpoint|`/department/{departmentId}/freelancers`|
|---|---|
|método|**GET**|
|cabeçalho|`Authorization: Bearer <token>`|
|corpo|Nenhum|

---

### Descrição

Esta rota permite listar todos os freelancers que estão atualmente trabalhando em um departamento específico. Apenas o dono da empresa ou um administrador vinculado à empresa podem acessar essa rota. O identificador único do departamento (`departmentId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`departmentId`|`string`|Identificador único do departamento (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `200 OK`

Se a listagem for bem-sucedida, a resposta conterá os detalhes dos freelancers associados ao departamento:

```json
[
	{
		"id": "13998e14-1152-4cbd-abff-d066e1290d5b",
		"name": "João Silva",
		"email": "joao.silva@example.com",
		"imgUrl": "image@",
		"typeUser": "FREELANCER",
		"createdAt": "2025-02-12T21:23:28.291459",
		"updatedAt": "2025-02-26T20:11:04.4444",
		"specialization": "Desenvolvedor Full Stack",
		"bio": "Tenho 60 anos e desenvolvo códigos"
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
  "timestamp": "2025-01-15T20:00:00.123456"
}
```

#### Permissão Negada - **403 Forbidden**

Caso o usuário autenticado não seja o dono da empresa ou um administrador vinculado ao departamento:

```json
{
  "status": 403,
  "message": "Access denied: You do not have permission to access this resource.",
  "timestamp": "2025-01-15T20:01:00.654321"
}
```

#### Departamento Não Encontrado - **404 Not Found**

Caso o `departmentId` fornecido não corresponda a um departamento existente:

```json
{
  "status": 404,
  "message": "Department not found",
  "details": "The department with the given ID does not exist.",
  "timestamp": "2025-01-15T20:02:00.789123"
}
```

---

### Notas Adicionais

- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono da empresa ou um administrador vinculado ao departamento podem acessar esta rota.
- Certifique-se de que o `departmentId` fornecido seja um UUID válido.