#api #departamento #empresa #put

### Propriedades

| endpoint  | `/company/{companyId}/department/{departmentId}`                               |
| --------- | ------------------------------------------------------------------------------ |
| método    | **PUT**                                                                        |
| cabeçalho | `Authorization: Bearer <token>`                                                |
| corpo     | [PAYLOAD](#Exemplo%20de%20Payload%20(Request%20Body)#Atualizar%20Departamento) |

---

### Descrição

Esta rota permite atualizar as informações de um departamento específico dentro de uma empresa. Apenas o dono da empresa (usuário autenticado) ou um administrador vinculado à empresa podem realizar essa operação. Os identificadores únicos da empresa (`companyId`) e do departamento (`departmentId`) devem ser fornecidos como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`companyId`|`string`|Identificador único da empresa (UUID).|_Obrigatório_|
|`departmentId`|`string`|Identificador único do departamento (UUID).|_Obrigatório_|

---

### Exemplo de Payload (Request Body)
#### Atualizar Departamento
```json
{
  "name": "Novo Nome do Departamento",
  "description": "Descrição atualizada do departamento."
}
```

### Parâmetros do Payload

|**Chave**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`name`|`string`|Nome atualizado do departamento.|_Opcional_|
|`description`|`string`|Descrição atualizada do departamento.|_Opcional_|

---
### Exemplo de Resposta

#### Sucesso - `200 OK`
Se a atualização for bem-sucedida, a resposta conterá os dados atualizados do departamento:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174999",
  "name": "Novo Nome do Departamento",
  "description": "Descrição atualizada do departamento.",
   "company": {
		"id": "123e4567-e89b-12d3-a456-426614174999", 
		"name": "Empresa Exemplo",
		"description": "contato@empresaexemplo.com",
  },
  "createdAt": "2024-11-22T16:38:25.309715",
  "updatedAt": "2024-11-23T10:15:00.123456"
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
  "timestamp": "2024-11-23T10:15:00.123456"
}
```

#### Permissão Negada - **403 Forbidden**
Caso o usuário não seja o dono da empresa ou um administrador vinculado a ela:
```json
{
  "status": 403,
  "message": "Forbidden",
  "details": "You do not have permission to update this department.",
  "timestamp": "2024-11-23T10:15:00.123456"
}
```

#### Departamento Não Encontrado - **404 Not Found**
Caso o `departmentId` fornecido não corresponda a nenhum departamento vinculado à empresa:
```json
{
  "status": 404,
  "message": "Department not found",
  "details": "The department with the given ID does not exist in the specified company.",
  "timestamp": "2024-11-23T10:15:00.123456"
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
    "name: must be a non-empty string"
  ],
  "timestamp": "2024-11-23T10:15:00.123456"
}
```

--- 
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas usuários autenticados que sejam donos da empresa ou administradores vinculados a ela podem acessar esta rota.
- Apenas os campos fornecidos no payload serão atualizados. Campos não incluídos no payload permanecerão inalterados.
- Certifique-se de que os identificadores fornecidos (`companyId` e `departmentId`) sejam UUIDs válidos.