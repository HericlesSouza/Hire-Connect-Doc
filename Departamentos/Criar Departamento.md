#api #departamento #empresa #post

### Propriedades

| endpoint  | `/company/{companyId}/department`                                                       |
| --------- | --------------------------------------------------------------------------------------- |
| método    | **POST**                                                                                |
| cabeçalho | `Authorization: Bearer <token>`                                                         |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Cadastro%20de%20um%20Departamento) |

---

### Descrição

Esta rota permite criar um novo departamento em uma empresa específica. Apenas o dono ou administradores da empresa (usuário autenticado) pode realizar esta operação. O identificador único da empresa (`companyId`) deve ser fornecido como parte da URL.

---

### Parâmetros da Rota

| **Parâmetro** | **Tipo** | **Descrição**                          | **Obrigatoriedade** |
| ------------- | -------- | -------------------------------------- | ------------------- |
| `companyId`   | `string` | Identificador único da empresa (UUID). | _Obrigatório_       |

---

### Exemplo de Payload (Request Body)
#### Cadastro de um Departamento
```json
{
  "name": "Departamento de TI",
  "description": "Responsável por gerenciar a infraestrutura de tecnologia."
}
```

### Parâmetros do Payload

|**Chave**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`name`|`string`|Nome do departamento.|_Obrigatório_|
|`description`|`string`|Descrição do departamento.|_Opcional_|

---

### Exemplo de Resposta

#### Sucesso - `201 Created`
Se a criação for bem-sucedida, a resposta será:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174999",
  "name": "Departamento de TI",
  "description": "Responsável por gerenciar a infraestrutura de tecnologia.",
  "company": {
		"id": "123e4567-e89b-12d3-a456-426614174999", 
		"name": "Empresa Exemplo",
		"description": "contato@empresaexemplo.com",
  },
  "createdAt": "2024-11-22T16:38:25.309715",
  "updatedAt": "2024-11-22T16:38:25.309715"
}
```

--- 

### Exceções

#### Erro de Autorização - **401 Unauthorized**
Caso o token de autenticação seja inválido ou esteja ausente:
```json
{
  "status": 401,
  "message": "Unauthorized",
  "details": "You must be authenticated to access this resource.",
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário não seja o dono da empresa:
```json
{
  "status": 403,
  "message": "Forbidden",
  "details": "You do not have permission to create a department for this company.",
  "timestamp": "2024-11-22T16:38:25.309715"
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

#### Erro de Validação - **400 Bad Request**
Caso algum campo do payload seja inválido ou mal formatado:
```json
{
  "status": 400,
  "message": "Validation error",
  "errors": [
    "name: name is required"
  ],
  "timestamp": "2024-11-22T16:38:25.309715"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas o dono ou administradores da empresa pode criar departamentos.
- Certifique-se de que o `companyId` fornecido é um UUID válido.