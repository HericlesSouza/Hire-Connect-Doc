#api #departamento #empresa #delete

### Propriedades

|endpoint|`/company/{companyId}/department/{departmentId}`|
|---|---|
|método|**DELETE**|
|cabeçalho|`Authorization: Bearer <token>`|

---

### Descrição

Esta rota permite deletar um departamento específico dentro de uma empresa, com a restrição de que o departamento não deve possuir freelancers associados. Apenas o dono da empresa (usuário autenticado) ou um administrador vinculado à empresa podem realizar essa operação. Os identificadores únicos da empresa (`companyId`) e do departamento (`departmentId`) devem ser fornecidos como parte da URL.

---

### Parâmetros da Rota

|**Parâmetro**|**Tipo**|**Descrição**|**Obrigatoriedade**|
|---|---|---|---|
|`companyId`|`string`|Identificador único da empresa (UUID).|_Obrigatório_|
|`departmentId`|`string`|Identificador único do departamento (UUID).|_Obrigatório_|

---

### Exemplo de Resposta

#### Sucesso - `204 No Content`
Se a exclusão for bem-sucedida, a resposta será:
```http
204 No Content
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
  "details": "You do not have permission to delete this department.",
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
  "timestamp": "2024-11-23T10:15:00.123456"
}
```

#### Conflito - **409 Conflict**
Caso o departamento tenha freelancers associados e, portanto, não possa ser deletado:
```json
{
  "status": 409,
  "message": "Cannot delete department",
  "details": "The department has freelancers associated and cannot be deleted.",
  "timestamp": "2024-11-23T10:15:00.123456"
}
```

---
### Notas Adicionais
- O token JWT deve ser incluído no cabeçalho da requisição utilizando o formato `Authorization: Bearer <token>`.
- Apenas usuários autenticados que sejam donos da empresa ou administradores vinculados a ela podem acessar esta rota.
- Certifique-se de que os identificadores fornecidos (`companyId` e `departmentId`) sejam UUIDs válidos.
- Antes de realizar a exclusão, o sistema verifica se o departamento possui freelancers associados. Caso tenha, a exclusão será bloqueada.